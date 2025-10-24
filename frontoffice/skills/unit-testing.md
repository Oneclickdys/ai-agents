---
name: unit-test
description: standards for creating unit tests for components in the application.
---

# Unit Testing Guidelines for Core Components

This guide establishes standards for creating unit tests for core components in the Tangerine application.

## Directory Structure

```
ComponentName/
├── ComponentName.js      # Main component
├── _component-name.scss  # Styles
└── __tests__/           # Test directory
    ├── ComponentName.test.js  # Test file
    └── README.md              # Test documentation
```

## Test File Structure

Each test file should follow this general structure:

```javascript
/**
 * @jest-environment jsdom
 */
import '@testing-library/jest-dom/extend-expect';
import { render, screen, fireEvent } from '@testing-library/react';
import React from 'react';
import ComponentName from '../ComponentName';

// Any required mocks go here, before importing the component

describe('ComponentName Component', () => {
  // Test basic rendering with default props
  test('renders correctly with default props', () => {
    render(<ComponentName />);
    // Assertions for default state
  });

  // Test prop variations
  test('handles custom props correctly', () => {
    render(<ComponentName customProp="value" />);
    // Assertions for custom props
  });

  // Test user interactions
  test('handles user interactions', () => {
    render(<ComponentName onClick={jest.fn()} />);
    // Trigger events and check results
  });

  // Test variations of the component
  test('renders different variations', () => {
    const variations = ['option1', 'option2', 'option3'];

    variations.forEach((variation) => {
      const { unmount } = render(<ComponentName variation={variation} />);
      // Assertions for this variation
      unmount(); // Clean up between iterations
    });
  });
});
```

## Mocking Strategy

### Simple Mocks

For simple dependencies:

```javascript
jest.mock('dependency-name', () => ({
  someFunction: jest.fn(() => 'mocked result'),
}));
```

### Complex Components

For components that require complex mocking (like those using `require.context`):

```javascript
jest.mock('../ComponentName', () => {
  return function MockComponent(props) {
    // Return a simple element that mimics the original component's structure
    return (
      <div data-testid="mock-component" className={`component ${props.variant ? `component--${props.variant}` : ''}`}>
        {props.children}
      </div>
    );
  };
});
```

### External Modules

When mocking external modules that require context:

```javascript
// For modules that use require.context or import.meta
const originalRequireContext = require.context;
require.context = jest.fn(() => {
  const context = (module) => mockResult;
  context.keys = () => ['./mock1.ext', './mock2.ext'];
  return context;
});

// Restore after tests
afterAll(() => {
  if (typeof originalRequireContext !== 'undefined') {
    require.context = originalRequireContext;
  }
});
```

## Test Assertions

Use these assertion patterns:

1. **Rendering tests**:

   ```javascript
   const element = screen.getByTestId('element-id');
   expect(element).toBeInTheDocument();
   ```

2. **Class presence**:

   ```javascript
   expect(element).toHaveClass('expected-class');
   ```

3. **Attributes**:

   ```javascript
   expect(element).toHaveAttribute('attributeName', 'expectedValue');
   ```

4. **Content**:

   ```javascript
   expect(element).toHaveTextContent('Expected text');
   ```

5. **Event handling**:
   ```javascript
   const mockHandler = jest.fn();
   render(<Component onClick={mockHandler} />);
   fireEvent.click(screen.getByRole('button'));
   expect(mockHandler).toHaveBeenCalledTimes(1);
   ```

## Required Test Coverage

Each component should test:

1. **Default rendering** - How the component renders with default props
2. **Prop variations** - Test all meaningful prop combinations
3. **User interactions** - If interactive, test click, hover, focus states
4. **Visual states** - Different sizes, colors, variants, etc.
5. **Error states** - How the component handles invalid props or errors
6. **Accessibility** - Basic a11y tests like keyboard navigation and ARIA attributes

## Test Documentation

Include a README.md in the `__tests__` directory that explains:

1. The testing approach for the component
2. Any special mocking strategies
3. How to run the component's tests
4. Any known limitations of the tests

## Running Tests

Run tests for a specific component:

```bash
npm run test:core -- --testPathPattern=ComponentName.test
```

Run tests with detailed output:

```bash
npm run test:core -- --verbose
```

## Best Practices

1. Test component behavior, not implementation details
2. Use meaningful test and variable names
3. Keep tests independent of each other
4. Mock only what's necessary
5. Clean up after each test
6. Prioritize testing from a user's perspective
7. Use the React Testing Library guiding principle: "The more your tests resemble the way your software is used, the more confidence they can give you."

## Example Test Case

```javascript
/**
 * @jest-environment jsdom
 */
import '@testing-library/jest-dom/extend-expect';
import { render, screen, fireEvent } from '@testing-library/react';
import React from 'react';
import Button from '../Button';

describe('Button Component', () => {
  test('renders correctly with default props', () => {
    render(<Button />);

    const button = screen.getByRole('button');
    expect(button).toBeInTheDocument();
    expect(button).toHaveClass('button--default');
  });

  test('renders with custom text', () => {
    render(<Button text="Custom Button" />);

    const button = screen.getByRole('button');
    expect(button).toHaveTextContent('Custom Button');
  });

  test('handles click events', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick} />);

    const button = screen.getByRole('button');
    fireEvent.click(button);

    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```
