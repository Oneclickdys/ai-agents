# Backend vs Frontend: Decision Guide

## Quick Reference

**You are currently in the FRONTEND repository** (`tangerine-frontoffice`)

This guide helps determine when a LINEAR task requires backend changes vs when it's frontend-only.

## Decision Tree

```
START: Analyzing LINEAR Task
    │
    ├─ Does it require NEW data that doesn't exist?
    │  └─ YES → Backend changes needed (i2c_v2 or tangerine-api-v3)
    │  └─ NO → Continue
    │
    ├─ Does it require CHANGING how data is stored/retrieved?
    │  └─ YES → Backend changes needed
    │  └─ NO → Continue
    │
    ├─ Does it require NEW business logic on the server?
    │  └─ YES → Backend changes needed
    │  └─ NO → Continue
    │
    ├─ Does it require NEW permissions/authorization?
    │  └─ YES → Backend changes needed
    │  └─ NO → Continue
    │
    └─ Is it ONLY about UI, display, or client-side logic?
       └─ YES → Frontend-only ✅
       └─ NO → Re-evaluate above questions
```

## Frontend-Only Tasks (This Repo)

Work **ONLY** in `tangerine-frontoffice` when the task involves:

### UI/UX Changes
- ✅ Changing component layout or styling
- ✅ Adding animations or transitions
- ✅ Updating color schemes, fonts, spacing
- ✅ Responsive design adjustments
- ✅ Accessibility improvements (ARIA labels, keyboard nav)

### Client-Side Logic
- ✅ Form validation (visual feedback)
- ✅ Client-side filtering/sorting of existing data
- ✅ Client-side calculations (non-persistent)
- ✅ Local state management (React state, context)
- ✅ Navigation and routing

### Display Logic
- ✅ Formatting dates, numbers, currency
- ✅ Conditional rendering based on existing data
- ✅ Showing/hiding elements
- ✅ Modal dialogs and popups
- ✅ Toast notifications

### Consuming Existing APIs
- ✅ Calling existing API endpoints
- ✅ Updating how data is displayed (already fetched)
- ✅ Improving error messages for existing errors
- ✅ Adding loading states

### Examples of Frontend-Only Tasks:

**Example 1**: TAN-3554 - Fix TypeError in IframeViewer
- ✅ Frontend-only: Add validation to existing component
- ✅ No new data needed
- ✅ No API changes
- ✅ Pure UI bug fix

**Example 2**: Add dark mode toggle
- ✅ Frontend-only: CSS theming + local storage
- ✅ No backend needed (preference stored locally or in existing user settings)

**Example 3**: Improve course card layout
- ✅ Frontend-only: CSS and component restructuring
- ✅ Uses existing course data

## Backend-Required Tasks

You need to work in `i2c_v2` or `tangerine-api-v3` when the task involves:

### New Data Requirements
- ❌ Creating new database tables/models
- ❌ Adding new fields to existing models
- ❌ Storing new types of data
- ❌ Persisting user preferences server-side

### Data Processing
- ❌ Complex calculations that should be server-side
- ❌ Aggregations across large datasets
- ❌ Data transformations before sending to frontend
- ❌ Scheduled jobs or background processing

### Business Logic
- ❌ Payment processing
- ❌ Grade calculations
- ❌ Access control rules
- ❌ Validation that must be enforced server-side

### New API Endpoints
- ❌ Creating new REST endpoints
- ❌ Adding new query parameters to endpoints
- ❌ Changing response structure significantly
- ❌ New WebSocket events

### Security & Authentication
- ❌ New permission levels
- ❌ Authentication flow changes
- ❌ New user roles
- ❌ API security updates

### Examples of Backend-Required Tasks:

**Example 1**: Add student attendance tracking
- ❌ Backend needed: New attendance table, POST/GET endpoints
- ✅ Then frontend: UI to mark/view attendance

**Example 2**: Export grades to Excel
- ❌ Backend needed: Server-side Excel generation endpoint
- ✅ Then frontend: Download button that calls endpoint

**Example 3**: Real-time collaborative editing
- ❌ Backend needed: WebSocket server, conflict resolution
- ✅ Then frontend: Real-time UI updates

## Full-Stack Task Workflow

When a task requires **both** backend and frontend changes:

### Phase 1: Backend (Different Repo)
1. Switch to backend repo: `cd /Users/camilolopez/DEVS/repos/i2c_v2`
2. Create feature branch
3. Implement API endpoints
4. Add tests
5. Create backend PR
6. Get PR reviewed and merged
7. Deploy to dev environment
8. Test endpoints with Postman/curl

### Phase 2: Frontend (This Repo)
9. Switch to frontend repo: `cd /Users/camilolopez/DEVS/repos/tangerine-frontoffice`
10. Create feature branch
11. Implement UI components
12. Integrate with new API endpoints
13. Add tests
14. Create frontend PR
15. Test full integration

### Communication in LINEAR

When creating a backend task, include:
```markdown
## Backend Requirements
- [ ] POST /api/endpoint-name - Description
- [ ] GET /api/endpoint-name - Description
- [ ] Database changes: new table `table_name`

## API Contract
Request:
```json
{ "field": "value" }
```

Response:
```json
{ "result": "value" }
```

## Frontend Task
Will be implemented in separate task: TAN-XXXX
```

## Common Confusion Points

### "Should grade calculation be frontend or backend?"
❌ **Backend** - Business logic and calculations should be server-side for consistency and security.

### "Should date formatting be frontend or backend?"
✅ **Frontend** - Display logic should be client-side. Backend sends ISO dates, frontend formats for display.

### "Should form validation be frontend or backend?"
✅ **Both** - Frontend for UX (immediate feedback), Backend for security (never trust client).

### "Should I paginate on frontend or backend?"
❌ **Backend** - Large datasets should be paginated server-side for performance.

### "Should I filter a list of 20 items on frontend or backend?"
✅ **Frontend** - Small datasets can be filtered client-side for better UX.

## Red Flags: Backend Changes Needed

If you see these requirements in a LINEAR task, backend changes are likely needed:

- 🚩 "Store X in the database"
- 🚩 "Send email notification when Y happens"
- 🚩 "Calculate X based on all users' data"
- 🚩 "Generate report from historical data"
- 🚩 "Schedule X to happen every day"
- 🚩 "Only allow users with role Y to do Z"
- 🚩 "Export to PDF/Excel/CSV"
- 🚩 "Import data from file"

## When in Doubt

**ASK YOURSELF:**
1. Can this work without an internet connection using only cached data?
   - YES → Probably frontend-only
   - NO → Probably needs backend

2. If I refresh the page, should the data still be there?
   - YES → Backend needed
   - NO → Frontend-only (local state)

3. Should this be accessible from a mobile app or different frontend?
   - YES → Backend logic
   - NO → Could be frontend

**IF STILL UNSURE:**
- Review LINEAR task description carefully
- Check if backend team is assigned
- Ask in team chat/standup
- Default to frontend-only and note backend assumptions in PR

## Repository Locations

**Frontend (Current Repo):**
```bash
/Users/camilolopez/DEVS/repos/tangerine-frontoffice
```

**Backend Options:**
```bash
# Core API
/Users/camilolopez/DEVS/repos/i2c_v2

# Extended API
/Users/camilolopez/DEVS/repos/tangerine-api-v3
```

## Summary

- 📱 **Frontend**: Anything the user sees and interacts with
- ⚙️ **Backend**: Data storage, business logic, security, server processing
- 🔄 **Full-Stack**: Features that require both (e.g., new CRUD features)

**Most LINEAR tasks in this repo are frontend-only.** When backend is needed, coordinate with the backend team first.
