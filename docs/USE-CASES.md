# Use cases

The same widget, twelve different questions. Each one ends with the business owning a dataset it could not otherwise buy, because a third-party labeling platform never sees its context.

Every entry answers four questions: what the gate asks, what data comes out, why AI is weak on it, and who uses the result.

---

## The selection rule

Not every task works. A task fits if all four are true:

1. **A person can answer it in about five seconds.** Longer than that and you are running a survey, not a gate. People leave, and the traffic that mattered never labels anything.
2. **AI does it well but not perfectly.** If the model is 99% right, the labels are worthless, and an API call costs a fraction of a cent. The value is in the last stretch.
3. **The business actually wants the answer.** Data nobody uses is a cost with no revenue attached.
4. **Answering is not extractive.** The task cannot be done properly by harvesting the item from elsewhere.

Rule 2 is the one people get wrong. "Is this review positive?" is the textbook example of a task an LLM already does well. The interesting tasks are the ones where language, local context, or domain judgment makes a model guess.

---

## 1. Online store

- **Gate asks:** How does this review feel? (Positive / Neutral / Negative)
- **Data out:** Labeled customer reviews, one row per review, with the source product.
- **Why AI is weak here:** sarcasm, mixed opinions, and local slang. "Great, another shirt that shrinks. Thanks a lot." reads as positive to a model. A person reads it correctly in under two seconds.
- **Who uses it:** the store itself for search ranking and merchandising, and any team building review tooling.

## 2. Support desk

- **Gate asks:** Which team should get this message? (Billing / Technical / Sales)
- **Data out:** A routing dataset that is specific to this company's products and vocabulary.
- **Why AI is weak here:** a message that mentions a failed login *and* a double charge is two tickets. Splits and priority calls depend on the company's own context.
- **Who uses it:** the company's own support assistant. This is the clearest "the data pays for itself internally" case, because the alternative is paying a labeling vendor for the same rows.

## 3. News site

- **Gate asks:** Does this headline match the article? (Accurate / Misleading / Unclear)
- **Data out:** Examples of misleading headlines, each with the article it came from.
- **Why AI is weak here:** it is a judgement about emphasis, not fact-checking. "Company slashes 500 jobs" over an article about relocating 500 roles is technically sourced and still wrong in the way that matters.
- **Who uses it:** fact-checking tools, trust and safety teams, and the publisher's own standards process.

## 4. Job board

- **Gate asks:** What experience level is this job? (Junior / Mid / Senior)
- **Data out:** Job posts tagged by real level.
- **Why AI is weak here:** the signal is contradiction and euphemism. "Entry level role. Requires 7 years of experience" is a real and common posting.
- **Who uses it:** job matching and search ranking, and anyone training a classifier on postings.

## 5. Insurance

- **Gate asks:** Does this photo show damage? (Yes / No / Unsure)
- **Data out:** Sorted claim photos with a human-verified flag on each.
- **Why AI is weak here:** a cracked window from a storm and a crack from an attempted theft look identical until you know the story.
- **Who uses it:** claims triage, and fraud models, which need both the photos and reliable flags.

## 6. Real estate

- **Gate asks:** Which room is in this photo? (Living / Kitchen / Bedroom / Bathroom)
- **Data out:** Tagged property photos from the listing's own traffic.
- **Why AI is weak here:** open-plan layouts, and photos that show a room only partially.
- **Who uses it:** listing search, virtual tours, and automatic floor-plan drafting.

## 7. Education

- **Gate asks:** Is this answer correct? (Correct / Partly / Wrong)
- **Data out:** Graded short answers, which is scarce data for tutoring models.
- **Why AI is weak here:** partially correct answers with a specific misconception are exactly what a tutoring system needs, and exactly what a model grades worst.
- **Who uses it:** the education platform itself, and tutoring model developers.

## 8. Local directory

- **Gate asks:** Is this business open right now? (Open / Closed / Moved / Unsure)
- **Data out:** Fresh opening-hours data with a timestamp and a human behind it.
- **Why AI is weak here:** the answer changes, and the web is always stale. A live human is the only cheap source of truth.
- **Who uses it:** the directory, and local search generally.

## 9. Moderation for any community

- **Gate asks:** Is this comment violating rule 3? (Yes / No)
- **Data out:** A moderation decision set covering the rules this community actually enforces.
- **Why AI is weak here:** every community's rules are local. A classifier trained on one forum's notion of harassment fails on the next one.
- **Who uses it:** the community's own moderators, and its trust and safety tooling.

## 10. e-commerce returns triage

- **Gate asks:** Why are you returning this? (Wrong size / Not as described / Damaged / Changed mind)
- **Data out:** Return reasons that are actually true rather than whatever the dropdown offered.
- **Why AI is weak here:** the offered options capture maybe 40% of reality. The rest is free text nobody reads.
- **Who uses it:** merchandising and supply chain decisions, which are expensive to get wrong.

## 11. Document intake for any back office

- **Gate asks:** Which department does this invoice belong to? (Accounts payable / Procurement / Facilities / Other)
- **Data out:** A routing dataset on real documents from that organisation.
- **Why AI is weak here:** internal structures, acronyms and approval chains are invisible from outside, so no general model can be right.
- **Who uses it:** the organisation's own document automation.

## 12. Marketplace trust and safety

- **Gate asks:** Is this listing description accurate for the photo? (Yes / No)
- **Data out:** Verified listing mismatches, which is the raw material for a trust score.
- **Why AI is weak here:** it is a comparison task, and "technically true but misleading" is the entire problem marketplaces have.
- **Who uses it:** the marketplace itself, and the payment and insurance partners that price risk off it.

---

## Sectors with unusually strong demand

Not all twelve are equal. These four have a reason to pay that is not "the data might be useful":

- **Marketplaces and e-commerce.** They already pay for trust and safety headcount, and the data comes from the same traffic they already have.
- **Insurance and financial services.** Model risk is a regulatory problem. A documented, human-verified map of where a model fails is worth real money.
- **Enterprise document automation.** The buyers already budget for labeled training data and already have a vendor relationship where Yuno could slot in.
- **Education and hiring.** Both have a public, well-funded interest in unbiased model evaluation, and both are short of real labeled data.

## Where this does not fit

Honesty about the edges:

- **No traffic, no data.** A site with 200 visitors a month produces about 60 labeled items a month. Not a product.
- **Low-stakes tasks.** If the answer does not change a decision, the label has no value regardless of who gave it.
- **Pages people are trying to escape.** A gate on a required step converts into abandonment, and the data volume disappears with the traffic.
- **Anything where collecting the answer would be creepy.** Collecting the answer is fine. Collecting the behaviour behind it is where the legal risk lives, and that is a real constraint, not a formality.
- **Tasks needing expertise.** If a person needs training to answer in five seconds, it is not a gate. It is a study.