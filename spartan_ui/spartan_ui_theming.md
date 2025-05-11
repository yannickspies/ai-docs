# Spartan UI Theming

This document explains how theming works in Spartan UI and how to customize the appearance of your application.

## Theming Approach

Spartan UI uses CSS variables for theming, following a simple `background` and `foreground` convention for colors. This approach makes it easy to create consistent themes across your application and to switch between different themes (like light and dark modes).

## Background and Foreground Convention

In Spartan UI's theming system:

- The `background` variable is used for the background color of a component
- The `foreground` variable is used for the text color of a component

Example usage:

```html
<div class="bg-background text-foreground">Content with base theme colors</div>
<div class="bg-primary text-primary-foreground">Content with primary colors</div>
```

## CSS Variables

Spartan UI defines its colors as HSL values in CSS variables:

```css
--primary: 222.2 47.4% 11.2%;
--primary-foreground: 210 40% 98%;
```

When used in CSS, these variables are referenced with the HSL function:

```css
background-color: hsl(var(--primary));
color: hsl(var(--primary-foreground));
```

In Tailwind classes, they're referenced like this:

```html
<div class="bg-primary text-primary-foreground">Hello</div>
```

## Default Theme Variables

Here's the complete list of variables used in Spartan UI:

### Base Colors
Base background and text colors:
```css
--background: 0 0% 100%;
--foreground: 240 10% 3.9%;
```

### Muted Elements
Used for less prominent UI elements:
```css
--muted: 240 4.8% 95.9%;
--muted-foreground: 240 3.8% 46.1%;
```

### Card Component
Colors for card components:
```css
--card: 0 0% 100%;
--card-foreground: 240 10% 3.9%;
```

### Popover Elements
Colors for popover/dropdown elements:
```css
--popover: 0 0% 100%;
--popover-foreground: 240 10% 3.9%;
```

### Border Colors
Colors for borders and dividers:
```css
--border: 240 5.9% 90%;
```

### Input Elements
Colors for form inputs:
```css
--input: 240 5.9% 90%;
```

### Primary Elements
Used for primary actions and highlights:
```css
--primary: 240 5.9% 10%;
--primary-foreground: 0 0% 98%;
```

### Secondary Elements
Used for secondary actions:
```css
--secondary: 240 4.8% 95.9%;
--secondary-foreground: 240 5.9% 10%;
```

### Accent Elements
Used for accents and hover states:
```css
--accent: 240 4.8% 95.9%;
--accent-foreground: 240 5.9% 10%;
```

### Destructive Elements
Used for destructive actions:
```css
--destructive: 0 84.2% 60.2%;
--destructive-foreground: 0 0% 98%;
```

### Focus Ring
Used for focus indicators:
```css
--ring: 240 5.9% 10%;
```

### Border Radius
Default border radius for components:
```css
--radius: 0.5rem;
```

## Dark Mode Variables

Spartan UI includes a set of variables for dark mode, applied when the `dark` class is present on the `html` element:

```css
.dark {
  --background: 240 10% 3.9%;
  --foreground: 0 0% 98%;
  --card: 240 10% 3.9%;
  --card-foreground: 0 0% 98%;
  --popover: 240 10% 3.9%;
  --popover-foreground: 0 0% 98%;
  --primary: 0 0% 98%;
  --primary-foreground: 240 5.9% 10%;
  --secondary: 240 3.7% 15.9%;
  --secondary-foreground: 0 0% 98%;
  --muted: 240 3.7% 15.9%;
  --muted-foreground: 240 5% 64.9%;
  --accent: 240 3.7% 15.9%;
  --accent-foreground: 0 0% 98%;
  --destructive: 0 62.8% 30.6%;
  --destructive-foreground: 0 0% 98%;
  --border: 240 3.7% 15.9%;
  --input: 240 3.7% 15.9%;
  --ring: 240 4.9% 83.9%;
  color-scheme: dark;
}
```

## Customizing the Theme

### Modifying Existing Variables

To modify the default theme, simply override the CSS variables in your global styles:

```css
:root {
  /* Change primary color to blue */
  --primary: 210 100% 50%;
  --primary-foreground: 210 100% 98%;
  
  /* Change border radius */
  --radius: 0.25rem;
}

.dark {
  /* Adjust dark mode primary color */
  --primary: 210 100% 40%;
  --primary-foreground: 210 100% 98%;
}
```

### Adding New Colors

To add new colors to your theme:

1. Add the new CSS variables to your style file:

```css
:root {
  /* Existing variables... */
  
  /* New warning color */
  --warning: 38 92% 50%;
  --warning-foreground: 48 96% 89%;
}

.dark {
  /* Existing dark mode variables... */
  
  /* Dark mode warning color */
  --warning: 48 96% 89%;
  --warning-foreground: 38 92% 50%;
}
```

2. Add the new colors to your Tailwind configuration:

```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        warning: "hsl(var(--warning))",
        "warning-foreground": "hsl(var(--warning-foreground))",
      },
    },
  },
}
```

Now you can use the new color with Tailwind classes:

```html
<div class="bg-warning text-warning-foreground">Warning message</div>
```

### Creating Multiple Themes

You can create multiple themes by defining different sets of variables and applying them with CSS classes:

```css
:root {
  /* Default theme variables... */
}

.dark {
  /* Dark theme variables... */
}

.brand-theme {
  --primary: 260 100% 50%;
  --primary-foreground: 260 100% 98%;
  /* Other theme variables... */
}
```

Then apply the theme class to your root or container element:

```html
<div class="brand-theme">
  <button class="bg-primary text-primary-foreground">Branded Button</button>
</div>
```

## Using Other Color Formats

While HSL is recommended for its readability and ease of adjustment, you can use other color formats:

### RGB Format

In your CSS:
```css
:root {
  --primary-rgb: 25, 30, 150;
}
```

In your Tailwind config:
```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: "rgb(var(--primary-rgb))",
      },
    },
  },
}
```

### Hex Format

Direct in your Tailwind config:
```javascript
module.exports = {
  theme: {
    extend: {
      colors: {
        custom: "#1e40af",
      },
    },
  },
}
```

## Best Practices

1. **Maintain Contrast**: Ensure sufficient contrast between background and foreground colors for accessibility.
2. **Use Semantic Colors**: Use theme colors according to their semantic meaning (primary for primary actions, destructive for delete actions, etc.).
3. **Test in Both Modes**: Always test your custom theme in both light and dark modes.
4. **Limit Custom Colors**: Try to work within the existing color system and only add new colors when necessary.
5. **Document Custom Colors**: If you add custom colors, document their intended use to maintain consistency.

## Additional Resources

- [Official Spartan UI Theming Documentation](https://www.spartan.ng/documentation/theming)
- [HSL Colors in CSS](https://www.smashingmagazine.com/2021/07/hsl-colors-css/)
- [TailwindCSS Color Customization](https://tailwindcss.com/docs/customizing-colors#using-css-variables)