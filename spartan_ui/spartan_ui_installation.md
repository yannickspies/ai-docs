# Spartan UI Installation

This document provides detailed instructions for installing and setting up Spartan UI for Angular.

## Prerequisites

Before installing Spartan UI, ensure your project has:

- **TailwindCSS**: Spartan UI is built on top of TailwindCSS. Make sure your application has a working TailwindCSS setup. [Tailwind installation instructions for Angular](https://tailwindcss.com/docs/installation/framework-guides/angular)
- **Angular CDK**: Spartan UI builds on top of Angular CDK: 
  ```bash
  npm i @angular/cdk
  ```

## Step 1: Install Spartan CLI

Spartan UI provides a CLI for easier installation:

```bash
npm i -D @spartan-ng/cli
```

## Step 2: Set Up Tailwind Configuration

Add Spartan-specific configuration to your TailwindCSS setup using the preset from the `@spartan-ng/brain` package.

### For Tailwind 3

Add to your `tailwind.config.js` file:

```javascript
const { createGlobPatternsForDependencies } = require('@nx/angular/tailwind');
const { join } = require('path');

/** @type {import('tailwindcss').Config} */
module.exports = {
  presets: [require('@spartan-ng/brain/hlm-tailwind-preset')],
  content: [
    join(__dirname, 'src/**/!(*.stories|*.spec).{ts,html}'),
    ...createGlobPatternsForDependencies(__dirname),
  ],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

### For Tailwind 4

Add to your global stylesheet:

```css
@import '@spartan-ng/brain/hlm-tailwind-preset.css';
```

## Step 3: Add CSS Variables

Add Spartan-specific CSS variables to your style entry point (typically `styles.css`).

### Using Nx

If you're using Nx, run:

```bash
npx nx g @spartan-ng/cli:ui-theme
```

### Manual Addition

If you're not using Nx, manually add these variables to your style entry point:

```css
:root {
  --font-sans: '';
  --background: 0 0% 100%;
  --foreground: 240 10% 3.9%;
  --card: 0 0% 100%;
  --card-foreground: 240 10% 3.9%;
  --popover: 0 0% 100%;
  --popover-foreground: 240 10% 3.9%;
  --primary: 240 5.9% 10%;
  --primary-foreground: 0 0% 98%;
  --secondary: 240 4.8% 95.9%;
  --secondary-foreground: 240 5.9% 10%;
  --muted: 240 4.8% 95.9%;
  --muted-foreground: 240 3.8% 46.1%;
  --accent: 240 4.8% 95.9%;
  --accent-foreground: 240 5.9% 10%;
  --destructive: 0 84.2% 60.2%;
  --destructive-foreground: 0 0% 98%;
  --border: 240 5.9% 90%;
  --input: 240 5.9% 90%;
  --ring: 240 5.9% 10%;
  --radius: 0.5rem;
  color-scheme: light;
}

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

@layer base {
  * {
    @apply border-border;
  }
}
```

Don't forget to import the Angular CDK overlay styles:

```css
@import '@angular/cdk/overlay-prebuilt.css';
```

## Step 4: Add UI Components

### Using Nx

With the Nx plugin, adding primitives is simple:

```bash
npx nx g @spartan-ng/cli:ui
```

This command allows you to pick and choose which primitives to add to your project. It will add all brain dependencies and copy helm code into its own library.

### Manually

If you're not using Nx, you'll need to manually install each component you want to use. The specific installation commands will depend on which components you need.

## Step 5: Verify Installation

After completing the installation steps, you can verify your setup by using a basic Spartan UI component in your application.

For example, add a button component to your template:

```html
<button hlmBtn variant="primary">Click Me</button>
```

If you don't see any errors and the button appears with the correct styling, your installation is successful.

## Troubleshooting

- **Tailwind Classes Not Applied**: Make sure your Tailwind configuration is correctly set up and that Spartan preset is properly imported.
- **Component Not Found**: Ensure you've properly installed and imported the specific component you're trying to use.
- **CSS Variables Not Working**: Check that your CSS variables are properly defined in your styles entry point and that the file is being loaded in your application.

## Additional Resources

- [Official Spartan UI Documentation](https://www.spartan.ng/documentation/installation)
- [Spartan UI CLI Documentation](https://www.spartan.ng/documentation/cli)