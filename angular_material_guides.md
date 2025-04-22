# Angular Material Guides

This document contains information from the Angular Material guides for reference.

## Getting Started

Angular Material offers prebuilt UI components that implement Google's Material Design specification. It provides reusable, well-tested, and accessible UI components.

### Installation

```bash
ng add @angular/material
```

This command:
- Installs Angular Material, the Component Dev Kit (CDK), and Angular Animations
- Sets up the global styles (typography, theme)
- Adds browser animations
- Includes a prebuilt theme
- Adds necessary imports to your app module

### Basic Usage

1. Import the component modules you need
2. Add them to your module's imports array
3. Use the components in your templates

```typescript
// app.module.ts
import { MatButtonModule } from '@angular/material/button';
import { MatCardModule } from '@angular/material/card';

@NgModule({
  imports: [
    MatButtonModule,
    MatCardModule,
    // other imports
  ]
})
export class AppModule { }
```

```html
<!-- app.component.html -->
<mat-card>
  <mat-card-title>Hello World</mat-card-title>
  <mat-card-content>
    <p>This is a basic Angular Material card</p>
  </mat-card-content>
  <mat-card-actions>
    <button mat-button>OK</button>
  </mat-card-actions>
</mat-card>
```

## Theming

Angular Material's theming system allows you to customize your application's color palette, typography, and component density.

### Theme Structure

A theme consists of:
- **Primary palette**: Main colors for key parts of the UI
- **Accent palette**: Colors for floating action buttons and interactive elements
- **Warn palette**: Colors for error states
- **Background and foreground palettes**: Colors for UI backgrounds and text

### Prebuilt Themes

Angular Material includes several prebuilt themes:
- `deeppurple-amber.css`
- `indigo-pink.css`
- `pink-bluegrey.css`
- `purple-green.css`

To use a prebuilt theme, include it in your `angular.json` file:

```json
"styles": [
  "@angular/material/prebuilt-themes/indigo-pink.css",
  "src/styles.css"
]
```

### Custom Themes

To create a custom theme:

1. Create a SCSS file for your theme (e.g., `custom-theme.scss`)
2. Import the Angular Material theming functions
3. Define your color palettes
4. Create your theme
5. Include your theme file in `angular.json`

```scss
@use '@angular/material' as mat;

// Define custom palettes
$my-primary: mat.define-palette(mat.$indigo-palette, 500);
$my-accent: mat.define-palette(mat.$pink-palette, A200, A100, A400);
$my-warn: mat.define-palette(mat.$red-palette);

// Create the theme
$my-theme: mat.define-light-theme((
  color: (
    primary: $my-primary,
    accent: $my-accent,
    warn: $my-warn,
  ),
  typography: mat.define-typography-config(),
  density: 0,
));

// Apply the theme to components
@include mat.all-component-themes($my-theme);
```

### Typography

Angular Material's typography system helps maintain consistent text styles throughout your application.

To customize typography:

```scss
$custom-typography: mat.define-typography-config(
  $font-family: 'Roboto, "Helvetica Neue", sans-serif',
  $headline-1: mat.define-typography-level(96px, 96px, 300, $letter-spacing: -0.015em),
  $headline-2: mat.define-typography-level(60px, 60px, 300, $letter-spacing: -0.005em),
  // ... other levels
);

$my-theme: mat.define-light-theme((
  color: (
    primary: $my-primary,
    accent: $my-accent,
    warn: $my-warn,
  ),
  typography: $custom-typography,
  density: 0,
));
```

## Schematics

Angular Material provides schematics to help you quickly generate common app elements and components.

### Available Schematics

- `ng generate @angular/material:address-form` - Creates a form for collecting address information
- `ng generate @angular/material:navigation` - Creates a responsive navigation menu
- `ng generate @angular/material:dashboard` - Creates a dashboard with card layout
- `ng generate @angular/material:table` - Creates a table with sorting and pagination
- `ng generate @angular/material:tree` - Creates a tree with expandable/collapsible nodes

### Usage Example

```bash
ng generate @angular/material:dashboard my-dashboard
```

This creates:
- Component files for the dashboard
- Necessary Angular Material imports
- Basic styling

## System Variables

Angular Material provides a set of system variables for theming that can be customized.

### Color Variables

- `--mat-primary`: Primary color
- `--mat-accent`: Accent color
- `--mat-warn`: Warning color
- `--mat-background`: Background color

### Theme Variables

- `--mat-theme-color-primary`: Primary theme color
- `--mat-theme-color-accent`: Accent theme color
- `--mat-theme-color-warn`: Warning theme color

### Typography Variables

- `--mat-font-family`: Default font family
- `--mat-font-size`: Base font size
- `--mat-line-height`: Base line height

## Creating a Custom Form Field Control

You can create custom form field controls that work with Angular Material's form field component.

### Implementation Steps

1. Implement the `MatFormFieldControl` interface
2. Add provider for the control
3. Implement `ControlValueAccessor`
4. Register the control with `NG_VALUE_ACCESSOR`
5. Make the control compatible with Angular forms

### Example Implementation

```typescript
@Component({
  selector: 'custom-input',
  template: `
    <div class="custom-input-container"
        [class.floating]="shouldLabelFloat"
        [class.disabled]="disabled">
      <input [disabled]="disabled" [(ngModel)]="value">
    </div>
  `,
  providers: [
    {
      provide: MatFormFieldControl,
      useExisting: CustomInputComponent
    },
    {
      provide: NG_VALUE_ACCESSOR,
      useExisting: CustomInputComponent,
      multi: true
    }
  ]
})
export class CustomInputComponent implements MatFormFieldControl<string>, ControlValueAccessor {
  // Implementation details...
}
```

## Creating a Custom Stepper using CDK Stepper

You can create a custom stepper component using the CDK stepper as a base.

### Implementation Steps

1. Create a custom step component
2. Create a custom stepper component that extends `CdkStepper`
3. Add necessary template and styling

### Example Implementation

```typescript
@Component({
  selector: 'custom-step',
  template: '<ng-template><ng-content></ng-content></ng-template>'
})
export class CustomStepComponent extends CdkStep {
  // Custom step implementation
}

@Component({
  selector: 'custom-stepper',
  template: `
    <div class="custom-stepper-container">
      <!-- Header with step labels -->
      <div class="custom-stepper-header">
        <ng-container *ngFor="let step of steps; let i = index; let last = last">
          <div class="custom-step-header" [class.active]="selectedIndex === i">
            {{ i + 1 }}. {{ step.label }}
          </div>
          <div *ngIf="!last" class="custom-connector-line"></div>
        </ng-container>
      </div>
      
      <!-- Step content -->
      <div class="custom-stepper-content">
        <div [style.display]="selectedIndex === i ? 'block' : 'none'" *ngFor="let step of steps; let i = index">
          <ng-container [ngTemplateOutlet]="step.content"></ng-container>
        </div>
      </div>
      
      <!-- Navigation buttons -->
      <div class="custom-stepper-actions">
        <button *ngIf="selectedIndex > 0" (click)="previous()">Previous</button>
        <button *ngIf="selectedIndex < steps.length - 1" (click)="next()">Next</button>
        <button *ngIf="selectedIndex === steps.length - 1" (click)="complete()">Complete</button>
      </div>
    </div>
  `,
  providers: [
    {
      provide: CdkStepper,
      useExisting: CustomStepperComponent
    }
  ]
})
export class CustomStepperComponent extends CdkStepper {
  // Custom stepper implementation
}
```

## Additional Resources

For more detailed information, refer to the official Angular Material documentation.