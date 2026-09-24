# wwhw — from a plain request to a Who/What/How/Why prompt

A skill for Claude Code that turns a request written in ordinary words into a structured prompt — **Who**,
**What**, **How**, **Why** — shows it to you, and runs it only after you approve it.

It is a thinking step in front of the work, not the work itself. You get to see, and correct, the prompt that
will drive the task before a single file is written.

```
you   /wwhw create a website for my ice cream shop

wwhw  Current folder empty, no repository. The kind of site changes what gets built.
      If no option fits, write it in your own words.

      Question 1 of 1 — Deliverable: What kind of website should it be?
        A) Informational page (recommended) — Name, flavors, address and hours, contact.
           Recommended because it is what a local shop needs first and the fastest to publish.
        B) Online ordering — A catalog with a cart and checkout; needs payment and stock decisions.
        C) Landing page for an offer — One offer, one call to action; the rest is left out.
      Answer with the letter or in your own words.

you   A

wwhw  ## Proposed prompt (v1)
      … the four pillars, then the assumptions behind them, each tagged with where it came from …

      This is the prompt I will use (v1). Do you authorize me to run it now?
      1. Run now — I run this prompt now, in this session, exactly as written.
      2. Adjust — Tell me what to change; I revise only the affected parts and present it again as v2.
      3. Just the prompt — Nothing is run; the text above is the result.
```

The exchange above is shown in plain text. In Claude Code the questions arrive as an interactive dialog you pick
from; the plain-text form is the fallback when that dialog is unavailable, and it carries the same content.

## How a turn goes

1. **Reads what is already there.** One read-only look at the working directory, plus the files the request
   points at. Nothing is created or changed at this stage.
2. **Decides whether it has to ask.** Four checks and a fork test: is the objective identifiable, is the
   deliverable defined, are the constraints known, is anything contradictory, and would two plausible readings
   produce different artifacts? Only a gap that changes what gets built becomes a question.
3. **Asks at most three questions, in one round**, each with two to four concrete options that state what
   choosing them costs and gives. At most one option is marked as recommended, with the reason. There is always
   a way to answer in your own words. Zero questions is a valid outcome.
4. **Proposes the prompt**, with an `Assumptions made` list where every item says where it came from: `[user]`
   for what you said, `[context: <source>]` for what was read, `[suggested]` for what the skill decided,
   `[deferred]` and `[open]` for what is still undecided. A suggestion never turns into a requirement you
   appear to have given.
5. **Asks for approval**, always, even when the request already said "and do it". `Run now` executes the prompt
   as written; `Adjust` revises only the affected pillars and presents the next version; `Just the prompt`
   hands you the text and stops.

Unknown facts inside the prompt become visible markers such as `[FILL IN: shop name]`, each one covered by an
assumption, instead of invented details. Prices, names, tickers and repository facts either trace to you, trace
to something that was read, or are marked.

## The gate

Until you answer `Run now` for the version on screen, nothing is created or changed for the task: no files, no
code, no data fetching. Presenting a prompt and starting the work in the same message is not allowed. Approval
binds to the version you saw — change the goal afterwards and it becomes a new version with a new approval.

## Languages

Two languages are in play, and they are independent.

- **The conversation language** is the language of your request, and it covers everything outside the prompt
  block: questions, options, assumptions, the fidelity line, the approval question.
- **The prompt language** is the language of the prompt itself. English is the default, because prompts in
  English travel better between models and tools.

If you write in a language other than English and have not said which language you want the prompt in, the
first reply is a single question about exactly that, before anything else is asked. Answer once and it holds for
the rest of the conversation. Say it in the request — "with the prompt in Portuguese" — and no question is
asked. Ask for no questions at all and you get a prompt in English with a marked suggestion telling you how to
switch.

The deliverable's own language is a third, separate thing: an English prompt can ask for website copy in
Portuguese, and the prompt says so explicitly.

`skills/prompt/references/localization.md` holds the canonical English strings, the rules for translating them into any
language, and a checklist for spotting a translation that drifted. Portuguese is worked out in full there as the
example to derive other languages from — it is a demonstration, not a privileged language.

## Install

This repository is both a plugin and its own marketplace, so Claude Code can install it in two commands:

```
/plugin marketplace add gabrielvisconti89/wwhw
/plugin install wwhw@wwhw
```

Installed that way the skill is namespaced, as plugin skills always are: you type **`/wwhw:prompt your
request`**. Claude can still invoke it on its own when you ask, in your own words, to have a request structured
before it runs.

To install it as a personal skill instead, and keep the shorter **`/wwhw your request`**:

```sh
git clone https://github.com/gabrielvisconti89/wwhw.git
ln -s "$PWD/wwhw/skills/prompt" ~/.claude/skills/wwhw   # or: cp -R wwhw/skills/prompt ~/.claude/skills/wwhw
```

The command is the name of the directory you create under `~/.claude/skills/`, so linking it as `wwhw` gives
`/wwhw`; name it something else and that becomes the command.

For a single project instead of your whole account, use `<project>/.claude/skills/wwhw`. Installing both ways
is harmless: the plugin's `/wwhw:prompt` and a personal `/wwhw` coexist, neither overrides the other.

One note: to upload the skill to claude.ai instead, remove the `argument-hint` line from the front matter; it
is a Claude Code convenience and other hosts reject unknown keys.

## Files

| Path | What it is |
|---|---|
| `skills/prompt/SKILL.md` | The skill: the gate, the four checks, the question contract, the proposal layout, the approval question and the language rules. |
| `skills/prompt/references/examples.md` | Nine worked transcripts — a simple request, a vague one, an empty one, a contradiction, a correction that changes the audience, a mixed-language conversation. |
| `skills/prompt/references/framework.md` | Which pillar a fact belongs to, which fields a correction invalidates, and the framework as originally written. |
| `skills/prompt/references/localization.md` | The canonical English strings, how to translate them into any language, and how to detect a drifted translation. |
| `.claude-plugin/plugin.json` | The plugin manifest: name, version, license and metadata. |
| `.claude-plugin/marketplace.json` | Makes this repository its own marketplace, so `/plugin marketplace add` finds the plugin at the root. |
| `CONTRIBUTING.md` | What is cheap to change and what is verified elsewhere, so a pull request does not arrive dead. |

The reference files are read only when needed, so a simple request costs one file.

## What it is not for

Doing the task without showing the prompt, answering questions about prompt engineering, or editing prompt
files in a repository. It structures a request and waits for your word.

## Contributing

Bug reports, new languages for `localization.md` and new worked transcripts are welcome as they stand; changes
to the skill's fixed strings are validated before they ship, so start those with an issue. See
[CONTRIBUTING.md](CONTRIBUTING.md).

## Author and license

Written by Gabriel Visconti (<https://github.com/gabrielvisconti89>) and released under the MIT License — see
[LICENSE](LICENSE). A copy of the license also sits inside `skills/prompt/`, so the skill carries its terms
wherever the folder is copied. If you list this skill in a catalog, please keep the credit and the license
with it.
