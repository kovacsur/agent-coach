# Changelog

All notable changes to Agent Coach are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-09-17

### Added

- Protected-list item 6, *stated failure-exit on a critical path* — always-on, so an existing escape hatch is never flagged for deletion at any level. Previously such a line tripped the `soft hedging` red flag and was a deletion candidate under `compress`.
- Category 14, *closed-world success condition* (`tighten` and `compress`) — flags directives that define success as one end state while foreclosing failure, leaving off-spec improvisation as the only route to the goal. Verdict is `tightenable`: rewrite to keep the goal and rank the outcomes. Fires only where foreclosing language is present.
- `Closed-world success cues` red-flag bullet in `level-tighten.md` and `level-compress.md`.
- Pair 6 in `references/examples.md` — worked rewrite of a closed-world success condition, and why ranking outcomes beats a bare permission to abandon.
- Companion-rule bullet mirroring category 14, in both the Cursor `.mdc` and the Claude Code companion skill.
- `MAINTENANCE.md` provenance and calibration notes for the above, with sources.

### Changed

- `level-compress.md` categories renumbered: *position-poor placement* 14 → 15, *bridging prose between directives* 15 → 16.

## [1.0.0] - 2026-06-24

### Added

- `tighten-instructions` skill — audits an agent-facing instruction file for dead-weight prose at three aggressiveness levels (`polish` / `tighten` / `compress`), with per-level reference rubrics and worked rewrite examples.
- `tighten-instructions-rule` companion — distilled authoring-time discipline, loaded by description while drafting instruction prose.
- Cursor companion rule (`rules/tighten-instructions.mdc`) mirroring the Claude Code companion.
- Claude Code plugin manifest (`.claude-plugin/plugin.json`) for `/plugin install`.

[1.1.0]: https://github.com/kovacsur/agent-coach/releases/tag/v1.1.0
[1.0.0]: https://github.com/kovacsur/agent-coach/releases/tag/v1.0.0
