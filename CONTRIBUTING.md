# Contributing to pillbox

The flow is the same for humans and coding agents; [AGENTS.md](AGENTS.md) has the rules,
[DECISIONS.md](DECISIONS.md) the ADRs, [ROADMAP.md](ROADMAP.md) the plan.

1. **Pick or open an issue** (feature, bug or task form). It carries one `type:*`, one or
   more `area:*`, one `priority:*` label and the milestone of its roadmap step.
2. **Branch** from `main` as `<type>/<issue>-<short-slug>`, e.g. `feat/5-port-solver`.
3. **Commit** with [Conventional Commits](https://www.conventionalcommits.org/):
   `type(scope): lowercase summary`, e.g. `feat(solver): add balanced-hours objective`.
   `pre-commit install` adds the hooks, including the commit-message check.
4. **Never use real data** (ADR-008): fictitious names only, everywhere, including commit
   messages and screenshots. CI fails on denylisted terms.
5. **Open a pull request** with `Closes #N` and the template filled. The PR title must be a
   Conventional Commit: PRs are squash-merged and the title becomes the commit on `main`.
6. The maintainer reviews and merges.

Security problems and real data found in the repo: see [SECURITY.md](SECURITY.md).
