# Angular Component Development Kit (CDK)

The Angular CDK (Component Development Kit) provides a set of tools and behaviors for building custom components. It provides the foundation upon which Angular Material is built.

## CDK Categories

### Accessibility
Utilities for supporting accessibility features.

- **Live announcer** - Announces messages for screen-reader users
- **Focus trap** - Traps focus within an element
- **Focus monitor** - Monitors element focus
- **ARIA** - Utilities for adding ARIA attributes

### Layout
Tools for working with layout.

- **Breakpoints** - Utilities for responding to breakpoints
- **Flex layout** - Tools for flexbox layouts
- **Layout** - Layout directives and utilities

### Form Controls
Utilities to help build custom form controls.

- **Auto-size** - Directive to automatically resize a textarea
- **Text field** - Tools for building text field components

### Overlay
A flexible overlay/popup system.

- **Overlay** - Service for dynamically displaying floating panels
- **Position** - Utilities for positioning overlays
- **Scroll** - Directives for handling scroll events

### Portal
Utilities for projecting content.

- **Portal** - Component or template reference that can be attached to a host

### Drag and Drop
System for creating drag-and-drop interfaces.

- **Drag handle** - Specified element that can be used to drag
- **Drop list** - Container where items can be dropped
- **Drag preview** - Customizable preview when dragging

### Table
Tools for building tables with Material Design styling.

- **Data source** - Data source for connecting to different types of data
- **Table directives** - Directives for building tables

### Tree
Tools for generating tree structures.

- **Nested tree** - Tree with parent/child relationship
- **Flat tree** - Tree with flattened structure

### Platform
Information about the current platform.

- **Platform detection** - Service for detecting platform information
- **Feature detection** - Utilities for feature detection

### Clipboard
Utilities for working with the clipboard.

- **Clipboard** - Service for copying text to the clipboard

### Scrolling
Advanced scrolling capabilities.

- **Virtual scroll** - Efficient handling of large lists
- **Infinite scroll** - Loading more data as the user scrolls

### Text Field
Utilities for text fields.

- **Auto-size** - Directive for auto-sizing textareas
- **Input modality** - Service to track input modality

### Observers
Observables for various browser events.

- **Resize observer** - Observer for element resize events
- **Mutation observer** - Observer for DOM mutations
- **Content observer** - Observer for content changes

## Usage

To use a CDK module, you need to:

1. Import the specific CDK module in your Angular module
2. Add the module to your NgModule imports
3. Use the provided directives or services

Example:

```typescript
import { DragDropModule } from '@angular/cdk/drag-drop';

@NgModule({
  imports: [
    DragDropModule,
    // other imports
  ],
  // ...
})
export class AppModule { }
```

```html
<div cdkDrag>
  Drag me!
</div>
```

## Building Custom Components

The CDK provides the building blocks for creating custom components. These utilities can be used to implement complex behaviors without having to reinvent the wheel.

For example, you can use:
- `@angular/cdk/a11y` for accessibility features
- `@angular/cdk/overlay` for creating popups and modals
- `@angular/cdk/drag-drop` for drag and drop functionality

## Additional Resources

For more detailed information about each CDK module, refer to the official Angular CDK documentation.