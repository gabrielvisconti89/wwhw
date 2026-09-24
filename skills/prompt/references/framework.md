# Framework reference: Who, What, How, Why

Use this file to decide which pillar a fact belongs to and which fields a correction invalidates.

## Contents

- Pillars and intent register fields
- Which pillar does this fact belong to
- Which fields a correction invalidates
- Fidelity questions
- The framework as originally written

## Pillars and intent register fields

Canonical wording and order: Who → What → How → Why.

| Pillar | What to identify (intent register fields) |
|---|---|
| Who | Role and expertise the AI assumes; audience and its knowledge level. |
| What | Objective, task, deliverables, scope (in/out), success criteria. |
| How | Process, format and structure, depth, tools and environment, execution constraints. |
| Why | Motivation, real context, how the result will be used, decision priorities. |

Each row lists the fields of the intent register for that pillar. A field holds a value and a provenance tag: [user], [context: <source>] or [suggested], as defined in SKILL.md, Step 1. In the premises, [deferred] marks a decision the user deferred and [open] an item still unresolved when the proposal is shown. The tags are written here in English; in a conversation in another language the premises print them as references/localization.md gives them.

## Which pillar does this fact belong to

Match the fact to the field it fills; the field's row gives the pillar.

| The fact says… | Field | Pillar |
|---|---|---|
| which role or expertise the AI assumes | role and expertise | Who |
| who receives the result and how much they already know | audience and its knowledge level | Who |
| what the user wants achieved or done | objective, task | What |
| which artifacts must exist at the end | deliverables | What |
| what is included and what is left out | scope (in/out) | What |
| how a satisfactory result is recognized | success criteria | What |
| which steps or method to follow | process | How |
| what shape the output takes and how deep it goes | format and structure, depth | How |
| which tools are used and where the work happens | tools and environment | How |
| which limits apply while the work is carried out | execution constraints | How |
| why the request exists and what situation surrounds it | motivation, real context | Why |
| what happens to the result afterwards | how the result will be used | Why |
| what wins when two goals compete | decision priorities | Why |

Neighbouring fields that are easy to confuse:

- Audience (Who) and use of the result (Why): who reads the result is Who; what that reader does with it is Why.
- Deliverables (What) and format and structure (How): the thing produced is What; the shape it takes is How.
- Scope (What) and execution constraints (How): what the deliverable includes or leaves out is What; a limit on how the executor works is How.
- Success criteria (What) and decision priorities (Why): a test that tells when the deliverable is satisfactory is What; a rule for choosing between competing options during the work is Why.
- A fact that answers two rows fills both fields, each in the pillar of its own row.

## Which fields a correction invalidates

A correction changes one or more fields. In each row, the fields in the right column depend on the field in the left column, so they are the ones to look at again.

| Corrected field | Fields it invalidates |
|---|---|
| objective (What) | deliverables (What), format (How), tools (How), use of the result (Why) |
| deliverables (What) | the How pillar |
| use of the result (Why) | depth (How), audience (Who) |
| audience (Who) | tone, format and depth (How); scope and success criteria (What); use of the result (Why) |

Reading the map:

- Set the corrected field from the user's words, tagged [user].
- Among the invalidated fields, re-derive only the [suggested] ones. A field tagged [user] or [context: <source>] stays as it is.
- A field that is neither corrected nor invalidated keeps its value and its wording, and is not asked again.
- Drop a premise that no pillar uses any more.
- A [context: <source>] fact stays true after a correction; whether the deliverable still includes it is scope, re-derived for a new audience unless the user stated it.

## Fidelity questions

Before presenting a proposal, answer these six questions and fix inline whatever fails. The visible result of this review is the fidelity line.

1. Who — does the role help execute the task and serve the audience?
2. What — is it clear what to produce and how to recognize a satisfactory result?
3. How — are format and constraints executable in the available environment?
4. Why — does the context say which decisions and results to prioritize?
5. Was any original requirement lost?
6. Did any suggestion become an obligation without the user agreeing?

## The framework as originally written

The passages quoted below are the framework's original statement, translated faithfully into English; everything above them is scaffolding for applying it.

> ## The framework (Who, What, How, Why)
> A prompt-writing framework based on four fundamental questions, for turning generic instructions into clear, focused objectives. As Professor Hanspeter Pfister points out, writing prompts and managing inputs are part of context engineering (the prompt is the control/instruction layer; the context is the supply layer). The quality of the answer depends on the clarity and richness of the instructions.
>
> ### 1. Who? — The Persona and the Audience
> - Objective: defines the personality, perspective, expertise and role the AI should assume, and the target audience of the answer.
> - Importance: a specific role helps the model adopt suitable vocabulary, tone and criteria.
> - Example: "Act as a senior corporate tax strategy consultant with experience in US federal, state and international taxes. Assume you are advising a CFO and legal counsel..."
>
> ### 2. What? — The Task and the Objectives
> - Objective: specifies the central question or problem; defines direct goals and what the AI must carry out.
> - Importance: prevents vague answers by delimiting the scope.
> - Example: "Analyze the most common tax issues and identify the main legal questions, likely treatments and decision points. Provide a clear recommendation with alternative options..."
>
> ### 3. How? — The Process and the Formatting
> - Objective: sets the format of the answer, the structure of the document, the tone of voice and the output constraints.
> - Importance: guarantees the exact format needed to decide or to share, without manual reformatting.
> - Example: "Structure your answer as: (1) Summary, (2) Applicable rules and assumptions, (3) Analysis, (4) Risks and uncertainties, (5) Recommended approach, (6) Next steps and questions for confirmation. Use plain language and include a short checklist..."
>
> ### 4. Why? — The Situation, the Context and the Constraints
> - Objective: explains the reason for the request, the real context and how the answer will be used.
> - Importance: makes it possible to prioritize what matters for the decision (practical clarity instead of an academic treatise).
> - Example: "This analysis will be used to support an internal go/no-go decision on a proposed transaction and to prepare a discussion with outside counsel. Optimize for clarity and usefulness in decision-making, not for exhaustive academic-level coverage."
>
> ### Why use it
> - Avoids the oversimplification trap: simple prompts produce shallow, generic answers.
> - Plan alignment: the AI executes plans, but it is up to the human expert to define the problem and the constraints.
> - Improvement through iteration: after the first answer, review it critically and iterate (adjust the prompt or provide additional context with real data).
