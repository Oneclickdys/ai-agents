# Tangerine Frontoffice - Repository Structure

## Overview

This is the **React frontend application** for the Tangerine learning platform. It uses a **Git submodule architecture** for shared core components and communicates with backend APIs.

## Ecosystem Architecture

The Tangerine platform consists of multiple repositories:

### Frontend Repositories

1. **tangerine-frontoffice** (THIS REPO)
   - Main React application
   - Location: `/Users/camilolopez/DEVS/repos/tangerine-frontoffice`
   - URL: https://github.com/Oneclickdys/tangerine-frontoffice
   - Branch: `main`

2. **i2c_frontoffice_core** (Submodule of this repo)
   - Shared React components, hooks, utilities
   - Location: `src/_core/` (within this repo)
   - URL: https://github.com/Oneclickdys/i2c_frontoffice_core
   - Branch: `main-core`

### Backend Repositories

3. **i2c_v2**
   - Core backend API (Node.js)
   - Location: `/Users/camilolopez/DEVS/repos/i2c_v2`
   - URL: https://github.com/Oneclickdys/i2c_v2
   - Purpose: Main API for authentication, courses, users, resources

4. **tangerine-api-v3**
   - Extended backend API (Node.js)
   - Location: `/Users/camilolopez/DEVS/repos/tangerine-api-v3`
   - URL: https://github.com/Oneclickdys/tangerine-api-v3
   - Purpose: Additional features and microservices

### Other Related Repositories

5. **tangerine-ai**
   - AI/chatbot features
   - Location: `/Users/camilolopez/DEVS/repos/tangerine-ai`

## When to Work in Each Repository

### Work in `tangerine-frontoffice` (THIS REPO) when:
- ✅ Adding new pages or routes
- ✅ Implementing app-specific features
- ✅ Updating app configuration
- ✅ Working on app-level state management
- ✅ UI changes specific to tangerine app

### Work in `i2c_frontoffice_core` (submodule) when:
- ✅ Creating reusable components (atoms, modules)
- ✅ Adding shared hooks or utilities
- ✅ Updating core UI components used across apps
- ✅ Changes that should be shared with other frontend apps

### Work in backend repos when:
- ✅ Adding new API endpoints
- ✅ Changing database models
- ✅ Implementing business logic
- ✅ Backend authentication/authorization
- ⚠️ **Note**: Most frontend tasks don't require backend changes

## Full-Stack Feature Development

When implementing a feature that requires **both frontend and backend changes**:

1. **Backend First** (`i2c_v2` or `tangerine-api-v3`):
   - Create API endpoints
   - Implement business logic
   - Add database migrations
   - Test with Postman/curl
   - Deploy to dev environment

2. **Frontend** (this repo):
   - Consume new API endpoints
   - Implement UI components
   - Add state management
   - Test integration

### Example Workflow:

```
Feature: Add user profile photo upload

1. Backend (i2c_v2):
   - POST /api/users/:id/upload-photo
   - File storage implementation
   - Database update

2. Frontend (tangerine-frontoffice):
   - Create UploadPhotoButton component (in _core if reusable)
   - Call API endpoint
   - Update UI after successful upload
```

## Repository Information

### Parent Repository
- **Name**: `tangerine-frontoffice`
- **URL**: https://github.com/Oneclickdys/tangerine-frontoffice
- **Main Branch**: `main`
- **Purpose**: Main application code, app-specific features, routing, pages

### Submodule Repository
- **Name**: `i2c_frontoffice_core` (located at `src/_core`)
- **URL**: https://github.com/Oneclickdys/i2c_frontoffice_core
- **Main Branch**: `main-core` ⚠️ (different from parent!)
- **Purpose**: Shared core components, atoms, modules, hooks, utilities

## Directory Structure

```
tangerine-frontoffice/
├── src/
│   ├── _core/              ← GIT SUBMODULE (separate repo!)
│   │   ├── modules/        # Shared React components
│   │   ├── atoms/          # Base UI components
│   │   ├── hooks/          # Shared React hooks
│   │   ├── utils/          # Utility functions
│   │   ├── lite/           # Lite version components/views
│   │   └── ...
│   ├── app/                # App-specific code
│   ├── App.js
│   └── index.js
├── public/
├── .claude/                # Claude Code configuration
└── package.json
```

## Working with the Submodule

### Key Concepts

1. **Two Separate Git Repositories**:
   - Parent: `tangerine-frontoffice`
   - Submodule: `i2c_frontoffice_core`

2. **Different Main Branches**:
   - Parent uses `main`
   - Submodule uses `main-core`

3. **Submodule Reference**:
   - Parent repo stores a reference (commit SHA) to a specific submodule version
   - Changing files in `src/_core` requires committing in BOTH repos

### When You Modify Files

#### Scenario 1: Files in `src/_core/` (Submodule)

**You need TWO commits and TWO PRs:**

1. **Commit in submodule**:
   ```bash
   cd src/_core
   git checkout main-core
   git pull origin main-core
   git checkout -b TAN-XXXX-feature
   git add <files>
   git commit -m "TAN-XXXX: Description"
   git push -u origin TAN-XXXX-feature
   ```

2. **Commit in parent** (updates submodule reference):
   ```bash
   cd ../..
   git checkout main
   git pull origin main
   git checkout -b TAN-XXXX-feature
   git add src/_core
   git commit -m "TAN-XXXX: Description

   Updates _core submodule to include [changes]"
   git push -u origin TAN-XXXX-feature
   ```

3. **Create TWO PRs**:
   - Submodule PR: base = `QA-core`
   - Parent PR: base = `QA`, mention submodule PR

#### Scenario 2: Files Outside `src/_core/` (Parent Only)

**You need ONE commit and ONE PR:**

```bash
git checkout main
git pull origin main
git checkout -b TAN-XXXX-feature
git add <files>
git commit -m "TAN-XXXX: Description"
git push -u origin TAN-XXXX-feature
```

Create PR with base = `QA`

### Common Commands

```bash
# Check which repo has changes
git status                    # Parent repo
cd src/_core && git status    # Submodule

# Update submodule to latest
git submodule update --init --recursive

# Check submodule branch
cd src/_core && git branch --show-current

# View submodule commit reference
git ls-tree HEAD src/_core
```

## Branch Naming Convention

Format: `TAN-XXXX-short-description` or `TANAI-XXX-short-description`

Examples:
- `TAN-3554-fix-invalid-url-error`
- `TAN-3310-qualitative-grading`
- `TANAI-329-activity-session-response`

## Commit Message Format

```
TAN-XXXX: Descriptive message in imperative mood

Optional longer description explaining the change.
Can include multiple paragraphs.
```

Examples:
- `TAN-3554: Add three-layer validation to prevent TypeError in IframeViewer`
- `TAN-3310: Implement qualitative grading scale with color indicators`

## Pull Request Workflow

### For Submodule Changes:

1. **Submodule PR** (i2c_frontoffice_core):
   - Base: `main-core`
   - Contains actual code changes
   - URL: `https://github.com/Oneclickdys/i2c_frontoffice_core/pull/new/BRANCH`

2. **Parent PR** (tangerine-frontoffice):
   - Base: `main`
   - Updates submodule reference
   - Links to submodule PR
   - URL: `https://github.com/Oneclickdys/tangerine-frontoffice/pull/new/BRANCH`

### PR Description Template

See `/Users/camilolopez/DEVS/repos/tangerine-frontoffice/.claude/commands/create-mr.md` for full templates.

## Important Notes

⚠️ **Always Remember:**

1. Submodule uses `main-core` branch, parent uses `main`
2. Commit in submodule FIRST, then parent
3. Create submodule PR FIRST, then parent PR
4. Parent PR should reference submodule PR
5. Both repos must be deployed together for submodule changes

## Troubleshooting

### Submodule in Detached HEAD State

```bash
cd src/_core
git checkout main-core
git pull origin main-core
git checkout -b your-feature-branch
# Reapply your changes or cherry-pick commits
```

### Wrong Branch in Submodule

```bash
cd src/_core
git branch --show-current  # Check current branch
git checkout main-core        # Switch to main-core
git pull origin main-core     # Update
```

### Submodule Reference Not Updated

```bash
git status  # Should show "modified: src/_core (new commits)"
git add src/_core
git commit -m "TAN-XXXX: Update _core submodule reference"
```

## Related Documentation

- `/commit` command: `.claude/commands/commit.md`
- `/create-mr` command: `.claude/commands/create-mr.md`
- `/start-work` command: `.claude/commands/start-work.md`

## API Integration

### Backend API Endpoints

The frontend communicates with:

1. **i2c_v2 API** (Core API)
   - Base URL: Usually configured in `.env`
   - Authentication: JWT tokens
   - Main endpoints:
     - `/api/auth/*` - Authentication
     - `/api/users/*` - User management
     - `/api/courses/*` - Course data
     - `/api/lessons/*` - Lesson content

2. **tangerine-api-v3** (Extended API)
   - Additional features and microservices
   - Often used for new feature development

### Working with API Changes

**If LINEAR task requires API changes:**

1. ✅ Coordinate with backend team
2. ✅ Backend PR should be merged first
3. ✅ Update frontend to use new endpoints
4. ✅ Update API documentation
5. ✅ Test integration thoroughly

**In LINEAR task description, mention:**
- "Requires backend changes in i2c_v2"
- Link to backend PR/task
- API endpoint documentation

## Project Links

### Issue Tracking & Monitoring
- Linear: https://linear.app/oneclick
- Sentry: https://oneclick-2bae4c6fd.sentry.io

### Frontend Repositories
- This Repo (Parent): https://github.com/Oneclickdys/tangerine-frontoffice
- Submodule (_core): https://github.com/Oneclickdys/i2c_frontoffice_core

### Backend Repositories
- Core API (i2c_v2): https://github.com/Oneclickdys/i2c_v2
- Extended API (tangerine-api-v3): https://github.com/Oneclickdys/tangerine-api-v3

### Local Paths
- Frontend: `/Users/camilolopez/DEVS/repos/tangerine-frontoffice`
- Backend v2: `/Users/camilolopez/DEVS/repos/i2c_v2`
- Backend v3: `/Users/camilolopez/DEVS/repos/tangerine-api-v3`
