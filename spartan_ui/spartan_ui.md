# Spartan UI for Angular

This document provides comprehensive information about Spartan UI for Angular, a collection of UI primitives that are both beautiful and accessible.

## Overview

Spartan UI is a collection of Angular UI primitives that are both beautiful and accessible. It is built on top of TailwindCSS and provides a set of components that follow best practices for accessibility and user experience.

## Installation

Getting up and running with Spartan UI requires only a couple of steps.

### Prerequisites

- **TailwindCSS**: Spartan UI is built on top of TailwindCSS. Make sure your application has a working TailwindCSS setup before continuing. [Tailwind installation instructions](https://tailwindcss.com/docs/installation/framework-guides/angular)
- **Angular CDK**: Spartan UI builds on top of Angular CDK: `npm i @angular/cdk`

### Installing the CLI

Spartan UI provides a CLI for easier installation:

```bash
npm i -D @spartan-ng/cli
```

### Setting Up Tailwind

Add the Spartan-specific configuration to your TailwindCSS setup using the preset from the `@spartan-ng/brain` package.

For Tailwind 3, add to your config file:

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

For Tailwind 4, add to your global stylesheet:

```css
@import '@spartan-ng/brain/hlm-tailwind-preset.css';
```

### Adding CSS Variables

Add Spartan-specific CSS variables to your style entry point (typically `styles.css`). If using Nx:

```bash
npx nx g @spartan-ng/cli:ui-theme
```

If not using Nx, manually add these variables to your style entry point:

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

### Adding Primitives

With the Nx plugin, adding primitives is simple:

```bash
npx nx g @spartan-ng/cli:ui
```

This command allows you to pick and choose which primitives to add to your project.

## Theming

Spartan UI uses CSS variables for theming, following a simple `background` and `foreground` convention for colors:

```html
<div class="bg-background text-foreground">spartan</div>
```

### Convention

The `background` variable is used for the background color of the component, and the `foreground` variable is used for the text color:

```css
--primary: 222.2 47.4% 11.2%;
--primary-foreground: 210 40% 98%;
```

Using these variables in a component:

```html
<div class="bg-primary text-primary-foreground">Hello</div>
```

### List of Variables

- **Base Colors**:
  ```css
  --background: 0 0% 100%;
  --foreground: 222.2 47.4% 11.2%;
  ```

- **Muted Elements**:
  ```css
  --muted: 210 40% 96.1%;
  --muted-foreground: 215.4 16.3% 46.9%;
  ```

- **Card Component**:
  ```css
  --card: 0 0% 100%;
  --card-foreground: 222.2 47.4% 11.2%;
  ```

- **Popover Elements**:
  ```css
  --popover: 0 0% 100%;
  --popover-foreground: 222.2 47.4% 11.2%;
  ```

- **Border Colors**:
  ```css
  --border: 214.3 31.8% 91.4%
  ```

- **Input Elements**:
  ```css
  --input: 214.3 31.8% 91.4%;
  ```

- **Primary Elements**:
  ```css
  --primary: 222.2 47.4% 11.2%;
  --primary-foreground: 210 40% 98%;
  ```

- **Secondary Elements**:
  ```css
  --secondary: 210 40% 96.1%;
  --secondary-foreground: 222.2 47.4% 11.2%;
  ```

- **Accent Elements**:
  ```css
  --accent: 210 40% 96.1%;
  --accent-foreground: 222.2 47.4% 11.2%;
  ```

- **Destructive Elements**:
  ```css
  --destructive: 0 100% 50%;
  --destructive-foreground: 210 40% 98%;
  ```

- **Focus Ring**:
  ```css
  --ring: 215 20.2% 65.1%;
  ```

- **Border Radius**:
  ```css
  --radius: 0.5rem;
  ```

### Adding New Colors

To add new colors:

1. Add them to your CSS file:
   ```css
   :root {
     --warning: 38 92% 50%;
     --warning-foreground: 48 96% 89%;
   }

   .dark {
     --warning: 48 96% 89%;
     --warning-foreground: 38 92% 50%;
   }
   ```

2. Add them to your `tailwind.config.js` file:
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

## Dark Mode

Spartan UI supports dark mode through Tailwind CSS by toggling a `dark` class on the root element of your page.

### Implementation with Angular

Here's a comprehensive approach to implementing dark mode in Angular:

1. **Add initialization script to `index.html`**:
   This script ensures the appropriate theme is applied before the page renders:

   ```html
   <script>
     if (
       localStorage.theme === 'dark' ||
       (!('theme' in localStorage) &&
         window.matchMedia('(prefers-color-scheme: dark)').matches)
     ) {
       if (document && document.documentElement) {
         document.documentElement.classList.add('dark');
         localStorage.setItem('theme', 'dark');
       }
     } else {
       if (document && document.documentElement) {
         document.documentElement.classList.remove('dark');
         localStorage.setItem('theme', 'light');
       }
     }
   </script>
   ```

2. **Create a ThemeService to manage the theme**:
   
   ```typescript
   @Injectable({
     providedIn: 'root',
   })
   export class ThemeService implements OnDestroy {
     private _platformId = inject(PLATFORM_ID);
     private _renderer = inject(RendererFactory2).createRenderer(null, null);
     private _document = inject(DOCUMENT);

     private _theme$ = new ReplaySubject<'light' | 'dark'>(1);
     public theme$ = this._theme$.asObservable();
     private _destroyed$ = new Subject<void>();

     constructor() {
       this.syncThemeFromLocalStorage();
       this.toggleClassOnThemeChanges();
     }

     private syncThemeFromLocalStorage(): void {
       if (isPlatformBrowser(this._platformId)) {
         this._theme$.next(
           localStorage.getItem('theme') === 'dark' ? 'dark' : 'light'
         );
       }
     }

     private toggleClassOnThemeChanges(): void {
       this.theme$.pipe(takeUntil(this._destroyed$)).subscribe((theme) => {
         if (theme === 'dark') {
           this._renderer.addClass(this._document.documentElement, 'dark');
         } else {
           if (this._document.documentElement.className.includes('dark')) {
             this._renderer.removeClass(this._document.documentElement, 'dark');
           }
         }
       });
     }

     public toggleDarkMode(): void {
       const newTheme =
         localStorage.getItem('theme') === 'dark' ? 'light' : 'dark';
       localStorage.setItem('theme', newTheme);
       this._theme$.next(newTheme);
     }

     public ngOnDestroy(): void {
       this._destroyed$.next();
       this._destroyed$.complete();
     }
   }
   ```

3. **Use the service in your components**:
   
   ```typescript
   @Component({
     selector: 'app-root',
     template: `
       <header class="p-4">
         <button (click)="toggleTheme()">Toggle theme</button>
       </header>
       <main>
         Current theme: {{ theme$ | async }}
       </main>
     `,
     host: {
       class: 'block h-full bg-background text-foreground',
     },
   })
   export class AppComponent {
     private _themeService = inject(ThemeService);
     public theme$ = this._themeService.theme$;

     public toggleTheme(): void {
       this._themeService.toggleDarkMode();
     }
   }
   ```

## Typography

Spartan UI provides styles for headings, paragraphs, lists, and other text elements.

### Installation

```bash
npx nx g @spartan-ng/cli:ui typography
```

### Typography Elements

Spartan UI supports various typography elements:

- **Headings**: h1, h2, h3, h4
- **Paragraphs**: p
- **Blockquotes**: blockquote
- **Lists**: ul, ol, li
- **Inline Code**: code
- **Lead**: For introducing sections with larger text
- **Large**: For emphasizing text
- **Small**: For less important text
- **Muted**: For secondary information

## Available Components

Spartan UI provides a wide range of components:

1. **Accordion**: A vertically stacked set of interactive headings that each reveal a section of content.
2. **Alert**: Displays a callout for user attention.
3. **Alert Dialog**: A modal dialog that interrupts the user with important content.
4. **Aspect Ratio**: Maintains consistent width/height ratios.
5. **Avatar**: An image element with a fallback for representing the user.
6. **Badge**: Small status descriptors.
7. **Breadcrumb**: Navigation aid showing hierarchy.
8. **Button**: Interactive elements for actions.
9. **Calendar**: Displays and selects dates.
10. **Card**: Container for grouping related content.
11. **Carousel**: Cycles through elements with animation.
12. **Checkbox**: Toggle selection state.
13. **Collapsible**: Toggle visibility of content.
14. **Combobox**: Input with a filterable dropdown.
15. **Command**: Keyboard-accessible command menu.
16. **Context Menu**: Menu that appears on right-click.
17. **Data Table**: Display and interact with structured data.
18. **Date Picker**: Interface for selecting dates.
19. **Dialog**: Modal window for focused tasks.
20. **Dropdown Menu**: Toggleable menu for options.
21. **Form**: Manages form input validation and submission.
22. **Form Field**: Wrapper for form inputs.
23. **Hover Card**: Card displayed on hover.
24. **Icon**: Vector graphics for visual aids.
25. **Input**: Text entry field.
26. **Input OTP**: One-time password input.
27. **Label**: Text descriptor for form fields.
28. **Menubar**: Horizontal navigation component.
29. **Navigation Menu**: Hierarchical navigation component.
30. **Pagination**: Interface for navigating pages.
31. **Popover**: Floating content panel.
32. **Progress**: Visual indicator of completion.
33. **Radio Group**: Select one option from a set.
34. **Scroll Area**: Custom scrollable area.
35. **Select**: Dropdown selection component.
36. **Separator**: Horizontal line for separating content.
37. **Sheet**: Side-anchored dialog.
38. **Skeleton**: Loading placeholder.
39. **Slider**: Select a value from a range.
40. **Sonner**: Toast notification manager.
41. **Spinner**: Loading indicator.
42. **Switch**: Toggle between states.
43. **Table**: Display tabular data.
44. **Tabs**: Organize content into separate views.
45. **Textarea**: Multi-line text input.
46. **Toast**: Brief notifications.
47. **Toggle**: Two-state button.
48. **Toggle Group**: Group of toggle buttons.
49. **Tooltip**: Short information on hover.

## Additional Resources

- [Official Spartan UI Documentation](https://www.spartan.ng/documentation/installation)
- [Dark Mode with Analog & Tailwind](https://dev.to/this-is-angular/dark-mode-with-analog-tailwind-4049)