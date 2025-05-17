# Tailwind CSS for Angular

This guide walks through the process of setting up Tailwind CSS in an Angular project.

## Installation Steps

### 1. Create your project

Start by creating a new Angular project if you don't have one set up already. The most common approach is to use [Angular CLI](https://angular.dev/tools/cli/setup-local).

```bash
ng new my-project --style css
cd my-project
```

### 2. Install Tailwind CSS

Install `@tailwindcss/postcss` and its peer dependencies via npm.

```bash
npm install tailwindcss @tailwindcss/postcss postcss --force
```

### 3. Configure PostCSS Plugins

Create a `.postcssrc.json` file in the root of your project and add the `@tailwindcss/postcss` plugin to your PostCSS configuration.

```json
{
  "plugins": {
    "@tailwindcss/postcss": {}
  }
}
```

### 4. Import Tailwind CSS

Add an `@import` to `./src/styles.css` that imports Tailwind CSS.

```css
@import "tailwindcss";
```

### 5. Start your build process

Run your build process with `ng serve`.

```bash
ng serve
```

### 6. Start using Tailwind in your project

Start using Tailwind's utility classes to style your content.

```html
<h1 class="text-3xl font-bold underline">
  Hello world!
</h1>
```

## Additional Resources

- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [Angular Framework](https://angular.dev)
- [PostCSS](https://postcss.org)