# Changelog

All notable changes to this repository. Dates are 2026.

## 2026-10-18

### Added

- Interactive proof of concept at `poc/index.html`, one self-contained file, no dependencies. Five sections: the reCAPTCHA precedent, why Yuno differs, twelve business use cases, a three-step walkthrough the visitor runs themselves, and an honest proven-versus-unproven section.
- A plain-text summary section near the bottom of the page, written for an AI assistant that reads the HTML directly.
- `README.md` as the front door, with a reading path per audience type.
- `docs/CONCEPT.md`: the full idea in plain language, no jargon.
- `docs/HOW-IT-WORKS.md`: the mechanism. Two lanes, access rules separate from collection, `p_human` scoring, blind arbitration, exports.
- `docs/TECHNICAL-DESIGN.md`: stack, endpoint surface, submit ordering, data model, security, build order, verification gates.
- `docs/USE-CASES.md`: twelve industries, each with the gate question, the dataset produced, why AI is weak on it, and who uses it. Includes a selection rule and the cases where it does not fit.
- `docs/EVIDENCE.md`: every load-bearing claim with its source, what the source proves, and what it does not prove. Includes a section on claims deliberately not made.
- `docs/BUSINESS.md`: where value actually comes from, volume arithmetic, three products, pricing reasoning, the decision points.
- `docs/RISKS.md`: risks ordered by damage, how each gets tested, and the three results that would end the project.
- `docs/LATER.md`: speculative directions, explicitly not a roadmap, plus what is out of scope and why.
- `docs/PROJECT.md`: state, conventions, naming rules, and instructions for agents working on this repository.
- `LICENSE`: AGPL-3.0, the same license as `djazayer-khadhra`. Open source, with network use and modification required to stay open.
- `.gitignore`, including an explicit exclusion of `Sifeddine_OS.md` and the local PRD.
- GitHub Pages, serving the proof of concept at `meykiio.github.io/yuno`.

### Verified

- All six sources cited in the proof of concept re-checked against the source pages. arXiv 2401.09760 (ICASSP 2024) and arXiv 2603.07116 confirmed to exist; the HUMAN Security June 2026 agent-traffic figures match the page exactly (Comet 47.6%, Claude 20.8%, Atlas 16.5%, product and search routes 79%, authentication routes 5.38%).
- All repository and page links return HTTP 200. The Science DOI returns 403 to automated requests because of its bot protection and works in a browser.
- The proof of concept's inline script parses clean under `node --check`.

### Notes

- The internal PRD is deliberately not in this repository. It names a third party's company and frames the wedge around a specific regional language, both of which conflict with the English-only, any-industry positioning used publicly.
- No number in the proof of concept is presented as more certain than its source. Simulated content is labelled as simulated on the page itself.

### Changed

- Licence switched from proprietary to AGPL-3.0 after the first commit, at the owner's decision. README, `docs/PROJECT.md`, the proof of concept footer and this file were updated to match, so no stale reference remains.