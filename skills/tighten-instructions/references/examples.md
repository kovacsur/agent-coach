# Examples: before/after tightenings

Each pair: a generic instruction paragraph, the tightened rewrite, and the rubric category that fired.

## Pair 1 — verbose justification before the directive

**Before** (~120 tokens):

> Because users may sometimes work in environments where the linter is not configured, and because we have found that some teammates have on occasion run the build before checking the lint output, we strongly encourage you to make sure that you always run the linter as a separate step before kicking off any build pipeline. This way, lint errors are caught early and the build does not have to be re-run, saving CI minutes and reviewer attention.

**After** (~25 tokens):

> Run the linter before the build. Failed lint blocks the build pipeline.

**Cut:** category 4 (verbose justification). The *"because users may sometimes…"* preamble and the cost-justification tail add zero behavioural directive. The imperative survives intact in the rewrite.

## Pair 2 — sprawling example

**Before** (~150 tokens):

> When generating a database migration, follow the project's naming convention. For example, if you're adding a column called `email_verified_at` to the `users` table, the migration file should be named with a UTC timestamp prefix, then `add_email_verified_at_to_users.rb`, so the full filename ends up looking something like `20260115093000_add_email_verified_at_to_users.rb`. Similarly, if you are dropping a column called `legacy_token` from the `sessions` table, the file would be named `20260115094500_drop_legacy_token_from_sessions.rb`. The key thing is that the timestamp comes first and the verb-noun-table pattern follows.

**After** (~40 tokens):

> Migration filenames: `<UTC-timestamp>_<verb>_<column>_<from|to>_<table>.<ext>`. See `db/migrate/` for examples.

**Cut:** category 8 (sprawling examples). Two parallel walked-through illustrations replaced with the underlying pattern and a fixture pointer. The agent can read the directory if it needs more.

## Pair 3 — soft hedging

**Before** (~70 tokens):

> You may want to consider, where appropriate, adding tests for any new public function you write. It might also be worth thinking about whether the existing test suite covers your changes adequately, and adding cases if it does not.

**After** (~20 tokens):

> Add tests for every new public function. Extend existing suites that touch your changes.

**Cut:** category 7 (soft hedging). *"You may want to consider"*, *"where appropriate"*, *"it might be worth thinking about"* do not shift behaviour — the agent files them under "optional". Crisp imperatives raise compliance.

## Pair 4 — "don't do X" as a story (with a load-bearing payload)

**Before** (~110 tokens):

> Be careful with destructive shell commands. There was an incident some time back where an agent in another project ran `rm -rf` on what it thought was a build directory but turned out to be the source tree, and recovery took the better part of a day. Because of this, we now ask that you avoid running destructive commands without first verifying the path you're about to operate on, and ideally checking with the user if there is any doubt.

**After** (~35 tokens, two lines):

> Never run destructive shell commands (`rm -rf`, `git clean -fdx`, `DROP TABLE`) without explicit user confirmation.
>
> *Incident 2025-08-12*: agent ran `rm -rf` on source tree; full-day recovery.

**Cut + kept:** category 10 (story-framed prohibition) collapsed to a binary directive (protected: high-stakes negative invariant); the recurrence is preserved as a dated incident note (protected: load-bearing pattern 3). Net token cost roughly halved without losing the lesson.

## Pair 5 — body deletion that erodes selection surface (skills only)

Applies only to files with YAML frontmatter `description` (Cursor/Claude skills). The body of a skill loads after selection; the `description` is what the parent agent sees *at* selection time.

**Before** — a hypothetical `query-graph` skill with this frontmatter and body:

> ```yaml
> name: query-graph
> description: Run SPARQL queries against the project's knowledge graph. Use when the user asks to query the graph.
> ```
>
> ## When to use
>
> - "How many features mention X?" → `count`
> - "Top architects on Y?" → `top`
> - "Who's worked on both X and Y?" → `intersect`
> - "Most recent feature touching Z?" → `rank`
> - "What predicates does subject S have?" → `predicates-on`
>
> ## Commands
>
> | Command | Purpose |
> |---|---|
> | `count` | Count matches for a predicate. |
> | `top` | Top-N ranking by a predicate. |
> | `intersect` | Subjects appearing in both of two sets. |
> | `rank` | Order subjects by a numeric or temporal predicate. |
> | `predicates-on` | List predicates attached to a given subject. |

**Naive flag:** the body's `When to use` bullets paraphrase the Commands table directly below — category 6 (paraphrased redundant reinforcement). Delete the bullets.

**Why naive deletion is wrong:** the question shapes (*"how many … mention …"*, *"top … on …"*, *"who's worked on both …"*, *"most recent …"*) appear *only* in the body. The `description` mentions only "query the graph" — at selection time, the parent agent never sees the aggregate-analytical shapes. Deletion shrinks recall on exactly the questions this skill best answers; the agent is most likely to fail to load the skill in precisely the cases where it would have been most useful.

**Right move (promotion + deletion together):** lift the question shapes into the `description`, *then* delete the body bullets.

> ```yaml
> name: query-graph
> description: Run SPARQL queries against the project's knowledge graph — counts, top-N rankings, set intersections, recency ranking, predicates-on-subject. Use when the user asks aggregate or analytical questions over graph data ("how many features mention X", "top architects on Y", "who's worked on both X and Y", "most recent feature touching Z", "what predicates does subject S have").
> ```

The capability list keeps the WHAT half; the `Use when …` clause with concrete question shapes keeps the WHEN half. Both halves protect skill discovery (Cursor's `create-skill` documents the convention).

**Cut + promoted:** category 6 (body redundancy) on the bullets, *only after* the selection-surface check (workflow step 6 in `SKILL.md`) has lifted the trigger phrases into the description. The two findings ride together in the report — the promotion is what unblocks the deletion.

**Body-only deletion is correct without promotion** when:

- the description already surfaces the shape (the `When to use` bullets really are redundant in both directions); or
- the shape governs which *internal verb* to use (e.g., *"prefer `count` over `rank` for boolean questions"*) rather than whether to load the skill at all.
