# Spartan UI Documentation Index

This document provides an overview of all Spartan UI documentation files and their contents for easy navigation.

## Core Documentation

1. **[Spartan UI Overview](./spartan_ui.md)**
   - General overview of Spartan UI
   - Core concepts and features
   - Component architecture

2. **[Installation Guide](./spartan_ui_installation.md)**
   - Prerequisites and dependencies
   - Step-by-step installation instructions
   - Configuration setup

3. **[Component Reference](./spartan_ui_components.md)**
   - Comprehensive list of all available components
   - Usage examples for each component
   - Component composition patterns

## Theming and Customization

4. **[Theming Guide](./spartan_ui_theming.md)**
   - CSS variable system
   - Color conventions and patterns
   - Creating custom themes

5. **[Dark Mode Implementation](./spartan_ui_dark_mode.md)**
   - Setting up dark mode
   - Theme detection and toggling
   - Persisting user preferences

6. **[Typography System](./spartan_ui_typography.md)**
   - Text styles and hierarchies
   - Typography components
   - Customizing typography

7. **[Component Customization](./spartan_ui_customization.md)**
   - Styling existing components
   - Creating custom variants
   - Extending component functionality

## Tools and Best Practices

8. **[CLI Documentation](./spartan_ui_cli.md)**
   - Command-line interface usage
   - Automating component installation
   - Common CLI patterns

9. **[Accessibility Guide](./spartan_ui_accessibility.md)**
   - ARIA attributes and best practices
   - Keyboard navigation
   - Focus management
   - Contrast and color considerations

## Quick Reference

### Installation Quick Start

```bash
# Install CLI
npm i -D @spartan-ng/cli

# Install Angular CDK
npm i @angular/cdk

# Generate theme
npx nx g @spartan-ng/cli:ui-theme

# Add components
npx nx g @spartan-ng/cli:ui
```

### Core CSS Variables

```css
:root {
  --background: 0 0% 100%;
  --foreground: 240 10% 3.9%;
  --primary: 240 5.9% 10%;
  --primary-foreground: 0 0% 98%;
  --radius: 0.5rem;
  /* See theming guide for complete list */
}
```

### Component Import Example

```typescript
import { HlmButtonDirective } from '@spartan-ng/ui/button';

@Component({
  // ...
  imports: [HlmButtonDirective],
  // ...
})
export class AppComponent {}
```

### Common Component Usage

```html
<!-- Button -->
<button hlmBtn>Click Me</button>

<!-- Card -->
<div hlmCard>
  <div hlmCardHeader>
    <h3 hlmCardTitle>Card Title</h3>
  </div>
  <div hlmCardContent>Content here</div>
</div>

<!-- Dialog -->
<brn-dialog>
  <button brnDialogTrigger hlmBtn>Open Dialog</button>
  <brn-dialog-content hlm>Dialog content</brn-dialog-content>
</brn-dialog>
```

## Document Version Information

**Documentation Version**: 1.0.0  
**Last Updated**: May 2024  
**Compatible with Spartan UI**: Latest version  
**Source**: [Spartan UI Official Documentation](https://www.spartan.ng/documentation)