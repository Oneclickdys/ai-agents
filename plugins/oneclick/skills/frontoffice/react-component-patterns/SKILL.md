---
name: react-component-patterns
description: React component architecture and patterns for the frontoffice application. Use when creating or modifying React components, understanding component structure, or implementing UI features.
---

# React Component Patterns

## When to Use

- Creating new React components
- Understanding component structure
- Implementing UI features
- Following established patterns

## Module Structure

```
src/_core/modules/
├── atoms/          # Basic UI components (buttons, inputs)
├── components/     # Composite components (forms, cards)
├── compositions/   # Complex compositions (data grids, wizards)
├── containers/     # Containers and layouts
├── skeletons/      # Loading components
└── wrappers/       # HOCs and wrappers
```

## Component Types

### Atoms
- Basic UI elements
- Highly reusable
- No business logic
- Examples: Button, Input, Select

### Components
- Combination of atoms
- Specific functionality
- Encapsulated logic
- Examples: Forms, Cards, Modals

### Compositions
- Complex components
- Multiple component combinations
- Shared business logic
- Examples: DataGrid, Wizards

## Standard Component Pattern

```jsx
import React from 'react';
import PropTypes from 'prop-types';

const ComponentName = ({ prop1, prop2, onAction }) => {
  return (
    <div className="component-name">
      {/* Component content */}
    </div>
  );
};

ComponentName.propTypes = {
  prop1: PropTypes.string.isRequired,
  prop2: PropTypes.number,
  onAction: PropTypes.func,
};

export default ComponentName;
```

## Best Practices

1. **Keep components focused** - Single responsibility
2. **Use PropTypes** - Type checking
3. **Destructure props** - Clearer code
4. **Avoid inline functions** - Performance
5. **Use meaningful names** - Descriptive and clear
6. **Extract reusable logic** - Custom hooks
