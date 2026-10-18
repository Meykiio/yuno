# Technical design

How a production version would be built. This is a design, not an implementation: the repository currently contains one self-contained HTML proof of concept and no server.

Anyone competent could build this. That is the point of writing it down.

---

## 1. Stack

| Layer | Choice | Reason |
|---|---|---|
| Frontend | React 18 + TypeScript + Vite + Tailwind | Small widget bundle, fast to embed, boring and well understood |
| Backend | Supabase: Postgres, Auth, Edge Functions (Deno) | Postgres is required for the reconciliation queries. Row-level security is enforced per owner, which is the whole multi-tenant story |
| Tokens | JWT HS256 via WebCrypto | Single-use pass tokens, no extra dependency |
| Abuse control | Proof of work in a Web Worker | SHA-256 with a difficulty target, to make bulk requests cost something |
| Deployment | Vercel | Same as everything else |

One deployable. No services split out of a monolith before a measured reason exists.

## 2. Endpoint surface

Everything a visitor touches goes through Edge Functions with the service role. Everything an owner touches goes through the Supabase client under row-level security. That split is the entire authorisation model.

| Function | Caller | Purpose |
|---|---|---|
| `challenge` | widget | Check sitekey and origin, rate limit, pick items, issue a challenge plus proof-of-work salt |
| `submit` | widget | Verify proof of work, score the control item, compute `p_human`, **store all responses**, apply policy, return a token or a generic failure |
| `siteverify` | the site's own server | Takes a secret and a token, returns success, score, solver class and hostname. Single use |
| `aggregate` | submit, plus an owner button | Recompute item states |
| `export` | owner, authenticated | Stream the JSONL datasets |
| `reference-agent` | owner, authenticated, optional | Run a configured model over items to compare against human labels |
| `health` | anyone | Liveness plus database connectivity |

`submit` stores every response before it decides anything. If the policy rejects the visitor, the row is already written. That ordering is the product, not an implementation detail, and it is the one place a well-meaning refactor would break the business.

## 3. The submit path, in order

1. Consume the challenge atomically, so a token cannot be used twice.
2. Verify proof of work.
3. Score the control item.
4. Compute `p_human` from derived features.
5. **Insert one row per answered item.**
6. Apply the policy and return a token, or return a generic failure body.
7. Recompute state for the touched items.

The failure response never includes a reason. Telling a caller why they failed is a free oracle for tuning an attacker.

## 4. Data model

Five tables. The full migration belongs in `supabase/migrations`, versioned, never edited in place.

```sql
create table tasks (
  id uuid primary key default gen_random_uuid(),
  owner_id uuid not null references auth.users(id) on delete cascade,
  name text not null,
  instruction text not null,
  options jsonb not null,                        -- ["positive","neutral","negative"]
  sitekey text unique not null default encode(gen_random_bytes(12), 'hex'),
  secret_hash text not null,                     -- plain secret shown once, never stored
  allowed_hosts text[] not null default '{}',
  policy text not null default 'work_toll',      -- human_only | work_toll | collect_only
  votes_target smallint not null default 3,
  human_threshold numeric not null default 0.7,
  agent_threshold numeric not null default 0.3,
  consent_text text not null,
  active boolean not null default true,
  created_at timestamptz not null default now()
);

create table items (
  id uuid primary key default gen_random_uuid(),
  task_id uuid not null references tasks(id) on delete cascade,
  payload jsonb not null,                        -- {"text": "..."} or {"image_url": "...", "alt": "..."}
  is_gold boolean not null default false,
  gold_answer text,
  state text not null default 'collecting',      -- collecting|agreed|contested|final|unresolved
  human_label text, human_conf numeric, n_human int not null default 0,
  agent_label text, agent_conf numeric, n_agent int not null default 0,
  final_label text, final_conf numeric,
  gold_served int not null default 0,
  created_at timestamptz not null default now(),
  check (not is_gold or gold_answer is not null)
);

create table challenges (
  id uuid primary key default gen_random_uuid(),
  task_id uuid not null references tasks(id),
  human_item_ids uuid[] not null,
  agent_item_ids uuid[] not null,
  pow_salt text not null, pow_bits smallint not null,
  ip_hash text not null, ua_hash text not null,
  lanes_used text[] not null default '{}',
  issued_at timestamptz not null default now(),
  expires_at timestamptz not null
);

create table responses (
  id uuid primary key default gen_random_uuid(),
  challenge_id uuid not null references challenges(id),
  task_id uuid not null references tasks(id) on delete cascade,
  item_id uuid not null references items(id) on delete cascade,
  lane text not null,                            -- human | agent
  source text not null default 'observed',       -- observed | reference | simulated
  model_name text,
  answer text not null,
  rt_ms integer,
  gold_ok boolean,                               -- only set on control items
  pair_gold_ok boolean not null,
  p_human numeric not null,
  flagged boolean not null default false,
  signals jsonb not null,                        -- derived features only
  ip_hash text not null,
  created_at timestamptz not null default now(),
  unique (item_id, ip_hash, lane)
);

create table verifications (
  id uuid primary key default gen_random_uuid(),
  challenge_id uuid not null references challenges(id),
  jti text unique not null, lane text not null, solver_class text not null,
  score numeric, token_used boolean not null default false,
  created_at timestamptz not null default now()
);
```

Indexes go on the real query patterns only, once they are observed: item lookup by task and state, responses by task and item, challenge lookups by hashed IP and time. Guessing indexes upfront is how a write-heavy table ends up paying for indexes nobody queries.

## 5. Security

**Row-level security on every table.** No exceptions, and no policy of `using (true)` anywhere in the codebase.

- `tasks`: `owner_id = auth.uid()` for all operations.
- `items` and `responses`: readable and writable only where the parent task belongs to the caller.
- `challenges` and `verifications`: **no policies at all.** Service role only. They hold hashed IPs and are never read from a client.

**Secrets.** The site secret is shown once at task creation and stored hashed. Missing secret means HTTP 500 with a generic body. Never log a token, a secret, or a hashed IP alongside an item payload.

**Visitor data.** Hashed IP and hashed user agent with a server-side salt. Derived features only: counts, variances, timings, path length. Raw pointer traces stay in the browser. No cookies, no persistent visitor id.

**Abuse.** One vote per item per hashed IP per lane, enforced by the unique constraint rather than by application code, because application code is where rate limits get forgotten. Proof-of-work at 16 leading zero bits by default. Rate limit 30 challenges per minute per hashed IP. Gold items are rotated and capped at 200 uses each, with a pool of at least 10, so one control item is never shown to everybody.

**Origin checks.** `challenge` and `submit` validate the request origin against the task's `allowed_hosts`. A widget key leaked to another domain must not produce rows.

## 6. Token

HS256 JWT, signed with WebCrypto so there is no JWT library in the bundle.

```
claims: { jti, sub: challenge_id, sitekey, action, origin, lane, solver_class, score, iat, exp }
exp = iat + 120
```

Bound to sitekey and origin, single use enforced in `siteverify`. Two minutes is enough for a round trip and short enough that a leaked token is worthless quickly.

## 7. Widget

A single React component with five states: idle, question 1, question 2, verifying, and failed (which issues a fresh challenge on retry).

Requirements that are not negotiable:

- Consent text above question 1, plus a "No thanks" link. Declining replaces the new item with a second control item: the gate still works, no data is collected.
- 44px minimum tap targets, full keyboard operation, `aria-live` announcements, visible focus ring.
- Works at 360px wide with no horizontal scroll.
- Respects `prefers-reduced-motion`.
- Images carry alt text, and image items require a text alternative for agents that do not read images.
- The agent panel is rendered from JavaScript after load, never in the served HTML, and never in the main content flow.

## 8. Build order

Cut line: if time or credits run out, stop after M6 and the product still demos end to end.

| Milestone | What ships |
|---|---|
| M0 | Repository, docs, migrations directory, blank deploy |
| M1 | Migrations, RLS, auth, task and item CRUD, CSV import, seed data |
| M2 | `challenge` and `submit`: control scoring, proof of work, response storage, rate limits |
| M3 | Widget: human lane, consent, honeypot, agent panel, derived features |
| M4 | Scoring constants, policies, tokens, `siteverify` |
| M5 | Aggregation, item states, results view, exports |
| M6 | Demo site, simulated solvers behind a test panel, end-to-end run |
| M7 | Arbitration polish, optional reference-model runner behind an environment flag |
| M8 | Accessibility pass, privacy page, README with honest limits |

## 9. How it gets verified

Gates before anything is called working:

| Gate | Threshold |
|---|---|
| People finish both questions | median 6 seconds or less, 90% or more complete |
| Gate pass rate for people on a correct control item | 95% or more |
| Crowd consensus against known answers | 90% or more agreement in a test run |
| People and agents separated | at one threshold, 90%+ of real people above it and 15% or less of real agents above it. **If this fails, no separation is claimed** |
| Reconciliation pipeline | a contested item reaches final without manual SQL |
| Exports | valid JSONL, no IP, no user agent, simulated rows excluded |
| Outside signal | a pilot business or an advisor states a price per 1,000 items, or states there is none |

If the separation gate fails, ship it as "collect everything, separate by probability", and say so in the README. Hiding it would make the one asset this project has before revenue worthless.