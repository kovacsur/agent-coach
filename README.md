# Agent Coach

Meta-skills for auditing and tightening agent-facing instruction prose — Cursor rules, Claude skills, agent prompts, `AGENTS.md`, tool descriptions, system prompts.

Audits run on demand against existing files. The toolkit reports dead-weight prose and proposes deletions or rewrites; it does not edit files unilaterally.

## Why It Exists

Agent-facing instruction files age quickly: safety rules, workflow notes, examples, and tool guidance accrete until the agent spends more context on prose than behaviour. Agent Coach focuses on the recurring cases that show up in real Cursor rules, Claude skills, and agent prompts — redundant reinforcement, soft hedging, unactionable background, oversized examples, and author-side notes that belong outside runtime instructions.

The rubric was shaped from 40+ de-duplicated sources across private research passes, including academic papers, vendor documentation, engineering write-ups, and practitioner notes in addition to extensive personal experience. Source details are available on request.

## What's included

| Skill | Purpose |
|---|---|
| `tighten-instructions` | Audit an instruction file for dead-weight prose at chosen aggressiveness (`polish` / `tighten` / `compress`). |

Each shipped skill has a **companion authoring rule** — a distilled summary loaded while you draft instruction prose. The skill is the audit verb; the companion is authoring-time discipline.

## Install

### Cursor

```sh
cp -r skills/tighten-instructions ~/.cursor/skills/
cp rules/tighten-instructions.mdc ~/.cursor/rules/
```

- **User scope (cross-project):** `~/.cursor/skills/<name>/` and `~/.cursor/rules/<name>.mdc`
- **Project scope:** `<repo>/.cursor/skills/<name>/` and `<repo>/.cursor/rules/<name>.mdc`

The companion ships as a `.mdc` rule file activated by description.

### Claude Code

**Plugin install (recommended):**

```sh
/plugin install agent-coach@claude-community
```

Send as a standalone prompt (not mid-conversation). Requires the community marketplace — add it once with `/plugin marketplace add anthropics/claude-plugins-community` if you haven't already.

**Manual install:**

```sh
cp -r skills/tighten-instructions skills/tighten-instructions-rule ~/.claude/skills/
```

Claude Code has no separate rule-file primitive. The companion ships as `skills/tighten-instructions-rule/` — a model-invoked skill activated by description. On the audit skill, `allowed-tools: Read` ([Agent Skills](https://agentskills.io/specification) standard, experimental) pre-approves Read in Claude Code; `argument-hint: [polish|tighten|compress]` adds slash-command level hints. The read-only audit contract is enforced in the skill body, not frontmatter.

### Other harnesses

Shipped skills follow the [Agent Skills](https://agentskills.io/specification) open standard: a `skills/<name>/` directory with `SKILL.md` (`name`, `description`, optional `license` and `allowed-tools`) and on-demand files under `references/`. Map that layout to your harness's skill location; use `description` as the activation trigger.

`argument-hint` on the audit skill is a Claude Code extension. The Cursor companion ships separately as `rules/tighten-instructions.mdc`. Adapt or drop harness-specific fields (`argument-hint`; `alwaysApply` on Cursor rules) as needed.

## Usage

Invoke `tighten-instructions` when reviewing or editing an agent-facing instruction file. Pick an aggressiveness level:

| Level | When to use |
|---|---|
| `polish` | Battle-tested file; only obvious dead weight should be flagged. |
| `tighten` *(default)* | Mature file with room to sharpen. |
| `compress` | Fresh draft or known-bloated file; targets ~30–50% length reduction. |

The skill loads one per-level rubric file on demand and produces a structured audit report. Apply edits only after reviewing the report.

## Repository layout

```
agent-coach/
├── README.md
├── AGENTS.md                          ← agent orientation for contributors
├── CONTRIBUTING.md
├── .claude-plugin/plugin.json         ← Claude Code plugin manifest
├── .cursor/rules/agent-coach.mdc      ← maintainer conventions (project-scoped)
├── skills/                            ← runtime artefacts; what consumers deploy
│   ├── tighten-instructions/
│   │   ├── SKILL.md
│   │   ├── MAINTENANCE.md             ← author-only; not loaded at audit time
│   │   └── references/
│   └── tighten-instructions-rule/     ← companion (Claude Code)
└── rules/
    └── tighten-instructions.mdc       ← companion (Cursor)
```

Only `skills/<name>/SKILL.md` and `skills/<name>/references/*` ship to consumers. `MAINTENANCE.md` travels with the skill folder for editors but is not loaded at runtime.

## What this is not

- A linter — judgement is contextual; mechanisation produces false positives.
- A rewriter — edits happen only on explicit user request after review.
- A style guide — it cuts dead weight; it does not impose voice.
- A general prose editor — human-facing docs (READMEs, commit messages) are out of scope.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT — see [LICENSE](LICENSE).
