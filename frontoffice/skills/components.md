---
name: component-creator
description: Rules to create or modify a module component.
---

# Core

The core of the system represents the fundamental base of the application, providing reusable components, utilities, and logic that can be leveraged by any specific implementation.

## Base Modules

### Module Structure

```
src/_core/modules/
├── atoms/          # Basic atomic components
├── components/     # Composite components
├── compositions/   # Complex compositions
├── containers/     # Containers and layouts
├── skeletons/      # Loading components
└── wrappers/       # HOCs and wrappers
```

### Types of Modules

1. **Atoms**

   - Basic UI components (buttons, inputs, etc.)
   - Highly reusable
   - No business logic
   - Examples: Button, Input, Select, Icon

2. **Components**

   - Combination of atoms
   - Specific functionality
   - Encapsulated logic
   - Examples: Forms, Cards, Modals

3. **Compositions**

   - Complex components
   - Combination of multiple components
   - Shared business logic
   - Examples: DataGrid, Wizards

4. **Containers**

   - Layout management
   - State management
   - Routing and navigation
   - Examples: Layout, PageContainer

5. **Skeletons**

   - Loading states
   - Placeholders
   - Loading animations

6. **Wrappers**
   - HOCs (Higher Order Components)
   - Providers
   - Context Wrappers

## Reusable Components

### Atoms and Molecules

#### Base Atoms

- **Buttons**

  - Primary
  - Secondary
  - Icon Button
  - Link Button

- **Inputs**

  - Text Input
  - Number Input
  - Date Input
  - Search Input

- **Select**

  - Basic Select
  - Multi Select
  - Searchable Select
  - Custom Select

- **Display**
  - Typography
  - Icons
  - Labels
  - Badges

#### Molecules

- **Form Controls**

  - Input Groups
  - Form Fields
  - Validation Messages
  - Field Arrays

- **Data Display**
  - Cards
  - Lists
  - Tables
  - Panels

## Best Practices

1. **Component Development**

   - Keep components small and focused
   - Document props and behaviors
   - Maintain style consistency

2. **Reusability**

   - Favor composition over inheritance
   - Create generic and extensible components
   - Keep business logic separate
   - Document use cases

3. **Performance**

   - Implement lazy loading
   - Optimize renders
   - Minimize dependencies
   - Use memo when necessary

4. **Maintenance**
   - Follow naming conventions
   - Keep documentation updated
   - Conduct code reviews
   - Regularly update dependencies

## Implementation Guidelines

### Creating New Components

1. **Structure**

   ```
   ComponentName/
   ├── ComponentName.js
   ├── useComponentName.js (if the component manage business logic)
   ├── ComponentName.scss
   └── README.md (ALWAYS CREATE THIS FILE)
   ```

2. **Implementation**

   ```javascript
   import React from 'react';
   import PropTypes from 'prop-types';
   import './ComponentName.scss';

   function ComponentName({ prop1, prop2 }) {
     return <div className="component-name">{/* Implementation */}</div>;
   }

   ComponentName.propTypes = {
     prop1: PropTypes.string.isRequired,
     prop2: PropTypes.func,
   };

   ComponentName.defaultProps = {
     prop1: '',
     prop2: function () {},
   };

   export default ComponentName;
   ```

3. **Documentation**

   ````markdown
   # ComponentName

   Brief description of the component.

   ## Props

   | Prop  | Type     | Default | Description          |
   | ----- | -------- | ------- | -------------------- |
   | prop1 | string   | -       | Description of prop1 |
   | prop2 | function | -       | Description of prop2 |

   ## Usage Examples

   \```javascript
   <ComponentName
   prop1="value"
   prop2={() => {}}
   />
   \```
   ````

### Dialog System

The dialog system provides a consistent interface for displaying modals and popups in the application.

#### Types of Dialogs

1. **Base Dialog**
   - Base structure for all dialogs
   - Header management with title and icon
   - Support for tabs in the header
   - Configurable footer with action buttons
   - Different predefined sizes
   - Support for custom content

#### Base Dialog Props

```javascript
DialogDefault.propTypes = {
  // Button configuration
  buttonIconSucess: PropTypes.string, // Icon for the success button
  buttonTextCancel: PropTypes.string, // Text for the cancel button
  buttonTextSucess: PropTypes.string, // Text for the success button

  // Content and styles
  children: PropTypes.node, // Dialog content
  className: PropTypes.string, // Additional CSS class
  customFooterLeft: PropTypes.node, // Custom content in the left footer
  customHeader: PropTypes.node, // Custom header

  // States
  disabledSuccess: PropTypes.bool, // Disables success button
  footerWithBackground: PropTypes.bool, // Adds background to the footer
  hideHeader: PropTypes.bool, // Hides the entire header

  // Header
  iconTitle: PropTypes.string, // Icon next to the title
  title: PropTypes.oneOfType([
    // Dialog title
    PropTypes.string,
    PropTypes.object,
  ]),
  text: PropTypes.string, // Descriptive text under the title

  // Events
  onCancel: PropTypes.func, // Function on cancel
  onClose: PropTypes.func, // Function on close
  onSuccess: PropTypes.func, // Function on confirm

  // Control
  open: PropTypes.bool, // Controls dialog visibility

  // Style and layout
  separator: PropTypes.bool, // Shows separator under the header
  showHeader: PropTypes.bool, // Shows/hides header
  size: PropTypes.oneOf([
    // Dialog size
    'default',
    'xxs',
    'xs',
    's',
    'small',
    'xl',
    'xxl',
    'all-screen',
  ]),

  // Tabs
  tabActive: PropTypes.number, // Active tab
  tabs: PropTypes.array, // Tab configuration
  onChangeTab: PropTypes.func, // Function on tab change
};
```

#### Usage Example

```javascript
import Dialog from '_core/modules/components/dialogs/components/Dialog';

function MyComponent() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <Dialog
      open={isOpen}
      onClose={() => setIsOpen(false)}
      title="Dialog Title"
      iconTitle="icon-name"
      text="Optional descriptive text"
      buttonTextCancel={t('actions:Cancel')}
      buttonTextSucess={t('actions:Confirm')}
      onSuccess={() => {
        // Action on confirm
        setIsOpen(false);
      }}
      onCancel={() => setIsOpen(false)}
      size="default"
      separator
    >
      <div>Dialog content</div>
    </Dialog>
  );
}
```

#### Common Variants

1. **Dialog with Tabs**

```javascript
<Dialog
  open={isOpen}
  title="Dialog with Tabs"
  tabs=[
    { id: 1, label: 'Tab 1' },
    { id: 2, label: 'Tab 2' },
  ]
  tabActive={activeTab}
  onChangeTab={handleTabChange}
>
  {/* Content according to active tab */}
</Dialog>
```

2. **Dialog with Custom Footer**

```javascript
<Dialog open={isOpen} title="Dialog with Custom Footer" customFooterLeft={<div>Left content</div>} buttonTextSucess="Save" onSuccess={handleSave}>
  {/* Dialog content */}
</Dialog>
```

3. **Simple Confirmation Dialog**

```javascript
<Dialog
  open={isOpen}
  title="Confirm Action"
  text="Are you sure you want to continue?"
  buttonTextCancel="No"
  buttonTextSucess="Yes"
  onSuccess={handleConfirm}
  onCancel={handleCancel}
  size="xs"
/>
```

### Integration with the Application

1. Import components from the core
2. Extend functionality as needed
3. Maintain style consistency
4. Document customizations

## Additional Resources

- Storybook for component documentation
- Style and design guides
- Implementation examples
