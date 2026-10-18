# Business

How value is created, what could be sold, and what is still unknown. Nothing in this document is a validated price. The pricing section explains why, and what to do about it.

---

## 1. Where the value actually comes from

Three different things happen in one interaction, and they have very different values. Confusing them is the main way a version of this product loses money.

**Value 1: the gate itself.** The business needed a way to tell people and programs apart on a route it wanted protected. That need already exists and already has an incumbent. Yuno does not win here on price, and should not try. The gate is the entry ticket, not the product.

**Value 2: the labels.** A row of labeled data that the business owns, in its own domain, produced by its own traffic, with a record of where every row came from. This is real, and it is what the business would otherwise pay a labeling vendor for.

**Value 3: the disagreements.** Rows where a model and people disagreed, settled by people, with provenance. This is the expensive, scarce, defensible output. A labeling vendor does not produce it. An API call does not produce it. It exists because the same item was answered by both.

**The trap:** value 2 alone is a commodity, and the commodity floor is close to zero. A competent model labels English sentiment for a fraction of a cent per item over an API. Any business model that says "we sell labeled data" without a wedge is competing against a price that goes to zero.

That is why the pitch is never "we sell labels". It is one of:

- We sell data about **your** domain that no general model has access to.
- We sell **verified failure cases** for models in your domain.
- We convert your **agent traffic** into work instead of a block page.

## 2. Volume math

Straight arithmetic, using the demo's own numbers.

**Human answers.** One new item per visitor. Each item needs 3 human votes before it is labelled.

| Monthly visitors | Answers per month | Labelled items per month |
|---|---|---|
| 1,000 | 1,000 | about 330 |
| 10,000 | 10,000 | about 3,300 |
| 100,000 | 100,000 | about 33,000 |

**Agent answers are a multiplier, not a substitute.** An agent visit answers 5 items instead of 1, so 40% agent traffic on a 10,000-visitor site adds 20,000 answers. Those answers do **not** become settled labels. Humans settle labels. What agent traffic produces is a cheap, constant stream of *disagreement candidates*, which is the scarce output.

**Read this honestly:** at 10,000 visitors a month the volume is small. A single site is not a business. Either the business needs real traffic, or it needs the data to be scarce enough that small volume is still valuable. Both are true for some sites and false for most.

**Two levers make small sites viable.** Gating a high-traffic route instead of a low-traffic one multiplies volume by whatever the traffic ratio is. And task design changes the value per item far more than volume does.

## 3. The three products

### Product 1: Self-serve data collection

The business writes its own task, embeds the widget, and exports rows from its own traffic.

- **Buyer:** any site with traffic and a task it needs labelled. E-commerce, marketplaces, directories, job boards, support-heavy SaaS.
- **Value:** rows nobody else can produce, because they come from that site's traffic and its context.
- **What it competes with:** an internal labelling budget, a labelling vendor, or an API call that is cheap and wrong on the hard 20%.
- **Failure mode:** a site with 500 visitors a month finds the data volume embarrassing and stops.

### Product 2: Agent toll

Agent traffic is turned into work. The agent reads the disclosed note and labels items; the site decides access separately, so it can block the agent and still keep the answers.

- **Buyer:** sites whose agents scrape, fetch or crawl in volume: marketplaces, price aggregators, news, directories, anything with a public API that is being scraped anyway.
- **Value:** the agent stops being a cost centre. It becomes a data source that was going to hit the server regardless.
- **What it competes with:** blocking (which loses the race as agents improve), and ignoring it (which is status quo and produces nothing).
- **This is the most novel claim and the least proven.** It rests on two untested assumptions: that agents will comply, and that their answers are worth having. Both are being measured.

### Product 3: Model evaluation reports

A standing, per-domain record of how often AI matches human judgement, with the disagreement cases attached.

- **Buyer:** teams shipping AI into a regulated or high-stakes domain, and their customers, who increasingly ask "can we trust this model here?"
- **Value:** evidence, not opinion. A per-domain agreement rate with named failure cases is a document a compliance reviewer accepts and a benchmark dashboard does not replace.
- **What it competes with:** internal evaluation sets that go stale, and generic public benchmarks that do not describe the domain.
- **Failure mode:** this is the slowest to build and needs the most coverage before it says anything. It is also the product with the longest sales cycle and the least price pressure.

## 4. Pricing

**There is no validated price, and there cannot be one yet.** Writing a price here would be fiction. Here is the honest reasoning instead.

**The floor is near zero.** If a task is something a model does well, a buyer can produce the same row with an API for a fraction of a cent. Therefore:

- Easy, high-volume tasks: no margin. Do not sell these.
- Hard, domain-specific tasks: a real premium, because the buyer cannot get these rows any other way.
- Disagreement and verified failure cases: the highest premium, because nothing else produces them.

**The only honest way to price is against two numbers the buyer already has:**

1. **What they spend today** for the same volume of human review: staff time, or a labelling vendor quote. Get three quotes. That is their budget line.
2. **What an API call costs** for the same volume. That is the floor. The gap between the two is the entire addressable margin.

Price inside that gap. A common mistake is to price against the first number and ignore the second, then discover the buyer can automate it next quarter.

**The experiment.** Before building product 1 at scale, ask the question in the demo directly: of the three products, which would you pay for, and roughly how much per 1,000 labelled items? Then ask a second business the same. Two data points do not validate a market, but they will tell you whether you are talking to a budget line or to a nice idea.

## 5. Why a business would pay at all

The objection to answer, every time: *"An LLM already does this."*

Four reasons the answer still holds, and one where it does not.

1. **Context.** A support desk knows that "SC-4471" is a subscription code and "not as described" means the size chart is wrong. No general model has that, and none will.
2. **The last stretch.** The easy 80% is already automated by the buyer. The remaining 20% is the entire problem, and it is where the labels have value.
3. **Provenance.** A row that says where it came from, who answered, and whether a model disagreed is auditable. An API output is not. In a regulated domain, provenance is the product.
4. **Consent and access.** Some sources cannot be scraped. Data from your own users, answering your own question, is clean by construction.
5. **Where it does not hold:** tasks a model does well. There is no defence there, and pretending otherwise is how this fails.

## 6. What is not validated

Stated plainly, because an advisor will ask:

- Buyer demand: unknown. Two conversations are not validation.
- Price per item: unknown.
- Whether a business owner sees "data from my own sign-ups" as valuable enough to gate a route they care about converting.
- Whether agents comply at a rate that makes the toll real.
- Whether the moat exists. Right now there is no technical moat. The defensible asset, if one forms, is the accumulated disagreement corpus per domain, and that requires volume nobody has yet.

## 7. The decision points

| Decision | Trigger | What it depends on |
|---|---|---|
| Continue or stop | The calibration run shows agents find and follow the disclosed note | [RISKS.md](RISKS.md) |
| Build product 1 or product 2 | An advisor or a pilot business states a price per 1,000 items | This document |
| Self-serve or sales-led | Whether a business owner can write a good task unaided | [USE-CASES.md](USE-CASES.md) |
| Sell labels or sell verification | Which of the two a buyer will pay a premium for | Section 1 |

None of these are decided yet. The honest summary: the mechanic is proven by someone else at enormous scale, the agent traffic is real, the hybrid quality result is peer-reviewed, and the commercial half of the claim has not been tested with a single paying buyer.