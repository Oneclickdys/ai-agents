# Tangerine Frontoffice - Development Guidelines

You are an expert React frontend developer working on the Tangerine Frontoffice application. This document provides comprehensive guidelines for proper development practices.

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Development Rules](#development-rules)
- [Architecture Guidelines](#architecture-guidelines)
- [Quality Assurance](#quality-assurance)
- [Workflow](#workflow)

---

## Project Overview

### Tech Stack

- **Framework**: React 17 (functional components with hooks)
- **State Management**: Redux + Redux Toolkit (ducks pattern)
- **Routing**: React Router v5
- **Styling**: SASS with design system tokens
- **Testing**: Jest + React Testing Library
- **Code Quality**: ESLint + Prettier
- **Architecture**: Modular Core/App separation with atomic design

### Project Structure

```
tangerine-frontoffice/
├── src/
│   ├── _core/          # Reusable core system (don't modify unless necessary)
│   │   ├── modules/    # Atomic components (atoms/components/compositions)
│   │   ├── store/      # Redux ducks pattern
│   │   ├── style/      # Design system (variables, utilities, layouts)
│   │   ├── hooks/      # Reusable React hooks
│   │   ├── crud/       # API service layer
│   │   └── utils/      # Helper functions
│   └── app/            # Application-specific implementation
│       ├── pages/      # Page-level components
│       ├── modules/    # App-specific components
│       ├── config/     # Environment & feature configs
│       ├── hooks/      # App-specific hooks
│       └── translations/ # i18n (es/en/pt)
├── .cursor/rules/      # Detailed development rules
└── CLAUDE.md          # This file
```

---

## Development Rules

### Core Principles

1. **Read Before Writing**: Always review existing code patterns before implementing new features
2. **Use the Design System**: Never hardcode colors, fonts, or spacing - use SASS variables
3. **Follow Existing Patterns**: Match the architectural patterns already in the codebase
4. **Test Your Code**: Write unit tests for new components and hooks
5. **Document Everything**: Every component needs a README.md
6. **Ask When Uncertain**: Clarify requirements before implementing

### Required Reading

Before any development, familiarize yourself with these files:

- `@.cursor/rules/general.mdc` - Overall architecture and conventions
- `@.cursor/rules/core-components.mdc` - Component development guidelines
- `@.cursor/rules/styles.mdc` - Design system and styling rules
- `@.cursor/rules/global-state-management.mdc` - Redux patterns
- `@.cursor/rules/unit-testing.mdc` - Testing standards
- `@.cursor/rules/crud.mdc` - API integration patterns
- `@.cursor/rules/cursor-rules.mdc` - Tooling configuration

---

## Architecture Guidelines

### Component Development

#### File Structure (MANDATORY)

Every component must follow this structure:

```
ComponentName/
├── ComponentName.js           # Main component (required)
├── useComponentName.js        # Business logic hook (if needed)
├── ComponentName.test.js      # Unit tests (required)
├── _component-name.scss       # Styles (if needed)
└── README.md                  # Documentation (REQUIRED)
```

#### Component Template

```javascript
// ComponentName.js
import React from 'react';
import PropTypes from 'prop-types';
import './_component-name.scss';

/**
 * ComponentName - Brief description of what this component does
 *
 * @param {Object} props - Component props
 * @returns {JSX.Element} Rendered component
 */
function ComponentName({ prop1, prop2, prop3 }) {
  // Component logic here

  return <div className="component-name">{/* JSX here */}</div>;
}

// REQUIRED: Define PropTypes
ComponentName.propTypes = {
  prop1: PropTypes.string.isRequired,
  prop2: PropTypes.func,
  prop3: PropTypes.bool,
};

// REQUIRED: Define defaultProps
ComponentName.defaultProps = {
  prop2: () => {},
  prop3: false,
};

// REQUIRED: Export at the end
export default ComponentName;
```

### Styling Guidelines

#### ALWAYS Use Design System Variables

```scss
// ✅ CORRECT - Use variables
.my-component {
  color: $color-first;
  background-color: $color-bg-01;
  font-family: $font-first;
  font-size: $font-size-05;
  padding: $spacing-medium;
  border-radius: $border-radius-default;
  box-shadow: $shadow-default;
}

// ❌ WRONG - Never hardcode values
.my-component {
  color: #ff5322;
  background-color: #ffffff;
  font-family: 'Arial';
  font-size: 16px;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

#### Import Order

```javascript
// 1. External dependencies
import React from 'react';
import { useDispatch, useSelector } from 'react-redux';
import PropTypes from 'prop-types';

// 2. Core imports (from _core/)
import Button from '_core/modules/atoms/Button';
import Dialog from '_core/modules/components/dialogs/components/Dialog';

// 3. App imports (from app/)
import { selectUser } from 'app/store/user.duck';
import initLayoutConfig from 'app/config/layout/LayoutConfig';

// 4. Styles (always last)
import './_my-component.scss';
```

### State Management

#### Redux Ducks Pattern

Each Redux module follows this structure:

```javascript
// user.duck.js

// 1. Action Types
const PREFIX = '@user';
const LOAD_USER = `${PREFIX}/LOAD`;
const LOAD_USER_SUCCESS = `${PREFIX}/LOAD_SUCCESS`;
const LOAD_USER_ERROR = `${PREFIX}/LOAD_ERROR`;

// 2. Action Creators
export const loadUser = (userId) => ({
  type: LOAD_USER,
  payload: userId,
});

// 3. Async Actions (Thunks)
export const fetchUser = (userId) => async (dispatch) => {
  dispatch(loadUser(userId));
  try {
    const response = await api.getUser(userId);
    dispatch(loadUserSuccess(response));
  } catch (error) {
    dispatch(loadUserError(error));
  }
};

// 4. Selectors
export const selectUser = (state) => state.user.data;
export const selectUserLoading = (state) => state.user.loading;

// 5. Reducer
const initialState = {
  data: null,
  loading: false,
  error: null,
};

export default function reducer(state = initialState, action) {
  switch (action.type) {
    case LOAD_USER:
      return { ...state, loading: true };
    case LOAD_USER_SUCCESS:
      return { ...state, loading: false, data: action.payload };
    case LOAD_USER_ERROR:
      return { ...state, loading: false, error: action.payload };
    default:
      return state;
  }
}
```

### Internationalization (i18n)

```javascript
// Using translations
import { withTranslation } from 'react-i18next';

function MyComponent({ t }) {
  return (
    <div>
      <h1>{t('common:Welcome')}</h1>
      <p>{t('messages:UserGreeting', { name: 'John' })}</p>
    </div>
  );
}

export default withTranslation()(MyComponent);
```

**Translation Files Location:**

- `src/app/translations/es/` (Spanish)
- `src/app/translations/en/` (English)
- `src/app/translations/pt/` (Portuguese)

---

## Quality Assurance

### Pre-Commit Checklist

Before committing any code, ensure:

- [ ] Code follows existing patterns and conventions
- [ ] PropTypes are defined for all components
- [ ] defaultProps are provided where applicable
- [ ] No hardcoded values (colors, fonts, spacing)
- [ ] README.md exists for new components
- [ ] Unit tests written and passing
- [ ] No console errors or warnings
- [ ] ESLint passes with no errors
- [ ] Code is properly formatted (Prettier)
- [ ] Translations added for all user-facing text
- [ ] Responsive design tested (mobile/tablet/desktop)

### Testing Standards

Every component must have tests covering:

1. **Default rendering** - Component renders with default props
2. **Prop variations** - Different prop combinations work correctly
3. **User interactions** - Click, input, hover behaviors
4. **Edge cases** - Error states, loading states, empty states
5. **Accessibility** - Keyboard navigation, ARIA attributes

```javascript
// ComponentName.test.js
import '@testing-library/jest-dom/extend-expect';
import { render, screen, fireEvent } from '@testing-library/react';
import React from 'react';
import ComponentName from '../ComponentName';

describe('ComponentName', () => {
  test('renders correctly with default props', () => {
    render(<ComponentName />);
    expect(screen.getByRole('button')).toBeInTheDocument();
  });

  test('handles click events', () => {
    const mockHandler = jest.fn();
    render(<ComponentName onClick={mockHandler} />);
    fireEvent.click(screen.getByRole('button'));
    expect(mockHandler).toHaveBeenCalledTimes(1);
  });
});
```

### Accessibility Requirements

- ✅ Semantic HTML elements
- ✅ ARIA labels where needed
- ✅ Keyboard navigation support
- ✅ Focus indicators visible
- ✅ Color contrast meets WCAG AA (4.5:1)
- ✅ Alt text for images
- ✅ Form labels associated with inputs

---

## Workflow

### Development Process

#### 1. Planning Phase

Before writing code:

1. **Understand Requirements**: Read the task/issue carefully
2. **Explore Codebase**: Use search tools to find similar implementations
3. **Review Guidelines**: Check relevant rule files
4. **Ask Questions**: Clarify any uncertainties with the user
5. **Plan Approach**: Outline your implementation strategy

#### 2. Implementation Phase

```bash
# 1. Create a feature branch
git checkout -b feature/TAN-1234-feature-name

# 2. Implement following guidelines
# - Use existing components from _core/modules/
# - Follow file structure requirements
# - Use design system variables
# - Write clean, documented code

# 3. Test your implementation
npm run test:core -- ComponentName.test.js
npm run lint
```

#### 3. Visual Verification Phase

**IMMEDIATELY after implementing any frontend change:**

1. **Start dev server** (if not running):

   ```bash
   npm run start
   ```

2. **Navigate to changed pages**:

   ```javascript
   // Use Playwright browser tools
   mcp__playwright__browser_navigate({ url: 'http://localhost:3000/your-page' });
   ```

3. **Take screenshots** at multiple viewports:

   - Desktop: 1440px width
   - Tablet: 768px width
   - Mobile: 375px width

4. **Check console for errors**:

   ```javascript
   mcp__playwright__browser_console_messages({ onlyErrors: true });
   ```

5. **Verify design compliance**:

   - Colors match design system
   - Typography uses correct variables
   - Spacing is consistent
   - Responsive behavior works
   - Accessibility is maintained

6. **Test interactions**:
   - Click all buttons
   - Fill all forms
   - Test keyboard navigation
   - Verify loading states
   - Test error states

#### 4. Code Review Phase

Before requesting review:

- [ ] Run all tests: `npm run test`
- [ ] Check linting: `npm run lint`
- [ ] Review your own code for quality
- [ ] Ensure all files are properly committed
- [ ] Update documentation if needed
- [ ] Verify no debug code or console.logs remain

#### 5. Documentation Phase

Update or create:

- Component README.md with usage examples
- CHANGELOG.md if significant changes
- Update main documentation if architecture changed

### Common Commands

```bash
# Development
npm run start                    # Start dev server
npm run build                    # Production build
npm run lint                     # Run ESLint
npm run lint:fix                 # Auto-fix linting issues
npm run format                   # Run Prettier

# Testing
npm run test                     # Run all tests
npm run test:watch              # Run tests in watch mode
npm run test:core               # Run core component tests
npm run test:coverage           # Generate coverage report

# Git Workflow
git status                      # Check status
git add .                       # Stage changes
git commit -m "feat: description"  # Commit with message
git push origin branch-name     # Push to remote
```

---

## Important Reminders

### DO's ✅

- ✅ Use functional components with hooks (not classes)
- ✅ Use `function` keyword (not arrow functions for components)
- ✅ Use `useMemo` to memoized values and avoid innecesary re-renders. But only use it when is necessary. Analize the scenario before use it
- ✅ Use `useCallback` to memoized function and avoid innecesary re-renders in childrens who receive the function in props. But only use it when is necessary. Analize the scenario before use it
- ✅ Export default at the end of file
- ✅ Sort props alphabetically
- ✅ Define PropTypes and defaultProps
- ✅ Use design system variables exclusively
- ✅ Write README.md for every component
- ✅ Write unit tests for new code
- ✅ Handle loading and error states
- ✅ Make components responsive
- ✅ Add proper ARIA labels
- ✅ Use translations (never hardcode text)
- ✅ Follow BEM naming for CSS classes
- ✅ Import from `_core/` for reusable components
- ✅ Check console for errors after changes

### DON'Ts ❌

- ❌ Don't use class components
- ❌ Don't use arrow functions for component definitions
- ❌ Don't hardcode colors, fonts, or spacing
- ❌ Don't skip PropTypes or defaultProps
- ❌ Don't skip tests
- ❌ Don't modify \_core/ unless absolutely necessary
- ❌ Don't commit console.log statements
- ❌ Don't ignore ESLint warnings
- ❌ Don't skip documentation
- ❌ Don't use inline styles
- ❌ Don't duplicate code (DRY principle)
- ❌ Don't ignore accessibility
- ❌ Don't hardcode user-facing strings

---

## Getting Help

If you're unsure about:

- **Architecture decisions**: Review @.cursor/rules/general.mdc
- **Component patterns**: Check @.cursor/rules/core-components.mdc
- **Styling questions**: See @.cursor/rules/styles.mdc
- **State management**: Read @.cursor/rules/global-state-management.mdc
- **Testing approach**: Consult @.cursor/rules/unit-testing.mdc
- **API integration**: Review @.cursor/rules/crud.mdc

**Always ask the user when you have doubts or need clarification!**

---

## Summary

This is a professional React application with established patterns and standards. Your role is to:

1. **Understand** the existing architecture before making changes
2. **Follow** the established patterns and conventions
3. **Use** the design system and existing components
4. **Test** your code thoroughly
5. **Document** what you create
6. **Verify** changes visually and functionally

Success comes from respecting the existing codebase structure and maintaining consistency across all implementations.
