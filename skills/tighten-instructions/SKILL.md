---
name: tighten-instructions
description: Audit and tighten agent-facing instructions (rules, skill files, agent prompts, AGENTS.md, tool descriptions) by identifying dead-weight prose while protecting load-bearing reinforcement. Use when reviewing or editing instruction files for context-budget efficiency.
license: MIT
argument-hint: [polish|tighten|compress]
allowed-tools: Read
---

# tighten-instructions

## When to use

Apply when reviewing or editing an **agent-facing** instruction file: a Cursor rule, Cursor/Claude skill, agent prompt, `AGENTS.md`, tool description, or system prompt. Out of scope: human-facing prose (READMEs, design docs, commit messages, code comments). One file at a time.

## Aggressiveness levels

The user picks a level; **default is `tighten`**. The level determines which dead-weight categories are in scope. Each level has its own self-contained reference file — load only the one for the chosen level.

| Level | When to use it | Reference |
|---|---|---|
| `polish` | Battle-tested file; user wants only obvious dead weight removed. | `references/level-polish.md` |
| `tighten` *(default)* | Mature file with room to sharpen; user wants a careful pass. | `references/level-tighten.md` |
| `compress` | Fresh draft or known-bloated file; user wants ~30–50 % shorter. | `references/level-compress.md` |

Levels are cumulative by design: `tighten` flags everything `polish` flags plus more; `compress` flags everything `tighten` flags plus more. The level files restate the inherited categories so you only ever read one.

## Protected list (always-on)

Never flag for deletion regardless of level:

1. **Dated incident notes** — *"2025-09-14 cleanup ran `rm -rf` on the source tree, full-day recovery"*. One line, load-bearing, encodes institutional memory.
2. **Binary safety/correctness invariants** — *"never push without explicit user request"*, *"never commit secrets"*. Negative framing is acceptable here; prominence is worth the cost.
3. **Brief role/identity framing** — one or two opening sentences for an agent with no prior context to anchor on.
4. **Tone/confidence calibration** — *"be conservative; verify before asserting"*. Looks woolly but shifts the generation distribution; not replaceable by a tool call.
5. **Explicitly separated constraints section** — a standalone "Constraints:" / "Directives:" / equivalent block whose separation from surrounding task prose is itself load-bearing. Separating constraints from task description text improves compliance measurably; do not dissolve it into surrounding prose to save a header line.

Tool descriptions are handled per level; see the level file for the rule.

## The 3-question load-bearing test

A line is load-bearing only if you can name **all three**:

1. **Failure mode it prevents.** What goes wrong without it?
2. **Why a tool call cannot replace it.** Could a CLI / MCP tool / recipe surface the same fact on demand?
3. **Why the agent wouldn't infer it from training.** Non-obvious to a frontier model with zero project context?

Can't answer one or more → deletion or rewrite candidate.

## Workflow

1. **Read the file in full** before classifying anything. Length and section position matter for the verdict.
2. **Confirm the level.** If the user didn't specify, assume `tighten` and say so in the report header.
3. **Load the level reference.** Read `references/level-<level>.md` once. That file is the complete category list and red-flag set for this audit.
4. **Skip the protected list** above. Apply the level file's tool-description rule.
5. **Classify each paragraph or directive** as `dead weight`, `tightenable` (load-bearing payload, expressible at lower token cost), or `load-bearing` (kept). Use the categories in the level file; reach for `references/examples.md` when shaping a rewrite.
6. **Selection-surface check** *(applies only to files with YAML frontmatter `description` — Cursor/Claude skills)*. A skill's body loads only after the parent agent has selected it; the `description` is the only signal at selection time. Before flagging a concrete user-question shape, trigger phrase, or capability bullet from a body `When to use` / `Use when` section for deletion, check whether the same shape appears in the `description`. If not, propose **promotion** into the description, not deletion. Body-only deletion is correct only when the description already surfaces the shape, or when the shape governs which verb to use *within* the skill rather than whether to load it. Any description amendment (or new description proposed in the report) must preserve both halves of the WHAT/WHEN split: capabilities (WHAT) plus a `Use when …` clause (WHEN). Out of scope: rules, `AGENTS.md`, tool descriptions, system prompts.
7. **Produce the report** in the format below. Quote the original; cite line ranges; one-line reason per finding. Promotions surfaced in step 6 ride alongside the deletion they unblock — see Pair 5 in `references/examples.md`.
8. **Stop.** Do not edit the audited file. Direct edits happen only when the user follows up with phrasing like *"apply the tightenings"* / *"do it"* / *"edit the file"*. Then produce a diff.

## Output format

```markdown
## Audit of <path/to/file>

**Level:** tighten
**Original length:** N lines / ~M tokens
**Proposed length:** N' lines / ~M' tokens (-X%)

### Findings

#### Candidate 1 — lines L₁–L₂ — `dead weight`

> <quoted excerpt>

**Why:** <one-line reason from the level file's category list>
**Proposal:** delete.

#### Candidate 2 — lines L₃–L₄ — `tightenable`

> <quoted excerpt>

**Why:** <one-line reason>
**Proposed rewrite:**

> <tighter version>

#### Candidate 3 — lines L₅–L₆ — `load-bearing`

> <quoted excerpt>

**Keep because:** <which of the three load-bearing criteria, in one line>

### Summary

- Dead weight: K candidates, ~T tokens.
- Tightenable: K candidates, ~T tokens saved.
- Load-bearing (kept): K candidates.
```

## Self-application

This skill's own files were audited under this rubric before shipping. The split into per-level reference files is itself an instance of the pattern: load only what's relevant to the active mode. Any apparent verbosity is intentional and load-bearing — flag it only if you can articulate which of the three load-bearing criteria the author got wrong.

## References

- `references/level-polish.md` / `level-tighten.md` / `level-compress.md` — load the one matching the chosen level.
- `references/examples.md` — before/after rewrite pairs across the main dead-weight categories.
