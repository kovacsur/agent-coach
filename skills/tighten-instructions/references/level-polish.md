# Level: `polish`

You are auditing under `polish`. This file is the complete category list for this level. `SKILL.md` carries the always-on protected list, the 3-question test, the workflow, and the output format.

## Disposition

Default-keep. Only flag the obvious. Bright-line dead weight only — anything requiring a judgement call belongs in `tighten` or `compress`, not here.

## Categories flagged at this level

1. **Restated model capabilities.** Explaining what JSON, Markdown, a stack trace, or a CLI flag *is*. The agent already knows.
2. **Verbatim duplicate reinforcement.** The same rule restated word-for-word in another section.
3. **Prose written for human reviewers.** Rationale paragraphs, *"as we discussed in the meeting…"*, *"this is somewhat controversial but…"*. Belongs in design docs, not instructions.

That is the entire list at this level. Everything else — hedging, preambles, sprawling examples, story-framed prohibitions — is out of scope here. Suggest the user run `tighten` if they want a sharper pass.

## Tool descriptions at this level

In scope, but only for category 2 (verbatim duplication). Verbosity in tool descriptions is the documented best practice (when / when-not / params / caveats); do not flag a four-sentence description for being four sentences.

## Sentence-level red flags

Fast first-pass triggers. Presence ≠ verdict; use as a prompt to read the surrounding paragraph against the categories above.

- **Capability restatement:** *"a JSON file is"*, *"Markdown supports"*, *"git allows you to"*, *"the agent is capable of"*.
- **Human-reviewer prose:** *"as you know"*, *"as we discussed"*, *"historically"*, *"the team agreed"*, *"this is somewhat controversial"*.
