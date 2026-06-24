# Level: `compress`

You are auditing under `compress`. This file is the complete category list for this level — it includes everything `tighten` flags (which itself includes everything `polish` flags), plus compress-specific categories. `SKILL.md` carries the always-on protected list, the 3-question test, the workflow, and the output format.

## Disposition

Burden of proof shifts to the line: each save must be justified by an articulable answer to all three load-bearing questions. Targets ~30–50 % length reduction overall. Keep the always-on protected list (in `SKILL.md`) untouched regardless.

## Categories flagged at this level

1. **Restated model capabilities.** Explaining what JSON, Markdown, a stack trace, or a CLI flag *is*.
2. **Verbatim duplicate reinforcement.** The same rule restated word-for-word elsewhere.
3. **Prose written for human reviewers.** Rationale paragraphs, *"as we discussed…"*, *"this is somewhat controversial…"*.
4. **Verbose justifications / preambles.** *"Because users may sometimes need…, you should…"*. Lead with the directive.
5. **Background unneeded to act.** Project history, design rationale, meeting context.
6. **Paraphrased redundant reinforcement.** Same rule restated in different words across sections. Each restatement adds compounding interference — not linear cost: degradation accelerates with each additional restatement. Keep the single clearest statement; delete or reword the weaker paraphrase. Do not merge the two into one compound sentence — merging is the least effective edit action.
7. **Soft hedging.** *"You may want to consider…"*. The agent treats hedging as noise.
8. **Sprawling examples.** Multi-paragraph illustrations where a one-liner or fixture pointer suffices.
9. **Demonstration–rule conflict.** Examples whose content or style contradicts the surrounding directive. The agent imitates examples more reliably than it interprets prose, so a contradicting example overrides its own rule. Fix by correcting the example, not deleting it.
10. **"Don't do X" stories.** Convert to a binary directive (and a dated note if recurrence matters). Pair non-safety prohibitions with the positive alternative the agent should produce instead.
11. **Walls of context fetchable on demand.** Inlined facts a tool call could surface.
12. **Project specifics in core skill files.** Move to overlays or project rules.
13. **Author-side provenance metadata.** Labels documenting where content came from / what level introduced it / what decision led to it, when the agent doesn't act on that information. Triggers: *"inherited from X"*, *"added at level Y"*, decision-log breadcrumbs in a directive file. Belongs in a maintenance sidecar (`MAINTENANCE.md`), the decisions log, or VCS history.
14. **Position-poor placement.** Critical rules buried mid-document where lost-in-the-middle hits hardest. Flag for *relocation* (top of file or end), not deletion. Within a constraint list, order hardest / highest-priority constraints first — this is simultaneously primacy-safe (causal transformers architecturally favour earlier positions) and difficulty-gradient-correct (hard-to-easy ordering outperforms alternatives across architectures). If constraints are embedded in task-description prose rather than a dedicated block, consider extracting them into an explicit constraints section — the separation itself improves compliance, independent of any content change.
15. **Bridging prose between directives** *(cautiously)*. A connective sentence between two rules earns its tokens only if removing it changes how the rules combine. If the two rules read identically when the bridge is gone, cut it.

## Tool descriptions at this level

**Excluded entirely.** Same as `tighten`. If the user wants a tool-description pass, run `polish`.

## Sentence-level red flags

Fast first-pass triggers. Presence ≠ verdict.

- **Hedging:** *"you may want to"*, *"it might be worth"*, *"consider whether"*, *"perhaps"*, *"if appropriate"*, *"where possible"*.
- **Justification preamble:** *"because users may sometimes"*, *"in order to ensure that"*, *"for the purpose of"*, *"the reason for this is"*.
- **Capability restatement:** *"a JSON file is"*, *"Markdown supports"*, *"git allows you to"*, *"the agent is capable of"*.
- **Human-reviewer prose:** *"as you know"*, *"as we discussed"*, *"historically"*, *"the team agreed"*, *"this is somewhat controversial"*.
- **Story-framed / bare prohibition:** *"there was a case where someone…"* without a date, *"don't use X"* with no affirmative replacement.
- **Provenance markers:** *"inherited from"*, *"added at level"*, *"originally written for"*, *"see decision Dn"*, *"this section was rewritten because"*. Move to sidecar / decisions log / VCS history.
- **Sprawl markers:** numbered lists longer than ~7 items, code blocks longer than ~15 lines without a fixture pointer, paragraphs longer than ~5 sentences.
- **Time-relative anchorless phrasing:** *"recent"*, *"modern"*, *"latest"*, *"current"*, *"today's"*, *"in the past few"*. Replace with an absolute anchor or a tool call.
- **Unanchored numeric precision:** *"exactly N words"*, *"in N lines"*, *"N bullet points"*, *"no more than N"*. Replace with qualitative phrasing or a range unless downstream tooling enforces the count.
- **Modal-verb inflation:** most directives marked *"must"*, *"always"*, or *"never"*. Reserve unconditional language for binary safety/correctness invariants and true hard constraints.
- **Position cues:** binary safety invariants or load-bearing directives appearing after long expository sections; the document's most important line being below the fold of a 200-line file.
