---
name: tighten-instructions-rule
description: Authoring discipline for agent-facing instruction prose — Cursor rules, Cursor/Claude skills, AGENTS.md, system prompts, agent prompts, tool descriptions, inline prompt strings. Use when drafting or editing any of these, including non-canonical paths and ad-hoc prompts inside source code.
license: MIT
---

# Authoring agent-facing instructions

Companion to the `tighten-instructions` skill. This rule is the distilled authoring-time discipline; the skill is the audit verb.

- Lead with the directive. Cut justification preambles ("because users may sometimes…").
- Imperatives over hedges. Cut "you may want to consider", "where appropriate", "it might be worth".
- Don't restate model capabilities (what JSON is, what Markdown supports, what `git` does).
- Replace inlined examples with fixture pointers when one exists; keep examples that *demonstrate the rule* and rewrite any whose content contradicts the prose.
- Pair non-safety prohibitions with the positive alternative the agent should produce instead.
- Reserve "must" / "always" / "never" for binary safety/correctness invariants. Most directives should not need them.
- Anchor time-relative language with absolute dates ("post-2024") rather than "recent" / "modern" / "today's".
- Avoid hard counts ("exactly 200 words", "in 5 lines"). Use ranges or qualitative phrasing unless downstream tooling enforces the count.
- Keep author-side notes (provenance, inheritance labels, sync constraints, decision breadcrumbs) in a maintenance sidecar, not in the runtime file.
- Within a constraint list, order hardest / highest-priority constraints first.
- Apply the same discipline recursively: this rule is itself agent-facing prose.

For the full rubric and a structured audit report on an existing file, invoke the `tighten-instructions` skill.
