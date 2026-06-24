# Maintenance notes — `tighten-instructions`

Author-only file. Cursor and other agents do not load this at runtime; nothing here enters the audit context. Read before editing the skill's runtime files.

## Sync constraints between level files

The per-level reference files duplicate categories by design. Inheritance is not labelled in the files themselves — such labels are author-side metadata that doesn't change agent behaviour and would be flagged by the skill's own rubric (category 13, *author-side provenance metadata*).

Sync rules:

- `references/level-tighten.md` categories 1–3 must mirror `references/level-polish.md` categories 1–3.
- `references/level-compress.md` categories 1–13 must mirror `references/level-tighten.md` categories 1–13.
- The `Provenance markers` red-flag bullet appears in both `level-tighten.md` and `level-compress.md`; keep them aligned.
- Tool-description rules: polish flags only verbatim duplication; tighten and compress exclude tool descriptions entirely.

If you change a category's wording in one file, update every file it appears in. There is no automated sync check.

## Sync constraint with the companion authoring rule

The companion authoring discipline ships in two files for different harnesses:

- `../../rules/tighten-instructions.mdc` — Cursor rule file (description-activated)
- `../../skills/tighten-instructions-rule/SKILL.md` — Claude Code model-invoked skill (description-activated)

Source of truth is the Cursor `.mdc` file; the Claude Code skill mirrors it with adapted frontmatter only.

Keep these in sync:

- A category added, removed, renumbered, or meaningfully reworded in `references/level-*.md` → update the matching bullet in the companion rule.
- A red flag added or removed → ditto.
- A protected-list change in `SKILL.md` → review the companion rule for any directive that contradicts the protected pattern. **Current protected-list items and the companion bullet each maps to:** (1) dated incident notes → no direct bullet; (2) binary safety invariants → "Reserve 'must'/'always'/'never' for binary safety/correctness invariants"; (3) brief role framing → no direct bullet; (4) tone calibration → no direct bullet; (5) explicitly separated constraints section → no direct bullet.

The companion rule is itself agent-facing prose. After a non-trivial edit to it, run `tighten-instructions` on it and apply the verdict. Do not let the companion rule grow into a second rubric — it is a summary, not a fork.

## Where each kind of content belongs

- **Always-on scaffolding** (workflow, output format, 3-question test, always-on protected list): `SKILL.md`.
- **Level-specific categories, dispositions, red flags, tool-description rules:** the matching `references/level-<name>.md`.
- **Worked rewrite examples:** `references/examples.md`.
- **Sync constraints, "if you change X also change Y" notes:** here.

## External convention referenced by the workflow

The selection-surface check (`SKILL.md` workflow step 6) relies on the WHAT/WHEN description-shape convention documented in Cursor's `create-skill` skill: a skill `description` should carry both capabilities (WHAT) and a `Use when …` trigger clause (WHEN). If `create-skill`'s convention changes, revisit step 6's wording and Pair 5 in `references/examples.md`.

## SKILL.md frontmatter (Agent Skills + harness extensions)

Runtime skills follow the [Agent Skills](https://agentskills.io/specification) open standard: `name` and `description` (required); optional `license`, `allowed-tools`; on-demand `references/` layout.

Harness extensions on `skills/tighten-instructions/SKILL.md` only:

- `allowed-tools: Read` — open-standard field (experimental). Pre-approves Read without a permission prompt in Claude Code; does not block other tools. The read-only audit contract is enforced in the skill body, not frontmatter.
- `argument-hint: [polish|tighten|compress]` — Claude Code extension for slash-command autocomplete. Silently ignored elsewhere.

Do not use `tools:` in `SKILL.md` — that field belongs to Claude Code **subagent** definitions (`agents/*.md`), not skills.

## Editing this file

If a fourth aggressiveness level is added, factor `MAINTENANCE.md` along with the new level file rather than letting sync constraints accumulate ad hoc.
