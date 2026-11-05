@AGENTS.md

## Claude Code notes

- `.claude/settings.json` allows `git push -u origin` for `feat/`, `fix/`, `chore/` and
  `docs/` branches and `gh pr create`. Pushes to `main` and force pushes are denied.
- Project allow rules only load once the workspace is trusted: run `claude` interactively in
  the repo and accept the trust dialog.
- The real pharmacy data lives only in production D1 and in an ignored `private/` folder on the
  maintainer's machine. Never read `private/` unless the user asks, and never copy from it.
