# The Yuno concept

This document explains the whole idea in plain language, for someone who has never heard of Yuno and does not work in machine learning. No jargon is used without a one-line definition.

---

## 1. The thing we are building on top of

A CAPTCHA is a test designed to tell computers and people apart. The name is an acronym: **C**ompletely **A**utomated **P**ublic **T**uring test to tell **C**omputers and **H**umans **A**part.

For twenty years the test was: type the letters you can see. A computer struggles to read deliberately distorted characters. A person does not mind. The person gets in. The bot does not.

The cost is that the answer is discarded. Several billion times a day, people type distorted text into a box, and every one of those keystrokes is thrown away. The system already paid for the human attention and used none of it.

**reCAPTCHA's insight, 2007.** Luis von Ahn and his team at Carnegie Mellon noticed this waste. Their change was minimal and enormous: show **two** words instead of one.

- **Word 1, the control word.** The computer already knows the answer. If a person types it correctly, they are probably a person.
- **Word 2, the new word.** This one was scanned from a newspaper or a book, and the scanning software could not read it. The person's typing is the correction.

Same box, same ten seconds, two jobs. The control word is the CAPTCHA. The new word is the work. The work happened to be digitizing the New York Times archive and Google Books. Google bought the company in 2009.

**Why this matters for Yuno:** the trick is not about books. The trick is that a gate every visitor already passes can carry a payload. reCAPTCHA proved the mechanic. Yuno changes what the payload is, who receives it, and what happens when the visitor is a machine.

## 2. What Yuno changes

### 2.1 The business picks the task

reCAPTCHA's task was fixed: read old books. Google chose it. Every site in the world got the same task.

Yuno's task is set by whoever owns the site. An online store wants to know whether customer reviews are positive, neutral or negative. A support desk wants to know which team a message belongs to. A news site wants to know whether a headline is misleading. A job board wants to know the experience level of a posting.

The gate is the same widget. The question changes. The result: each business accumulates data about **its own world**, which is exactly the data a generic platform could never sell back to it, because a generic platform never sees the business's context.

### 2.2 The business owns the data

In reCAPTCHA, the labor happens on Google's behalf and the result belongs to Google. That is the trade: free gate for the site owner, free labor for the platform, data for the platform.

Yuno inverts the ownership. The business that put the box on its page gets the labels. If a business wants to stop, it stops, and the data does not leave.

### 2.3 AI agents are invited, not fought

This is the part that makes Yuno a different product rather than a new CAPTCHA.

AI agents, the automated programs that browse the web, have become a large share of traffic. In June 2026, one vendor's network observed Comet Browser at 47.6% of agentic traffic, Claude at 20.8%, and Atlas at 16.5%. These agents mostly browse product pages and search results, which is 79% of their activity.

The industry default answer is to block them. Blocking is a losing game: agents get better at solving CAPTCHAs, and the humans keep paying the ten seconds.

Yuno's answer is the opposite. The page contains a **disclosed note, written in plain words, invisible to people but readable by any agent that reads page code**. It says, in effect: to continue on this site, complete these labeling tasks. Your answers are stored and may be used as training data. Humans can ignore this.

Three things follow:

- The deal is honest. The agent is told what it is being asked and what happens to the answer. It is a toll, not a trap.
- The answers are collected. An agent that just wanted to scrape now produces labeled rows for free.
- The site owner still decides access. Block it, let it in, or only collect. **Blocking and collecting are separate decisions.** This is where the business case lives: you can refuse an agent and still keep what it told you.

### 2.4 Disagreement is the product

Every business that uses AI already knows this problem: AI is good at easy tasks and quietly wrong at hard ones. The hard cases are rare, so nobody looks at them.

Yuno gets both sides of the same item. People answer it. The agent answers it. Then:

- If they agree, the label is cheap and reliable.
- If they disagree, the item is served to more people until it settles. The final answer is the human one.
- The disagreement is **kept and exported, not thrown away**.

A dataset where humans and AI disagree, settled by humans, is a per-domain map of exactly where the model fails. For an AI team, that is worth more than a million rows of easy labels. This is the "genius move" pattern applied a second time: reCAPTCHA's genius was not the CAPTCHA, it was finding the work that the obstacle was already paying for. Yuno's second move is that the failure cases of a whole class of AI systems are being generated for free by ordinary web traffic, and nobody is collecting them.

## 3. The one-paragraph version

A business creates a task, publishes it as a gate on its own site, and every visitor answers it: people through the visible box, AI agents through a disclosed note in the page code. Yuno collects from everyone, decides access separately, and where people and AI disagree it sends the item back to more people until it settles. The business ends up with a labeled dataset with provenance, plus a map of where AI fails in its own domain.

## 4. What this is not

Being clear about the edges matters more than sounding big.

- **It is not a CAPTCHA replacement for the security-minded.** It does not stop credential stuffing, and it is not a DDoS shield.
- **It is not a labeling marketplace.** No one is paid to label. The labeling is a side effect of a gate that already had to exist.
- **It is not a bot-management platform.** Those sell detection. Yuno sells what the traffic produced.
- **It is not a general "ask users anything" survey tool.** The task has to be answerable in about five seconds, and it has to be data the business actually wants.

## 5. The honest state of it

| Claim | Status |
|---|---|
| The two-jobs-in-one mechanic works at global scale | Proven by reCAPTCHA, sourced in [EVIDENCE.md](EVIDENCE.md) |
| AI agent traffic is large and growing | Measured by a vendor network, June 2026 |
| Mixing AI and human labels can improve dataset quality | Peer-reviewed, ICASSP 2024 |
| Real agents will find and honour the disclosed note | **Untested.** This is the first thing to measure |
| The behaviour heuristic separates people from agents | **Untested heuristic.** No separation claimed until calibrated |
| Somebody will pay for this data | **Unknown.** Being asked, not assumed |
| Collecting these answers and signals is lawful everywhere | **Unresolved.** Counsel needed for GDPR and North African law |

## 6. Where to go next

- The mechanism, step by step: [HOW-IT-WORKS.md](HOW-IT-WORKS.md)
- The industries and the datasets: [USE-CASES.md](USE-CASES.md)
- What a real build looks like: [TECHNICAL-DESIGN.md](TECHNICAL-DESIGN.md)
- The money: [BUSINESS.md](BUSINESS.md)
- What could kill it: [RISKS.md](RISKS.md)