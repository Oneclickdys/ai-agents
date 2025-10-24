---
name: crud-usage
description: This document describes the architecture and usage of CRUD (Create, Read, Update, Delete) operations in the system.
---

# CRUD Operations

## Structure

```
src/_core/crud/
├── normalizers/        # Data normalizers
├── schema.js           # Data schemas
├── utils.js            # Common utilities
└── *.crud.js           # Specific CRUD modules
```

## Base Architecture

### Structure of a CRUD Module

```javascript
// example.crud.js
import { createCRUD } from '@core/crud/utils';
import { schema } from './schema';

const crud = createCRUD({
  prefix: '/api/example',
  schema: schema.example,
  normalizer: normalizers.example,
});

export const { getAll, getById, create, update, remove } = crud;
```

## Main Modules

### 1. Authentication (`auth.crud.js`)

```javascript
import { login, logout, refreshToken } from '@core/crud/auth.crud';

// Login
await login(credentials);

// Logout
await logout();

// Refresh Token
await refreshToken();
```

### 2. Users (`users.crud.js`)

```javascript
import { getUsers, createUser, updateUser } from '@core/crud/users.crud';

// Get users
const users = await getUsers(filters);

// Create user
const newUser = await createUser(userData);

// Update user
await updateUser(id, updates);
```

### 3. Courses (`courses.crud.js`)

```javascript
import { getCourses, getCourseDetails, updateCourse } from '@core/crud/courses.crud';

// List courses
const courses = await getCourses(filters);

// Course details
const course = await getCourseDetails(id);

// Update course
await updateCourse(id, updates);
```

## API Structure

### 1. Base Endpoints

- GET `/api/{resource}` - List resources
- GET `/api/{resource}/{id}` - Get resource
- POST `/api/{resource}` - Create resource
- PUT `/api/{resource}/{id}` - Update resource
- DELETE `/api/{resource}/{id}` - Delete resource

### 2. Common Parameters

```javascript
// Pagination
const params = {
  page: 1,
  limit: 10,
  sort: 'createdAt',
  order: 'desc',
};

// Filters
const filters = {
  status: 'active',
  type: 'course',
};

// Includes
const includes = ['author', 'categories'];
```

## Error Handling

### 1. Error Structure

```javascript
class APIError extends Error {
  constructor(status, message, details = {}) {
    super(message);
    this.status = status;
    this.details = details;
  }
}
```

### 2. Error Codes

```javascript
export const ErrorCodes = {
  NOT_FOUND: 404,
  UNAUTHORIZED: 401,
  FORBIDDEN: 403,
  BAD_REQUEST: 400,
  INTERNAL_ERROR: 500,
};
```

### 3. Error Handling

```javascript
try {
  await api.call();
} catch (error) {
  if (error instanceof APIError) {
    // Handle API error
    handleAPIError(error);
  } else {
    // Handle general error
    handleGeneralError(error);
  }
}
```

## Interceptors

### 1. Request Interceptor

```javascript
api.interceptors.request.use(
  (config) => {
    // Add token
    const token = getToken();
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);
```

### 2. Response Interceptor

```javascript
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response.status === 401) {
      // Handle expired token
      return refreshAndRetry(error);
    }
    return Promise.reject(error);
  }
);
```

## Cache and Optimizations

### 1. In-Memory Cache

```javascript
const cache = new Map();

export async function getCachedData(key, fetchFn) {
  if (cache.has(key)) {
    return cache.get(key);
  }

  const data = await fetchFn();
  cache.set(key, data);
  return data;
}
```

### 2. Cache Invalidation

```javascript
export function invalidateCache(pattern) {
  for (const key of cache.keys()) {
    if (key.match(pattern)) {
      cache.delete(key);
    }
  }
}
```

## Data Normalization

### 1. Schemas

```javascript
// schema.js
export const schema = {
  course: {
    id: 'string',
    name: 'string',
    description: 'string',
    status: ['draft', 'published', 'archived'],
    createdAt: 'date',
    updatedAt: 'date',
  },
};
```

### 2. Normalizers

```javascript
// normalizers/course.js
export function normalizeCourse(data) {
  return {
    id: data.id,
    name: data.name,
    description: data.description,
    status: data.status || 'draft',
    createdAt: new Date(data.created_at),
    updatedAt: new Date(data.updated_at),
  };
}
```

## Specific Modules

### 1. Contents (`contents.crud.js`)

```javascript
import { getContents, createContent, updateContent } from '@core/crud/contents.crud';

// Operations with contents
const contents = await getContents(filters);
const newContent = await createContent(contentData);
await updateContent(id, updates);
```

### 2. Lessons (`lessons.crud.js`)

```javascript
import { getLessons, createLesson, updateLesson } from '@core/crud/lessons.crud';

// Operations with lessons
const lessons = await getLessons(courseId);
const newLesson = await createLesson(lessonData);
await updateLesson(id, updates);
```

## Best Practices

### 1. Batch Operations

```javascript
// Create multiple resources
export async function bulkCreate(items) {
  return Promise.all(items.map((item) => create(item)));
}

// Update multiple resources
export async function bulkUpdate(updates) {
  return Promise.all(updates.map(({ id, data }) => update(id, data)));
}
```

### 2. Optimistic Updates

```javascript
function optimisticUpdate(id, updates) {
  // Update UI immediately
  updateLocalState(id, updates);

  try {
    // Perform server update
    await api.update(id, updates);
  } catch (error) {
    // Revert changes in case of error
    revertLocalState(id);
    throw error;
  }
}
```

### 3. Retry Logic

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

## Testing

```javascript
describe('CRUD operations', () => {
  it('should create resource', async () => {
    const data = { name: 'Test' };
    const response = await create(data);
    expect(response).toHaveProperty('id');
    expect(response.name).toBe(data.name);
  });

  it('should handle errors', async () => {
    await expect(create({ invalid: 'data' })).rejects.toThrow(APIError);
  });
});
```

## Implementation Guidelines

### 1. New CRUD Module

1. Create file `{resource}.crud.js`
2. Create normalizer if necessary
3. Implement specific operations
4. Add tests

### 2. Modify Existing Module

1. Maintain backward compatibility
2. Update normalizer
3. Update tests
4. Document changes

### 3. Deprecate Module

1. Mark as deprecated
2. Provide alternative
3. Maintain temporary support
4. Communicate removal timeline
