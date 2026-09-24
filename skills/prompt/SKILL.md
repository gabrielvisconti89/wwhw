---
name: prompt
description: Turns a plain request into a structured Who/What/How/Why prompt that the user approves before anything runs. Use when the user invokes /wwhw, or explicitly asks, in any language, to structure, refine or turn a request into a good prompt or brief before executing it (e.g. "draft the prompt first", "structure this request", "I want a who/what/how/why brief"). Not for executing the task itself, answering questions about prompting, or editing prompt files in a repository.
argument-hint: "[request in natural language]"
---

Request (the text the user typed after the command): $ARGUMENTS

If nothing follows the colon on that line, the request is empty (recipe in Step 3). If the line still shows the unexpanded argument token — a dollar sign followed by the word ARGUMENTS in capitals — this file was read as a file rather than invoked: take the request from the task you were given, minus any leading slash-command token; if nothing remains, the request is empty.

# From a plain request to a Who/What/How/Why prompt

This skill turns the request into a Who/What/How/Why prompt; it does not perform the task. Normalize a request already in Who/What/How/Why form into the four labels, preserving its wording. This skill calls itself "this skill" and prints no command name. The first visible text of a reply is the context lines, the `Context is sufficient` line, the `Revised:` line, the proposal header, the empty-request explainer, a hand-off line or `What do you want to change?`. These rules also govern every later turn. The fixed strings below are English; the Language section says when another language replaces them.

<hard_gate>
Do not start the task in the request — no file writes, no code, no analysis or data fetching for the task — until the user answers the closing question with `Run now` for the version currently shown. Before that answer, nothing is created or changed for the task: the only tool calls that touch the user's project or working directory are read-only (Read, Glob, Grep, LS, read-only Bash such as `ls`, `cat`, `git log`), plus AskUserQuestion. Presenting the proposal and starting the work in the same message is skipping the gate. Zero questions is allowed; skipping the closing question is not. The ceremony scales with the task; the gate does not.
</hard_gate>

## Step 0 — Read relevant context

Unless the request is empty, take one read-only look at the top-level tree of the working directory (Glob or `ls`). Open CLAUDE.md, manifests, README/CONTRIBUTING, the source files the deliverable is about, or files the request names only when the request concerns the current project or names a path, and read only what bears on the request. Step 0 prints no line of its own: what it found (an empty directory counts) goes inside the context lines when asking, or into [context: <source>] premises when proposing. Options grounded in real context make questions disappear.

## Step 1 — Intent register

Fill this register internally; the user sees it only compressed, as the premises.

| Pillar | Fields to identify |
|---|---|
| Who | Role and expertise the AI assumes; audience and its knowledge level. |
| What | Objective, task, deliverables, scope (in/out), success criteria. |
| How | Process, format and structure, depth, tools and environment, execution constraints. |
| Why | Motivation, real context, how the result will be used, decision priorities. |

Each field carries a value and a provenance tag. [user]: a user message contains it, verbatim or as an unambiguous paraphrase; a chosen option counts for exactly what its label encodes, and details of its description the user did not choose stay [suggested]. [context: <source>]: the conversation or a file read, relevant to this request. [suggested]: anything you decide. The tags keep your assumption from becoming a requirement the user apparently gave.

## Step 2 — Classify

Decide with these yes/no checks.

<checks>
- C1 Identifiable objective: one sentence "the user wants <verb> <object> so that <outcome>" that the user would recognize, without inventing object or outcome.
- C2 Defined deliverable: artifact type and boundary nameable, so "done" is recognizable. Placeholder words in the request ("asset X", "exchange Y", "that file") fail C2 and are asked as free text, named as placeholders in the context lines, never guessed; when the artifact type is already clear the request stays Medium.
- C3 Relevant constraints understood: every constraint that would change the work (stack, data source, language, depth, environment, deadline, audience level) is given, inferable from cited context, or defaultable with a reversible default.
- C4 No material contradiction.
- F Fork test: do two plausible readings produce different artifact types, purposes, audiences, data sources, or scopes differing by roughly an order of magnitude? Material = changes what the executor builds; presentation-only differences (headings, tone, section order) become [suggested].
</checks>

| Situation | Behavior |
|---|---|
| Sufficient, consistent context: C1–C4 pass and F is false | Present the proposal directly; the proposal is the confirmation round. |
| Low-impact gaps only | Adopt a reasonable suggestion and list it as [suggested]; no question. |
| Ambiguity that changes objective or scope (F true) | Ask one specific question with concrete options. |
| Contradictory requirements (C4 fails) | Show the conflict in the user's own terms and ask for a choice among 2–4 resolutions; the recommended option's "because" names whom each side of the conflict serves; no proposal in that turn. |
| The user does not yet know what they need | Offer paths as options and state the consequence of each. |
| Empty request | The Step 3 recipe; no proposal. |

The language round (Step 3) comes before this table's questions and proposals when it applies.

Tiers:
- **Shallow** — C1 fails, or C2 fails at the artifact-type level, and the plausible readings are different projects (generic verb, no object, no usable context): one question, three interpretations.
- **Medium** — C1 passes; C2 passes, or fails only on a placeholder word whose artifact type is clear; and F is true within the deliverable, or a material C3 gap cannot be safely defaulted, or C4 fails: one round of 1–3 questions.
- **Sufficient** — C1–C4 pass and F is false: no question.

Present a proposal carrying suggestions first; ask first only when F is true, C4 fails, or a placeholder word must be replaced by a value only the user can give (C2). Context moves a request up a tier: a manifest that pins stack and purpose can make it Sufficient. When the user asks for no questions ("no questions, just do it", in any language), ask none: each gap takes the least-assumption reading as a [suggested].

## Step 3 — Ask

<question_contract>
Ask every question to the user through the AskUserQuestion tool, including the closing question; this skill relies on that tool for decisions only the user can make.
Per round: one call with 1–3 questions (the tool allows 4; this skill caps at 3), most impactful first, independent of each other. Each question ends with "?". `header` ≤ 12 characters counting accents (Goal, Deliverable, Scope, Audience, Format, Data, Deadline, Decision, Conflict, Approval and Language all fit; Interpretation does not). 2–4 options; `multiSelect` set explicitly. Option `label` 1–5 words, not counting a `(recommended)` marker; option `description` states the consequence of choosing it (what the user gets, must provide, will not get), not marketing. At most one option per question carries `(recommended)` at the end of its label, listed first, with a "because …" reason in the description grounded in context, least-assumption/reversibility, or convention; with no basis, mark none (two recommendations are no recommendation). Do not add an "Other" option: the tool adds a free-text row automatically. Each answer comes back paired with its question text: the chosen label, or the free text the user typed; free text is new intent, parsed into the register as [user].
Before the call, ≤2 plain lines (the context lines): what is already clear, which gap changes the deliverable and why, and `If no option fits, write it in your own words.`
Shallow tier: exactly three interpretations (plus `Diagnose first` when answering needs knowledge the user may lack), varying the single axis that changes the deliverable most.
When a value must be typed (a ticker, a name), route to free text with an option such as `I'll type it`; use only the fields question, header, options and multiSelect.
Tool absent from the tool list, or the call errors: print the same content as text, one block per question `Question N of M — <header>: <question>`, options lettered `A) B) C)` as `<label> — <description>` (the recommended one keeps its "(recommended) … because …"), then `Answer with the letter or in your own words.`, and end the turn; the user's next message is the answer. Tool present but the harness reports that the question auto-closed because the user was away: adopt the recommended option as [suggested], say so in the premises, and continue.
</question_contract>

Language round. When the request is not in English and the user has not said which language the prompt should be in, the first reply holds only that round, before any question about the request and before any proposal: it opens with the context line `I'll keep talking in <language>; first, the language of the prompt. If no option fits, write it in your own words.`, then AskUserQuestion, header `Language`, question `In which language should I write the prompt?`, options `English (recommended)` — "The prompt is written in English; we keep talking in <language>. Recommended because English is this skill's default for prompts." — and `<language name>` — "The prompt is written in <language>, like our conversation." The answer holds for every later version and request in the conversation; after an empty request, the answer to the Goal question is the request this rule looks at. This round does not count toward the round cap or as a question about the request. When the user asks for no questions and has not stated the prompt language, skip the round: the prompt is in English, with the premise `[suggested] Prompt in English, this skill's default; if you prefer <language>, choose Adjust and ask.`

Empty request: open with `This skill turns a request of yours — for example "create a website for an ice cream shop" — into a structured prompt (Who/What/How/Why), confirms it with you and runs it only if you authorize it.`, add one sentence inviting the user to write freely or pick a direction, then ask with AskUserQuestion, header `Goal`, question `What do you want to achieve?`, exactly three options `Create something new` / `Analyze or research` / `Improve something that exists`. No command name is printed. That reply is about ten lines and invents no task.

Interview rules:
- Impact order: What, Why, How, Who (usually derivable). Shallow interpretations vary one axis — the first that applies of deliverable type, purpose, scope, audience, depth — as coherent one-line readings (typically minimal, expansive, different purpose), each a result the user could want, named in the user's terms rather than as a change to the material Step 0 read (for a request to improve something that exists, the quality that gets better), its description one line stating the consequence, a recommendation's because included; none is chosen for the user.
- Nothing answered or derivable from context is asked again. Ask about outcomes, symptoms, audience and use, not the technical solution. A free-text option names what to type, without sample values.
- Re-evaluate the whole request after every answer.
- The user replies that they cannot decide ("I don't know", "whatever, you decide"): adopt the recommended option as a [suggested] whose premise says the choice was left to this skill; with no recommended option, make `Diagnose first` the executor's first step or tag the decision [deferred]. That field is not asked again.
- A wider scope, or the decomposition of a request that bundles independent subsystems, is offered as a [suggested], not asked.
- At most 3 question rounds; then propose. The closing question and `What do you want to change?` do not count; a correction that opens a new material ambiguity may add one round.

## Step 4 — Proposal

User-facing text is plain terminal Markdown: no tables, emojis, decorative separators or files. The four-backtick fences below are not output.

<proposal_layout>
````markdown
## Proposed prompt (vN)

```text
**Who:** …

**What:** …

**How:** …

**Why:** …
```

**Assumptions made**
- [user] …
- [context: <source>] …
- [suggested] …
- [deferred] …      (only when the user deferred a decision)
- [open] …    (only when an item is still unresolved as the proposal is shown: after the round cap, or an open contradiction or gap; phrased in the generated prompt as the executor's first step)

Fidelity check: original request preserved (<key terms of the request>); <N> suggestions marked, none became a requirement.
````

Rules. On revisions, two lines `Revised: …` / `Kept: …` come first, then `## Proposed prompt (vN+1)`. Simple task: four short paragraphs, each pillar ≤ ~90 words. Complex task (analysis, code changes, multi-step deliverables): What ends with `Acceptance criteria:` (a sentence or short list); How names the references the executor must consult (files, sources, data, prior documents) as a `References:` sentence or inline ("Read X, Y and Z before …") and ends with `Limits:`; the block stays under ~650 words, and beyond that a split is offered as a [suggested]. Premises: at most 7 bullets (seven fit one screen), each starting with one provenance tag; only non-user items need detail, and the user's own statements may be summarized in a single [user] bullet; every [context: <source>] premise names its source. Unknown facts inside the block are fill-in markers `[FILL IN: <what>]`, each matched by a [suggested], [deferred] or [open] premise; no other bracketed holes. Nothing follows the closing question, so the turn ends waiting for the answer.
A first proposal that no question about the request preceded (Sufficient tier, a no-questions request, or right after the language round): the line `Context is sufficient — no questions about the request. The proposal below is the confirmation; correct anything that is wrong.` precedes the header.
</proposal_layout>

A format, structure or section list the user gave belongs in How, in the user's order and wording; What names the deliverable and what each part must contain, without repeating the list. The role, audience and tone you chose share one [suggested] bullet. A premise covering fill-in markers names each one by a word copied from the marker in the marker's own language, never translated: a Portuguese premise about `[FILL IN: shop name]` says shop name. With one suggestion, the fidelity line is singular. A task also takes the complex shape when the executor must first read existing material: repository files, data, or documents and APIs the request names. Check the ~650-word ceiling by estimate or with a command that writes no file.

Every concrete fact in the generated prompt (names, places, prices, tickers, repository facts) traces to [user] or [context], or is a fill-in marker. The generated How states that rule in words — date and source figures, name what is unknown — and never hands the executor the marker notation as a pattern. For a diagnosis (slowness, errors, a failing result), How also names existing evidence to check first — logs (slow-query, error), metrics — conditionally ("if they exist") when the user has not said what exists.

For a conversation in a language other than English, read references/localization.md before writing anything user-facing.

The Example near the end is the simple-task template.

## Step 5 — Fidelity review

Before presenting, check and fix inline:
- Who — does the role help execute the task and serve the audience?
- What — is it clear what to produce and how to recognize a satisfactory result?
- How — are format and constraints executable in the available environment?
- Why — does the context say which decisions and results to prioritize?
- Was any original requirement lost?
- Did any suggestion become an obligation without the user agreeing?

The visible residue is the fidelity line.

## Step 6 — Closing question and hand-off

<closing_question>
After every proposal, ask with AskUserQuestion, `header: "Approval"`, `multiSelect: false`, question `This is the prompt I will use (vN). Do you authorize me to run it now?`, options in this order:
- `Run now` — "I run this prompt now, in this session, exactly as written."
- `Adjust` — "Tell me what to change; I revise only the affected parts and present it again as vN+1."
- `Just the prompt` — "Nothing is run; the text above is the result."
Plain-text fallback: the same three as `<label> — <description>`, numbered 1–3, then `Answer with the number or in your own words.`, and the turn ends.
Hand-off. After `Run now` (or `1`): print `Running the confirmed prompt (vN).` and proceed with the generated prompt as the instruction for this session; normal session rules apply (permission mode, plan mode). After `Just the prompt` (or `3`): print `Prompt (vN) delivered above. Nothing was run.` and stop. After `Adjust` (or `2`) with text, or free text describing a change: revise only the affected pillars and re-present as vN+1 with the closing question again. After `Adjust` with no text: reply with the single line `What do you want to change?`, end the turn, and treat the next user message as the correction. A bare affirmative typed as free text ("yes", "go ahead", "run it", in any language) counts as `Run now`; a bare negative ("no", "don't run it") counts as `Just the prompt`; anything else is a correction. The closing question is asked after every version, identical, even when the request already said "and run it". Approval binds to the version shown; a changed goal, even after approval, is a new version. A separate request later in the conversation starts again at v1.
</closing_question>

The closing question's plain-text fallback prints no header: it starts with the question text. Every `vN` and `vN+1` above is printed as its version number (`v1`, `v2`, `v3` …), never as the token.

The gate, restated: until `Run now` answers the version currently shown, nothing is created or changed for the task.

Corrections: turn the change into field-level deltas tagged [user]; re-derive only the [suggested] fields that depend on them (map in references/framework.md); leave the other [user] and [context: …] fields untouched; drop premises no pillar uses any more. When the audience changes, rebuild the What and How contents the user did not state from the new audience's use of the result (Why) and knowledge level (Who): an element of the previous version, reworded or re-purposed, stays only when that use needs it and that audience can follow it.

## Language

Two languages are in play. The conversation language covers everything the user sees outside the prompt block: context lines, questions and options, the proposal header, premises and their tags, the fidelity line, the closing question and the hand-off lines. It is the language of the request; for a bare fragment (a ticker, a file name) or an empty request, the language of the conversation so far; with none, English. The prompt language covers the text inside the prompt block: the language the user stated ("prompt in Portuguese", "the prompt in English"), in the request or later; otherwise English for a request in English, and for any other request the answer to the language round. In a conversation language other than English, the fixed strings come from references/localization.md. The generated prompt names the language of the deliverable's own text (site copy, an e-mail, a README): what the user said, else what the context shows (a repository's existing documents), else the conversation language. The fill-in marker and the `Acceptance criteria:` / `References:` / `Limits:` labels follow the prompt language; a prompt in another language labels its pillars `**Who — <word>:**`, with the words in localization.md. In a mixed-language request follow the sentence structure, not borrowed technical terms. Proper nouns, identifiers, paths, commands and terms the user wrote in another language stay as written.

## Example

Example (request `create a website for an ice cream shop`, English, empty directory, the user chose the informational site); it doubles as the template for a simple task:

````markdown
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

## Red flags

| Thought | Reality |
|---|---|
| "The request is obviously clear, I'll just do it." | Present the proposal and wait. Simple requests are where unexamined assumptions cost most. |
| "The user said no questions." | Zero questions is allowed; the proposal and the closing question are not optional. |
| "One more question would make it perfect." | If it does not change the deliverable, it is a [suggested]. |
| "AskUserQuestion is unavailable, so I'll take the recommended option." | Print the question as text and end the turn. |
| "This is a follow-up; the gate was already passed." | Approval binds to the version shown; a changed goal is a new version. |
| "I'm 95% sure I understood." | Run C1–C4 and F; no confidence scores. |

## References

- [references/examples.md](references/examples.md) — read when unsure about the shape or length of a question round or proposal.
- [references/framework.md](references/framework.md) — read when unsure which pillar a fact belongs to, or which fields a correction invalidates.
- [references/localization.md](references/localization.md) — read before writing anything user-facing when the conversation language is not English.
