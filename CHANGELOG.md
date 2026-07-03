# Changelog

Notable changes to the patas-arriba-monorepo workspace (harness, tooling,
and orchestration glue). Submodule code changes are recorded in their own
upstream repositories.

## 2026-07-03

### Harness template upgrade (v0.47.0 → v0.64.0)

- **Adopted the Affordances feature** — added the `## Affordances` section
  (with the template's example entries) plus two commented affordance
  constraints and three commented affordance GC rules to `HARNESS.md`, so the
  tool-identity governance surface is available to populate. Gitignored the
  per-machine `observability/affordance-invocations.json` recorder log.
- **Adopted the Cognitive reservoir block** — added the commented, opt-in
  advisory watch on verifier fatigue to `HARNESS.md`.
- **Migrated reflections to the per-fragment model** — split the monolithic
  `REFLECTION_LOG.md` into 12 per-entry fragments under `reflections/active/`,
  scaffolded `reflections/archive/`, and made `REFLECTION_LOG.md` a generated
  aggregate. Updated the reflection-archival GC rule and the `CLAUDE.md`
  Learnings section to describe the fragment model. Content preserved; same-date
  entries now sort deterministically by fragment filename.
- **Fixed the vendored-tool-path problem for reflections** — added repo-local
  wrapper scripts (`scripts/archive-promoted-reflections.sh`,
  `scripts/regenerate-reflection-log.sh`) that resolve the ai-literacy-superpowers
  plugin cache and its newest installed version at runtime and dispatch to the
  upstream script, so the GC-rule Tool: path stays stable across plugin upgrades.
  Pattern borrowed from `avatia/monorepo`. Repointed the reflection-archival GC
  rule at the wrapper.
- **Bumped the template-version marker** to `0.64.0` so the Template-currency
  GC rule reflects the current plugin.

### CodeGraph code intelligence across the monorepo

- **Wired CodeGraph into the workspace** — registered the `codegraph` MCP
  server (`.mcp.json`), allowlisted its read-only tools (`.claude/settings.json`),
  added the CodeGraph usage guidance (`.claude/CLAUDE.md`), and gitignored the
  local index files so the `.codegraph/` database never gets committed
  (`.codegraph/.gitignore`). A single query now spans both the `client/` and
  `server/` submodules.
- **Excluded `.claude-user/` from the index** via a root `codegraph.json`
  `exclude` pattern. The plugin marketplace under
  `.claude-user/plugins/marketplaces/` is an embedded git repo, which CodeGraph
  ≤1.0.1 indexed despite the `.gitignore` rule (upstream #514). Upgrading to
  1.2.0 makes gitignored embedded repos respect `.gitignore` by default, and the
  explicit `exclude` guarantees they stay out even for the one tracked file.
  Reindex dropped from indexing plugin/tooling code to 126 files of real app
  code (0 `.claude-user` nodes remain).

## 2026-06-14

### Harness audit follow-ups

- **Adopted the `docs/superpowers/` tree** as the home for all spec-first
  artefacts. Migrated the two existing specs from `/specs/` to
  `docs/superpowers/specs/`, created `objections/` and `stories/` directories
  (each with a README explaining its agent and the constraint it backs), and
  repointed the spec-location mandate in `CLAUDE.md` and `.claude/agents/spec-writer.md`.
  This resolves the divergence where the two agent constraints referenced a
  `docs/superpowers/` tree that did not exist. Pre-existing specs are marked
  `diaboli`/`cartographer: exempt-pre-existing` so they do not trip the PR gates.
- **Promoted two constraints from `unverified` to `agent` enforcement** —
  "Consistent formatting (client)" (ESLint) and "Tests must pass"
  (Playwright/Vitest by scope), both gated by harness-enforcer at PR-review
  time since the monorepo root has no CI by design. Enforcement ratio rose
  from 2/7 to 6/7.
- **Narrowed the "Convention file sync" GC rule** to the project's actual
  convention surfaces (the CLAUDE.md hierarchy and AGENTS.md), removing the
  phantom Cursor/Copilot/Windsurf references that were reported as missing.
- **Created the first observability snapshot** at
  `observability/snapshots/2026-06-14-snapshot.md` via `/harness-health`;
  refreshed the Status section and the README enforcement + health badges.
- **Upgraded the HARNESS.md template marker** from 0.39.0 to 0.47.0 via
  `/harness-upgrade` (no new template content to adopt — the harness was
  already a superset).
