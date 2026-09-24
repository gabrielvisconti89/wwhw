# Contributing

Thank you for looking. This repository holds one skill, and one thing about it is unusual enough to say before
anything else.

## The visible strings are a contract

Every fixed string the skill prints — the question headings, the option labels and their descriptions, the
assumption tags, the fidelity line, the `Context is sufficient` line, the approval question and the hand-off
lines — is verified character for character against an eval suite that does not live in this repository. That
suite runs the skill across two dozen scenarios and checks hundreds of assertions about what it says and when.

So a change that reads like a harmless rewording ("this sentence flows better") can break behavior that is
tested elsewhere, and the maintainer has no way to accept it without re-running that validation. That is not a
reason to stay away — it is the reason this file exists, so you know which changes are cheap and which are not.

## Cheap and welcome

- **A bug report.** The skill broke one of its own rules: it started the task before you approved it, skipped
  the approval question, asked more than three questions in a round, invented a fact that was not in your
  request, or answered in the wrong language. Open an issue and paste the exchange, including what you typed.
  A transcript is worth more than a description.
- **A new language.** `skills/prompt/references/localization.md` is the one file built to grow. English is
  canonical; Portuguese is worked out in full as the example to derive from; the file's own rules say what to
  keep, what to translate and how to check a rendering for drift. A pull request that adds a language following
  those rules is welcome as it stands.
- **A worked transcript** in `skills/prompt/references/examples.md`, when it shows a case the existing ones do
  not: a kind of request, a correction, a conflict.
- **README fixes**: typos, a broken link, an install step that did not work on your machine.

## Open an issue first

Anything that edits the fixed strings or the rules in `skills/prompt/SKILL.md` — the question contract, the
proposal layout, the gate, the approval question, the language rules. These go through validation before they
ship, so a pull request that changes them will wait for that run rather than be merged on review alone. An
issue describing the behavior you want, with the case that motivates it, moves faster than a diff.

## Out of scope

Changes that make the skill perform the task instead of proposing it, or that remove the approval step. The
point of the skill is that nothing happens until you have seen the prompt and said so.

## Before you open a pull request

```sh
claude plugin validate .            # must print: ✔ Validation passed
ln -s "$PWD/skills/prompt" ~/.claude/skills/wwhw-dev   # then try /wwhw-dev in a scratch directory
```

Try your change on a real request before sending it. The skill's failure mode is subtle — it still produces a
prompt, just a worse one — so reading the output matters more than a green check.

## License and credit

This project is MIT licensed (see [LICENSE](LICENSE); a copy also sits inside `skills/prompt/` so the terms
travel with the skill folder). By contributing you agree that your contribution is offered under the same
license. If you list or redistribute the skill somewhere, please keep the author credit and the license with
it — the folder carries both, so copying the folder is enough.
