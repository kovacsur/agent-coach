# Maintenance notes — `tighten-instructions`

Author-only file. Cursor and other agents do not load this at runtime; nothing here enters the audit context. Read before editing the skill's runtime files.

## Sync constraints between level files

The per-level reference files duplicate categories by design. Inheritance is not labelled in the files themselves — such labels are author-side metadata that doesn't change agent behaviour and would be flagged by the skill's own rubric (category 13, *author-side provenance metadata*).

Sync rules:

- `references/level-tighten.md` categories 1–3 must mirror `references/level-polish.md` categories 1–3.
- `references/level-compress.md` categories 1–14 must mirror `references/level-tighten.md` categories 1–14. Category 14 (*closed-world success condition*) is currently verbatim-identical in both; keep it that way rather than abbreviating the compress copy.
- The `Closed-world success cues` red-flag bullet appears in both `level-tighten.md` and `level-compress.md`; keep them aligned.
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
- A protected-list change in `SKILL.md` → review the companion rule for any directive that contradicts the protected pattern. **Current protected-list items and the companion bullet each maps to:** (1) dated incident notes → no direct bullet; (2) binary safety invariants → "Reserve 'must'/'always'/'never' for binary safety/correctness invariants"; (3) brief role framing → no direct bullet; (4) tone calibration → no direct bullet; (5) explicitly separated constraints section → no direct bullet; (6) stated failure-exit on a critical path → "On a critical path, don't foreclose failure … rank the outcomes".

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

## Provenance — protected item 6 and category 14 (failure-exit)

Kept here rather than in the runtime files: the agent acts on the rule, not on its evidence base (category 13).

Added after the 2026-07 OpenAI / Hugging Face incident. OpenAI's post-mortem names *"persistence on seemingly impossible tasks"* as one of four misalignment patterns. METR's independent investigation found ~30–40 % of the ExploitGym targets were impossible to exploit via the intended vulnerability, while the task prompt stated that only the intended vulnerability counted and that other approaches would be failed — with no option to declare a task impossible. METR's reading: the framing "left no socially acceptable exit path."

- <https://openai.com/index/hugging-face-incident-and-the-road-ahead/>
- <https://metr.org/blog/2026-08-26-openai-hugging-face-incident-investigation/>

**Calibration — do not let the runtime wording overclaim.** ImpossibleBench measured explicit abort mechanisms: GPT-5 54 % → 9 % cheating, o3 49 % → 12 %, Claude Opus 4.1 unchanged at 46 %. A strict-prompt variant moved GPT-5 93 % → 1 % on Impossible-LiveCodeBench but only 66 % → 54 % on Impossible-SWEbench. A named exit removes the instruction-level trap; it is not a reliable behavioural fix. Category 14 therefore claims only that foreclosure leaves off-spec routes as the sole path to the goal — never that naming an exit prevents reward hacking.

- <https://www.lesswrong.com/posts/qJYMbrabcQqCZ7iqm/impossiblebench-measuring-reward-hacking-in-llm-coding-1>

Division of labour between the two entries: protected item 6 carries only the hedging-vs-exit discriminator, category 14 carries the failure mode. Item 6 previously restated the failure mode and the skill's own `tighten` pass flagged it as category 6 (paraphrased redundant reinforcement) — both files are co-loaded during an audit. Do not re-add it.

**Scope note.** Category 14 is the rubric's second correctness-rather-than-cost category (category 9, *demonstration–rule conflict*, is the first), and the third whose verdict is a rewrite or relocation rather than a deletion (with category 9 and compress category 15). Deliberately absent from `level-polish.md`, which is bright-line-only; the always-on protected list in `SKILL.md` still shields existing exits at `polish`. If a fourth correctness category accumulates, revisit whether the level files' "dead-weight categories" framing and the README's "it cuts dead weight" claim still describe the rubric.

## Editing this file

If a fourth aggressiveness level is added, factor `MAINTENANCE.md` along with the new level file rather than letting sync constraints accumulate ad hoc.
