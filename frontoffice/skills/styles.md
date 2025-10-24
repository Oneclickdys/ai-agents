---
name: styles
description: Follow these rules when working with the styles of the components
---

# Tangerine Frontend Style System Documentation

## 📁 Style System Overview

The Tangerine style system is a comprehensive SCSS architecture organized into a modular structure that provides a consistent design language across the application. This documentation will help you understand how to effectively use and apply the styling system.

## 🧩 Directory Structure

```
src/_core/style/
├── _index.scss       # Main entry point that imports all other styles
├── _fonts.scss       # Font face definitions
├── base/             # Foundation styles
│   ├── reset/        # Browser normalization
│   ├── variables/    # Design tokens
│   ├── variables-elements/ # Element-specific variables
│   └── classess/     # Utility classes
├── layout/           # Layout components
└── modules/          # Component styles
    ├── components/   # Individual UI components
    └── compositions/ # Complex UI compositions
```

## 🎨 Design Tokens

### Colors

Colors are defined in `base/variables/_colors.scss` and follow a structured system:

```scss
// Primary color (from CSS variables)
$color-first: var(--color-first) !default;
$color-first-alpha: var(--color-first-alpha) !default;

// Base colors
$color-white: #ffff !default;
$color-black: #000000 !default;

// Gray scale
$color-gray-01: #333333 !default; // Darkest
$color-gray-02: #4e4e4e !default;
// ... through ...
$color-gray-07: var(--color-gray-07) !default; // Lightest

// Background colors
$color-bg-01: var(--color-bg-01) !default;
// ... through ...
$color-bg-05: var(--color-bg-05) !default;

// Status colors
$color-error: #f66868 !default;
$color-alert: #f2aa3f !default;
$color-success: #40d158 !default;
$color-link: #4991e5 !default;
$color-info: #4991e5 !default;

// Secondary palette
// Various secondary colors with different shades
```

**Usage example:**

```scss
.my-element {
  color: $color-first;
  background-color: $color-bg-01;
  border: 1px solid $color-gray-05;
}
```

### Typography

Typography variables are defined in `base/variables/_typography.scss`:

```scss
// Font families
$font-first: var(--font-first);
$font-first-semi: var(--font-first-semi);
$font-second: var(--font-second);
$font-second-bold: var(--font-second-bold);

// Font sizes (from largest to smallest)
$font-size-01: 48px; // Largest
$font-size-02: 36px;
// ... through ...
$font-size-11: 10px; // Smallest
```

**Usage example:**

```scss
.my-heading {
  font-family: $font-first;
  font-size: $font-size-02;
}
```

### Other Variables

Additional design tokens include:

- **Breakpoints** (`_breakpoints.scss`): Media query breakpoints
- **Z-index** (`_z-index.scss`): Z-index layers management
- **Shadows** (`_shadow.scss`): Box-shadow definitions
- **Borders** (`_border.scss`): Border styles
- **Transitions** (`_transition.scss`): Animation transitions
- **Grid** (`_grid.scss`): Grid system variables

## 🧰 Utility Classes

The system provides pre-built utility classes in `base/classess/`:

### Typography Classes

```scss
.title-h1 {
  font-family: $font-first;
  font-size: $font-size-01;
  line-height: 61px;
}

.title-h2 {
  font-family: $font-first;
  font-size: $font-size-02;
  line-height: 46px;
}
// ... and so on
```

### Other Utility Classes

- **Border utilities** (`_borders.scss`): Border-related classes
- **Card styles** (`_card.scss`): Card styling utilities
- **Grid utilities** (`_grid.scss`): Grid layout helpers
- **Transitions** (`_transition.scss`): Animation classes
- **General utilities** (`_utils.scss`): Miscellaneous helpers

## 📏 Layout System

The layout system in `layout/` provides consistent page structures:

- **Default layout** (`_layout-default.scss`): Base layout
- **Header layout** (`_layout-header.scss`): Header styling
- **Progress layout** (`_layout-progress.scss`): Progress indicators
- **Login layout** (`_layout-login.scss`): Login page styling
- **Layout with image** (`_layout-with-image.scss`): Layouts with background images
- **Calendar layout** (`layout-calendar/`): Calendar-specific layouts

## 🧠 Best Practices for Using the Style System

1. **Always use variables instead of hardcoded values**

   ```scss
   // Good
   color: $color-first;

   // Bad
   color: #ff5322;
   ```

2. **Leverage existing utility classes**

   ```scss
   // Good
   <div class="title-h2">Heading</div>

   // Instead of creating custom CSS for common patterns
   ```

3. **Follow the modular structure**

   - Put component styles in the appropriate modules directory
   - Keep layouts in the layout directory
   - Add new variables to the appropriate variables file

4. **Use the reset styles**

   - The reset styles provide a consistent base across browsers
   - Don't override these unless absolutely necessary

5. **Responsive design with breakpoints**
   ```scss
   @media screen and (min-width: $breakpoint-tablet) {
     // Tablet and above styles
   }
   ```

## 🔄 How to Extend the System

1. **Adding new variables**

   - Add to the appropriate file in `base/variables/`
   - Follow the naming convention (e.g., `$color-new-variant`)
   - Use the `!default` flag to allow overrides

2. **Creating new components**

   - Add component styles to `modules/components/`
   - Use existing variables and utility classes
   - Follow BEM naming convention

3. **Adding new utility classes**
   - Add to the appropriate file in `base/classess/`
   - Keep them simple and focused on one responsibility
   - Document their purpose with comments

## 💡 Tips for AI Code Generation

When generating styles for this project:

1. Always import variables from the appropriate files instead of using hardcoded values
2. Reference the existing class structure for consistency
3. Use the existing layout components rather than creating custom layouts
4. Leverage utility classes instead of writing new styles when possible
5. Follow the modular architecture and put new styles in the appropriate directories
6. Use the CSS variable system with fallbacks: `var(--custom-prop, $fallback)`

By following this style system, you'll maintain consistency across the application and leverage the powerful tools built into the architecture. 😊
