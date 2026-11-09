# Roadmap

Each step is a [GitHub milestone](https://github.com/enzo-thuilliez/pillbox/milestones); each
line is an issue carrying the scope and acceptance criteria.

## M0 — Foundations ([milestone](https://github.com/enzo-thuilliez/pillbox/milestone/1))

- [x] Repo docs and rules: AGENTS.md, DECISIONS.md, CLAUDE.md
- [x] Repo tooling: issue and PR templates, labels, PR-title lint, gitleaks, denylist check,
      git hooks with the conventional commit-msg check, Dependabot (#1)
- [x] Docs aligned with my other repos: README, ROADMAP links (#2)
- [ ] Monorepo scaffold: npm workspaces, uv, lint, format, typecheck, test runners (#3)
- [ ] CI: lint, typecheck and tests for TypeScript and Python behind `ci-pass` (#4)

## M1 — Parity with the prototype ([milestone](https://github.com/enzo-thuilliez/pillbox/milestone/2))

- [ ] Port the CP-SAT solver + golden test on fictitious data (#5)
- [ ] D1 schema and migrations; CRUD for employees, rules, absences (#6)
- [ ] Web: A/B schedule view, edit, Excel export (#7)
- [ ] Deploy: Pages (preview per PR), Worker, solver to HF Space; Cloudflare Access in front (#8)

## M2 — AI copilot ([milestone](https://github.com/enzo-thuilliez/pillbox/milestone/3))

- [ ] Event schemas (absence, peak, swap, …) with required fields (#9)
- [ ] Extraction with Workers AI tool calling; ask only for missing fields (#10)
- [ ] Multi-objective solving → 2–3 options shown as visual diffs; apply and undo (#11)
- [ ] Plain-French explanation when no schedule is feasible (#12)
- [ ] Evals: extraction dataset + CI workflow on prompt changes (#13)
- [ ] Observability: AI Gateway logs, latency and token dashboards (#14)

## M3 — Showcase ([milestone](https://github.com/enzo-thuilliez/pillbox/milestone/4))

- [ ] README with demo GIF, architecture diagram and eval scores (#15)
- [ ] Public demo with fictitious data (#16)
- [ ] Releases and v1.0 (#17)
