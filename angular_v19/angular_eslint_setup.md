# Adding Angular ESLint to Your Angular Project

This guide explains how to add ESLint to an Angular project along with Prettier, Husky, and lint-staged for a complete code quality setup.

## Why Angular ESLint?

Angular ESLint provides specialized linting rules for Angular projects, helping to:

- Enforce Angular best practices and style guidelines
- Catch common errors during development
- Maintain consistency across your codebase
- Improve code quality

## Setting Up Angular ESLint

### Installing Angular ESLint

For an existing Angular project, use the Angular CLI to add Angular ESLint:

```bash
ng add @angular-eslint/schematics
```

This command:
- Installs all relevant dependencies
- Sets up the necessary configuration in your workspace
- Updates `angular.json` to include linting configuration
- Creates `.eslintrc.json` files as needed

If your project previously used TSLint or Codelyzer, the Angular CLI will help migrate your existing rules.

For a new Angular project, you can add Angular ESLint during project creation:

```bash
ng new my-app --eslint
```

### Manual Installation

If you prefer to install Angular ESLint manually:

```bash
# Install the necessary packages
npm install -D @angular-eslint/builder @angular-eslint/eslint-plugin @angular-eslint/eslint-plugin-template @angular-eslint/schematics @angular-eslint/template-parser eslint
```

Then, generate the ESLint configuration:

```bash
ng g @angular-eslint/schematics:add-eslint-to-project [PROJECT_NAME]
```

### Configuration Structure

After installation, you'll have several ESLint configuration files:

- `.eslintrc.json` in the root for project-wide settings
- Additional `.eslintrc.json` files in specific project directories

A typical `.eslintrc.json` for an Angular project might look like:

```json
{
  "root": true,
  "ignorePatterns": ["projects/**/*"],
  "overrides": [
    {
      "files": ["*.ts"],
      "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:@angular-eslint/recommended",
        "plugin:@angular-eslint/template/process-inline-templates"
      ],
      "rules": {
        "@angular-eslint/directive-selector": [
          "error",
          {
            "type": "attribute",
            "prefix": "app",
            "style": "camelCase"
          }
        ],
        "@angular-eslint/component-selector": [
          "error",
          {
            "type": "element",
            "prefix": "app",
            "style": "kebab-case"
          }
        ]
      }
    },
    {
      "files": ["*.html"],
      "extends": [
        "plugin:@angular-eslint/template/recommended"
      ],
      "rules": {}
    }
  ]
}
```

### ESLint v9 and Flat Config

For Angular v18+ projects, you can use ESLint's new flat config format through the `eslint.config.js` file:

```javascript
// eslint.config.js
import angular from 'angular-eslint';

export default [
  ...angular.configs.recommended,
  {
    files: ['src/**/*.ts'],
    rules: {
      // Custom rules here
    }
  },
  {
    files: ['src/**/*.html'],
    rules: {
      // Custom template rules here
    }
  }
];
```

## Running ESLint

### Using Angular CLI

To lint your project using the Angular CLI:

```bash
ng lint
```

To automatically fix issues where possible:

```bash
ng lint --fix
```

### Using npm Scripts

Add these scripts to your `package.json`:

```json
{
  "scripts": {
    "lint": "ng lint",
    "lint:fix": "ng lint --fix"
  }
}
```

## Adding Prettier for Code Formatting

While ESLint helps with code quality, Prettier focuses on consistent formatting.

### Installation

```bash
npm install -D prettier
```

### Configuration

Create a `.prettierrc` file in your project root:

```json
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "proseWrap": "always"
}
```

Also, create a `.prettierignore` file:

```
dist
node_modules
coverage
.angular
```

### Integrating Prettier with ESLint

It's recommended to have Prettier handle formatting and ESLint handle code quality. To prevent conflicts:

```bash
npm install -D eslint-config-prettier
```

Then update your `.eslintrc.json`:

```json
{
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/recommended",
    "plugin:@angular-eslint/recommended",
    "plugin:@angular-eslint/template/process-inline-templates",
    "prettier" // Add this line
  ]
}
```

If using ESLint flat config:

```javascript
// eslint.config.js
import angular from 'angular-eslint';
import prettier from 'eslint-config-prettier';

export default [
  ...angular.configs.recommended,
  prettier,
  // Your custom configurations...
];
```

### Adding Unused Imports Plugin

To automatically remove unused imports, install:

```bash
npm install -D eslint-plugin-unused-imports
```

Update your ESLint configuration:

```json
{
  "plugins": ["unused-imports"],
  "rules": {
    "unused-imports/no-unused-imports": "error",
    "unused-imports/no-unused-vars": [
      "warn",
      { "vars": "all", "varsIgnorePattern": "^_", "args": "after-used", "argsIgnorePattern": "^_" }
    ]
  }
}
```

## Setting Up Pre-commit Hooks with Husky and lint-staged

### Why Use Pre-commit Hooks?

Pre-commit hooks ensure that only properly formatted and error-free code is committed to your repository.

### Installing Husky and lint-staged

```bash
npm install -D husky lint-staged
```

### Initializing Husky

```bash
npx husky init
```

This creates a `.husky` directory with initial configuration.

### Configuring lint-staged

Create a `.lintstagedrc` file:

```json
{
  "*.ts": [
    "prettier --write",
    "eslint --fix"
  ],
  "*.html": [
    "prettier --write",
    "eslint --fix"
  ],
  "*.{css,scss}": [
    "prettier --write"
  ],
  "*.{json,md}": [
    "prettier --write"
  ]
}
```

### Setting Up Husky Pre-commit Hook

Edit `.husky/pre-commit`:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged
```

Make the file executable if needed:

```bash
chmod +x .husky/pre-commit
```

### Adding Scripts to package.json

Update your `package.json` scripts:

```json
{
  "scripts": {
    "lint": "ng lint",
    "lint:fix": "ng lint --fix",
    "format": "prettier --write \"src/**/*.{ts,html,css,scss,json,md}\"",
    "prepare": "husky"
  }
}
```

## Adding Sheriff for Architectural Rules

Sheriff allows you to enforce module boundaries and dependency rules.

### Installation

```bash
npm install -D @softarc/sheriff-core @softarc/eslint-plugin-sheriff
```

### Configuration

Create a `sheriff.config.ts` file:

```typescript
import { ShepherdConfig } from '@softarc/sheriff-core';

const config: ShepherdConfig = {
  projects: [
    {
      name: "my-app",
      root: "src",
      sourceRoot: "src",
      buildable: true,
      tags: ["app"]
    }
  ],
  boundaries: [
    {
      tags: ["app"],
      allowedTags: ["feature", "shared"]
    },
    {
      tags: ["feature"],
      allowedTags: ["shared"]
    }
  ]
};

export default config;
```

Add Sheriff to your ESLint configuration:

```json
{
  "plugins": ["@softarc/sheriff"],
  "extends": [
    "plugin:@softarc/sheriff/recommended"
  ],
  "rules": {
    "@softarc/sheriff/dependency-rule": "error"
  }
}
```

## Best Practices

1. **Gradually Adopt Rules**: Start with basic rules and gradually add more as your team gets comfortable.

2. **Document Customizations**: Keep a record of why you've customized specific rules for your project.

3. **Regular Updates**: Keep your ESLint and plugins updated to benefit from new rules and improvements.

4. **CI Integration**: Add linting to your CI pipeline to catch issues early.

```yaml
# Example GitHub Action
name: Lint

on: [push, pull_request]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - uses: actions/setup-node@v3
      with:
        node-version: 18
    - run: npm ci
    - run: npm run lint
```

5. **VSCode Integration**: Install the ESLint extension for Visual Studio Code for real-time linting.

## Troubleshooting

### Common Issues

1. **Configuration Conflicts**: If you see conflicting configuration errors, ensure you're properly extending configurations and using the correct order.

2. **Performance Issues**: Large Angular projects may have slower lint times. Consider using `--cache` option or linting only changed files.

3. **Rule Conflicts**: If ESLint and Prettier rules conflict, ensure the prettier config is the last extension in your ESLint configuration.

### Disabling Rules

Sometimes you might need to disable specific rules:

```typescript
// Disable for a file
/* eslint-disable @angular-eslint/component-selector */

// Disable for a line
const x = 10; // eslint-disable-line @typescript-eslint/no-magic-numbers

// Disable for next line
// eslint-disable-next-line @typescript-eslint/no-explicit-any
const data: any = {};
```

For templates:

```html
<!-- eslint-disable-next-line @angular-eslint/template/no-negated-async -->
<div *ngIf="!(isLoading$ | async)">Content</div>
```

## Conclusion

By using Angular ESLint with Prettier, Husky, lint-staged, and Sheriff, you create a comprehensive code quality system that:

- Enforces consistent code style
- Finds potential bugs and issues early
- Maintains architectural boundaries
- Ensures all committed code meets your team's standards

This setup reduces review cycles, prevents common errors, and helps maintain a clean, consistent codebase as your project grows.

## Additional Resources

- [Angular ESLint GitHub Repository](https://github.com/angular-eslint/angular-eslint)
- [ESLint Documentation](https://eslint.org/docs/user-guide/)
- [Prettier Documentation](https://prettier.io/docs/en/)
- [Husky Documentation](https://typicode.github.io/husky/)
- [lint-staged Documentation](https://github.com/okonet/lint-staged)
- [Sheriff Documentation](https://github.com/softarc-consulting/sheriff)