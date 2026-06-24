# Agent orientation — Agent Coach

Agent Coach is a small library of meta-skills that audit and tighten agent-facing instruction files. One skill is shipped: `tighten-instructions`.

Project conventions live in `.cursor/rules/agent-coach.mdc` (Cursor loads automatically) or [CONTRIBUTING.md](CONTRIBUTING.md).

## Reading paths

- **Auditing a target instruction file:** read `skills/tighten-instructions/SKILL.md` and the matching `references/level-<level>.md`. Nothing else required.
- **Editing runtime skill files:** read `skills/<skill>/MAINTENANCE.md` first, then the runtime files. Run `tighten-instructions` on your edits.
- **Editing the companion rule:** update `rules/tighten-instructions.mdc` and mirror to `skills/tighten-instructions-rule/SKILL.md`. See sync constraints in `skills/tighten-instructions/MAINTENANCE.md`.
