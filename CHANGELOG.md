# Changelog

All notable changes to Agent Coach are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project
adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-06-24

### Added

- `tighten-instructions` skill — audits an agent-facing instruction file for dead-weight prose at three aggressiveness levels (`polish` / `tighten` / `compress`), with per-level reference rubrics and worked rewrite examples.
- `tighten-instructions-rule` companion — distilled authoring-time discipline, loaded by description while drafting instruction prose.
- Cursor companion rule (`rules/tighten-instructions.mdc`) mirroring the Claude Code companion.
- Claude Code plugin manifest (`.claude-plugin/plugin.json`) for `/plugin install`.

[1.0.0]: https://github.com/kovacsur/agent-coach/releases/tag/v1.0.0
