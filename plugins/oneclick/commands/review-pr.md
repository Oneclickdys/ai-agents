---
argument-hint: [pr-number]
description: Review a pull request and provide feedback
allowed-tools: Task, Read, Grep, Bash
---

Review pull request: **$ARGUMENTS**

## Purpose

Review an existing pull request on GitHub, analyze changes, identify issues, and provide feedback for the PR author.

**This is for reviewing PRs** - use `/review` for local changes before PR.

---

## Step 1: Fetch PR Details

Use GitHub CLI to get PR information:

```bash
gh pr view $ARGUMENTS --json title,body,headRefName,baseRefName,commits,additions,deletions,changedFiles
```

Show:
- PR title and description
- Branch names (head → base)
- Commit count
- Files changed count
- Lines added/removed

## Step 2: Get PR Diff

Fetch the PR diff:

```bash
gh pr diff $ARGUMENTS
```

Optionally show changed files:
```bash
gh pr diff $ARGUMENTS --name-only
```

## Step 3: Checkout PR Branch (Optional)

If deeper analysis needed:

```bash
gh pr checkout $ARGUMENTS
```

This allows:
- Running tests locally
- Running linter
- Building the project
- Deeper code inspection

## Step 4: Code Review

Launch 2-3 `code-reviewer` agents in parallel with different focuses:

**Agent 1 - Code Quality**:
- Focus on simplicity, DRY, maintainability
- Check code duplication
- Review naming and structure
- Confidence scoring (≥80)
- Include file:line references

**Agent 2 - Functional Correctness**:
- Focus on bugs, logic errors, edge cases
- Verify error handling
- Check security vulnerabilities
- Confidence scoring (≥80)
- Include file:line references

**Agent 3 - Conventions & Architecture**:
- Focus on project conventions
- Check consistency with codebase
- Review architecture fit
- Confidence scoring (≥80)
- Include file:line references

**After agents complete**: Consolidate findings by severity.

## Step 5: Check PR Description

Review PR description quality:
- Clear summary?
- Links to LINEAR ticket?
- Test plan included?
- Breaking changes noted?
- Deployment notes present?

Suggest improvements if needed.

## Step 6: Present Review Findings

Provide comprehensive review:

**Summary**:
- Overall assessment
- Key concerns
- Strengths

**Issues by Severity**:

**Critical Issues** (Score ≥90):
- Description with file:line reference
- Why it's critical
- Suggested fix

**Important Issues** (Score 80-89):
- Description with file:line reference
- Impact on quality
- Suggested fix

**Suggestions** (Optional improvements):
- Nice-to-have improvements
- Best practice recommendations

**Positive Observations**:
- What was done well
- Good patterns followed

## Step 7: Provide Recommendation

**Approve**: If no critical issues and important issues are minor
**Request Changes**: If critical issues exist or significant improvements needed
**Comment**: If feedback is provided but no blocking issues

---

## Output Format

Format review comments for GitHub:

```markdown
## Review Summary

[Overall assessment]

## Critical Issues

### 1. [Issue Title]
**File**: `path/to/file.js:123`
**Severity**: Critical (Score: 95)

[Description of issue]

**Suggested Fix**:
```suggestion
[code suggestion]
```

## Important Issues

[Similar format]

## Suggestions

[Similar format]

## Positive Notes

- [Good things observed]
```

## Next Steps

After review:
- Post review comments on GitHub using `gh pr review`
- Or provide formatted comments for manual posting
- Track PR updates and re-review if needed

**Starting PR review...**
