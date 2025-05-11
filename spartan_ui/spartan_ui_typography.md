# Spartan UI Typography

This document details the typography system in Spartan UI and how to use it in your Angular applications.

## Overview

Spartan UI provides a comprehensive typography system with styles for headings, paragraphs, lists, and other text elements. The typography system is designed to be consistent, accessible, and customizable.

## Installation

To add typography components to your project:

```bash
npx nx g @spartan-ng/cli:ui typography
```

This will add the necessary typography components to your project.

## Typography Elements

### Headings

Spartan UI provides styles for four levels of headings:

```html
<h1 hlmH1>The King's Plan</h1>
<h2 hlmH2>The People's Rebellion</h2>
<h3 hlmH3>The Joke Tax</h3>
<h4 hlmH4>People stopped telling jokes</h4>
```

You can also apply these styles to other elements:

```html
<div hlmH1>Looks like a heading 1</div>
```

### Paragraphs

Standard paragraph styling:

```html
<p hlmP>The king, seeing how much happier his subjects were, realized the error of his ways and repealed the joke tax.</p>
```

### Blockquotes

For quoted content:

```html
<blockquote hlmBlockquote>
  "After all," he said, "everyone enjoys a good joke, so it's only fair that they should pay for the privilege."
</blockquote>
```

### Lists

Styled lists:

```html
<ul hlmUl>
  <li>1st level of puns: 5 gold coins</li>
  <li>2nd level of jokes: 10 gold coins</li>
  <li>3rd level of one-liners: 20 gold coins</li>
</ul>

<ol hlmOl>
  <li>Find the court jester</li>
  <li>Pay the joke tax</li>
  <li>Tell your joke</li>
</ol>
```

### Inline Code

For code snippets:

```html
<code hlmCode>@radix-ui/react-alert-dialog</code>
```

### Lead Paragraph

For introducing sections with larger, more prominent text:

```html
<p hlmLead>A modal dialog that interrupts the user with important content and expects a response.</p>
```

### Large Text

For emphasizing text:

```html
<p hlmLarge>Are you sure absolutely sure?</p>
```

### Small Text

For de-emphasized or supporting text:

```html
<p hlmSmall>Email address</p>
```

### Muted Text

For secondary information:

```html
<p hlmMuted>Enter your email address.</p>
```

## Customizing Typography

### Font Family

The font family is controlled by the `--font-sans` CSS variable. To customize the font family:

1. Add your preferred font to your project (e.g., via Google Fonts or local fonts)
2. Update the CSS variable in your global stylesheet:

```css
:root {
  --font-sans: 'Your Font', system-ui, sans-serif;
}
```

### Font Sizes

Font sizes are managed through Tailwind CSS classes. You can customize them in your `tailwind.config.js`:

```javascript
module.exports = {
  theme: {
    extend: {
      fontSize: {
        'xs': ['0.75rem', { lineHeight: '1rem' }],
        'sm': ['0.875rem', { lineHeight: '1.25rem' }],
        'base': ['1rem', { lineHeight: '1.5rem' }],
        'lg': ['1.125rem', { lineHeight: '1.75rem' }],
        'xl': ['1.25rem', { lineHeight: '1.75rem' }],
        '2xl': ['1.5rem', { lineHeight: '2rem' }],
        '3xl': ['1.875rem', { lineHeight: '2.25rem' }],
        '4xl': ['2.25rem', { lineHeight: '2.5rem' }],
        '5xl': ['3rem', { lineHeight: '1' }],
        '6xl': ['3.75rem', { lineHeight: '1' }],
      }
    }
  }
}
```

### Line Heights

Line heights are typically included with font sizes in Tailwind, but you can customize them separately:

```javascript
module.exports = {
  theme: {
    extend: {
      lineHeight: {
        'tight': '1.25',
        'normal': '1.5',
        'relaxed': '1.75',
        'loose': '2',
      }
    }
  }
}
```

### Colors

Text colors can be customized using the theming system's CSS variables. For example:

```css
:root {
  --foreground: 240 10% 3.9%; /* Default text color */
  --muted-foreground: 240 3.8% 46.1%; /* Muted text color */
}

.dark {
  --foreground: 0 0% 98%; /* Dark mode text color */
  --muted-foreground: 240 5% 64.9%; /* Dark mode muted text color */
}
```

## Typography Component Structure

Under the hood, Spartan UI typography components apply appropriate tailwind classes. For example, the heading components might look like:

```typescript
@Component({
  selector: 'h1[hlmH1], [hlmH1]',
  standalone: true,
  template: '<ng-content></ng-content>',
  host: {
    class: 'scroll-m-20 text-4xl font-extrabold tracking-tight lg:text-5xl'
  }
})
export class HlmH1Component {}
```

You can create similar custom typography components if needed.

## Responsive Typography

Spartan UI typography components are responsive by default, using Tailwind's responsive prefixes:

```html
<h1 hlmH1 class="text-2xl md:text-3xl lg:text-4xl">Responsive Heading</h1>
```

## Combining with Other Styles

Typography components can be combined with other Tailwind classes:

```html
<p hlmP class="mt-4 mb-2 text-blue-600 dark:text-blue-400">
  Customized paragraph
</p>
```

## Accessibility Considerations

When using typography components, keep these accessibility considerations in mind:

1. **Heading Structure**: Use headings in a logical hierarchy (h1 → h2 → h3)
2. **Text Contrast**: Ensure sufficient contrast between text and background colors
3. **Font Size**: Avoid very small font sizes that are difficult to read
4. **Line Length**: Keep paragraphs to a reasonable width (around 60-75 characters)
5. **Line Height**: Use adequate line height for readability (typically 1.5-2x font size)

## Examples

### Article Layout

```html
<article>
  <h1 hlmH1>The King's Plan</h1>
  <p hlmLead>Once upon a time, in a far-off land, there was a very lazy king who spent all day lounging on his throne.</p>
  
  <h2 hlmH2>The Joke Tax</h2>
  <p hlmP>The king's subjects were not amused. They grumbled and complained, but the king was firm.</p>
  
  <blockquote hlmBlockquote>
    "After all," he said, "everyone enjoys a good joke, so it's only fair that they should pay for the privilege."
  </blockquote>
  
  <h3 hlmH3>Tax Rates</h3>
  <ul hlmUl>
    <li>1st level of puns: 5 gold coins</li>
    <li>2nd level of jokes: 10 gold coins</li>
    <li>3rd level of one-liners : 20 gold coins</li>
  </ul>
</article>
```

### Form Labels

```html
<form>
  <div class="mb-4">
    <label hlmLabel for="email">Email address</label>
    <p hlmMuted>Enter your email address.</p>
    <input id="email" type="email" />
  </div>
  
  <div class="mb-4">
    <label hlmLabel for="password">Password</label>
    <p hlmSmall>Must be at least 8 characters.</p>
    <input id="password" type="password" />
  </div>
</form>
```

### Card Content

```html
<div hlmCard>
  <div hlmCardHeader>
    <h3 hlmCardTitle>Feature Highlight</h3>
    <p hlmCardDescription>Learn about this amazing feature</p>
  </div>
  <div hlmCardContent>
    <p hlmP>This feature will revolutionize your workflow by automating repetitive tasks.</p>
    
    <p hlmLead class="mt-4">Key benefits include:</p>
    <ul hlmUl>
      <li>Increased productivity</li>
      <li>Reduced errors</li>
      <li>Better collaboration</li>
    </ul>
  </div>
  <div hlmCardFooter>
    <p hlmMuted>Available in the premium plan</p>
  </div>
</div>
```

## Additional Resources

- [Spartan UI Documentation](https://www.spartan.ng/documentation/typography)
- [Tailwind Typography Plugin](https://tailwindcss.com/docs/typography-plugin)
- [Web Content Accessibility Guidelines - Text Alternatives](https://www.w3.org/WAI/WCAG21/Understanding/text-alternatives)