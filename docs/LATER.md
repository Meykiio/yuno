# Later

Speculation, not roadmap. Nothing here is committed, scheduled, or built. It is written down so the ideas are not lost, and so nobody mistakes them for things that exist.

The bar for anything in this file: it only matters if the core works, meaning real agents do the work and a buyer will pay for the result.

---

## 1. A map of where AI is wrong, by industry

The disagreement corpus, accumulated per domain, is a standing test set describing exactly which tasks a model still fails for that industry, with named cases rather than an aggregate number.

Why it would be valuable: model builders benchmark on general datasets. What they cannot get is a domain-specific failure set with human-adjudicated ground truth, gathered continuously, without paying anyone.

Why it is hard: coverage. A report about "insurance claims" is only credible with enough rows behind it, and that volume takes many businesses. This is a coverage product, not a launch product.

## 2. An audit trail for AI

The same corpus yields a running per-domain agreement rate: how often does a model match human judgement on this kind of work, this quarter.

The buyer is not only AI teams. It is the compliance reviewer asking "can we trust this model for this use", and the procurement team asking the same question of a vendor. That question has no good answer today, and an answer backed by continuous human adjudication would sell.

## 3. Preferences, not labels

Labels ask "what is the answer". A harder and more valuable question is "which of these two answers is better", asked of real customers in the middle of a real decision: two product photos, two candidate headlines, two price framings.

Preference data is what tuning a model to human taste actually requires, it is scarce, and it can be gathered in the same five-second slot.

## 4. A shared task network

Businesses with the same underlying task pool it instead of each running out of items. A dozen job boards feeding one "what level is this job" item pool reach usable coverage; one does not.

This is also the path to a defensible position: the network's value grows with the number of participants, and the disagreement corpus is the asset that compounds.

## 5. Cheaper data for small players

Good labeled data is expensive and hoarded, and the buyers who most need it are the ones who can least afford it. A business with 5,000 visitors a month currently has three options: pay a vendor, use a model that is wrong on its domain, or have no data.

None of those are good. This is the accessibility argument, and it is a stronger pitch to a technical buyer than the data-scarcity argument, because it has an obvious fairness answer.

## 6. Verification by machine, arbitration by people

Every reconciliation round today sends items back to people. For an ambiguous item that is correct and cheap. For a large majority of items that were only slightly uncertain, a model can propose the label and people confirm it, at a fraction of the cost.

The research on hybrid annotation reports time savings with consensus intact, and also reports anchoring risk when people see a model answer first. So: machine proposes, people confirm without seeing the proposal. Keep the blind arbitration rule. It is the one thing that must not be optimised away, because it is what makes the corpus worth more than an API call.

## 7. Signed agents as an identity primitive

If agents sign their requests, then "who is this" and "should they be in" become the same question, answered by a cryptographic fact rather than a behavioural guess. The agent lane stops being a heuristic and becomes a declared channel.

This is listed later rather than now because it is a dependency on an ecosystem standard, not a decision.

---

## Explicitly out of scope

Naming these prevents the scope from eroding through reasonable-sounding additions.

- **A labeling workforce marketplace.** The labor is a side effect of a gate. Building a workforce is a different, much larger, much worse business.
- **A bot-management or fingerprinting platform.** That market exists, is crowded, and is not where the value is.
- **Selling raw behavioral traces.** Derived features only, hashed identifiers, never trajectories. The moment raw traces are collected for sale, the legal position in [RISKS.md](RISKS.md) becomes unmanageable.
- **Surveillance-adjacent positioning.** There is a version of this product that watches people. It should not be built, and the reason should stay written down so a future decision does not argue it from scratch.

## Why this file exists

Every one of the items above is a smaller, more fundable company than the whole. If the core works, the right move is to pick one and go deep. If it does not, none of them exist.

Writing them down is enough for now.