# Evidence

Every load-bearing claim in this repository, with the source and what the source actually proves. Where a source is weaker than the claim it is attached to, that is stated.

Sources were read on 2026-10-17 and re-verified on 2026-10-18. Links were checked and return HTTP 200 (the Science DOI returns 403 to automated requests because of its bot protection, and works normally in a browser).

---

## 1. The mechanic: reCAPTCHA proved it

| Claim | Source | Strength |
|---|---|---|
| reCAPTCHA was built in 2007 by Luis von Ahn and colleagues at Carnegie Mellon, after noticing that millions of people were already typing distorted characters for nothing | [Science 1160379](https://www.science.org/doi/10.1126/science.1160379) | Primary, peer-reviewed |
| The method shows two words: one whose answer is known (the control, used to verify the user) and one the OCR software could not read (the work) | [Science 1160379](https://www.science.org/doi/10.1126/science.1160379), [Google's explainer, archived](https://web.archive.org/web/20140219113020/www.google.com/recaptcha/learnmore) | Primary |
| Transcription accuracy exceeded 99%, matching professional human transcribers | [Science 1160379](https://www.science.org/doi/10.1126/science.1160379) | Primary, measured |
| Deployed on more than 40,000 sites, having transcribed over 440 million words | [Science 1160379](https://www.science.org/doi/10.1126/science.1160379) | Primary, measured |
| Roughly 200 million solved every day, about 10 seconds each, more than 150,000 hours of human time per day | [Google's explainer, archived](https://web.archive.org/web/20140219113020/www.google.com/recaptcha/learnmore), [von Ahn, Human Computation, DAC 2009](https://dl.acm.org/doi/pdf/10.1145/1629911.1630023) | Primary, self-reported by the operator |
| Google acquired reCAPTCHA in September 2009 | [NYT via ReadWriteWeb, 2009](https://archive.nytimes.com/www.nytimes.com/external/readwriteweb/2009/09/16/16readwriteweb-google-acquires-recaptcha-to-fight-spam-and-66930.html) | Secondary but contemporaneous |
| It became the primary method Google used for Google Books OCR | [NYT 2011](https://www.nytimes.com/2011/03/29/science/29recaptcha.html) | Secondary |

**What this proves:** the two-jobs-in-one mechanic works at global scale, with no extra friction for the visitor, and the resulting data is accurate enough to be worth 99% against professional transcribers.

**What it does not prove:** that the model transfers to arbitrary custom tasks. Every word reCAPTCHA served came from a corpus of scanned documents that could be verified automatically. A business's own task cannot. That gap is exactly what the design has to close, and it is why Yuno's control item is a *known answer the business supplies*, not a machine-checkable one.

## 2. AI agents are now a large share of traffic

Source: [HUMAN Security, State of Agentic Traffic, June 2026](https://www.humansecurity.com/learn/blog/state-of-agentic-traffic-june-2026-browser-agent-tooling-for-developers-is-catching-on-fast/) (published 2026-07-06). Figures re-verified against the page on 2026-10-18.

| Figure, June 2026 | Share of agentic traffic |
|---|---|
| Comet Browser | 47.6% |
| Claude | 20.8% |
| Atlas | 16.5% |
| ChatGPT Agent | 7.0% |
| Browserbase (developer platform) | 2.8%, up 4x month over month |
| Genspark | 2.6% |
| Browser Use | 1.2% |
| AgentCore | 0.6% |

By sector: e-commerce 43.8%, media 41.3%, travel 13.5%.
By route: product and search 79%, account routes 5.7%, authentication 5.38%, checkout 2.34%.

**What this proves:** agent traffic is a large, measurable, growing share of real web traffic, concentrated exactly on the pages a data gate would sit on.

**What it does not prove:** anything about Yuno. This is one vendor's observation of traffic crossing its own network, with its own classification method. The figures are a vendor's marketing-adjacent reporting, so treat them as directional, not as an audited market share. No claim in this repository depends on the exact percentages; only on the order of magnitude and the route distribution, which is what matters for gate placement.

## 3. Mixing AI and human labels improves datasets

Source: [arXiv 2401.09760](https://arxiv.org/abs/2401.09760), Jiyi Li, ICASSP 2024, *A Comparative Study on Annotation Quality of Crowdsourcing and LLM via Label Aggregation*.

Verbatim finding: adding LLM labels from good LLMs to existing crowdsourcing datasets **enhances the quality of the aggregated labels**, which is also higher than the quality of the LLM labels themselves.

Also relevant from the same paper: aggregated labels, not individual labels, are what actually ship in a dataset, so the aggregation method matters more than worker quality.

**What this proves:** the hybrid design (people label, AI labels, humans arbitrate the difference) is supported by peer-reviewed work, not just intuition.

**What it does not prove:** that it holds for a business's private custom task, where the model has no context advantage at all. The paper's setting is existing NLP benchmarks with models that do well on them. Yuno's wedge is the opposite case, tasks where the model is weak. Extrapolating "hybrid helps" to "hybrid helps when the model is the weak party" is a hypothesis, not a finding.

## 4. Agents can declare themselves, and agents are already being courted

| Claim | Source |
|---|---|
| Agents can sign HTTP requests so sites can verify who they are (Web Bot Auth). ChatGPT's agent, Goose, Browserbase and Anchor Browser sign their requests | [Cloudflare](https://blog.cloudflare.com/signed-agents/) |
| Reverse-CAPTCHA work exists: tasks designed to be fast for AI and slow for humans | [aCAPTCHA, arXiv 2603.07116](https://arxiv.org/abs/2603.07116) (submitted 2026-03-07). a formalises a three-class taxonomy (human, script, agent) and a time-constrained verification game. Preliminary: the human side is modelled, not measured |
| hCaptcha sells data labeling, but sales-led only: "speak with a domain expert", run a test job, then pay. No self-serve task builder on the public page | [hCaptcha labeling](https://www.hcaptcha.com/labeling) |

**What this proves:** the "agents sign in as agents" primitive already exists, and competitors verify agent identity without collecting labels from them. That is the gap Yuno aims at.

**Caution on aCAPTCHA:** it is a preprint, and its human-side evaluation is modelled rather than measured. It is evidence that the direction is being researched, not evidence that the approach works.

## 5. Claims this repository does not make

To be explicit about the absence of evidence:

- No source establishes that people will find and follow a disclosed note that is invisible to them. Untested.
- No source establishes a price per labeled item for this kind of data. Untested.
- No source establishes that the behaviour heuristic separates sophisticated agents from real people. Untested.
- No legal opinion supports collecting visitor answers and behavioural signals in the target markets. Pending.
- No source claims a product combines custom self-serve tasks, collection from both people and agents on one gate, and human reconciliation. **Absence of a find is not absence of a product.** That search must be redone immediately before any "first ever" claim is made.

## 6. Citation hygiene

Every number in the demo and in these docs traces to a row above. Anything not in this file is either arithmetic performed on those rows (and shown) or a labelled hypothesis. When a vendor figure is quoted, the vendor is named in the same sentence, so no claim in this repository is stronger than its source.