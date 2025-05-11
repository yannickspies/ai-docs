# Spartan UI CLI Guide

This document details the usage of the Spartan UI CLI to automate installation and management of Spartan UI components in your Angular project.

## Overview

The Spartan UI CLI (`@spartan-ng/cli`) is a command-line tool designed to streamline the setup, installation, and maintenance of Spartan UI components in your Angular applications. It provides commands for:

- Initializing the theme
- Adding components
- Managing dependencies
- Updating existing components

## Installation

Install the CLI as a dev dependency in your Angular project:

```bash
npm i -D @spartan-ng/cli
```

## Core Commands

### UI Theme Command

The UI theme command sets up the basic CSS variables for your Spartan theme.

```bash
npx nx g @spartan-ng/cli:ui-theme
```

This command:
- Creates or updates CSS variables in your global styles file
- Adds Spartan's default color scheme
- Sets up both light and dark mode variables
- Configures Tailwind appropriately

#### Options

- `--project=<project-name>`: Target a specific project in an Nx workspace
- `--theme=[default|modern|retro]`: Choose a predefined theme
- `--path=<path-to-styles.css>`: Specify a custom path for your global styles file

### UI Command

The UI command is used to add components to your project:

```bash
npx nx g @spartan-ng/cli:ui
```

This interactive command:
- Allows you to select which components to add
- Automatically installs required dependencies
- Creates necessary component files
- Sets up module imports

#### Options

- `--project=<project-name>`: Target a specific project in an Nx workspace
- `--components=<comma-separated-list>`: Directly specify components to add
- `--path=<path>`: Set a custom path for generated components
- `--dry-run`: Preview changes without making them

### UI Add Command

The UI add command installs specific components:

```bash
npx nx g @spartan-ng/cli:ui-add --components=button,card,dialog
```

This focused command:
- Adds only the specified components
- Handles dependencies automatically
- Is ideal for adding components incrementally

#### Options

- `--project=<project-name>`: Target a specific project in an Nx workspace
- `--components=<comma-separated-list>`: Directly specify components to add (required)
- `--path=<path>`: Set a custom path for generated components
- `--dry-run`: Preview changes without making them

## Usage Examples

### Basic Setup for a New Project

1. Initialize Spartan UI theme:

```bash
npx nx g @spartan-ng/cli:ui-theme
```

2. Add initial components:

```bash
npx nx g @spartan-ng/cli:ui
```

Follow the interactive prompts to select components.

### Adding Specific Components

To add only buttons and cards:

```bash
npx nx g @spartan-ng/cli:ui-add --components=button,card
```

### Multiple Projects in Nx Workspace

For a specific project in an Nx workspace:

```bash
npx nx g @spartan-ng/cli:ui-theme --project=my-app
npx nx g @spartan-ng/cli:ui --project=my-app
```

### Custom Component Path

To specify a custom path for generated components:

```bash
npx nx g @spartan-ng/cli:ui --path=libs/ui/components
```

## Component Selection Guide

When using the interactive UI command, here are recommendations for common use cases:

### Minimal UI Starter
- Button
- Card
- Dialog
- Input
- Typography

### Forms-Focused Application
- Button
- Checkbox
- Form
- Input
- Label
- Radio Group
- Select
- Switch
- Textarea

### Dashboard Application
- Button
- Card
- Data Table
- Dropdown Menu
- Tabs
- Toast

### Content-Rich Application
- Accordion
- Carousel
- Collapsible
- Table
- Tabs
- Typography

## CLI Workflow Patterns

### Pattern 1: Initial Setup, Incremental Additions

1. Start with theme and essential components:
   ```bash
   npx nx g @spartan-ng/cli:ui-theme
   npx nx g @spartan-ng/cli:ui-add --components=button,card,dialog,input
   ```

2. Add components as needed during development:
   ```bash
   npx nx g @spartan-ng/cli:ui-add --components=select,checkbox
   ```

### Pattern 2: Comprehensive Setup

Add all potentially needed components at the start:
```bash
npx nx g @spartan-ng/cli:ui-theme
npx nx g @spartan-ng/cli:ui
```

Select a wide range of components during the interactive prompts.

### Pattern 3: Themed Setup

1. Create a custom theme first:
   ```bash
   npx nx g @spartan-ng/cli:ui-theme --theme=modern
   ```

2. Add components after the theme is established:
   ```bash
   npx nx g @spartan-ng/cli:ui
   ```

## Managing Updates

When new versions of Spartan UI are released:

1. Update the Spartan UI packages:
   ```bash
   npm update @spartan-ng/brain @spartan-ng/cli
   ```

2. Run the update command (if available):
   ```bash
   npx nx g @spartan-ng/cli:ui-update
   ```

## Troubleshooting

### Common Issues

#### Error: Cannot find module '@spartan-ng/cli'

**Solution**: Ensure you've installed the CLI package:
```bash
npm i -D @spartan-ng/cli
```

#### Error: Failed to find project in workspace

**Solution**: Verify your project name and structure. In an Nx workspace, use:
```bash
npx nx g @spartan-ng/cli:ui-theme --project=correct-project-name
```

#### Error: Multiple projects in the workspace

**Solution**: Specify the target project:
```bash
npx nx g @spartan-ng/cli:ui --project=my-app
```

#### Warning: Component files already exist

**Solution**: To overwrite existing files, use:
```bash
npx nx g @spartan-ng/cli:ui-add --components=button --force
```

### Debug Mode

For more verbose output, use the `--debug` flag:
```bash
npx nx g @spartan-ng/cli:ui --debug
```

## Best Practices

### Project Organization

- **Create a UI Library**: In Nx workspaces, generate components in a dedicated UI library
  ```bash
  npx nx g @spartan-ng/cli:ui --project=ui-lib
  ```

- **Component Grouping**: Group related components in dedicated folders
  ```bash
  npx nx g @spartan-ng/cli:ui-add --components=form,input,checkbox,label --path=libs/ui/forms
  ```

### Performance Optimization

- **Selective Imports**: Only add components you'll actually use
- **Lazy Loading**: Consider lazy loading complex components
- **Bundle Size**: Monitor bundle size after adding new components

### Integration with Other Tools

- **Storybook**: Generate Storybook stories for your Spartan UI components
- **Custom Generators**: Create custom generators that extend Spartan UI CLI
- **CI/CD**: Automate component installation in CI/CD pipelines

## Advanced Usage

### Creating Custom Component Generators

You can extend the CLI with custom generators:

1. Create a generator file:
   ```typescript
   // my-custom-button.generator.ts
   import { generateFiles, Tree } from '@nrwl/devkit';
   import { join } from 'path';
   
   export default async function(tree: Tree, options: any) {
     generateFiles(
       tree,
       join(__dirname, 'files'),
       options.path,
       {
         ...options,
         template: '',
       }
     );
   }
   ```

2. Create template files in a 'files' folder
3. Integrate with Nx or Angular CLI

### Automated Component Updates

Create a script to update components automatically:

```bash
#!/bin/bash
# update-spartan.sh
npm update @spartan-ng/brain @spartan-ng/cli
npx nx g @spartan-ng/cli:ui-theme
npx nx g @spartan-ng/cli:ui-add --components=button,card,dialog --force
```

## Additional Resources

- [Spartan UI Documentation](https://www.spartan.ng/documentation/cli)
- [Nx Documentation](https://nx.dev/getting-started/nx-setup)
- [Angular CLI Documentation](https://angular.io/cli)