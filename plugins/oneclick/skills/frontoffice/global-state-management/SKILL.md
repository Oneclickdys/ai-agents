---
name: global-state-management
description: Global state management patterns using Redux/Context in the frontoffice application. Use when working with application state, Redux actions/reducers, or shared state across components.
---

# Global State Management

## When to Use

- Managing application-wide state
- Creating Redux actions/reducers
- Sharing state across components
- Implementing state persistence

## State Architecture

### When to Use Global State
- Authentication status
- User profile data
- Application settings
- Shared UI state (modals, notifications)

### When to Use Local State
- Form inputs
- Component-specific UI state
- Temporary data

## Redux Pattern

### Actions
```javascript
// actionTypes.js
export const FETCH_DATA = 'FETCH_DATA';
export const FETCH_DATA_SUCCESS = 'FETCH_DATA_SUCCESS';
export const FETCH_DATA_ERROR = 'FETCH_DATA_ERROR';

// actions.js
export const fetchData = (params) => ({
  type: FETCH_DATA,
  payload: params,
});
```

### Reducers
```javascript
const initialState = {
  data: [],
  loading: false,
  error: null,
};

export default function reducer(state = initialState, action) {
  switch (action.type) {
    case FETCH_DATA:
      return { ...state, loading: true };
    case FETCH_DATA_SUCCESS:
      return { ...state, loading: false, data: action.payload };
    case FETCH_DATA_ERROR:
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
}
```

### Selectors
```javascript
export const selectData = (state) => state.module.data;
export const selectLoading = (state) => state.module.loading;
```

## Best Practices

1. **Normalize state** - Flat structure
2. **Use selectors** - Encapsulate state access
3. **Keep reducers pure** - No side effects
4. **Use action creators** - Consistent actions
5. **Immutable updates** - Spread operator
