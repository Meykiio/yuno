# Risks and open questions

The honest list. Ordered by how much damage each one does if it is true.

---

## 1. Will real agents find and follow the disclosed note?

**Status: untested. This is the highest-value unknown in the project.**

The agent lane works by being invisible to people and readable by programs that parse page code. That means it is invisible to:

- **Screenshot agents.** They see a picture of the page. There is nothing there.
- **Accessibility-tree agents.** Content marked `aria-hidden` is correctly excluded from the accessibility tree, because that is what the attribute is for.
- **Agents that render and read only visible text.**

It is visible to agents that read the raw DOM or fetch the HTML.

Three outcomes are possible, and they point in very different directions:

| Outcome | Meaning |
|---|---|
| Most agents find it | The toll works. This is the best case and the whole of product 2. |
| A minority find it | Partial toll. Still valuable, and the way to detect the rest is the behaviour heuristic. |
| Almost none find it | Product 2 collapses. The disclosed note becomes a fairness disclosure with no commercial effect. |

**How it gets tested:** a calibration run with at least three real agents (a browser-use setup, a commercial browser agent, and a raw-DOM fetcher) running 20 sessions each through the real widget, counting how many ever touch the agent lane and how many answer the visible widget instead. No claim about agent compliance goes in any pitch until this number exists.

## 2. Will the behaviour heuristic separate people from agents?

**Status: untested heuristic. No separation is claimed.**

`p_human` combines answer speed, pointer-movement counts, synthetic-event detection and the automation flag. It will work on naive automation. It will not reliably work on a capable agent deliberately imitating a human, because there is no deeper signal in the browser that distinguishes them, only behavioural patterns that can be copied.

**What the design survives:** probabilities plus provenance. Every row carries its score, so the dataset can be filtered at any threshold, and a buyer can see exactly what was inferred and from what. That holds even if the score is mostly wrong.

**What it does not survive:** any claim that a dataset is "human data". If the heuristic fails, the product becomes "collect everything, separate by probability, report the separation quality honestly", which is a weaker but still sellable position.

**How it gets tested:** 30 real people on desktop and phone, and at least 3 real agents, 20 runs each, through the real widget, with ground truth recorded manually.

## 3. Is collecting these answers and signals lawful?

**Status: unresolved. Counsel needed before real users in the EU or North Africa.**

Open questions, in order of exposure:

- Does a behavioural signal plus a submitted answer constitute personal data under GDPR? The conservative reading says yes, which means a lawful basis, a notice, a retention limit, and a deletion path.
- Is opt-in required, or does legitimate interest carry it? Not settled. The design ships a notice and a "No thanks" path that still lets the gate work without collecting anything, which is the strongest position available at MVP.
- North African data-protection law, including Algeria's Law 18-07 as amended, has its own registration and localisation expectations. Not researched to conclusion here.
- **The agent note is the sharp edge.** Collecting a person's answer is ordinary. Asking an automated program to answer on your behalf and then storing what it said raises questions about whether the agent's operator agreed. The note is disclosed precisely so the answer is yes, and that disclosure has to stay in the design. Removing it to make the toll sneakier converts a legal risk into an ethical one and buys nothing.

## 4. Does a business owner actually want data from its own sign-ups?

**Status: untested with any real buyer.**

The failure mode is mundane: an owner hears "collect labeled data from your traffic", says "interesting", and never embeds the widget because their real problem is conversion, not data. The gate has to feel like it does not cost anything, and it has to be understood in under a minute.

**How it gets tested:** one pilot business, one live task, 30 days. Instrument the drop-off at the gate. If completion is under 90% at a median of 6 seconds, the gate is too expensive and the whole volume model changes.

## 5. AI labels beat crowd labels on easy tasks

**Status: confirmed for easy tasks, which is why easy tasks are excluded.**

The peer-reviewed comparison found the best individual crowd worker could beat ChatGPT, and that a weak LLM label rarely helps. If the task is easy, the API is both cheaper and better.

**Consequence, already applied:** the use cases in this repo are selected for tasks where the model is weak and people are strong. If a buyer picks an easy task, Yuno has no product to sell them and should say so instead of taking the money. That is a positioning constraint, not a bug.

## 6. Volume is too small to matter

**Status: arithmetic, not opinion. See [BUSINESS.md](BUSINESS.md) section 2.**

A 10,000-visitor site produces roughly 3,300 labelled items a month. Most businesses do not have 10,000 visitors a month on the pages they would gate. Single-site volume is not a business.

**Mitigations:** gate high-traffic routes rather than login; focus on tasks where small volume is still valuable because the data is scarce; accept that the volume story requires many businesses on one network, which is a later-stage problem, not an MVP one.

## 7. Search engines treat hidden text as cloaking

**Status: unresearched, low but non-zero risk.**

The agent note is hidden text. Search engines penalise hidden text. Mitigation: render it from JavaScript inside the widget after load, never in the served HTML, and keep it out of the main content flow. Must be tested against a real crawler before launch, not assumed.

## 8. The ethics of the whole thing

Worth stating because it will be asked, and because the answer shapes the design rather than decorating it.

- A visitor is told, in the widget, that their answer may be used as training data, and is given a way to decline that still lets them pass.
- An agent is told the same thing in the same terms, in a form it can read. Nobody is deceived.
- A blocked agent's answers are still kept. This is the most aggressive part of the design, and it only stays defensible because the terms were disclosed in advance. If that disclosure is ever removed, this becomes the thing that ends the project.

## 9. What would prove this wrong

Three concrete results that would end it, not just weaken it:

1. **Agents ignore the note in more than 90% of sessions.** Product 2 is dead. What remains is product 1, a custom-task widget competing with free APIs, which is a hard business.
2. **No pilot business will name a price for the data after 30 days.** There is no market for this output, and the remaining option is selling the gate itself, where the incumbent is free.
3. **The calibration run cannot separate people from agents at any threshold.** Then the headline claim collapses into "labeled data with a guess about who labelled it", which buyers already get cheaper elsewhere.

If any of those land, say so publicly in this repository rather than quietly reframing. The credibility of the honest version is the only asset this project has before it has revenue.