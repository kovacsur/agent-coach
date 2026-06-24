# Level: `tighten` (default)

You are auditing under `tighten`. This file is the complete category list for this level — it includes everything `polish` flags, plus the tighten-specific categories. `SKILL.md` carries the always-on protected list, the 3-question test, the workflow, and the output format.

## Disposition

Default-keep with an explicit deletion bar. Apply the 3-question test (in `SKILL.md`) to every borderline line. Prefer rewriting in place when the payload survives at lower token cost; only delete when there is no payload to preserve.

## Categories flagged at this level

1. **Restated model capabilities.** Explaining what JSON, Markdown, a stack trace, or a CLI flag *is*.
2. **Verbatim duplicate reinforcement.** The same rule restated word-for-word elsewhere.
3. **Prose written for human reviewers.** Rationale paragraphs, *"as we discussed…"*, *"this is somewhat controversial…"*.
4. **Verbose justifications / preambles.** *"Because users may sometimes need…, you should…"*. The story delays the imperative; lead with the directive.
5. **Background unneeded to act.** Project history, design rationale, meeting context. Belongs in design docs, not always-loaded prompts.
6. **Paraphrased redundant reinforcement.** Same rule restated in different words across sections. Each restatement adds compounding interference — not linear cost: degradation accelerates with each additional restatement. Keep the single clearest statement; delete or reword the weaker paraphrase. Do not merge the two into one compound sentence — merging is the least effective edit action.
7. **Soft hedging.** *"You may want to consider…"*, *"it might be worth…"*. The agent files hedging under low-priority noise.
8. **Sprawling examples.** Multi-paragraph illustrations where a one-liner or a *see `<fixture>`* pointer would suffice.
9. **Demonstration–rule conflict.** Examples whose content or style contradicts the surrounding directive. The agent imitates examples more reliably than it interprets prose, so a contradicting example overrides its own rule. Fix by correcting the example, not deleting it.
10. **"Don't do X" stories.** Narrative explanation of past failure instead of a crisp directive. Verbose *and* triggers the pink-elephant effect. Convert to a binary directive plus (if recurrence matters) a one-line dated incident note. Pair non-safety prohibitions with the positive alternative the agent should produce instead.
11. **Walls of context fetchable on demand.** Inlined facts that a CLI / MCP / tool call can surface when actually needed.
12. **Project specifics in core skill files.** Repo-specific naming, paths, conventions in a skill meant to be portable. Move to overlays or project rules.
13. **Author-side provenance metadata.** Labels documenting where content came from / what level introduced it / what decision led to it, when the agent doesn't act on that information. Triggers: *"inherited from X"*, *"added at level Y"*, decision-log breadcrumbs in a directive file. Belongs in a maintenance sidecar (`MAINTENANCE.md`), the decisions log, or VCS history.

## Tool descriptions at this level

**Excluded entirely.** Verbosity in tool descriptions (3–4 sentences with when / when-not / params / caveats) is the documented best practice. Skip them. If the user wants a tool-description pass, run `polish`.

## Sentence-level red flags

Fast first-pass triggers. Presence ≠ verdict.

- **Hedging:** *"you may want to"*, *"it might be worth"*, *"consider whether"*, *"perhaps"*, *"if appropriate"*, *"where possible"*.
- **Justification preamble:** *"because users may sometimes"*, *"in order to ensure that"*, *"for the purpose of"*, *"the reason for this is"*.
- **Capability restatement:** *"a JSON file is"*, *"Markdown supports"*, *"git allows you to"*, *"the agent is capable of"*.
- **Human-reviewer prose:** *"as you know"*, *"as we discussed"*, *"historically"*, *"the team agreed"*, *"this is somewhat controversial"*.
- **Story-framed / bare prohibition:** *"there was a case where someone…"* without a date, *"we have found that occasionally…"*, *"don't use X"* with no affirmative replacement. Convert to a dated incident note (keep), a crisp directive (rewrite), or a positive alternative.
- **Provenance markers:** *"inherited from"*, *"added at level"*, *"originally written for"*, *"see decision Dn"*, *"this section was rewritten because"*. Move to sidecar / decisions log / VCS history.
- **Sprawl markers:** numbered lists longer than ~7 items, code blocks longer than ~15 lines without a fixture-pointer alternative, paragraphs longer than ~5 sentences.
- **Time-relative anchorless phrasing:** *"recent"*, *"modern"*, *"latest"*, *"current"*, *"today's"*, *"in the past few"*. Replace with an absolute anchor or a tool call.
- **Unanchored numeric precision:** *"exactly N words"*, *"in N lines"*, *"N bullet points"*, *"no more than N"*. Replace with qualitative phrasing or a range unless downstream tooling enforces the count.
- **Modal-verb inflation:** most directives marked *"must"*, *"always"*, or *"never"*. Reserve unconditional language for binary safety/correctness invariants and true hard constraints.
