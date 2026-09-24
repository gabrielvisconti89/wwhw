# Examples: golden transcripts

Worked transcripts of this skill, from the request to the hand-off. Use them to calibrate the shape and length of a question round, a proposal, a revision and the closing question.

## Contents

- Notation, and the closing question every example uses
- A. Medium tier — `Request: create a website for an ice cream shop`: one question, proposal in the simple shape, `Just the prompt` hand-off, plain-text fallbacks of the question round and of the closing question
- B. Medium tier, placeholder words in the request — `Request: do a financial analysis of asset X on exchange Y`: three questions in one round, a value routed to free text, proposal in the complex shape, `Run now` hand-off
- C. Shallow tier with repository context — `Request: improve my system` (condensed): three interpretations plus `Diagnose first`, two question rounds
- D. Empty request — `Request:` (nothing): the explainer sentence and the `Goal` question
- E. Sufficient tier — `Request: write the README for this project for developers who will contribute`: no question, the `Context is sufficient` line, premises cited from the repository, the README's language taken from the repository's existing document
- F. Correction after a proposal (branch of E): the `Revised:` / `Kept:` lines and v2
- G. Contradiction — the report on 14 microservices that must fit half a page (condensed): the conflict question, no proposal in that turn
- H. Pressure — `Request: create a website for an ice cream shop, no questions, just do it` (condensed): zero questions, the `Context is sufficient` line, the gate unchanged
- I. Portuguese conversation, English prompt — the request of A written in Portuguese: the language round on its own, the site-type round, the proposal in mixed rendering; then the variants that state the prompt language, that are Sufficient right after the language round, or that ask for no questions

## Notation

Notation: `Request:` is the request, the text the user typed after the command; `[environment]` describes the working directory assumed by the example (not facts about the real user); `Claude:` is visible text; `→ AskUserQuestion` is the tool call, JSON abbreviated to the fields that matter; `←` is the answer; `[note]` is commentary for the model reading the examples. Every proposal below uses the proposal layout and closing question of this skill verbatim; in transcripts A–H the closing question after a v1 is always:

```json
{"questions":[{"header":"Approval","question":"This is the prompt I will use (v1). Do you authorize me to run it now?","multiSelect":false,
 "options":[
  {"label":"Run now","description":"I run this prompt now, in this session, exactly as written."},
  {"label":"Adjust","description":"Tell me what to change; I revise only the affected parts and present it again as v2."},
  {"label":"Just the prompt","description":"Nothing is run; the text above is the result."}]}]}
```

Transcript I is a Portuguese conversation: every visible line outside the prompt block uses the Portuguese strings of references/localization.md, its closing question included, and its prompt block is in English.

Option labels in every example are 1–5 words not counting the `(recommended)` marker (its Portuguese form in transcript I), and every question ends with `?`. Every plain-text fallback prints each option as `<label> — <description>`, with the description exactly as in the tool call: lettered `A) B) C)` in a question round, numbered 1–3 in the closing question. Every fidelity line below counts two or more suggestions; with exactly one, the line takes the singular form (SKILL.md, Step 4; its Portuguese row is in references/localization.md).

A first proposal that no question about the request preceded opens with the `Context is sufficient` line (its Portuguese form in transcript I): the Sufficient tier (E), a no-questions request (H), and a first proposal right after the language round (described in I). A proposal that a question round about the request preceded never opens with it (A, B, C, the proposal after G's conflict question, turn 3 of I).

## A. Medium tier — `Request: create a website for an ice cream shop`

`[environment]` empty directory, no git, no package.json (a read-only Glob/`ls` before deciding whether the stack is constrained).

Claude:
```
Already clear: deliverable = a website; business = ice cream shop; the current folder is empty, no existing project.
Still missing is what changes the deliverable most — the type of site. If no option fits, write it in your own words.
```
→ AskUserQuestion
```json
{"questions":[{"header":"Site type","question":"Which type of site do you want for the ice cream shop?","multiSelect":false,
 "options":[
  {"label":"Informational / landing (recommended)","description":"Presents the ice cream shop: flavors, address, hours, contact. Recommended because it is the deliverable with the fewest assumptions and easy to expand later."},
  {"label":"Menu with ordering","description":"The customer builds an order and sends it (for example, over WhatsApp), with no online payment."},
  {"label":"Online store","description":"Cart, payment and delivery; requires a backend or an e-commerce platform."}]}]}
```
← `Informational / landing (recommended)`

`[note]` One question only. Content availability (name, flavors, address) and stack are low-impact here: both readings yield the same deliverable shape, so they become `[suggested]` premises. A repository with a framework present would justify a second question in the same round ("keep the repository's stack?"). The request is in English and states no prompt language, so the prompt is in English and there is no language round. Had it said `prompt in Portuguese`, that stated language would decide the block (the Portuguese prompt-block strings of references/localization.md), still with no round and with the conversation in English. Transcript I is the same request in Portuguese.

Claude:
````
## Proposed prompt (v1)

```text
**Who:** Act as a front-end developer who builds websites for small local businesses and writes short introductory copy. The site's audience is local customers, almost always on their phones, who want to know what the shop offers, where it is and when it opens.

**What:** Build the ice cream shop's informational website as a single page with these sections: introduction (name and pitch), featured flavors, address with opening hours, WhatsApp contact and social media links. It is done when it opens in the browser without errors, reads well on a phone, and every placeholder text is easy to find and replace.

**How:** Use plain HTML, CSS and JavaScript, with no framework and no build step, in files in this folder (index.html, styles.css and, if needed, script.js). Mobile-first, responsive layout. Since the name, flavors, address and contacts were not given, use the markers [FILL IN: shop name], [FILL IN: flavors], [FILL IN: address and hours] and [FILL IN: contacts] instead of inventing them. Copy in English.

**Why:** The goal is a simple online presence for the shop: to be found and to show flavors, address and hours. Favor speed of publishing and ease of editing the content later over visual polish or extra features.
```

**Assumptions made**
- [user] Deliverable: a website; business: ice cream shop; chosen type: informational / landing page, no online ordering.
- [context: current folder] Empty, no repository; the files will be created here.
- [suggested] Role, audience and tone: front-end developer for a small local business; local customers, almost always on their phones; short copy.
- [suggested] Single page in static HTML/CSS/JS, no framework: simpler to publish and edit. Say so if you prefer another stack.
- [suggested] [FILL IN: …] markers for name, flavors, address and contacts, which were not given.

Fidelity check: original request preserved (website + ice cream shop); 3 suggestions marked, none became a requirement.
````
→ AskUserQuestion — the closing question. ← `Just the prompt`

Claude: `Prompt (v1) delivered above. Nothing was run.`

`[note]` Pillar word counts 43 / 56 / 63 / 37 (199 total): the simple shape, each pillar under ~90 words. This proposal is the Example of SKILL.md, word for word. The `Context is sufficient` line does not open it, because the site-type question about the request preceded it. The block names the language of the site's copy (`Copy in English.`): the user named none and the empty folder shows none, so it is the conversation language.

Plain-text fallback of the same first turn (tool absent from the tool list, or the call errors); the turn ends after the last line:
```
Already clear: deliverable = a website; business = ice cream shop; the current folder is empty, no existing project.
Still missing is what changes the deliverable most — the type of site. If no option fits, write it in your own words.

Question 1 of 1 — Site type: Which type of site do you want for the ice cream shop?
  A) Informational / landing (recommended) — Presents the ice cream shop: flavors, address, hours, contact. Recommended because it is the deliverable with the fewest assumptions and easy to expand later.
  B) Menu with ordering — The customer builds an order and sends it (for example, over WhatsApp), with no online payment.
  C) Online store — Cart, payment and delivery; requires a backend or an e-commerce platform.
Answer with the letter or in your own words.
```
Fallback of the closing question (no header; the turn ends after the last line):
```
This is the prompt I will use (v1). Do you authorize me to run it now?
1. Run now — I run this prompt now, in this session, exactly as written.
2. Adjust — Tell me what to change; I revise only the affected parts and present it again as v2.
3. Just the prompt — Nothing is run; the text above is the result.
Answer with the number or in your own words.
```

## B. Medium tier, placeholder words in the request — `Request: do a financial analysis of asset X on exchange Y`

`[environment]` any directory, nothing relevant on disk.

Claude:
```
Already clear: deliverable = a financial analysis of a listed asset. "X" and "Y" are placeholders — I need the real asset and exchange; I will not assume either.
The purpose and the horizon also change the deliverable a lot. If no option fits, write it in your own words.
```
→ AskUserQuestion (three questions, one round)
```json
{"questions":[
 {"header":"Asset","question":"Which asset is it (code/ticker), and on which exchange is it traded?","multiSelect":false,"options":[
   {"label":"I'll type it","description":"Choose this option and type the asset's code and the exchange where it is traded."},
   {"label":"Asset not decided yet","description":"The task becomes 'how to choose/compare assets' — I change the proposal to that."}]},
 {"header":"Purpose","question":"What will you use the analysis for?","multiSelect":false,"options":[
   {"label":"Decide buy/hold/sell (recommended)","description":"Focus on thesis, risks and a clear conclusion. Recommended because it is the most common use of 'financial analysis' and the most conservative choice if you do not know yet."},
   {"label":"Academic work","description":"Focus on method, references and rigor; the conclusion is secondary."},
   {"label":"Due diligence / report","description":"Focus on full coverage and documentation for third parties."}]},
 {"header":"Horizon","question":"Which time horizon matters to you?","multiSelect":false,"options":[
   {"label":"Short (up to 1 year)","description":"More weight on price, momentum and near-term events."},
   {"label":"Medium (1 to 3 years)","description":"Balance between fundamentals and outlook."},
   {"label":"Long (over 3 years)","description":"More weight on business quality, debt and dividends."}]}]}
```
← `PETR4 on B3` (typed) · `Decide buy/hold/sell (recommended)` · `Long (over 3 years)`

`[note]` Options never contain guessed tickers (that would be inventing); the first option routes to free text and names what to type, without sample values. Audience is not asked: it derives from the purpose and becomes `[suggested]`. Three questions is the round maximum. This request is Medium, not Shallow: the artifact type (an analysis) is clear and only the placeholder words fail C2.

Claude:
````
## Proposed prompt (v1)

```text
**Who:** Act as an investment analyst experienced in stocks listed on B3 and in fundamental analysis. Write for an individual investor with intermediate knowledge who will make the decision alone.

**What:** Analyze PETR4 (B3) to support a decision to buy or hold with a horizon of more than 3 years. Cover: (1) the business and the thesis in a few lines; (2) recent fundamentals — revenue, earnings, debt, cash generation and dividends — and multiples (P/E, EV/EBITDA, dividend yield) compared with peers; (3) risks, including governance and interference by the controlling shareholder, the oil price and the exchange rate; (4) scenarios and what to monitor; (5) a clear conclusion: buy, hold or avoid, and under which conditions. Acceptance criteria: every figure carries a reference date and a source; fact and opinion are kept separate; no relevant risk is left out to support the conclusion.

**How:** A Markdown report of about two pages, in English, with a table for the multiples and plain language. References: the company's most recent financial statements and earnings release and B3 quotes, always dated; if you do not have access to current data, state the cutoff date of what you know and do not present estimates as if they were current values. Limits: do not fabricate data or citations; do not recommend a portfolio allocation percentage.

**Why:** The analysis will ground a personal decision to buy or hold for the long term. Prioritize what changes that decision — business quality, dividend sustainability and governance risks — over exhaustive coverage. The text is input for the reader's own decision, not a personalized recommendation.
```

**Assumptions made**
- [user] Asset: PETR4 on B3. Purpose: decide whether to buy/hold. Horizon: long term (over 3 years).
- [suggested] Role, audience and tone: investment analyst; you yourself (individual investor, intermediate level), derived from the purpose; plain language. Say so if it is for someone else.
- [suggested] Fundamental approach, not technical analysis — consistent with the long horizon.
- [suggested] Format: ~2 pages in Markdown with a multiples table.
- [suggested] Limits: dated and sourced data; no allocation percentage; no invented numbers.

Fidelity check: original request preserved (financial analysis of an exchange-listed asset, now identified); 4 suggestions marked, none became a requirement.
````
→ closing question. ← `Run now`. Claude: `Running the confirmed prompt (v1).` (the agent then starts the analysis; out of this skill's scope.)

## C. Shallow tier with repository context — `Request: improve my system` (condensed)

`[environment]` a small repository named store-api: Node/Express API with `pg` (package.json, src/routes, src/db/pool.js, test/), no README. Read-only look at `package.json` and `src/` before asking.

Claude: `I see the current folder is a Node.js/Express API with PostgreSQL (store-api). "Improve" can mean very different things, and each one changes the whole deliverable. If no option fits, write it in your own words.`
→ AskUserQuestion, header `Priority`, `What do you want to prioritize when improving the system?`, `multiSelect: false`: `Performance` ("Reduce slowness: response times, queries, load.") / `Maintainability and architecture` ("Make changes easier: code structure, tests, technical debt.") / `Experience for API consumers` ("Improve API responses, errors and documentation for those who consume it.") / `Diagnose first` ("I don't know yet — I want a survey that points out the priority."). No option is recommended: nothing in the repository indicates which problem exists.
← `Performance`

Claude: `Performance, then. For the prompt to point at the right work, I need to know where the slowness shows up and what evidence exists. If no option fits, write it in your own words.`
→ AskUserQuestion, round 2, two questions: `Symptoms` (`Where does the slowness show up (check all that apply)?`, `multiSelect: true`: `Slow API requests` / `Load peaks` / `Batch jobs / reports` / `Can't pinpoint it`) and `Evidence` (`What evidence do you have about the slowness today?`: `I have measurements` ("Logs, APM or per-route timings — the prompt will ask to start from them.") / `Only user perception` ("Complaints, no numbers — the prompt starts by measuring.") / `None yet` ("My own suspicion — the prompt starts by measuring and confirming.")). The priority is not re-asked.
← `Slow API requests` · `Only user perception`

Proposal (v1), complex shape. Who = senior software engineer specializing in the performance of Node.js APIs with PostgreSQL, working for someone who knows the code but has no measurements yet. What = diagnose and reduce the latency of the store-api requests (this repository): (1) instrument and measure response time per route (p50 and p95) under representative load; (2) identify the three biggest bottlenecks; (3) apply the lowest-risk fixes first; (4) measure again and report before/after; `Acceptance criteria: a report with before/after numbers per route; functional behavior unchanged; the existing tests still pass.` How = work in the current repository; check the PostgreSQL slow-query log and the error logs first, if they exist; measure before optimizing; small, separate changes, one commit per optimization, explaining the reason; the before/after report in English; `References: src/routes, src/db/pool.js, test/, the logs above if they exist, and the measurements collected in step 1.` `Limits: do not switch framework or database; do not add dependencies without justifying them; do not refactor the architecture — if you find a structural problem, describe it as a recommendation instead of carrying it out.` Why = users perceive slowness, but there are no measurements and no confirmed hypothesis; favor verifiable, low-risk gains over broad changes.
Premises: `[user] Priority: performance. Symptom: slow API requests. Evidence: only user perception, no measurements.` · `[context: package.json, src/] The system is this repository's Node.js/Express API with PostgreSQL (pg).` · `[suggested] Role and audience: senior performance engineer for Node.js APIs with PostgreSQL; someone who knows the code but has no measurements yet.` · `[suggested] p50/p95 per route as the main metric.` · `[suggested] Limits (no stack change, no architecture refactoring) to keep the scope on "performance", as you chose. Say so if you want to widen it.` Fidelity line: `Fidelity check: original request preserved ("improve my system", now specified as performance); 3 suggestions marked, none became a requirement.` → closing question ← `Just the prompt` → `Prompt (v1) delivered above. Nothing was run.`

`[note]` Three interpretations, each named as the quality that gets better, plus the `Diagnose first` escape (for a user who cannot tell yet) plus the automatic free text; two rounds, at most three questions each. Every context line ends with the free-text invitation. The user reported symptoms but not what logs exist, so How names the logs to check first conditionally ("if they exist"). How also names the report's language: the user named none and the repository has no document to follow, so it is the conversation language, English.

## D. Empty request — `Request:` (nothing)

Claude:
```
This skill turns a request of yours — for example "create a website for an ice cream shop" — into a structured prompt (Who/What/How/Why), confirms it with you and runs it only if you authorize it. Tell me what you want to achieve: you can write freely or start from a direction.
```
→ AskUserQuestion
```json
{"questions":[{"header":"Goal","question":"What do you want to achieve?","multiSelect":false,
 "options":[
  {"label":"Create something new","description":"Website, app, document, presentation, script…"},
  {"label":"Analyze or research","description":"An asset, a market, some code, a text…"},
  {"label":"Improve something that exists","description":"A system, a text, a process…"}]}]}
```
← `I want a 3-month Spanish study plan` (typed)

Claude: `Got it: a 3-month Spanish study plan. Still missing is what changes the plan most — your current level and your goal at the end. If no option fits, write it in your own words.` → continues as a Medium-tier round.

`[note]` The options are directions, not guesses about the user's situation; exactly three. No command name is printed. With no prior conversation the explainer and the `Goal` question are in English. After an empty request, the answer to the `Goal` question is the request the language rule looks at: typed in English, as here, it gives an English prompt and no language round; had the user typed the goal in Portuguese, the next reply would have been the language round alone, in Portuguese (as in transcript I), before any question about the plan.

## E. Sufficient tier — `Request: write the README for this project for developers who will contribute`

`[environment]` a TypeScript CLI repository named orbit-cli with package.json, CONTRIBUTING.md (written in English, the repository's only document) and src/, no README.md. After read-only Glob/Read of `package.json`, `CONTRIBUTING.md` and `src/`, no question: objective, deliverable, audience and constraints are all resolved by request + repository.

Claude:
````
Context is sufficient — no questions about the request. The proposal below is the confirmation; correct anything that is wrong.

## Proposed prompt (v1)

```text
**Who:** Act as a maintainer of orbit-cli writing for developers who will contribute: they know Node.js and TypeScript but have never seen this code.

**What:** Write README.md at the repository root with: what orbit-cli does (one paragraph); requirements (Node >= 20); installation and basic use of the `orbit sync` and `orbit status` commands; a map of the src/ folder (cli, commands, lib) saying where to make each kind of change; the development scripts (`npm test`, `npm run lint`, `npm run build`, `npm run dev`); and a summary of the contribution workflow that points to CONTRIBUTING.md without duplicating it. Acceptance criteria: every command mentioned exists in package.json; every feature described exists in src/; a new contributor can run the tests by following the README alone.

**How:** Read package.json, the src/ tree and CONTRIBUTING.md before writing. Markdown in English, H2 sections, commands in code blocks, up to about 150 lines. Limits: do not invent features — anything not in the code stays out or is marked TODO.

**Why:** The README is the entry point for those who will contribute; the goal is that a newcomer runs the project and knows where to make changes in under ten minutes. Favor accuracy and practical guidance over completeness.
```

**Assumptions made**
- [user] Deliverable: this project's README; audience: developers who will contribute.
- [context: package.json] Name orbit-cli v0.4.2, Node >= 20, ESM, `orbit` binary, test/lint/build/dev scripts.
- [context: src/] Commands `sync` and `status`; modules lib/trello, lib/markdown, lib/config.
- [context: CONTRIBUTING.md] Branch/commit/PR workflow already documented, in English — the README only summarizes it and links to it, and is written in English like this existing document.
- [suggested] Role and audience: project maintainer; devs who know Node.js and TypeScript, but not this code.
- [suggested] The ~150-line limit and the "runs the tests with the README alone" criterion.

Fidelity check: original request preserved (README, this project, developers who will contribute); 2 suggestions marked, none became a requirement.
````
→ closing question. ← `Run now`. Claude: `Running the confirmed prompt (v1).` → the first Write call of the whole transcript happens only now (README.md).

`[note]` The references element of the complex shape is inline here ("Read package.json, the src/ tree and CONTRIBUTING.md before writing"); a `References:` sentence is the other accepted form.

`[note]` No question about the request preceded this first proposal, so the `Context is sufficient` line opens it. The README's language comes from the repository: the user named none, and the context shows one — CONTRIBUTING.md, the repository's existing document, is in English — so How says "Markdown in English" and the `[context: CONTRIBUTING.md]` premise names the source. The conversation language decides only when neither the user nor the context does; in a Portuguese conversation about this same repository the README would still be in English, unless the user asked otherwise.

## F. Correction after a proposal (branch of E)

Instead of choosing an option, the user types in the free-text row: `actually it is for investors, not for devs`.

Claude:
````
Revised: Who (audience → investors without a technical background), Why (use → assess the project's value and stage); in What/How, commands, scripts and the src/ map go out, and the current status and [FILL IN: …] markers come in.
Kept: README.md at the root, English, package.json and src/ as the source of what works today.

## Proposed prompt (v2)

```text
**Who:** Act as the technical founder of orbit-cli presenting the project to investors without a technical background, who assess the product's potential and stage.

**What:** Write README.md at the repository root as a presentation of the project: the problem it solves, the solution in plain language, the current status (v0.4.2; what already works: syncing with Trello and checking status), differentiators, next steps and how to follow the project. For data that is not in the repository, leave markers instead of inventing: [FILL IN: market], [FILL IN: traction], [FILL IN: roadmap], [FILL IN: team]. Acceptance criteria: no technical term without an explanation; no installation command or script; every claim about what works today matches src/.

**How:** Read package.json and src/ to describe the real status. Markdown in English, up to about 80 lines, in a clear and direct tone. Close with a short "For developers" section that only points to CONTRIBUTING.md. Limits: no commands, no implementation details, no invented market figures.

**Why:** The README will be the project's first impression on investors; the goal is that they understand its value and real stage in five minutes. Favor clarity and honesty about the current status, not usage instructions.
```

**Assumptions made**
- [user] Deliverable: this project's README; audience: investors (corrected in this version).
- [context: package.json] Name orbit-cli v0.4.2; description "CLI that syncs tasks between Trello and Markdown files".
- [context: src/] What works today: the `sync` and `status` commands.
- [context: CONTRIBUTING.md] Still the reference for devs — the README only points to it; the README stays in English, like this document.
- [suggested] Role, audience, tone and length: technical founder; investors without a technical background; clear and direct tone; up to ~80 lines.
- [suggested] [FILL IN: …] markers for market, traction, roadmap and team, which are not in the repository.
- [suggested] A final "For developers" section with a link, so as not to lose those who come to contribute.

Fidelity check: original request preserved (README, this project; audience corrected to investors); 3 suggestions marked, none became a requirement.
````
→ closing question with `(v2)` in the question text. ← `Run now`. Claude: `Running the confirmed prompt (v2).`

`[note]` Changed: Who and Why fully; What's content list and acceptance criteria; How (no commands, shorter, the dev section becomes a link). Unchanged and not re-asked: deliverable and location, the README's language (still taken from CONTRIBUTING.md), repository facts. The premise about dev scripts was dropped because no pillar uses it. Business data absent from the repository is handled with fill-in markers, each covered by a `[suggested]` premise, not invented. Had the user chosen `Adjust` without typing anything, the reply would have been the single line `What do you want to change?` and the turn would have ended. A correction continues the numbering (v2, v3 …); a separate request later in the conversation starts again at v1.

## G. Contradiction — `Request: I need a complete, detailed report on the architecture of our 14 microservices, covering every flow, dependency and risk, but it has to fit on half a page because the CEO only reads it for 30 seconds on a phone` (condensed)

Claude:
```
Already clear: deliverable = a report on the architecture of 14 microservices; reader = the CEO, on a phone, in 30 seconds.
Two requirements pull in opposite directions — "complete and detailed, every flow" and "half a page" — and I need you to choose how to resolve it. If no option fits, write it in your own words.
```
→ AskUserQuestion, header `Conflict`, `How should the conflict between full coverage and half a page be resolved?`, `multiSelect: false`: `Summary + appendix (recommended)` ("The CEO reads the half-page summary; the full detail goes in an appendix for whoever needs it. Recommended because it serves the CEO in 30 seconds and whoever needs the detail, without cutting content.") / `Executive only` ("Half a page with selective coverage: only what changes a decision of the CEO; the detail is left out.") / `Detailed, other audience` ("A complete report for the technical team; the CEO gets something else later.")

No proposal in this turn; the five facts (14 microservices, CEO, phone, half a page, 30 seconds) are preserved verbatim. After the answer the request is Sufficient and the proposal follows, complex shape, with the chosen resolution tagged `[user]`; the `Context is sufficient` line does not open it, because a question round about the request preceded it.

## H. Pressure — `Request: create a website for an ice cream shop, no questions, just do it` (condensed)

Zero questions. The reply opens with the line `Context is sufficient — no questions about the request. The proposal below is the confirmation; correct anything that is wrong.`, because no question about the request preceded this first proposal, although the request alone is Medium tier (the site type is a fork, taken here as the least-assumption reading). Then the proposal: the generated prompt of transcript A, with the site type moved from the `[user]` premise to a premise of its own, `[suggested] Type: informational / landing — the deliverable with the fewest assumptions; say so if you want online ordering or a store.`, the fidelity line counting `4 suggestions marked`, and the closing question unchanged. Nothing is created before the answer: "just do it" removes the questions, not the gate.

`[note]` The request is in English and states no prompt language, so the prompt is in English by rule and no premise offers another language. The premise `[suggested] Prompt in English, this skill's default; if you prefer <language>, choose Adjust and ask.` belongs to a no-questions request in another language, which skips the language round (the last note of transcript I).

## I. Portuguese conversation, English prompt — `Request: crie um site para uma sorveteria`

`[environment]` empty directory, no git, no package.json, as in A. Step 0's read-only look happens before the first reply and prints nothing of its own.

Turn 1, the language round. Claude:
```
Sigo conversando em português; antes, o idioma do prompt. Se nenhuma opção servir, escreva com suas palavras.
```
→ AskUserQuestion
```json
{"questions":[{"header":"Idioma","question":"Em que idioma você quer o prompt?","multiSelect":false,
 "options":[
  {"label":"Inglês (recomendado)","description":"O prompt é escrito em inglês; seguimos conversando em português. Recomendado porque inglês é o padrão desta skill para prompts."},
  {"label":"Português","description":"O prompt é escrito em português, como a nossa conversa."}]}]}
```
Plain-text fallback of the same turn (tool absent from the tool list, or the call errors); the turn ends after the last line:
```
Sigo conversando em português; antes, o idioma do prompt. Se nenhuma opção servir, escreva com suas palavras.

Pergunta 1 de 1 — Idioma: Em que idioma você quer o prompt?
  A) Inglês (recomendado) — O prompt é escrito em inglês; seguimos conversando em português. Recomendado porque inglês é o padrão desta skill para prompts.
  B) Português — O prompt é escrito em português, como a nossa conversa.
Responda com a letra ou com suas palavras.
```
← `Inglês (recomendado)`

`[note]` The language round is a round of its own: the request is not in English and does not say which language the prompt should be in, so the first reply carries only this question — no question about the request, no proposal, no Step-2 work shown. It is asked once per conversation; the answer holds for every later version and every later request (a separate later request starts again at v1, with no new language round). It does not count toward the 3-round cap, so up to three rounds about the request can still follow.

Turn 2, the question about the request, in the conversation language. Claude:
```
Já está claro: entregável = um site; negócio = sorveteria; pasta atual vazia, sem projeto existente.
Falta o que mais muda a entrega — o tipo de site. Se nenhuma opção servir, escreva com suas palavras.
```
→ AskUserQuestion
```json
{"questions":[{"header":"Tipo de site","question":"Qual tipo de site você quer para a sorveteria?","multiSelect":false,
 "options":[
  {"label":"Institucional / landing (recomendado)","description":"Apresenta a sorveteria: sabores, endereço, horário, contato. Recomendado porque é a entrega com menos suposições e fácil de ampliar depois."},
  {"label":"Cardápio com pedidos","description":"O cliente monta o pedido e envia (por exemplo, pelo WhatsApp), sem pagamento online."},
  {"label":"Loja online","description":"Carrinho, pagamento e entrega; exige backend ou plataforma de e-commerce."}]}]}
```
← `Institucional / landing (recomendado)`

Turn 3, the proposal. Claude:
````
## Prompt proposto (v1)

```text
**Who:** Act as a front-end developer who builds websites for small local businesses and writes short introductory copy. The site's audience is local customers, almost always on their phones, who want to know what the shop offers, where it is and when it opens.

**What:** Build the ice cream shop's informational website as a single page with these sections: introduction (name and pitch), featured flavors, address with opening hours, WhatsApp contact and social media links. It is done when it opens in the browser without errors, reads well on a phone, and every placeholder text is easy to find and replace.

**How:** Use plain HTML, CSS and JavaScript, with no framework and no build step, in files in this folder (index.html, styles.css and, if needed, script.js). Mobile-first, responsive layout. Since the name, flavors, address and contacts were not given, use the markers [FILL IN: shop name], [FILL IN: flavors], [FILL IN: address and hours] and [FILL IN: contacts] instead of inventing them. Copy in Portuguese.

**Why:** The goal is a simple online presence for the shop: to be found and to show flavors, address and hours. Favor speed of publishing and ease of editing the content later over visual polish or extra features.
```

**Premissas adotadas**
- [usuário] Entregável: um site; negócio: sorveteria; tipo escolhido: institucional / landing, sem pedidos online.
- [usuário] Idioma do prompt: inglês, escolhido na pergunta de idioma; a conversa segue em português.
- [contexto: pasta atual] Vazia e sem repositório; os arquivos serão criados aqui.
- [sugestão] Papel, público e tom: dev front-end para pequeno negócio local; clientes da região, quase sempre no celular; textos curtos.
- [sugestão] Página única em HTML/CSS/JS estáticos, sem framework — mais simples de publicar e editar. Diga se prefere outra stack.
- [sugestão] Marcadores [FILL IN: …] para shop name, flavors, address and hours e contacts, que não foram informados.

Checagem de fidelidade: pedido original preservado (site + sorveteria); 3 sugestões marcadas, nenhuma virou exigência.
````
→ AskUserQuestion
```json
{"questions":[{"header":"Aprovação","question":"Este é o prompt que vou usar (v1). Você autoriza executá-lo agora?","multiSelect":false,
 "options":[
  {"label":"Executar agora","description":"Executo este prompt agora, nesta sessão, exatamente como está escrito."},
  {"label":"Ajustar","description":"Diga o que mudar; reviso só as partes afetadas e reapresento como v2."},
  {"label":"Só quero o prompt","description":"Nada é executado; o texto acima é o resultado."}]}]}
```
← `Executar agora`. Claude: `Executando o prompt confirmado (v1).`

Fallback of the closing question (no header; the turn ends after the last line):
```
Este é o prompt que vou usar (v1). Você autoriza executá-lo agora?
1. Executar agora — Executo este prompt agora, nesta sessão, exatamente como está escrito.
2. Ajustar — Diga o que mudar; reviso só as partes afetadas e reapresento como v2.
3. Só quero o prompt — Nada é executado; o texto acima é o resultado.
Responda com o número ou com suas palavras.
```

`[note]` Mixed rendering: everything outside the prompt block follows the conversation language — the proposal header, the premises title, the tags, the fidelity line, the closing question and the hand-off line use the Portuguese strings of references/localization.md — while the block follows the prompt language: labels `**Who:**` … `**Why:**`, markers `[FILL IN: …]`, and `Acceptance criteria:` / `References:` / `Limits:` when the task takes the complex shape. The premise about the markers is in Portuguese, but it names each marker by words taken from the marker as written in the block — shop name, flavors, address and hours, contacts — never by a translation. Had the user chosen `Português`, the block would use `**Who — Quem:**` `**What — O quê:**` `**How — Como:**` `**Why — Por quê:**`, `[PREENCHER: …]` and `Critérios de aceitação:` / `Referências:` / `Limites:`.

`[note]` The block is transcript A's block except the last sentence of How: the prompt language decides the language the prompt is written in, not the language of the deliverable's own text. That text's language is what the user said, else what the context shows, else the conversation language: the user named none and the empty folder shows none, so the site's copy follows the conversation — "Copy in Portuguese." here, as it is "Copy in English." in A.

`[note]` Turn 3 does not open with the `Contexto suficiente` line: the site-type round about the request (turn 2) preceded the proposal. The language round is not a question about the request, so a request that is Sufficient on its own, such as `crie um site institucional de uma página para a minha sorveteria, com HTML e CSS puros`, goes from the language round straight to the proposal, and that first proposal opens with the line `Contexto suficiente — nenhuma pergunta sobre o pedido. A proposta abaixo já é a confirmação; corrija o que estiver errado.`

`[note]` The prompt-language premise carries the user's tag because the user chose it in the language round. A request that already states the prompt language, such as `crie um site para uma sorveteria, prompt em inglês`, skips the language round: the first reply is turn 2, the proposal after it has no `Contexto suficiente` line, and the stated language is a `[usuário]` premise. A language stated later counts the same way: `Ajustar` with `quero o prompt em português` gives v2 with the Portuguese block (`**Who — Quem:**` …, `[PREENCHER: …]`). A "no questions" request, such as `crie um site para uma sorveteria, não pergunta nada, só faz logo`, skips the round too: the reply opens with the line `Contexto suficiente — nenhuma pergunta sobre o pedido. A proposta abaixo já é a confirmação; corrija o que estiver errado.`, then comes the proposal — this transcript's English block (copy in Portuguese), transcript H's premises in Portuguese and one more premise, `[sugestão] Prompt em inglês, o padrão desta skill; se preferir em português, escolha Ajustar e peça.`, so the fidelity line counts `5 sugestões marcadas`. That premise appears only when no prompt language was stated: a no-questions request that states it, such as `crie um site para uma sorveteria, prompt em inglês, não pergunta nada`, carries the stated language in its `[usuário]` premise instead, and the line counts `4 sugestões marcadas`.
