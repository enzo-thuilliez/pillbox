# Roadmap

Each milestone is a GitHub milestone; each line becomes one or more issues.

## M0 — Foundations

- Repo docs and rules (AGENTS.md, DECISIONS.md, CLAUDE.md)
- Monorepo scaffold: npm workspaces, uv, lint, format, typecheck, test runners
- Commit conventions (commitlint), git hooks, Dependabot
- CI: lint, typecheck, tests (TS + Python), gitleaks, denylist check
- Issue and PR templates, labels, milestones, branch protection on `main`

## M1 — Parity with the prototype

- Port the CP-SAT solver + golden test on fictitious data
- D1 schema and migrations; CRUD for employees, rules, absences
- Web: A/B schedule view, edit, Excel export
- Deploy: Pages (preview per PR), Worker, solver to HF Space
- Cloudflare Access in front of everything

## M2 — AI copilot

- Event schemas (absence, peak, swap, …) with required fields
- Extraction with Workers AI tool calling; ask only for missing fields
- Multi-objective solving → 2–3 options shown as visual diffs; apply and undo
- Plain-French explanation when no schedule is feasible
- Evals: extraction dataset + CI workflow on prompt changes
- Observability: AI Gateway logs, latency and token dashboards

## M3 — Showcase

- README with demo GIF, architecture diagram and eval scores
- Public demo with fictitious data
- v1.0 release
