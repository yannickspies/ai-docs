# Spartan UI Components

This document provides an overview of the components available in Spartan UI for Angular.

## Component Architecture

Spartan UI follows a design pattern where each component consists of:

1. **Brain (BRN)**: The core functionality without styling
2. **Helm (HLM)**: The styling layer built with Tailwind CSS

This separation allows for flexibility in styling while maintaining consistent functionality.

## Available Components

### Accordion

A vertically stacked set of interactive headings that each reveal a section of content.

```html
<brn-accordion>
  <brn-accordion-item>
    <brn-accordion-trigger hlm>Item 1</brn-accordion-trigger>
    <brn-accordion-content hlm>Content for item 1</brn-accordion-content>
  </brn-accordion-item>
</brn-accordion>
```

### Alert

Displays a callout for user attention.

```html
<div hlmAlert variant="default">
  <hlm-alert-icon></hlm-alert-icon>
  <hlm-alert-title>Alert Title</hlm-alert-title>
  <hlm-alert-description>Alert Description</hlm-alert-description>
</div>
```

### Alert Dialog

A modal dialog that interrupts the user with important content.

```html
<brn-alert-dialog>
  <button brnAlertDialogTrigger hlmBtn>Delete Account</button>
  <brn-alert-dialog-overlay hlm />
  <brn-alert-dialog-content hlm>
    <hlm-alert-dialog-header>
      <h3 hlmAlertDialogTitle>Are you sure?</h3>
      <p hlmAlertDialogDescription>This action cannot be undone.</p>
    </hlm-alert-dialog-header>
    <hlm-alert-dialog-footer>
      <button brnAlertDialogCancel hlmBtn variant="outline">Cancel</button>
      <button brnAlertDialogAction hlmBtn variant="destructive">Delete</button>
    </hlm-alert-dialog-footer>
  </brn-alert-dialog-content>
</brn-alert-dialog>
```

### Aspect Ratio

Maintains consistent width/height ratios.

```html
<div hlmAspectRatio [ratio]="16/9">
  <img src="..." alt="..." />
</div>
```

### Avatar

An image element with a fallback for representing the user.

```html
<div hlmAvatar>
  <img src="..." alt="user" hlmAvatarImage />
  <span hlmAvatarFallback>JD</span>
</div>
```

### Badge

Small status descriptors.

```html
<span hlmBadge>Badge</span>
<span hlmBadge variant="outline">Outline Badge</span>
<span hlmBadge variant="secondary">Secondary Badge</span>
<span hlmBadge variant="destructive">Destructive Badge</span>
```

### Breadcrumb

Navigation aid showing hierarchy.

```html
<nav hlmBreadcrumb>
  <ol>
    <li hlmBreadcrumbItem>
      <a hlmBreadcrumbLink href="/">Home</a>
    </li>
    <li hlmBreadcrumbItem>
      <a hlmBreadcrumbLink href="/components">Components</a>
    </li>
    <li hlmBreadcrumbItem>
      <span hlmBreadcrumbPage>Breadcrumb</span>
    </li>
  </ol>
</nav>
```

### Button

Interactive elements for actions.

```html
<button hlmBtn>Default</button>
<button hlmBtn variant="destructive">Destructive</button>
<button hlmBtn variant="outline">Outline</button>
<button hlmBtn variant="secondary">Secondary</button>
<button hlmBtn variant="ghost">Ghost</button>
<button hlmBtn variant="link">Link</button>
```

### Calendar

Displays and selects dates.

```html
<brn-calendar hlm [(selected)]="date" />
```

### Card

Container for grouping related content.

```html
<div hlmCard>
  <div hlmCardHeader>
    <h3 hlmCardTitle>Card Title</h3>
    <p hlmCardDescription>Card Description</p>
  </div>
  <div hlmCardContent>
    <p>Card Content</p>
  </div>
  <div hlmCardFooter>
    <button hlmBtn>Action</button>
  </div>
</div>
```

### Carousel

Cycles through elements with animation.

```html
<brn-carousel hlm>
  <brn-carousel-item *ngFor="let item of items">
    <img [src]="item.image" alt="..." />
  </brn-carousel-item>
  <brn-carousel-prev hlm />
  <brn-carousel-next hlm />
</brn-carousel>
```

### Checkbox

Toggle selection state.

```html
<brn-checkbox hlm [(checked)]="checked" />
```

### Collapsible

Toggle visibility of content.

```html
<brn-collapsible>
  <button brnCollapsibleTrigger hlmBtn>Toggle</button>
  <brn-collapsible-content hlm>
    Content that can be collapsed
  </brn-collapsible-content>
</brn-collapsible>
```

### Combobox

Input with a filterable dropdown.

```html
<brn-combobox hlm [options]="options" [(value)]="selectedValue">
  <brn-combobox-trigger hlm>
    <span>{{ selectedValue || 'Select option...' }}</span>
  </brn-combobox-trigger>
  <brn-combobox-content hlm>
    <brn-combobox-input hlm placeholder="Search..." />
    <brn-combobox-empty hlm>No results found.</brn-combobox-empty>
    <brn-combobox-viewport hlm>
      <brn-combobox-option
        *ngFor="let option of filteredOptions"
        [value]="option.value"
        hlm
      >
        {{ option.label }}
      </brn-combobox-option>
    </brn-combobox-viewport>
  </brn-combobox-content>
</brn-combobox>
```

### Command

Keyboard-accessible command menu.

```html
<brn-command hlm>
  <brn-command-input hlm placeholder="Search..." />
  <brn-command-list hlm>
    <brn-command-empty hlm>No results found.</brn-command-empty>
    <brn-command-group hlm heading="Suggestions">
      <brn-command-item hlm value="home">Home</brn-command-item>
      <brn-command-item hlm value="settings">Settings</brn-command-item>
    </brn-command-group>
  </brn-command-list>
</brn-command>
```

### Context Menu

Menu that appears on right-click.

```html
<div [brnContextMenuTriggerFor]="contextMenu">
  Right-click me
</div>
<brn-context-menu #contextMenu>
  <brn-menu-item hlm>Copy</brn-menu-item>
  <brn-menu-item hlm>Paste</brn-menu-item>
  <brn-menu-separator hlm />
  <brn-menu-item hlm>Delete</brn-menu-item>
</brn-context-menu>
```

### Data Table

Display and interact with structured data.

```html
<table hlmTable>
  <thead>
    <tr>
      <th hlmTh>Name</th>
      <th hlmTh>Email</th>
      <th hlmTh>Actions</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users" hlmTr>
      <td hlmTd>{{ user.name }}</td>
      <td hlmTd>{{ user.email }}</td>
      <td hlmTd>
        <button hlmBtn variant="ghost" size="sm">Edit</button>
      </td>
    </tr>
  </tbody>
</table>
```

### Date Picker

Interface for selecting dates.

```html
<brn-date-picker hlm [(selected)]="date">
  <brn-date-picker-trigger hlm>
    <button hlmBtn variant="outline">
      {{ date ? date.toLocaleDateString() : 'Pick a date' }}
    </button>
  </brn-date-picker-trigger>
  <brn-date-picker-content hlm>
    <brn-calendar hlm />
  </brn-date-picker-content>
</brn-date-picker>
```

### Dialog

Modal window for focused tasks.

```html
<brn-dialog>
  <button brnDialogTrigger hlmBtn>Open Dialog</button>
  <brn-dialog-overlay hlm />
  <brn-dialog-content hlm>
    <hlm-dialog-header>
      <h3 hlmDialogTitle>Dialog Title</h3>
      <p hlmDialogDescription>Dialog Description</p>
    </hlm-dialog-header>
    <div>Dialog content</div>
    <hlm-dialog-footer>
      <button brnDialogClose hlmBtn>Close</button>
    </hlm-dialog-footer>
  </brn-dialog-content>
</brn-dialog>
```

### Dropdown Menu

Toggleable menu for options.

```html
<brn-dropdown>
  <button brnDropdownTrigger hlmBtn>Open Menu</button>
  <brn-dropdown-content hlm>
    <brn-dropdown-group>
      <brn-dropdown-item hlm>Item 1</brn-dropdown-item>
      <brn-dropdown-item hlm>Item 2</brn-dropdown-item>
    </brn-dropdown-group>
    <brn-dropdown-separator hlm />
    <brn-dropdown-item hlm>Item 3</brn-dropdown-item>
  </brn-dropdown-content>
</brn-dropdown>
```

### Form

Manages form input validation and submission.

```html
<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <div hlmFormField>
    <hlm-form-label for="name">Name</hlm-form-label>
    <input hlmInput id="name" formControlName="name" />
    <hlm-form-error *ngIf="name.invalid && name.touched">
      Name is required
    </hlm-form-error>
  </div>
  <button hlmBtn type="submit">Submit</button>
</form>
```

### Hover Card

Card displayed on hover.

```html
<brn-hover-card>
  <div brnHoverCardTrigger>Hover me</div>
  <brn-hover-card-content hlm>
    <div hlmHoverCardArrow />
    <div class="p-4">
      <h4 class="font-semibold">User Details</h4>
      <p>Additional information on hover</p>
    </div>
  </brn-hover-card-content>
</brn-hover-card>
```

### Icon

Vector graphics for visual aids.

```html
<hlm-icon name="user" />
<hlm-icon name="settings" class="w-6 h-6" />
```

### Input

Text entry field.

```html
<input hlmInput placeholder="Type here..." />
<input hlmInput type="password" placeholder="Password" />
```

### Input OTP

One-time password input.

```html
<brn-input-otp hlm [(value)]="otpValue" length={6}>
  <brn-input-otp-field *ngFor="let i of [0,1,2,3,4,5]" index="{{i}}" hlm />
</brn-input-otp>
```

### Label

Text descriptor for form fields.

```html
<label hlmLabel for="email">Email</label>
<input hlmInput id="email" type="email" />
```

### Menubar

Horizontal navigation component.

```html
<brn-menubar hlm>
  <brn-menubar-menu>
    <brn-menubar-trigger hlm>File</brn-menubar-trigger>
    <brn-menubar-content hlm>
      <brn-menubar-item hlm>New</brn-menubar-item>
      <brn-menubar-item hlm>Open</brn-menubar-item>
    </brn-menubar-content>
  </brn-menubar-menu>
  <brn-menubar-menu>
    <brn-menubar-trigger hlm>Edit</brn-menubar-trigger>
    <brn-menubar-content hlm>
      <brn-menubar-item hlm>Cut</brn-menubar-item>
      <brn-menubar-item hlm>Copy</brn-menubar-item>
      <brn-menubar-item hlm>Paste</brn-menubar-item>
    </brn-menubar-content>
  </brn-menubar-menu>
</brn-menubar>
```

### Navigation Menu

Hierarchical navigation component.

```html
<brn-navigation-menu hlm>
  <brn-navigation-menu-list>
    <brn-navigation-menu-item>
      <brn-navigation-menu-trigger hlm>Getting Started</brn-navigation-menu-trigger>
      <brn-navigation-menu-content hlm>
        <brn-navigation-menu-link hlm routerLink="/introduction">
          Introduction
        </brn-navigation-menu-link>
        <brn-navigation-menu-link hlm routerLink="/installation">
          Installation
        </brn-navigation-menu-link>
      </brn-navigation-menu-content>
    </brn-navigation-menu-item>
  </brn-navigation-menu-list>
</brn-navigation-menu>
```

### Pagination

Interface for navigating pages.

```html
<nav hlmPagination>
  <ul>
    <li>
      <button hlmPaginationPrevious>Previous</button>
    </li>
    <li *ngFor="let page of [1,2,3,4,5]">
      <button hlmPaginationLink [active]="currentPage === page">
        {{ page }}
      </button>
    </li>
    <li>
      <button hlmPaginationNext>Next</button>
    </li>
  </ul>
</nav>
```

### Popover

Floating content panel.

```html
<brn-popover>
  <button brnPopoverTrigger hlmBtn>Open Popover</button>
  <brn-popover-content hlm>
    <div class="p-4">
      <h4 class="font-semibold">Popover Title</h4>
      <p>Popover content goes here.</p>
    </div>
  </brn-popover-content>
</brn-popover>
```

### Progress

Visual indicator of completion.

```html
<progress hlmProgress [value]="33" [max]="100" />
```

### Radio Group

Select one option from a set.

```html
<brn-radio-group hlm [(value)]="selectedValue">
  <brn-radio-button hlm value="option1">Option 1</brn-radio-button>
  <brn-radio-button hlm value="option2">Option 2</brn-radio-button>
  <brn-radio-button hlm value="option3">Option 3</brn-radio-button>
</brn-radio-group>
```

### Scroll Area

Custom scrollable area.

```html
<brn-scroll-area hlm class="h-[200px] w-[350px]">
  <div class="p-4">
    <!-- Long content here -->
  </div>
</brn-scroll-area>
```

### Select

Dropdown selection component.

```html
<brn-select hlm [(value)]="selectedValue">
  <brn-select-trigger hlm>
    {{ selectedValue || 'Select an option' }}
  </brn-select-trigger>
  <brn-select-content hlm>
    <brn-select-item hlm value="option1">Option 1</brn-select-item>
    <brn-select-item hlm value="option2">Option 2</brn-select-item>
    <brn-select-item hlm value="option3">Option 3</brn-select-item>
  </brn-select-content>
</brn-select>
```

### Separator

Horizontal line for separating content.

```html
<div hlmSeparator />
```

### Sheet

Side-anchored dialog.

```html
<brn-sheet>
  <button brnSheetTrigger hlmBtn>Open Sheet</button>
  <brn-sheet-overlay hlm />
  <brn-sheet-content hlm side="right">
    <hlm-sheet-header>
      <h3 hlmSheetTitle>Sheet Title</h3>
      <p hlmSheetDescription>Sheet Description</p>
    </hlm-sheet-header>
    <div class="p-4">Sheet content goes here.</div>
    <hlm-sheet-footer>
      <button brnSheetClose hlmBtn>Close</button>
    </hlm-sheet-footer>
  </brn-sheet-content>
</brn-sheet>
```

### Skeleton

Loading placeholder.

```html
<div hlmSkeleton class="h-[20px] w-[200px]" />
<div hlmSkeleton class="h-[100px] w-[100px] rounded-full" />
```

### Slider

Select a value from a range.

```html
<brn-slider hlm [(value)]="sliderValue" [min]="0" [max]="100" [step]="1" />
```

### Sonner

Toast notification manager.

```html
<brn-sonner hlm>
  <button hlmBtn (click)="toast.success('Success message')">Show Success</button>
  <button hlmBtn (click)="toast.error('Error message')">Show Error</button>
</brn-sonner>
```

### Spinner

Loading indicator.

```html
<hlm-spinner />
<hlm-spinner size="lg" />
```

### Switch

Toggle between states.

```html
<brn-switch hlm [(checked)]="isChecked" />
```

### Table

Display tabular data.

```html
<table hlmTable>
  <thead>
    <tr>
      <th hlmTh>Name</th>
      <th hlmTh>Email</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let user of users" hlmTr>
      <td hlmTd>{{ user.name }}</td>
      <td hlmTd>{{ user.email }}</td>
    </tr>
  </tbody>
</table>
```

### Tabs

Organize content into separate views.

```html
<brn-tabs hlm [(value)]="activeTab">
  <brn-tabs-list hlm>
    <brn-tabs-trigger hlm value="tab1">Tab 1</brn-tabs-trigger>
    <brn-tabs-trigger hlm value="tab2">Tab 2</brn-tabs-trigger>
  </brn-tabs-list>
  <brn-tabs-content hlm value="tab1">
    Content for tab 1
  </brn-tabs-content>
  <brn-tabs-content hlm value="tab2">
    Content for tab 2
  </brn-tabs-content>
</brn-tabs>
```

### Textarea

Multi-line text input.

```html
<textarea hlmTextarea placeholder="Type your message here." rows="4"></textarea>
```

### Toast

Brief notifications.

```html
<brn-toast hlm [open]="isToastOpen" title="Toast Title" description="Toast description">
  <button hlmBtn variant="outline" (click)="isToastOpen = false">Dismiss</button>
</brn-toast>

<button hlmBtn (click)="isToastOpen = true">Show Toast</button>
```

### Toggle

Two-state button.

```html
<button hlmToggle [(pressed)]="isPressed">Toggle</button>
```

### Toggle Group

Group of toggle buttons.

```html
<brn-toggle-group hlm [(value)]="toggleValue" type="single">
  <button brnToggleGroupItem hlmToggleItem value="bold">Bold</button>
  <button brnToggleGroupItem hlmToggleItem value="italic">Italic</button>
  <button brnToggleGroupItem hlmToggleItem value="underline">Underline</button>
</brn-toggle-group>
```

### Tooltip

Short information on hover.

```html
<brn-tooltip>
  <button brnTooltipTrigger hlmBtn>Hover Me</button>
  <brn-tooltip-content hlm>
    Tooltip information
  </brn-tooltip-content>
</brn-tooltip>
```

## Component Customization

### Styling Components

You can customize components by adding Tailwind classes:

```html
<button hlmBtn class="bg-purple-600 hover:bg-purple-700">
  Custom Button
</button>
```

### Creating Component Variants

To create custom component variants, extend your Tailwind configuration:

```javascript
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      // Custom color for new button variant
      colors: {
        'custom-blue': '#1a73e8',
      },
    },
  },
}
```

Then create a directive:

```typescript
// custom-btn.directive.ts
@Directive({
  selector: '[hlmBtn][variant=custom]',
  standalone: true,
  host: {
    class: 'bg-custom-blue hover:bg-custom-blue/90 text-white'
  }
})
export class HlmCustomBtnDirective {}
```

Use in template:

```html
<button hlmBtn variant="custom">Custom Variant</button>
```

## Using Component Composition

Components can be composed together to create complex UIs:

```html
<div hlmCard>
  <div hlmCardHeader>
    <h3 hlmCardTitle>User Profile</h3>
    <p hlmCardDescription>Update your profile information</p>
  </div>
  <div hlmCardContent>
    <form [formGroup]="profileForm">
      <div class="space-y-4">
        <div hlmFormField>
          <hlm-form-label for="name">Name</hlm-form-label>
          <input hlmInput id="name" formControlName="name" />
        </div>
        
        <div hlmFormField>
          <hlm-form-label for="bio">Bio</hlm-form-label>
          <textarea hlmTextarea id="bio" formControlName="bio" rows="3"></textarea>
        </div>
        
        <div class="flex items-center space-x-2">
          <brn-switch hlm formControlName="notifications" id="notifications" />
          <label hlmLabel for="notifications">Enable notifications</label>
        </div>
      </div>
    </form>
  </div>
  <div hlmCardFooter class="flex justify-between">
    <button hlmBtn variant="outline">Cancel</button>
    <button hlmBtn>Save Changes</button>
  </div>
</div>
```

## Accessibility

Spartan UI components are built with accessibility in mind:

- Keyboard navigation
- ARIA attributes
- Focus management
- Screen reader support

Always maintain these accessibility features when customizing components.

## Additional Resources

- [Spartan UI Documentation](https://www.spartan.ng/documentation)
- [Angular CDK](https://material.angular.io/cdk)
- [TailwindCSS](https://tailwindcss.com/)