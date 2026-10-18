# Yuno

**Every website already makes people prove they are human. Yuno makes that moment produce something instead of throwing it away.**

Live demo: **[meykiio.github.io/yuno](https://meykiio.github.io/yuno)** (one page, five minutes, no install)

Status: concept proof. The demo runs. There is no production system, no user base, and no revenue. Everything below is either verified fact with a source, or labelled as a hypothesis.

---

## The idea in one paragraph

A CAPTCHA asks a person a question a machine cannot answer, and throws the answer away. reCAPTCHA proved that the same ten seconds can also do real work: it showed two words, one the computer already knew (that one checks the visitor) and one the computer could not read (that one is the work). Millions of people per day were transcribing the New York Times archive and Google Books without being paid. Yuno keeps that trick and changes four things: the business chooses its own task and keeps the data, AI agents are told the deal in plain words and their answers are kept too, and where people and AI disagree, more people vote and the disagreement itself becomes the valuable output.

## The reCAPTCHA precedent, with numbers

| Fact | Source |
|---|---|
| Luis von Ahn and his team at Carnegie Mellon built reCAPTCHA in 2007 after noticing millions of people were already typing distorted words for nothing | [Science, 2008](https://www.science.org/doi/10.1126/science.1160379) |
| Roughly 200 million were solved every day, about 10 seconds each, over 150,000 hours of human time per day | [Google's own explainer](https://web.archive.org/web/20140219113020/www.google.com/recaptcha/learnmore) |
| Accuracy exceeded 99%, matching professional human transcribers, across 40,000+ sites and 440 million words transcribed | [Science, 2008](https://www.science.org/doi/10.1126/science.1160379) |
| Google acquired reCAPTCHA in September 2009, and it became the backbone of Google Books digitization | [NYT via ReadWriteWeb, 2009](https://archive.nytimes.com/www.nytimes.com/external/readwriteweb/2009/09/16/16readwriteweb-google-acquires-recaptcha-to-fight-spam-and-66930.html) |

The lesson: an obstacle every visitor already pays for can be turned into output, at no extra cost to the visitor.

## What Yuno changes

| | reCAPTCHA | Yuno |
|---|---|---|
| Who picks the task? | The platform (digitize books) | Each business picks its own task |
| Who owns the data? | The platform | The business that owns the site |
| What about AI agents? | Block them | Tell them the terms in plain words and keep their answers |
| When people and AI disagree? | Not part of the design | More people vote. The disagreement is the most valuable row |

## The three-step cycle (this is what the demo shows)

1. **A person visits.** A Yuno box asks two questions: one whose answer is already known (this checks the visitor), and one new question (this is the data). Both answers are stored. The site also stores a probability, `p_human`, built from simple behaviour signals. It is a probability, never a verdict, so the data is labelled `human-likely`, never "verified human".
2. **An AI agent visits.** The agent reads the page code, finds a disclosed note that humans cannot see, and answers the same task. The site owner picks the rule: humans only, anyone who does the work, or collect only. Either way the answers are stored, because **blocking and collecting are two separate decisions**.
3. **The business gets the result.** For every item it sees what people said, what the AI said, and a status: needs more votes, agreed, disagreement, settled, unclear. Disagreements are re-served to more people until settled. The export is JSONL with provenance on every row, plus a score of how often the AI matched the final human answer.

## The three products being tested

| Product | What the buyer gets | Who buys it |
|---|---|---|
| **1. Self-serve data collection** | Labeled rows from the business's own traffic, in its own domain | Sites with real traffic and data they cannot otherwise get |
| **2. Agent toll** | Agent traffic is converted into useful work instead of being blocked | Sites whose agents scrape or fetch in volume |
| **3. Model eval reports** | Where AI models fail on real domain data, with cases | Teams shipping AI, and buyers asking "can we trust this model here?" |

Which one is real is not decided. That is the first thing being measured. See [docs/BUSINESS.md](docs/BUSINESS.md).

## Proven vs not proven

**Solid ground**
- The mechanism works at scale. reCAPTCHA proved the two-jobs-in-one trick (sourced above).
- AI agent traffic is real and growing. In June 2026, Comet Browser was 47.6%, Claude 20.8% and Atlas 16.5% of agentic traffic observed by HUMAN Security. Product and search routes took 79% of agent activity, authentication routes only 5.38%.
- Mixing AI labels with human labels improved aggregated label quality on most datasets tested ([arXiv 2401.09760](https://arxiv.org/abs/2401.09760), ICASSP 2024).
- Agents can already identify themselves with signed requests ([Cloudflare](https://blog.cloudflare.com/signed-agents/)).
- This repository contains a working mock of the entire cycle.

**Not proven, and being measured**
- Whether real agents will find and follow the disclosed note. Agents that only read screenshots will not.
- Whether the `p_human` heuristic separates real people from real agents. Until a calibration run says otherwise, no separation is claimed.
- Who pays, and how much per labeled item.
- Legal basis for collecting answers and behaviour signals under GDPR and North African data-protection law. Counsel review pending.
- Easy tasks such as English sentiment are already cheap and often done better by AI. The value is where AI is weak and people know better.

Full list, including the risks that would kill this: [docs/RISKS.md](docs/RISKS.md).

## Run the demo locally

```
git clone https://github.com/Meykiio/yuno.git
cd yuno
start poc/index.html
```

No build, no dependencies, no network calls. Open the file, pick a business, answer two questions, run the simulated agent, read the dashboard, download the JSONL.

## Documentation

| Document | What it answers |
|---|---|
| [docs/CONCEPT.md](docs/CONCEPT.md) | The full idea in plain language. Start here if the idea is new to you. |
| [docs/HOW-IT-WORKS.md](docs/HOW-IT-WORKS.md) | The mechanism in detail: two lanes, scoring, voting, reconciliation, exports |
| [docs/TECHNICAL-DESIGN.md](docs/TECHNICAL-DESIGN.md) | How a real version would be built: stack, endpoints, data model, security, milestones |
| [docs/USE-CASES.md](docs/USE-CASES.md) | Twelve industries and the dataset each one would end up owning |
| [docs/EVIDENCE.md](docs/EVIDENCE.md) | Every claim with its source, and what each source actually proves |
| [docs/BUSINESS.md](docs/BUSINESS.md) | Products, unit economics, pricing hypotheses, how value is actually made |
| [docs/RISKS.md](docs/RISKS.md) | Open questions and the risks that would kill this |
| [docs/LATER.md](docs/LATER.md) | Directions worth testing if the basics hold. Speculation, not roadmap. |
| [docs/PROJECT.md](docs/PROJECT.md) | Project state, conventions, how to contribute or run an agent on this repo |

## How to read this if you are an AI agent

Read this file first, then [docs/CONCEPT.md](docs/CONCEPT.md), then `poc/index.html` top to bottom. The page contains a plain-text summary section near the bottom for exactly this purpose. Key facts: this is a concept proof, not a product. The AI agent, the crowd of earlier visitors, and all numbers in the demo are simulated and labelled as such. The reCAPTCHA precedent, the agent-traffic figures and the arXiv results are real and sourced.

## License

Proprietary. All rights reserved. See [LICENSE](LICENSE). Reading, running and sharing the link are permitted. Reuse is not.

Built by [Sifeddine Mebarki](https://github.com/Meykiio).