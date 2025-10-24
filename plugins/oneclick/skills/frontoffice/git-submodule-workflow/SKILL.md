---
name: git-submodule-workflow
description: Git submodule workflow for the frontoffice application where src/_core is a git submodule. Use when making changes that affect the core submodule or managing submodule updates.
---

# Git Submodule Workflow

## When to Use

- Making changes to `src/_core` files
- Understanding branch strategy
- Creating PRs for submodule changes
- Managing submodule updates

## Repository Structure

### Parent Repository
- **Name**: tangerine-frontoffice
- **Main branch**: `main`
- **PR target**: `QA`

### Submodule Repository
- **Path**: `src/_core`
- **Main branch**: `main-core`
- **PR target**: `QA-core`

## Identifying Change Scope

### Submodule-Only Changes
Files in `src/_core/`:
1. Create branch in submodule
2. Create matching branch in parent
3. Two separate PRs required

### Parent-Only Changes
Files outside `src/_core/`:
1. Create branch in parent only
2. One PR to parent repo

## Workflow Steps

### 1. Setup
```bash
# Update main
git checkout main && git pull

# Update submodule
git submodule update --init --recursive
cd src/_core
git checkout main-core
git pull origin main-core
cd ../..
```

### 2. Create Branches

**For submodule changes**:
```bash
cd src/_core
git checkout -b TAN-123-feature
cd ../..
git checkout -b TAN-123-feature
```

**For parent-only**:
```bash
git checkout -b TAN-123-feature
```

### 3. Make Changes

Work normally, commit as needed.

### 4. Commit Submodule Changes

**IMPORTANT**: Commit in submodule FIRST:
```bash
cd src/_core
git add .
git commit -m "TAN-123: Submodule changes"
git push -u origin TAN-123-feature
cd ../..
```

### 5. Commit Parent Changes

Then commit submodule reference in parent:
```bash
git add .
git add src/_core  # Submodule reference
git commit -m "TAN-123: Update with submodule changes"
git push -u origin TAN-123-feature
```

### 6. Create PRs

1. **Submodule PR** (base: `main-core`)
2. **Parent PR** (base: `main`) - Reference submodule PR

## Best Practices

- Always commit submodule before parent
- Create branches with matching names
- Reference submodule PR in parent PR
- Merge submodule PR first, then parent
