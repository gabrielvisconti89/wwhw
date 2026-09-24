# Localizing the fixed strings

SKILL.md holds the canonical wording of every fixed string, in English. The conversation language decides what
the user reads: in English, print SKILL.md's strings as they stand; in any other language, translate each one
once, faithfully, and print that translation the same way every time. The prompt language is separate and
governs only the text inside the prompt block, so an English string can surround a block in another language
and the other way round.

Portuguese is worked out in full at the end of this file. It is an example, not a privileged language: it shows
what a faithful translation looks like, and it is the yardstick for checking your own rendering of any other
language. Derive a new language the same way, by the rules below.

## Translating into another language

1. Keep the structure of every string: the same clauses in the same order, the same em dash ` — ` (never `-` or
   `–`), the same colons, the same ellipsis `…`, the same parentheses. A longer sentence is fine; a rearranged
   one is not.
2. Translate the words the user reads: question texts, option labels and descriptions, headers, the premise
   title, the tag names, the fidelity line, the Sufficient line, the closing question and the hand-off lines.
3. Fill in, never translate: `…` and `<…>` are replaced by real content, the counts `N` and `N of M` by
   numbers, and `vN` / `vN+1` by the version numbers (`v1`, `v2`, `v3` …).
4. Keep the recommendation mechanics: exactly one option may carry the language's own `(recommended)` marker at
   the end of its label, listed first, and its description states the reason with the language's own "because".
5. Make the count agree: with exactly one suggestion the fidelity line takes the singular row, with any other
   count the plural one.
6. Keep headers within 12 characters after translation, counting accented characters as one each; choose a
   shorter word rather than an abbreviation.
7. Pillar labels: a prompt written in English uses `**Who:**` `**What:**` `**How:**` `**Why:**`; a prompt in
   another language keeps the English word and adds the translation after an em dash, as `**Who — <word>:**`.
8. The fill-in marker takes the language's own word in a prompt written in that language, and a premise names
   each marker with the words of the marker itself, in the marker's language, never a translation of them.
9. What never changes in any language: proper nouns, identifiers, paths, commands, code, tool names, and terms
   the user wrote in another language.

## Checking a rendering against the English original

Compare string by string with SKILL.md — or, for Portuguese, with the tables below — and treat each of these as
a deviation to fix, not a stylistic choice:

- a paraphrase, a synonym or a reordered clause where a fixed string was expected;
- a missing clause, most often the tail of the fidelity line or of the Sufficient line;
- a hyphen or en dash where the string has an em dash, or a straight quote where it has none;
- an option description replaced by new wording, or a second option marked as recommended;
- a recommendation with no reason, or a reason that does not name what the choice costs or gives;
- a version token left as `vN` or `vN+1` instead of its number;
- a plural fidelity line with one suggestion, or a singular one with several;
- a header longer than 12 characters, or an added "Other" option;
- a marker name translated inside a premise instead of copied from the marker;
- the prompt block written in the conversation language when the user chose another prompt language, or the
  text around the block written in the prompt language instead of the conversation language.

When a string has no natural equivalent in the target language, keep its structure and accept the literal
rendering: the structure carries the contract, the style does not.

## Worked example: Portuguese

Print each string of the right column exactly as written here, with the same words, dashes and punctuation;
only the placeholders and counts of rule 3 are filled in.

### Conversation strings (everything outside the prompt block)

| English (default) | Portuguese |
|---|---|
| `If no option fits, write it in your own words.` | `Se nenhuma opção servir, escreva com suas palavras.` |
| `Question N of M — <header>: <question>` | `Pergunta N de M — <header>: <question>` |
| `Answer with the letter or in your own words.` | `Responda com a letra ou com suas palavras.` |
| `(recommended)` and "because …" | `(recomendado)` and "porque …" |
| `I'll type it` | `Vou digitar agora` |
| `Diagnose first` | `Diagnóstico primeiro` |
| Headers `Goal` `Deliverable` `Scope` `Audience` `Format` `Data` `Deadline` `Decision` `Conflict` `Approval` `Language` | `Objetivo` `Entrega` `Escopo` `Público` `Formato` `Dados` `Prazo` `Decisão` `Conflito` `Aprovação` `Idioma` |
| Language round: `I'll keep talking in <language>; first, the language of the prompt. If no option fits, write it in your own words.` | `Sigo conversando em português; antes, o idioma do prompt. Se nenhuma opção servir, escreva com suas palavras.` |
| `In which language should I write the prompt?` | `Em que idioma você quer o prompt?` |
| `English (recommended)` — "The prompt is written in English; we keep talking in <language>. Recommended because English is this skill's default for prompts." | `Inglês (recomendado)` — "O prompt é escrito em inglês; seguimos conversando em português. Recomendado porque inglês é o padrão desta skill para prompts." |
| `<language name>` — "The prompt is written in <language>, like our conversation." | `Português` — "O prompt é escrito em português, como a nossa conversa." |
| `This skill turns a request of yours — for example "create a website for an ice cream shop" — into a structured prompt (Who/What/How/Why), confirms it with you and runs it only if you authorize it.` | `Esta skill transforma um pedido seu — por exemplo "crie um site para uma sorveteria" — em um prompt estruturado (Who/What/How/Why), confirma com você e só executa se você autorizar.` |
| `What do you want to achieve?` with `Create something new` / `Analyze or research` / `Improve something that exists` | `O que você quer conseguir?` with `Criar algo novo` / `Analisar ou pesquisar` / `Melhorar algo que já existe` |
| `## Proposed prompt (vN)` | `## Prompt proposto (vN)` |
| `Revised: …` / `Kept: …` | `Revisado: …` / `Mantido: …` |
| `**Assumptions made**` | `**Premissas adotadas**` |
| `[user]` `[context: …]` `[suggested]` `[deferred]` `[open]` | `[usuário]` `[contexto: …]` `[sugestão]` `[adiado]` `[pendente]` |
| `Fidelity check: original request preserved (…); N suggestions marked, none became a requirement.` | `Checagem de fidelidade: pedido original preservado (…); N sugestões marcadas, nenhuma virou exigência.` |
| `[suggested] Prompt in English, this skill's default; if you prefer <language>, choose Adjust and ask.` | `[sugestão] Prompt em inglês, o padrão desta skill; se preferir em português, escolha Ajustar e peça.` |
| One suggestion: `Fidelity check: original request preserved (…); 1 suggestion marked, none became a requirement.` | `Checagem de fidelidade: pedido original preservado (…); 1 sugestão marcada, nenhuma virou exigência.` |
| `Context is sufficient — no questions about the request. The proposal below is the confirmation; correct anything that is wrong.` | `Contexto suficiente — nenhuma pergunta sobre o pedido. A proposta abaixo já é a confirmação; corrija o que estiver errado.` |
| `This is the prompt I will use (vN). Do you authorize me to run it now?` | `Este é o prompt que vou usar (vN). Você autoriza executá-lo agora?` |
| `Run now` — "I run this prompt now, in this session, exactly as written." | `Executar agora` — "Executo este prompt agora, nesta sessão, exatamente como está escrito." |
| `Adjust` — "Tell me what to change; I revise only the affected parts and present it again as vN+1." | `Ajustar` — "Diga o que mudar; reviso só as partes afetadas e reapresento como vN+1." |
| `Just the prompt` — "Nothing is run; the text above is the result." | `Só quero o prompt` — "Nada é executado; o texto acima é o resultado." |
| `Answer with the number or in your own words.` | `Responda com o número ou com suas palavras.` |
| `Running the confirmed prompt (vN).` | `Executando o prompt confirmado (vN).` |
| `Prompt (vN) delivered above. Nothing was run.` | `Prompt (vN) entregue acima. Nada foi executado.` |
| `What do you want to change?` | `O que você quer mudar?` |

### Prompt-block strings (the prompt language)

An English prompt uses the left column. A prompt in Portuguese uses the right column; a prompt in any other language keeps the English pillar word and adds its translation after the dash, and translates the other three strings.

| English prompt | Prompt in Portuguese |
|---|---|
| `**Who:**` `**What:**` `**How:**` `**Why:**` | `**Who — Quem:**` `**What — O quê:**` `**How — Como:**` `**Why — Por quê:**` |
| `[FILL IN: <what>]` | `[PREENCHER: <o quê>]` |
| `Acceptance criteria:` / `References:` / `Limits:` | `Critérios de aceitação:` / `Referências:` / `Limites:` |

The two languages can differ: a conversation in Portuguese with a prompt in English prints `## Prompt proposto (vN)`, `**Premissas adotadas**` and Portuguese tags around a block whose labels are `**Who:**` … `**Why:**` and whose markers are `[FILL IN: …]`. A premise then names each marker with the words of the marker itself, in the prompt's language: `[sugestão] Marcadores [FILL IN: …] para shop name, flavors, address and hours e contacts`, never a translation of them.
