# Spartan UI Customization Guide

This document provides guidance on customizing Spartan UI components to match your design needs while maintaining accessibility and consistency.

## Understanding the Component Architecture

Spartan UI follows a pattern where components are divided into two layers:

1. **Brain (BRN)**: The functional layer that provides behavior without styling
2. **Helm (HLM)**: The styling layer built with Tailwind CSS

This separation allows you to:
- Use just the brain components with your own styling
- Use the default helm styling as is
- Customize the helm styling for specific needs

## Methods of Customization

### 1. Using Tailwind Classes

The simplest method is to add Tailwind classes directly to components:

```html
<button 
  hlmBtn 
  class="bg-violet-500 hover:bg-violet-600 active:bg-violet-700 text-white"
>
  Custom Button
</button>
```

This approach is good for one-off customizations but can lead to inconsistency if overused.

### 2. Tailwind Configuration

For systematic changes, modify your `tailwind.config.js` file to customize the theme:

```javascript
module.exports = {
  presets: [require('@spartan-ng/brain/hlm-tailwind-preset')],
  theme: {
    extend: {
      colors: {
        // Customize primary color
        primary: {
          DEFAULT: 'hsl(var(--primary))',
          foreground: 'hsl(var(--primary-foreground))',
          // Add custom shades
          light: 'hsl(var(--primary-light))',
          dark: 'hsl(var(--primary-dark))',
        },
        // Add new semantic colors
        success: {
          DEFAULT: 'hsl(var(--success))',
          foreground: 'hsl(var(--success-foreground))',
        },
      },
      // Customize border radius
      borderRadius: {
        lg: 'var(--radius)',
        md: 'calc(var(--radius) - 2px)',
        sm: 'calc(var(--radius) - 4px)',
      },
    },
  },
}
```

And define the corresponding CSS variables in your styles:

```css
:root {
  --primary: 250 47% 45%;
  --primary-foreground: 0 0% 98%;
  --primary-light: 250 47% 55%;
  --primary-dark: 250 47% 35%;
  
  --success: 142 76% 36%;
  --success-foreground: 0 0% 98%;
}
```

### 3. Creating Custom Variants

For components that have variants (like buttons), you can create new variants:

#### Step 1: Create a directive

```typescript
// custom-button.directive.ts
import { Directive, HostBinding, Input } from '@angular/core';

@Directive({
  selector: 'button[hlmBtn][variant="brand"]',
  standalone: true,
})
export class HlmBrandButtonDirective {
  @HostBinding('class') get classes() {
    return 'bg-gradient-to-r from-brand-primary to-brand-secondary text-white hover:opacity-90';
  }
}
```

#### Step 2: Add to your module or component imports

```typescript
@Component({
  // ...
  imports: [HlmButtonDirective, HlmBrandButtonDirective],
  // ...
})
```

#### Step 3: Use the new variant

```html
<button hlmBtn variant="brand">Brand Button</button>
```

### 4. Extending Existing Components

You can create wrappers around Spartan UI components to add custom functionality:

```typescript
@Component({
  selector: 'app-custom-dialog',
  standalone: true,
  imports: [
    // Import needed Spartan UI components
    BrnDialogModule,
    HlmDialogModule,
  ],
  template: `
    <brn-dialog>
      <button brnDialogTrigger hlmBtn variant="outline">{{ triggerText }}</button>
      <brn-dialog-overlay hlm />
      <brn-dialog-content hlm class="max-w-md">
        <hlm-dialog-header>
          <h3 hlmDialogTitle>{{ title }}</h3>
          <p hlmDialogDescription>{{ description }}</p>
        </hlm-dialog-header>
        <div class="py-4">
          <ng-content></ng-content>
        </div>
        <hlm-dialog-footer>
          <ng-content select="[footer]"></ng-content>
        </hlm-dialog-footer>
      </brn-dialog-content>
    </brn-dialog>
  `,
})
export class CustomDialogComponent {
  @Input() title = '';
  @Input() description = '';
  @Input() triggerText = 'Open Dialog';
}
```

Usage:

```html
<app-custom-dialog
  title="User Settings"
  description="Configure your preferences"
  triggerText="Settings"
>
  <!-- Dialog content here -->
  <div>Main content for the dialog</div>
  
  <!-- Footer content -->
  <div footer>
    <button brnDialogClose hlmBtn variant="outline">Cancel</button>
    <button hlmBtn>Save</button>
  </div>
</app-custom-dialog>
```

## CSS Variable Customization

Spartan UI uses CSS variables for theming. You can customize these variables to create a cohesive design system:

### Base Variable Customization

```css
:root {
  /* Custom brand colors */
  --brand-primary: 250 70% 50%;
  --brand-secondary: 220 70% 50%;
  
  /* Override existing variables */
  --primary: 220 70% 50%;
  --primary-foreground: 220 10% 98%;
  
  /* Adjust radius for rounder corners */
  --radius: 1rem;
}
```

### Component-Specific Customization

You can scope CSS variables to target specific components:

```css
/* Custom card styling */
.custom-card {
  --card: 0 0% 97%;
  --card-foreground: 220 10% 2%;
  --card-border: 220 13% 91%;
}
```

## Creating a Custom Theme

For more extensive customization, create a complete theme:

1. Create a theme file `my-theme.css`:

```css
.theme-corporate {
  /* Base colors */
  --background: 0 0% 100%;
  --foreground: 222 47% 11%;
  
  /* Corporate palette */
  --primary: 210 100% 33%;
  --primary-foreground: 0 0% 100%;
  
  --secondary: 210 40% 96%;
  --secondary-foreground: 222 47% 11%;
  
  --accent: 196 100% 47%;
  --accent-foreground: 0 0% 100%;
  
  --destructive: 0 91% 42%;
  --destructive-foreground: 0 0% 100%;
  
  /* UI elements */
  --card: 0 0% 100%;
  --card-foreground: 222 47% 11%;
  
  --border: 214 32% 91%;
  --input: 214 32% 91%;
  
  --ring: 210 100% 33%;
  --radius: 0.375rem;
}

.theme-corporate.dark {
  --background: 222 47% 11%;
  --foreground: 210 40% 98%;
  
  --primary: 210 100% 50%;
  --primary-foreground: 0 0% 100%;
  
  /* Rest of dark theme variables */
}
```

2. Apply the theme class to your root component or body:

```html
<body class="theme-corporate">
  <!-- Your app here -->
</body>
```

Or dynamically:

```typescript
@Component({
  selector: 'app-root',
  template: `
    <div [class]="currentTheme">
      <!-- App content -->
    </div>
  `
})
export class AppComponent {
  currentTheme = 'theme-corporate';
}
```

## Best Practices for Customization

1. **Maintain Accessibility**
   - Preserve accessible contrast ratios when changing colors
   - Ensure focus states remain visible
   - Don't remove ARIA attributes from components

2. **Use Semantic Colors**
   - Stick to the color meaning convention (primary for primary actions, etc.)
   - Define new semantic colors for specific meanings in your UI

3. **Be Consistent**
   - Create a design system, not one-off modifications
   - Document custom components and variants
   - Create a component library for reuse

4. **Respect the Component API**
   - Modify styling, not core functionality
   - Extend rather than override when possible
   - Test customizations thoroughly

5. **Use Design Tokens**
   - Design tokens are named variables that represent design decisions
   - By using tokens consistently, you can easily update your design system
   - Example: `--spacing-4: 1rem` instead of hardcoding values

## Examples

### Custom Card Component

```typescript
@Component({
  selector: 'app-feature-card',
  standalone: true,
  imports: [HlmCardDirective, HlmCardHeaderDirective, HlmCardContentDirective],
  template: `
    <div 
      hlmCard 
      class="overflow-hidden border-l-4 hover:shadow-lg transition-all duration-200"
      [ngClass]="{'border-l-primary': variant === 'primary', 'border-l-accent': variant === 'accent'}"
    >
      <div class="flex items-start p-6">
        <div class="mr-4 text-primary">
          <ng-content select="[icon]"></ng-content>
        </div>
        <div>
          <h3 class="text-lg font-medium mb-2">{{ title }}</h3>
          <p class="text-muted-foreground">{{ description }}</p>
        </div>
      </div>
      <div hlmCardContent>
        <ng-content></ng-content>
      </div>
    </div>
  `
})
export class FeatureCardComponent {
  @Input() title = '';
  @Input() description = '';
  @Input() variant: 'primary' | 'accent' = 'primary';
}
```

### Custom Button Set

```typescript
@Component({
  selector: 'app-action-buttons',
  standalone: true,
  imports: [HlmButtonDirective],
  template: `
    <div class="flex space-x-2 justify-end">
      <button 
        hlmBtn 
        variant="outline" 
        (click)="onCancel.emit()"
      >
        {{ cancelText }}
      </button>
      <button 
        hlmBtn 
        [disabled]="isSubmitting"
        (click)="onSubmit.emit()"
      >
        <span *ngIf="isSubmitting" class="mr-2">
          <hlm-spinner size="sm" />
        </span>
        {{ submitText }}
      </button>
    </div>
  `
})
export class ActionButtonsComponent {
  @Input() cancelText = 'Cancel';
  @Input() submitText = 'Submit';
  @Input() isSubmitting = false;
  
  @Output() onCancel = new EventEmitter<void>();
  @Output() onSubmit = new EventEmitter<void>();
}
```

## Additional Resources

- [Tailwind CSS Customization Guide](https://tailwindcss.com/docs/theme)
- [Design Tokens Specification](https://design-tokens.github.io/community-group/format/)
- [Accessible Color Contrast Tools](https://webaim.org/resources/contrastchecker/)