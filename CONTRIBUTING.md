# Contributing

Agent Coach ships meta-skills as portable Markdown with YAML frontmatter. Contributions should keep runtime files repo-agnostic and judgement-heavy rather than mechanised.

## Where content lives

| Location | Loaded at runtime? | Purpose |
|---|---|---|
| `skills/<skill>/SKILL.md` | Yes | Skill body — workflow, protected list, output format |
| `skills/<skill>/references/*` | On demand | Per-level rubrics, examples |
| `skills/<skill>/MAINTENANCE.md` | No | Sync constraints and editor guidance for maintainers |
| `rules/<skill-name>.mdc` | On description match (Cursor) | Companion authoring discipline |
| `skills/<skill-name>-rule/SKILL.md` | On description match (Claude Code) | Same companion content; adapted frontmatter |

The skill folder and its companion are the deployment artefacts.

## Skill format

Runtime skills follow the [Agent Skills](https://agentskills.io/specification) open standard. Required frontmatter: `name` (must match the directory name), `description`. Optional: `license`, `allowed-tools`. Put on-demand rubrics in `references/`.

Harness extensions used here: `argument-hint` on the audit skill (Claude Code). Do not use `tools:` in `SKILL.md` — that field is for Claude Code subagents (`agents/*.md`), not skills. See `skills/tighten-instructions/MAINTENANCE.md` for frontmatter and sync constraints.

## Writing discipline

Any agent-facing prose you author or edit in this repo must pass the `tighten-instructions` rubric:

- **Self-apply** before opening a PR. If your draft contains a paragraph the toolkit would flag, rewrite it.
- **Conservative default** — protect dated incident notes, binary safety invariants, brief role framing, tone calibration, and (per-level) tool descriptions.
- **Repo-agnostic** — no codebase-specific names, paths, or conventions in shipped skills.
- **House style** — imperative, dense, concrete. No preambles. If a sentence doesn't change how the next agent acts, cut it.

Never put author-side provenance in runtime files: no *"inherited from X"*, *"added at level Y"*, *"see decision Dn"* breadcrumbs. Put sync notes in `MAINTENANCE.md`; put rationale in PR descriptions or git history.

## Editing an existing skill

1. Read `skills/<skill>/MAINTENANCE.md` for sync constraints.
2. Edit runtime files (`SKILL.md`, `references/level-*.md`).
3. If categories or red flags changed, update the companion rule (`rules/*.mdc` and `skills/*-rule/SKILL.md`). Source of truth for the companion is the Cursor `.mdc` file; the Claude Code skill mirrors it with adapted frontmatter only.
4. Run `tighten-instructions` on any non-trivial edit to agent-facing prose, including the companion rule itself.

## Adding a new skill

New skills are added only when a failure pattern recurs across enough independent instruction files to justify dedicated tooling. Until then, add the pattern as a rubric category in the most relevant existing skill.

Do not add CLI or lint companions. False positives train agents to ignore the rule.

## Pull requests

- One concern per PR when practical.
- Note which runtime files changed and whether companion files were synced.
- Confirm you ran `tighten-instructions` on edited agent-facing prose.

## Local config (do not commit)

| Path | Why |
|---|---|
| `.claude/` | Claude Code session settings (`settings.local.json`, etc.) |
| `.cursor/mcp.json` | Cursor MCP config — often contains tokens |
| `CLAUDE.local.md`, `*.local.json`, `*.local.md` | Personal overrides |

**Do commit:** `.claude-plugin/plugin.json` (plugin manifest for `/plugin install`) and `.cursor/rules/` (project maintainer rules).

## Naming

- **Agent Coach** — project name in prose.
- **`agent-coach`** — directory and repository paths.
