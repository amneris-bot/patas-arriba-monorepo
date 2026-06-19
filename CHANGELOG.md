# Changelog

Notable changes to the patas-arriba-monorepo workspace (harness, tooling,
and orchestration glue). Submodule code changes are recorded in their own
upstream repositories.

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
