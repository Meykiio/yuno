# Project

State, conventions, and how to work on this repository, including how to work on it with an AI agent.

## Current state

| | |
|---|---|
| Stage | Concept proof, pre-build |
| What works | The interactive proof of concept in `poc/index.html`: the full cycle runs in the browser, including scoring, voting, reconciliation and exports |
| What does not exist | No server, no database, no product, no user, no revenue |
| Next milestone | The calibration run described in [RISKS.md](RISKS.md) section 1: do real agents find and follow the disclosed note |
| Last verified | 2026-10-18: citations re-checked against source, all repository links return HTTP 200, inline demo script parses clean |

## What this repository is

Documentation plus one self-contained HTML file, for an audience that has never heard of Yuno and does not work in machine learning.

It is written to be read by three different readers, and the writing changes accordingly:

- **A curious person with no technical background.** [README.md](../README.md), then [CONCEPT.md](CONCEPT.md).
- **An investor, partner or domain expert evaluating the idea.** [USE-CASES.md](USE-CASES.md), [BUSINESS.md](BUSINESS.md), [RISKS.md](RISKS.md).
- **An engineer who wants to build or attack it.** [HOW-IT-WORKS.md](HOW-IT-WORKS.md), [TECHNICAL-DESIGN.md](TECHNICAL-DESIGN.md), [EVIDENCE.md](EVIDENCE.md).

Every claim in every document is either sourced in [EVIDENCE.md](EVIDENCE.md) or explicitly labelled a hypothesis. That line is the most important convention in the repository.

## Conventions

- **English only.** No em-dashes. Use commas, colons, parentheses.
- **Plain language first.** If a technical term is needed, define it in one clause at first use. A reader from any of the three audiences must be able to follow it.
- **No unverified claims.** A number without a source is either arithmetic that is shown, or it does not go in.
- **Honesty is a feature, not a liability.** The "not proven" sections are the most valuable pages here. Do not soften them to make a pitch better.
- **Max 250 lines per source file.** Applies to the product build. The documentation files are prose and are allowed to be long, because their job is to carry the argument.
- **Provenance on everything.** Every data row, every claim, every citation.

## If you are an AI agent working on this repository

1. Read `README.md` and `docs/CONCEPT.md` before changing anything. Do not infer the product from the code alone.
2. Do not treat anything in `docs/LATER.md` as an approved feature. Ideas there are recorded, not scheduled.
3. Do not soften `docs/RISKS.md`. If new evidence weakens a risk, add the evidence. If it strengthens one, add that too.
4. Do not add a claim without adding it to `docs/EVIDENCE.md`.
5. Do not commit `Sifeddine_OS.md` or anything matching it. It is gitignored on purpose: it contains personal details and a third party's company name.
6. Never add a dependency, a service, or an external call without approval. The proof of concept has zero dependencies and that is a deliberate property, not an accident.
7. Keep the proof of concept a single file with no build step, unless the owner asks otherwise.

## Naming

- **Cohorts are `human-likely` and `agent-likely`.** Never `verified human`. The score is an estimate from weak signals and the wording has to match the confidence.
- **`p_human`** is a probability in [0, 1], never a verdict.
- **"Control item"** is the question whose answer is already known. Not "gold item", which reads as valuable rather than as a check.
- **"Disagreement"** is where people and the AI differ on one item. Not "contested", which hides what is contested.

## History

See [../CHANGELOG.md](../CHANGELOG.md).

## Ownership

[Sifeddine Mebarki](https://github.com/Meykiio). Proprietary, all rights reserved. See [../LICENSE](../LICENSE).