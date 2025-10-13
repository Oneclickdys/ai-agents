---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*), Bash(git branch:*), Bash(git diff:*), Bash(git log:*), Bash(cd:*)
description: Create git commits with proper LINEAR references
---

# Create Git Commit

You are tasked with creating git commits for the changes made during this session, following GitHub and LINEAR conventions.

## Repository Structure

This is a **parent repository with a Git submodule**:
- **Parent repo**: `tangerine-frontoffice` (https://github.com/Oneclickdys/tangerine-frontoffice)
- **Submodule**: `src/_core` (https://github.com/Oneclickdys/i2c_frontoffice_core)
- **Main branches**: `main` in parent, `master` in submodule

## Process:

### 1. **Identify where changes are:**
   - Run `git status` in parent repo to see if changes are in parent or submodule
   - If you see `modified: src/_core (new commits)`, changes are in the submodule
   - Check both locations:
     ```bash
     git status
     cd src/_core && git status && cd ../..
     ```

### 2. **Extract LINEAR ticket ID:**
   - Get current branch name: `git branch --show-current`
   - Extract LINEAR ID from branch name (e.g., from `tan-3554-fix-invalid-url` get `TAN-3554`)
   - LINEAR IDs are case-sensitive and follow pattern: `TAN-XXXX`, `TANAI-XXX`, etc.

### 3. **For changes in SUBMODULE (`src/_core`):**

   a. Navigate to submodule and create branch if needed:
   ```bash
   cd src/_core
   git checkout master
   git pull origin master
   git checkout -b tan-XXXX-description
   ```

   b. Add and commit changes in submodule:
   ```bash
   git add <files>
   git commit -m "TAN-XXXX: Descriptive message"
   ```

   c. Return to parent and update submodule reference:
   ```bash
   cd ../..
   git add src/_core
   git commit -m "TAN-XXXX: Descriptive message

   Updates _core submodule to include [brief description]"
   ```

### 4. **For changes in PARENT repository only:**
   ```bash
   git add <files>
   git commit -m "TAN-XXXX: Descriptive message"
   ```

### 5. **Commit Message Format:**
   - Format: `TAN-XXXX: Descriptive message`
   - Examples:
     - `TAN-3554: Add three-layer validation to prevent TypeError in IframeViewer`
     - `TAN-3310: Implement qualitative grading scale`
   - Use imperative mood ("Add" not "Added", "Fix" not "Fixed")
   - First line should be concise (50-72 chars)
   - Add detailed explanation in body if needed (separated by blank line)

### 6. **Verify commits:**
   - Confirm with `git log --oneline -1`
   - For submodule changes, verify both:
     ```bash
     cd src/_core && git log --oneline -1
     cd ../.. && git log --oneline -1
     ```

## Important Notes:

- **NEVER add AI attribution or "Generated with Claude" messages**
- **NO Co-Authored-By lines**
- Always include LINEAR ticket ID from branch name
- Write commits as if the developer wrote them
- When working with submodules, **always commit in the submodule first**, then update the parent
- The parent repo commit should reference that it updates the submodule
- Use GitHub, not GitLab (this project is on GitHub)

## Common Scenarios:

### Scenario 1: Changes only in submodule files
1. Commit in `src/_core` first
2. Commit submodule reference update in parent

### Scenario 2: Changes only in parent files (outside `src/`)
1. Commit directly in parent repo

### Scenario 3: Changes in both submodule and parent files
1. Commit in `src/_core` first
2. Commit parent files + submodule reference update together

## Troubleshooting:

**If submodule is in detached HEAD state:**
```bash
cd src/_core
git checkout master
git pull origin master
git checkout -b your-feature-branch
# Cherry-pick or reapply your changes
```

**If you committed in wrong place:**
```bash
# Save changes
git diff HEAD~1 > /tmp/changes.patch
git reset --hard HEAD~1
# Apply in correct location
```