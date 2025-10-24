---
name: crud-operations
description: CRUD (Create, Read, Update, Delete) patterns for API operations in the frontoffice application. Use when working with data operations, API calls, or implementing data management features.
---

# CRUD Operations

## When to Use This Skill

Use this skill when:
- Implementing data fetching or mutations
- Working with API endpoints
- Creating new CRUD modules
- Understanding existing data operations
- Handling API errors and retries

## Architecture

### CRUD Module Location
```
src/_core/crud/
├── normalizers/        # Data normalizers
├── schema.js           # Data schemas
├── utils.js            # Common utilities
└── *.crud.js           # Specific CRUD modules
```

### Standard CRUD Module Pattern

```javascript
import { createCRUD } from '@core/crud/utils';
import { schema } from './schema';

const crud = createCRUD({
  prefix: '/api/resource',
  schema: schema.resource,
  normalizer: normalizers.resource,
});

export const { getAll, getById, create, update, remove } = crud;
```

## Key Modules

### Authentication (`auth.crud.js`)
```javascript
import { login, logout, refreshToken } from '@core/crud/auth.crud';

await login(credentials);
await logout();
await refreshToken();
```

### Standard Resource Operations
- `getAll(filters)` - List resources with pagination
- `getById(id)` - Get single resource
- `create(data)` - Create new resource
- `update(id, data)` - Update existing resource
- `remove(id)` - Delete resource

## Error Handling Pattern

```javascript
try {
  const data = await crud.getAll(filters);
} catch (error) {
  if (error instanceof APIError) {
    handleAPIError(error);
  } else {
    handleGeneralError(error);
  }
}
```

## Common Patterns

### With Retry Logic
```javascript
async function withRetry(operation, retries = 3) {
  for (let i = 0; i < retries; i++) {
    try {
      return await operation();
    } catch (error) {
      if (i === retries - 1) throw error;
      await delay(Math.pow(2, i) * 1000);
    }
  }
}
```

### Optimistic Updates
```javascript
function optimisticUpdate(id, updates) {
  updateLocalState(id, updates);

  try {
    await api.update(id, updates);
  } catch (error) {
    revertLocalState(id);
    throw error;
  }
}
```

## Implementation Guidelines

When creating a new CRUD module:
1. Create file `{resource}.crud.js`
2. Import `createCRUD` utility
3. Define schema and normalizer
4. Export standard operations
5. Add custom operations if needed
6. Include comprehensive error handling
7. Write tests for all operations
