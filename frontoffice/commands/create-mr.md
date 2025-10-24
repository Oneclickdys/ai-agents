---
allowed-tools: Bash(gh pr:*), Bash(git:*), Bash(npm test:*), Bash(npm run lint:*), Bash(cd:*), Read, Glob, Grep
description: Create a pull request from the current branch to main/master
---

# Create GitHub Pull Request

You are tasked with creating a pull request from the current branch to the main branch.

## Repository Structure

This is a **parent repository with a Git submodule**:
- **Parent repo**: `tangerine-frontoffice` (https://github.com/Oneclickdys/tangerine-frontoffice)
  - Main branch: `main`
  - **QA branch: `QA`** (target for pull requests)
- **Submodule**: `src/_core` (https://github.com/Oneclickdys/i2c_frontoffice_core)
  - Main branch: `main-core`
  - **QA branch: `QA-core`** (target for pull requests)

**IMPORTANT**: When changes are in the `src/_core` submodule, you need to create **TWO pull requests**:
1. PR in the submodule repository first (base: `QA-core`)
2. PR in the parent repository (base: `QA`) (which references the submodule PR)

## Process:

### 1. Pre-flight Checks
- Run `git status` to check for uncommitted changes
- Check if changes are in submodule: look for `modified: src/_core (new commits)`
- If uncommitted changes exist:
  - Show the changes clearly
  - Tell user: "You have uncommitted changes. Please run `/commit` first or stash them"
  - Wait for user decision
- Get branch names:
  ```bash
  git branch --show-current  # Parent repo
  cd src/_core && git branch --show-current && cd ../..  # Submodule
  ```
- Extract LINEAR ticket ID (e.g., `TAN-3554` from `tan-3554-fix-invalid-url`)

### 2. Identify Change Location

Check where changes are:
```bash
git status
cd src/_core && git status && cd ../..
```

**If changes are in `src/_core` submodule**, you'll create TWO PRs.
**If changes are only in parent**, you'll create ONE PR.

### 3. For SUBMODULE Changes - Create Submodule PR First

a. **Analyze submodule changes:**
```bash
cd src/_core
git log origin/QA-core..HEAD --oneline
git diff origin/QA-core...HEAD --stat
```

b. **Push submodule branch:**
```bash
git push -u origin HEAD
```

c. **Create submodule PR:**
- Use GitHub CLI if available, otherwise provide the URL
- Base branch: **`QA-core`**
- Command: `gh pr create --base QA-core --title "TITLE" --body "BODY"`
- URL format: `https://github.com/Oneclickdys/i2c_frontoffice_core/compare/QA-core...BRANCH-NAME?expand=1`

d. **Navigate back to parent:**
```bash
cd ../..
```

### 4. Create Parent Repository PR

a. **Analyze parent changes:**
```bash
git log origin/QA..HEAD --oneline
git diff origin/QA...HEAD --stat
```

b. **Push parent branch:**
```bash
git push -u origin HEAD
```

c. **Create parent PR:**
- Use GitHub CLI if available, otherwise provide the URL
- Base branch: **`QA`**
- Command: `gh pr create --base QA --title "TITLE" --body "BODY"`
- URL format: `https://github.com/Oneclickdys/tangerine-frontoffice/compare/QA...BRANCH-NAME?expand=1`

### 5. Draft PR Description

**For Submodule PR (_core):**
```markdown
## Summary
[Brief description - focus on technical changes in _core]

## Linear Task
[TAN-XXXX](https://linear.app/oneclick/issue/TAN-XXXX)

## Changes Made
- [List changes in _core modules/components]
- [File paths relative to _core root]

## Files Modified
- `modules/...`
- `lite/views/...`
- etc.

## Testing
- [ ] Linting passes
- [ ] Manual testing completed
- [ ] No regressions

## Impact
[Describe what this fixes or improves]
```

**For Parent Repository PR:**
```markdown
## Summary
[Overall description of the feature/fix]

## Linear Task
[TAN-XXXX](https://linear.app/oneclick/issue/TAN-XXXX)

## Sentry Issue (if applicable)
[Link to Sentry issue]

## Root Cause (for bug fixes)
[Explain what caused the issue]

## Changes Made
- [High-level changes]
- [Changes in parent repo]
- [Changes in submodule - reference submodule PR]

## Files Modified

**In `_core` submodule (commit XXXXXXX):**
- `path/to/file1.js`
- `path/to/file2.js`

**In parent repo:**
- `src/_core` - Updated submodule reference
- Other parent files if any

## Testing
- [ ] Linting passes
- [ ] Manual testing completed
- [ ] No regressions
- [ ] Valid cases continue working

## Deployment Notes
- This PR updates the `_core` submodule
- Submodule PR: [link to submodule PR]
- Both repositories need to be deployed together

## Checklist
- [ ] Code follows project conventions
- [ ] Appropriate error handling added
- [ ] Backward compatibility maintained
- [ ] LINEAR reference in commits
```

### 6. Create Pull Requests with Descriptions

**IMPORTANT**: Always create PRs with full descriptions automatically using `gh` CLI.

**With GitHub CLI (`gh`) - PREFERRED METHOD:**

For **submodule PR** (if applicable):
```bash
cd src/_core
gh pr create --base QA-core --title "TITLE" --body "$(cat <<'EOF'
[Full PR description here - use the template from section 5]
EOF
)"
cd ../..
```

For **parent PR**:
```bash
gh pr create --base QA --title "TITLE" --body "$(cat <<'EOF'
[Full PR description here - use the template from section 5]
EOF
)"
```

**If GitHub CLI (`gh`) is NOT available:**

1. Push branches first
2. Provide clickable URLs with `?expand=1` parameter
3. **CRITICAL**: Include the COMPLETE PR description formatted and ready to copy/paste
4. Instruct user to paste the description in the PR form

**For submodule changes:**
1. **Submodule PR**: `https://github.com/Oneclickdys/i2c_frontoffice_core/compare/QA-core...BRANCH-NAME?expand=1`
   - Provide complete formatted description
2. **Parent PR**: `https://github.com/Oneclickdys/tangerine-frontoffice/compare/QA...BRANCH-NAME?expand=1`
   - Provide complete formatted description
   - Include placeholder for submodule PR link to replace

**For parent-only changes:**
1. **Parent PR**: `https://github.com/Oneclickdys/tangerine-frontoffice/compare/QA...BRANCH-NAME?expand=1`
   - Provide complete formatted description

### 7. Important Notes

**Base Branches for PRs:**
- Parent repo: **`QA`** (NOT `main`)
- Submodule (_core): **`QA-core`** (NOT `master`)

**Main Development Branches:**
- Parent repo main branch: `main`
- Submodule main branch: `main-core`

**PR Creation Order:**
1. Always create submodule PR first (if applicable) - base: `QA-core`
2. Then create parent PR (referencing submodule PR) - base: `QA`

**When to create TWO PRs:**
- Changes exist in files under `src/_core/`
- Git status shows: `modified: src/_core (new commits)`

**When to create ONE PR:**
- Changes only in parent files (outside `src/_core/`)
- No submodule commit reference update

## Common Scenarios:

### Scenario 1: Changes only in submodule
1. Push submodule branch
2. Create submodule PR (base: `QA-core`)
3. Push parent branch (to update submodule reference)
4. Create parent PR (base: `QA`) - references submodule PR

### Scenario 2: Changes only in parent
1. Push parent branch
2. Create parent PR (base: `QA`)

### Scenario 3: Changes in both
1. Push submodule branch first
2. Create submodule PR (base: `QA-core`)
3. Push parent branch
4. Create parent PR (base: `QA`) - mentions both parent changes and submodule PR

## Troubleshooting:

**If push is rejected (non-fast-forward):**
```bash
git push -u origin HEAD --force-with-lease
```

**If branch already exists remotely:**
- User may have created it before
- Use `--force-with-lease` to update it safely

**If GitHub CLI (`gh`) is not available:**
- Always provide the direct GitHub compare URL with `?expand=1` parameter
- Format: `https://github.com/OWNER/REPO/compare/BASE...BRANCH?expand=1`
- Include formatted PR description for copy/paste

**GitHub CLI Usage:**
- Always use heredoc format for body to preserve formatting:
  ```bash
  gh pr create --base QA-core --title "TITLE" --body "$(cat <<'EOF'
  [Multi-line PR description]
  EOF
  )"
  ```
- Submodule: `cd src/_core && gh pr create --base QA-core --title "..." --body "$(cat <<'EOF' ... EOF)"`
- Parent: `gh pr create --base QA --title "..." --body "$(cat <<'EOF' ... EOF)"`
- **DO NOT use interactive mode** - always provide title and body directly
- Interactive mode (`gh pr create --base QA`) should only be used if heredoc approach fails