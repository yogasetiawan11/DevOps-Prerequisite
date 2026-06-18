# Pre-commit Hooks — Guidance, Configuration, and Use Cases

## Purpose

Pre-commit hooks run automatically before a commit is recorded. They help catch mistakes early, enforce project standards, and stop secrets, large files, failing tests, or policy violations from entering the repository.

## Quick choices
- Native Git hooks: lightweight, implemented as scripts under `.git/hooks/`.
- pre-commit (https://pre-commit.com): language-agnostic framework to manage and version hooks in the repository (`.pre-commit-config.yaml`).
- Husky: popular for Node/JS projects; installs Git hooks via npm/yarn.

## Recommended workflow
- Keep hook configuration in the repo (preferred: `pre-commit`).
- Ensure hooks are fast — use them for quick checks; run heavier scans in CI.
- Install hooks for every developer (document the install command in README).
- Also run hooks in CI: `pre-commit run --all-files` or equivalent.

## Example: `pre-commit` framework

Install (system or venv):

```bash
pip install pre-commit
pre-commit install
```

Sample `.pre-commit-config.yaml`:

```yaml
repos:
	- repo: https://github.com/pre-commit/pre-commit-hooks
		rev: v4.5.0
		hooks:
			- id: trailing-whitespace
			- id: end-of-file-fixer
			- id: check-yaml
			- id: check-added-large-files

	- repo: https://github.com/psf/black
		rev: 24.1.0
		hooks:
			- id: black

	- repo: https://github.com/PyCQA/flake8
		rev: 6.1.0
		hooks:
			- id: flake8

# Add other repos / hooks for your language (prettier, eslint, gofmt, goimports, etc.)
```

Usage:

```bash
# Install hooks (once per checkout)
pre-commit install

# Run hooks against all files (useful in CI)
pre-commit run --all-files
```

## Example: Husky (Node.js)

```bash
npm install --save-dev husky
npx husky install
npm pkg set scripts.prepare="husky install"
npx husky add .husky/pre-commit "npm test"
```

Contents of `.husky/pre-commit` (example):

```sh
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npm run lint
npm test
```

## Example: Native Git hook (generic)

Place a script at `.git/hooks/pre-commit` (executable):

```sh
#!/bin/sh
# simple example: prevent commits with TODOs and run quick tests
if git diff --cached --name-only | grep -E "\.(py|js|java)$" >/dev/null; then
	# run quick linter or unit-test subset
	./scripts/quick-checks.sh || exit 1
fi

exit 0
```

For Java/Maven projects you might run a fast verification:

```sh
# in a hook: run only unit tests, short mode
mvn -q -DskipITs -Dtest=!IntegrationTest test || exit 1
```

## Use cases (common hooks)
- Formatting: `black`, `prettier`, `gofmt` — enforce style before commit.
- Linting: `eslint`, `flake8`, `golangci-lint`.
- Tests: run fast unit tests or smoke tests to catch obvious failures.
- Secrets / sensitive data: run `gitleaks` or similar to detect secrets.
- Dependency / container checks: run `trivy` or SCA tools in CI; optionally a fast local check.
- License headers and copyright checks.
- Commit message checks: enforce Conventional Commits or JIRA ticket format.
- Block large files: `check-added-large-files`.
- Enforce changelog or PR template presence for release branches.

## Security scanning examples
- Local quick secret-scan (manual or via hook): `gitleaks detect --source . --report-path gitleaks-report.json`.
- Prefer running full container/image scans and SCA in CI rather than blocking local commits with very slow scans.

## Best practices
- Keep hooks fast and focused; offload heavy scans to CI.
- Pin hook versions (`rev`) so behavior is reproducible.
- Add installation instructions to your `README.md` and include the `pre-commit` config in the repo.
- Make hooks idempotent and safe to run multiple times.
- Allow bypass only with a strict policy (e.g., documented `--no-verify` exceptions with approvals).
- Use `pre-commit run --all-files` in CI to ensure history and branch-wide checks.

## Troubleshooting
- Hook not running: ensure `pre-commit install` was executed and `.git/hooks` contains the wrapper.
- Permission errors: make the hook executable (`chmod +x .husky/* .git/hooks/*`).
- Slow local hook: move the slow check to CI and keep a fast local pre-check.

## Quick checklist to add pre-commit to a repo
1. Choose framework: `pre-commit` (general) or `husky` (Node) or native scripts.
2. Add `.pre-commit-config.yaml` (or `.husky/*` scripts) to repo.
3. Document install steps in `README.md`.
4. Add `pre-commit run --all-files` to CI pipeline.
5. Keep hook versions pinned and review periodically.

---

If you want, I can also:
- generate a starter `.pre-commit-config.yaml` tuned to this repo's languages, or
- add a sample `.husky/pre-commit` and a native `.git/hooks/pre-commit` wrapper.

