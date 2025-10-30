---
name: frontend-testing
description: Frontend testing patterns using Jest and React Testing Library for the frontoffice application. Use when writing tests for React components, hooks, or utility functions.
---

# Frontend Testing

## When to Use

- Testing React components
- Testing custom hooks
- Testing utility functions
- Ensuring component behavior

## Testing Framework

- **Jest**: Test runner and assertions
- **React Testing Library**: Component testing
- **Mock Service Worker**: API mocking

## Component Testing Pattern

```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import ComponentName from './ComponentName';

describe('ComponentName', () => {
  it('should render correctly', () => {
    render(<ComponentName prop="value" />);
    expect(screen.getByText('Expected Text')).toBeInTheDocument();
  });

  it('should handle user interaction', () => {
    const onAction = jest.fn();
    render(<ComponentName onAction={onAction} />);

    fireEvent.click(screen.getByRole('button'));
    expect(onAction).toHaveBeenCalled();
  });
});
```

## Testing Best Practices

### Query Priority
1. `getByRole` - Accessibility first
2. `getByLabelText` - Form elements
3. `getByText` - User-visible text
4. `getByTestId` - Last resort

### What to Test
- ✅ User interactions
- ✅ Conditional rendering
- ✅ Props handling
- ✅ State changes
- ✅ Integration with hooks

### What NOT to Test
- ❌ Implementation details
- ❌ Third-party library code
- ❌ CSS/styling specifics

## Async Testing

```jsx
it('should handle async operations', async () => {
  render(<Component />);

  const element = await screen.findByText('Loaded Data');
  expect(element).toBeInTheDocument();
});
```

## Mocking

```jsx
jest.mock('@core/crud/users.crud', () => ({
  getUsers: jest.fn(() => Promise.resolve([...])),
}));
```
