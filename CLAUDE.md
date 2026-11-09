@AGENTS.md

## Claude Code notes

- `.claude/settings.json` allows the npm/uv commands, local git, `git push -u origin` for
  `feat/`, `fix/`, `chore/`, `docs/`, `refactor/`, `test/`, `perf/` and `ci/` branches,
  `gh issue create`, `gh pr create`, reading PR checks and the denylist check. Pushes to `main`,
  force pushes, `gh pr merge`, `rm -rf` and reading `private/` are denied.
- Project allow rules only load once the workspace is trusted: run `claude` interactively in
  the repo and accept the trust dialog.
- The real pharmacy data lives only in production D1 and in an ignored `private/` folder on the
  maintainer's machine. Never read `private/` unless the user asks, and never copy from it.
