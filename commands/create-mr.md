---
allowed-tools: Bash(gh pr:*), Bash(git:*), Bash(npm test:*), Bash(npm run lint:*), Bash(cd:*), Read, Glob, Grep
description: Create a Pull Request from the current branch to main (tangerine-ai)
response-language: es
---

# Create GitHub Pull Request

Create a PR from the current branch to `main` in the `tangerine-ai` repository.

## Repository Premises

- Project: `tangerine-ai` (AI API for Tangerine)
- Base branch: `main`
- Issue tracker: Linear (project `TANAI`)
- No submodules

## Process

### 1) Pre-flight checks

- Run `git status` and ensure no uncommitted changes
- Get branch name: `git branch --show-current`
- Extract Linear ID (e.g., `tanai-1234-x` → `TANAI-1234`)

### 2) Analyze local changes

```bash
git log origin/main..HEAD --oneline
git diff origin/main...HEAD --stat
```

### 3) Push the branch

```bash
git push -u origin HEAD
```

### 4) Create the Pull Request

- Prefer GitHub CLI:

```bash
gh pr create --base main --title "TANAI-XXXX: Short title" --body "$(cat <<'EOF'
## Summary
[Brief description of the change]

## Linear Task
[TANAI-XXXX](https://linear.app/oneclick/issue/TANAI-XXXX)

## Changes Made
- [List of relevant changes]

## Modified Files
- [Relevant paths]

## Testing
- [ ] Linter passes
- [ ] Relevant unit/integration tests (when applicable)
- [ ] Basic manual verification (affected endpoints)

## Impact
[Risks, migrations, flags, breaking changes]

## Checklist
- [ ] Project conventions respected
- [ ] Proper error handling
- [ ] TANAI reference in commits
EOF
)"
```

- If `gh` is not available, provide the URL:
    - `https://github.com/Oneclickdys/tangerine-ai/compare/main...BRANCH-NAME?expand=1`
    - Include the full description using the template above

## Notes

- Base for PRs is always `main`
- Ensure all commits are prefixed with `TANAI-XXXX`

## Troubleshooting

- If push is rejected (non-fast-forward):

```bash
git pull --rebase origin main
git push -u origin HEAD --force-with-lease
```

- If branch already exists remotely, use `--force-with-lease` with care
- Without `gh`, use the compare URL and paste the formatted body
