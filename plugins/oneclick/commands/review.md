---
description: Review local changes against base branch before creating PR
allowed-tools: Task, Read, Grep, Bash
---

Review local changes against base branch.

## Purpose

Review uncommitted or committed local changes by comparing with the base branch (e.g., `main`, `QA`). This is done BEFORE creating a pull request.

**This is NOT for reviewing PRs** - use `/review-pr` for that.

---

## Step 1: Identify Base Branch

Determine the base branch:
- Check repository conventions
- Common bases: `main`, `QA`, `master`, `develop`
- Use git to find: `git rev-parse --abbrev-ref origin/HEAD`

## Step 2: Show Diff Summary

Display changes against base branch:

```bash
git diff [base-branch]...HEAD --stat
```

Show:
- Files modified
- Lines added/removed
- Overall change scope

## Step 3: Run Quality Checks

Launch automated checks:

**Linter**:
```bash
npm run lint
# or appropriate linter command
```

**Tests**:
```bash
npm test
# or appropriate test command
```

**Build** (if applicable):
```bash
npm run build
# or appropriate build command
```

## Step 4: Code Review

Launch `code-reviewer` agent to review the diff:

**Focus areas**:
- Code quality and conventions
- Potential bugs or issues
- Security concerns
- Performance considerations
- Test coverage

**Output**:
- Issues found with file:line references
- Severity (Critical, Important, Optional)
- Confidence scoring (≥80)

## Step 5: Present Findings

Summarize review results:

**Automated Checks**:
- ✅/❌ Linter status
- ✅/❌ Tests status
- ✅/❌ Build status

**Code Review Issues**:
- Critical issues (must fix)
- Important issues (should fix)
- Suggestions (optional)

**Recommendation**:
- Ready to create PR
- Fix issues first
- Needs discussion

---

## Next Steps

After review:
- Fix any critical/important issues found
- Run `/review` again to verify fixes
- Use `/create-pr` when ready for pull request
- Use `/review-pr [pr-number]` after PR is created

**Starting local review...**
