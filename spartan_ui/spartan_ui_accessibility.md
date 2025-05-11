# Spartan UI Accessibility Guide

This document provides guidelines and best practices for ensuring accessibility when working with Spartan UI components.

## Accessibility Overview

Spartan UI is built with accessibility in mind, following the Web Content Accessibility Guidelines (WCAG) 2.1. The components include proper ARIA attributes, keyboard navigation support, and focus management to ensure usability for all users, including those who use assistive technologies.

## Keyboard Navigation

### Built-in Keyboard Support

Spartan UI components come with keyboard navigation built-in:

- **Buttons**: Actionable with Enter or Space
- **Dropdowns**: Open with Enter/Space, navigate with arrow keys
- **Dialog/Modal**: Close with Escape key
- **Tabs**: Navigate with arrow keys, activate with Enter/Space
- **Menu**: Navigate with arrow keys, select with Enter/Space, close with Escape

### Testing Keyboard Navigation

When customizing components, test keyboard navigation:

1. Tab through the interface to ensure all elements are focusable in a logical order
2. Check that focus is visible and obvious (focus ring)
3. Ensure all actions can be performed with keyboard alone
4. Verify that focus trapping works correctly in modals and dialogs

## ARIA Attributes

### Important ARIA Roles and Properties

Spartan UI components use appropriate ARIA attributes automatically:

- `role`: Defines the purpose of an element (e.g., `button`, `dialog`, `menu`)
- `aria-label`: Provides a text alternative for elements without visible text
- `aria-labelledby`: References another element that serves as a label
- `aria-describedby`: References element(s) providing additional description
- `aria-expanded`: Indicates if a collapsible element is expanded
- `aria-haspopup`: Indicates an element triggers a popup
- `aria-selected`/`aria-checked`: Indicates selection state
- `aria-hidden`: Hides content from screen readers

### Example: Accessible Dialog

```html
<brn-dialog>
  <button brnDialogTrigger hlmBtn>Open Settings</button>
  <brn-dialog-overlay hlm />
  <brn-dialog-content 
    hlm
    aria-labelledby="dialog-title"
    aria-describedby="dialog-desc"
  >
    <hlm-dialog-header>
      <h3 hlmDialogTitle id="dialog-title">Application Settings</h3>
      <p hlmDialogDescription id="dialog-desc">
        Customize your application preferences.
      </p>
    </hlm-dialog-header>
    
    <div class="py-4">
      <!-- Dialog content -->
    </div>
    
    <hlm-dialog-footer>
      <button brnDialogClose hlmBtn variant="outline">Cancel</button>
      <button hlmBtn>Save Changes</button>
    </hlm-dialog-footer>
  </brn-dialog-content>
</brn-dialog>
```

### Maintaining ARIA Support

When customizing components:

1. Preserve existing ARIA attributes
2. Add appropriate ARIA attributes to custom elements
3. Ensure relationships between elements are properly conveyed
4. Test with screen readers to verify accessibility

## Focus Management

### Automatic Focus Handling

Spartan UI's brain components handle focus management, including:

- **Focus Trapping**: In dialogs and modals to keep focus within the component
- **Focus Restoration**: Returns focus to triggering element when dialog closes
- **Initial Focus**: Moves focus to the first focusable element within a component

### Custom Focus Management

For custom components, implement proper focus management:

```typescript
@Component({
  selector: 'app-custom-modal',
  template: `
    <div 
      class="modal" 
      tabindex="-1"
      cdkTrapFocus
      [cdkTrapFocusAutoCapture]="true"
    >
      <button #initialFocus>Close</button>
      <!-- Modal content -->
    </div>
  `
})
export class CustomModalComponent implements AfterViewInit {
  @ViewChild('initialFocus') initialFocus: ElementRef<HTMLButtonElement>;
  
  ngAfterViewInit() {
    // Set initial focus when modal opens
    this.initialFocus.nativeElement.focus();
  }
}
```

## Color and Contrast

### Contrast Requirements

WCAG guidelines require sufficient contrast between text and background:

- **Normal Text**: Minimum contrast ratio of 4.5:1
- **Large Text**: Minimum contrast ratio of 3:1
- **UI Components and Graphics**: Minimum contrast ratio of 3:1

### Checking Contrast

When customizing colors, verify contrast ratios:

1. Use tools like [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/)
2. Ensure all text is readable in both light and dark modes
3. Pay special attention to subtle colors like placeholders and disabled states

### Color Independence

Never use color alone to convey information:

```html
<!-- Bad - relies solely on color -->
<span class="text-red-500">Error in form submission</span>

<!-- Good - uses color and icon -->
<span class="text-red-500 flex items-center">
  <hlm-icon name="alert-triangle" class="mr-2" />
  Error in form submission
</span>

<!-- Good - uses color and explicit label -->
<span class="text-red-500">
  Error: Form submission failed
</span>
```

## Text and Typography

### Text Size and Readability

Ensure text is readable:

- Minimum text size of 16px (1rem) for body content
- Avoid using `font-size` smaller than 12px (0.75rem)
- Maintain adequate line spacing (1.5x font size is recommended)
- Limit line length to 80 characters or less

### Text Alternatives

Provide alternatives for non-text content:

```html
<!-- Image with alt text -->
<img src="logo.png" alt="Company Logo" />

<!-- Icon with accessible label -->
<hlm-icon name="trash" aria-label="Delete item" />

<!-- Decorative icon that should be ignored by screen readers -->
<hlm-icon name="decorative-flourish" aria-hidden="true" />
```

## Form Elements

### Labels and Instructions

All form controls must have associated labels:

```html
<div hlmFormField>
  <!-- Explicit label with for attribute -->
  <label hlmLabel for="username">Username</label>
  
  <!-- Additional instructions -->
  <p hlmMuted id="username-hint">Must be 3-20 characters</p>
  
  <!-- Input with aria-describedby referencing instructions -->
  <input 
    hlmInput 
    id="username"
    aria-describedby="username-hint"
  />
  
  <!-- Error message linked with aria-describedby -->
  <p 
    hlmFormError 
    id="username-error"
    *ngIf="usernameHasError"
  >
    Username is already taken
  </p>
</div>
```

### Error States

Form errors should be:

1. Visually distinct (color, icon, etc.)
2. Explicitly associated with their field
3. Announced by screen readers
4. Clear and specific about the issue

```typescript
// Component class
@Component({
  selector: 'app-form',
  // Template omitted for brevity
})
export class FormComponent {
  validateForm() {
    if (this.form.invalid) {
      // Set specific error messages
      if (this.form.controls.email.hasError('required')) {
        this.emailError = 'Email address is required';
      } else if (this.form.controls.email.hasError('email')) {
        this.emailError = 'Please enter a valid email address';
      }
      
      // Focus the first invalid element
      const firstInvalidElement = document.querySelector('.ng-invalid');
      if (firstInvalidElement instanceof HTMLElement) {
        firstInvalidElement.focus();
      }
    }
  }
}
```

## Interactive Elements

### Buttons vs. Links

Use the appropriate element for each interaction:

- **Buttons**: For actions (submitting forms, toggling UI elements, etc.)
- **Links**: For navigation to a new page or location

```html
<!-- Button for an action -->
<button hlmBtn (click)="deleteItem()">Delete</button>

<!-- Link for navigation -->
<a hlmBtn variant="link" routerLink="/settings">Go to Settings</a>
```

### Touch Targets

Ensure interactive elements are large enough:

- Minimum size of 44x44 pixels for touch targets
- Adequate spacing between clickable elements (at least 8px)

```html
<!-- Small button with adequate padding -->
<button 
  hlmBtn 
  variant="ghost" 
  size="icon"
  class="p-3"
>
  <hlm-icon name="settings" />
</button>
```

## Animation and Motion

### Reduced Motion

Respect user preferences for reduced motion:

```css
/* In your global styles */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

For Angular animations, check for the preference:

```typescript
@Component({
  selector: 'app-animated-component',
  animations: [
    // Define animations
  ]
})
export class AnimatedComponent implements OnInit {
  prefersReducedMotion = false;
  
  ngOnInit() {
    // Check for reduced motion preference
    this.prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
    
    // Listen for changes
    window.matchMedia('(prefers-reduced-motion: reduce)').addEventListener('change', (e) => {
      this.prefersReducedMotion = e.matches;
    });
  }
  
  getAnimationState() {
    return this.prefersReducedMotion ? 'noAnimation' : 'animated';
  }
}
```

## Testing Accessibility

### Automated Testing

Use automated tools to catch common issues:

- [axe-core](https://github.com/dequelabs/axe-core) for JavaScript testing
- Browser extensions like [axe DevTools](https://www.deque.com/axe/devtools/)
- Integrate accessibility testing into CI/CD pipelines

### Manual Testing

Always combine automated testing with manual checks:

1. **Keyboard Testing**: Navigate the application using only keyboard
2. **Screen Reader Testing**: Test with NVDA, VoiceOver, or JAWS
3. **Zoom Testing**: Test at 200% zoom to ensure content remains usable
4. **Color Testing**: View in grayscale to check color independence

### Testing Checklist

For each new component or feature:

- [ ] Verify all interactive elements are keyboard accessible
- [ ] Check for appropriate ARIA attributes
- [ ] Test color contrast with WebAIM or similar tool
- [ ] Ensure text alternatives for all non-text content
- [ ] Test with screen reader
- [ ] Validate form error handling
- [ ] Check focus management
- [ ] Test at different zoom levels
- [ ] Verify content is understandable in sequence

## Additional Resources

- [Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG21/)
- [WAI-ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices-1.1/)
- [Angular Accessibility Guide](https://angular.io/guide/accessibility)
- [Inclusive Components by Heydon Pickering](https://inclusive-components.design/)
- [A11y Project Checklist](https://www.a11yproject.com/checklist/)