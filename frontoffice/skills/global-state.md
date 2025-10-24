---
name: global-state-manager
description: The application uses Redux as the main solution for global state management, implementing the "Redux Ducks" pattern for better code organization.
---

# State and Data Management

The application uses Redux as the main solution for global state management, implementing the "Redux Ducks" pattern for better code organization.

## Redux Setup

### Store Structure

```
src/_core/store/
├── index.js                    # Main store configuration
├── conf.js                     # General configuration
├── parsers.js                  # Common parsers
├── i18n.js                     # Internationalization configuration
├── mixins/                     # Reusable mixins
├── viewer/                     # Viewer state
└── *.duck.js                   # Redux modules (ducks)
```

### Main Modules

1. **Auth** (`auth.duck.js`)

   - Authentication
   - Session management
   - Permissions

2. **UI** (`ui.duck.js`)

   - Interface state
   - Themes
   - Responsive

3. **Courses** (`courses.duck.js`)

   - Course management
   - Scheduling
   - Course status

4. **Contents** (`contents.duck.js`)
   - Educational content
   - Resources
   - Materials

## Store Configuration

### Base Configuration

```javascript
// store/index.js
import { configureStore } from '@reduxjs/toolkit';
import { createLogger } from 'redux-logger';
import rootReducer from './reducers';

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(createLogger()),
  devTools: process.env.NODE_ENV !== 'production',
});

export default store;
```

### Duck Structure

```javascript
// example.duck.js

// Action Types
const PREFIX = '@example';
export const LOAD = `${PREFIX}/LOAD`;
export const SUCCESS = `${PREFIX}/SUCCESS`;
export const ERROR = `${PREFIX}/ERROR`;

// Action Creators
export const loadData = () => ({
  type: LOAD,
});

export const loadDataSuccess = (data) => ({
  type: SUCCESS,
  payload: data,
});

export const loadDataError = (error) => ({
  type: ERROR,
  payload: error,
});

// Reducer
const initialState = {
  data: null,
  loading: false,
  error: null,
};

export default function reducer(state = initialState, action) {
  switch (action.type) {
    case LOAD:
      return { ...state, loading: true };
    case SUCCESS:
      return { ...state, loading: false, data: action.payload };
    case ERROR:
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
}
```

## Actions and Reducers

### Types of Actions

1. **Synchronous**

   - Direct state updates
   - No side effects required

   ```javascript
   export const updateUI = (payload) => ({
     type: UPDATE_UI,
     payload,
   });
   ```

2. **Asynchronous**
   - API calls
   - Complex operations
   ```javascript
   export const fetchData = () => async (dispatch) => {
     dispatch(loadData());
     try {
       const response = await api.getData();
       dispatch(loadDataSuccess(response));
     } catch (error) {
       dispatch(loadDataError(error));
     }
   };
   ```

### Reducer Structure

```javascript
// Reducer with immer
import { createReducer } from '@reduxjs/toolkit';

const reducer = createReducer(initialState, {
  [ACTION_TYPE]: (state, action) => {
    // Direct mutation allowed by immer
    state.someProperty = action.payload;
  },
});
```

## Middlewares

### Core Middlewares

1. **Redux Thunk**

   - Handling asynchronous actions
   - Side effects

   ```javascript
   const thunkMiddleware = (store) => (next) => (action) => {
     if (typeof action === 'function') {
       return action(store.dispatch, store.getState);
     }
     return next(action);
   };
   ```

2. **Logger**

   - Development and debugging
   - Action traceability

3. **Router Middleware**
   - Synchronization with React Router
   - State-based navigation

### Custom Middlewares

```javascript
// Example of custom middleware
const analyticsMiddleware = (store) => (next) => (action) => {
  if (action.type.startsWith('@analytics')) {
    // Analytics logic
    trackEvent(action.type, action.payload);
  }
  return next(action);
};
```

## Best Practices

### 1. State Organization

- Keep state normalized
- Avoid data duplication
- Use selectors to access state
- Implement cache when necessary

### 2. Performance

```javascript
// Using createSelector for memoization
import { createSelector } from 'reselect';

const selectItems = (state) => state.items;
const selectFilter = (state) => state.filter;

export const selectFilteredItems = createSelector([selectItems, selectFilter], (items, filter) => items.filter((item) => item.type === filter));
```

### 3. Testing

```javascript
// Reducer test
describe('example reducer', () => {
  it('should handle LOAD_DATA_SUCCESS', () => {
    const initialState = { data: null };
    const action = {
      type: LOAD_DATA_SUCCESS,
      payload: { id: 1 },
    };

    expect(reducer(initialState, action)).toEqual({
      data: { id: 1 },
    });
  });
});
```

### 4. Debugging

- Use Redux DevTools
- Implement logging in development
- Handle errors consistently

## Integration with React

### 1. Hooks

```javascript
import { useSelector, useDispatch } from 'react-redux';
import { selectUser } from './selectors';
import { updateUser } from './actions';

function UserProfile() {
  const dispatch = useDispatch();
  const user = useSelector(selectUser);

  const handleUpdate = (data) => {
    dispatch(updateUser(data));
  };
}
```

### 2. Connecting Components

```javascript
// Example usage with hooks
function ConnectedComponent() {
  const data = useSelector((state) => state.example.data);
  const dispatch = useDispatch();

  useEffect(() => {
    dispatch(loadData());
  }, [dispatch]);

  return <div>{/* Render data */}</div>;
}
```

### 3. Optimization

- Use `useSelector` carefully
- Implement `useMemo` for expensive calculations
- Avoid unnecessary re-renders

## Guidelines and Conventions

1. **Action Naming**

   - Use format: `@domain/ACTION_TYPE`
   - Descriptive and unique
   - Follow pattern: REQUEST/SUCCESS/ERROR

2. **State**

   - Keep it flat
   - Normalize relational data
   - Document structure

3. **Side Effects**

   - Handle in thunks or sagas
   - Centralize asynchronous logic
   - Handle errors consistently

4. **Performance**
   - Implement memoization
   - Optimize selectors
   - Monitor re-renders
