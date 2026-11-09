# AGENTS.md

Instructions for coding agents (Claude Code, Codex, Cursor, ...) working on pillbox.

## Project

pillbox schedules the staff of a small pharmacy (about ten people) on a two-week A/B cycle.
A CP-SAT solver builds schedules that never break hard rules. An LLM copilot turns a request
("Sam is sick until Thursday") into a typed event, asks for missing fields, and explains the
2–3 options the solver returns. The user applies one with a click.

**The LLM never produces a schedule** (ADR-003). If you find yourself asking the model to place
shifts, stop: add a solver objective or constraint instead.

## Layout

```
apps/web          React + TS + Vite (Cloudflare Pages). UI in French.
apps/api          Cloudflare Worker: REST API, D1, Access JWT check, LLM orchestration
packages/schema   Zod schemas: employees, rules, events, schedules (source of truth, ADR-004)
services/solver   Python, FastAPI, OR-Tools CP-SAT (Hugging Face Space). Stateless (ADR-002)
evals/            LLM extraction dataset + runner; solver golden cases
docs/             architecture, privacy, runbooks
```

(Layout is created during M0; some folders may not exist yet.)

## Hard rules

1. **No real data, ever** (ADR-008). Fixtures use fictitious names. Never paste real names,
   the pharmacy's name, real schedules or exports into code, tests, issues, PRs or commit
   messages. If you see real data, stop and tell the user.
2. **Pseudonymize** (ADR-007). Nothing that leaves `apps/api` (LLM, solver, logs) contains a
   name or a free-text note. Use employee IDs.
3. **Hard constraints live in the solver** and are never optional from the API or the chat.
4. **Free tiers only** (ADR-009). Do not add a dependency or service that needs a paid plan.

## Domain glossary

| French (UI)          | Code                  | Meaning                                         |
| -------------------- | --------------------- | ----------------------------------------------- |
| Pharmacien           | `pharmacist`          | Must be present whenever the pharmacy is open   |
| Préparateur          | `technician`          | Pharmacy technician                             |
| Semaine A / B        | `week: "A" \| "B"`    | Two-week cycle; contract hours differ per week  |
| Vacation             | `shift`               | One continuous working block (min 2 h)          |
| Affluence            | `peak`                | Busy time slot needing more staff               |
| Planning de référence| `reference schedule`  | The schedule in force, used for stability       |

## Conventions

- **Never commit or push to `main`.** One issue → one branch named
  `<type>/<issue>-<short-slug>` (e.g. `feat/5-port-solver`; `<type>` one of `feat`, `fix`,
  `chore`, `docs`, `refactor`, `test`, `perf`, `ci`) → one PR → CI green (`ci-pass`, PR title,
  gitleaks) → squash merge by the maintainer. Once CI is green, stop: never merge.
- **Issues** carry one `type:*`, one or more `area:*`, one `priority:p0|p1|p2` label and the
  milestone of their roadmap step. A change with no issue gets one first.
- **Docs-only changes** go in their own PR, never mixed into a code PR.
- **Conventional commits**: `feat(solver): add balanced-hours objective`. Scopes: `web`, `api`,
  `schema`, `solver`, `evals`, `docs`, `ci`, `deps`, `repo`. Checked locally by the
  commit-msg hook (`pre-commit install`) and on the PR title in CI.
- TypeScript strict, ESM only. Python 3.12, typed, `ruff` + `mypy` clean.
- New architectural choice → new ADR in DECISIONS.md, in the same PR.

## Working from an issue

- **Start**: read the issue, its dependencies and the ADRs it names. If a dependency is open,
  stop and say so.
- **Branch**: one issue, one branch, one PR.
- **PR body**: first line `Closes #N` (or `Part of #N`), then What, Why, How to check.
- **Before opening a PR**: run lint, typecheck and tests for every package you touched.
- **GitHub**: use `gh` (or `gh api` for REST) for issues and PRs. Releases are off until #17.

## Code intelligence

There is no code to index yet. Once the monorepo scaffold (#3) lands, index the repo with
**GitNexus** (code-heavy; the docs are small and live next to the code):

- Refresh: `gitnexus analyze` at the repo root (the hooks also reindex after commits).
- Before changing a schema in `packages/schema`, an API contract or a solver input/output
  model, run GitNexus impact analysis and list the affected callers in the PR.
