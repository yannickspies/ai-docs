# Spartan UI Dark Mode

This document explains how to implement dark mode in a Spartan UI Angular application.

## Overview

Spartan UI supports dark mode through Tailwind CSS's dark mode functionality. The implementation uses a `dark` class on the root HTML element to trigger dark mode styles. This approach:

1. Respects user preferences from their operating system settings
2. Allows manual toggling between light and dark modes
3. Persists user preferences across visits
4. Prevents flash of wrong theme during page load

## Quick Implementation

### 1. Ensure Tailwind is Configured for Class-based Dark Mode

In your `tailwind.config.js`:

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: 'class',
  // other tailwind configuration...
}
```

### 2. Add Initial Theme Detection Script

Add this script to your `index.html` file, inside the `<head>` tag before any stylesheets are loaded:

```html
<script>
  if (
    // check if user had saved dark as their 
    // theme when accessing page before
    localStorage.theme === 'dark' ||
    // or user's requesting dark color 
    // scheme through operating system
    (!('theme' in localStorage) &&
      window.matchMedia('(prefers-color-scheme: dark)').matches)
  ) {
    // then if we have access to the document and the element
    // we add the dark class to the html element and
    // store the dark value in the localStorage
    if (document && document.documentElement) {
      document.documentElement.classList.add('dark');
      localStorage.setItem('theme', 'dark');
    }
  } else {
    // else if we have access to the document and the element
    // we remove the dark class to the html element and 
    // store the value light in the localStorage
    if (document && document.documentElement) {
      document.documentElement.classList.remove('dark');
      localStorage.setItem('theme', 'light');
    }
  }
</script>
```

This script:
- Checks if the user previously selected dark mode
- If not, checks if their system prefers dark mode
- Applies the appropriate class and saves the preference
- Executes before any content is rendered to prevent flashing

### 3. Create a Theme Service to Manage Dark Mode

Create a theme service to manage dark mode throughout your application:

```typescript
import { Injectable, OnDestroy, Renderer2, RendererFactory2, inject } from '@angular/core';
import { DOCUMENT } from '@angular/common';
import { PLATFORM_ID } from '@angular/core';
import { isPlatformBrowser } from '@angular/common';
import { ReplaySubject, Subject } from 'rxjs';
import { takeUntil } from 'rxjs/operators';

@Injectable({
  providedIn: 'root',
})
export class ThemeService implements OnDestroy {
  // Platform ID to check if we're in a browser context
  private _platformId = inject(PLATFORM_ID);
  // Renderer for DOM manipulation
  private _renderer = inject(RendererFactory2).createRenderer(null, null);
  // Document reference
  private _document = inject(DOCUMENT);

  // Theme state with replay to provide current value to new subscribers
  private _theme$ = new ReplaySubject<'light' | 'dark'>(1);
  public theme$ = this._theme$.asObservable();
  // Cleanup subject for unsubscribing on service destroy
  private _destroyed$ = new Subject<void>();

  constructor() {
    this.syncThemeFromLocalStorage();
    this.toggleClassOnThemeChanges();
  }

  // Load theme from localStorage
  private syncThemeFromLocalStorage(): void {
    if (isPlatformBrowser(this._platformId)) {
      this._theme$.next(
        localStorage.getItem('theme') === 'dark' ? 'dark' : 'light'
      );
    }
  }

  // Apply or remove dark class based on theme changes
  private toggleClassOnThemeChanges(): void {
    this.theme$.pipe(takeUntil(this._destroyed$)).subscribe((theme) => {
      if (theme === 'dark') {
        this._renderer.addClass(this._document.documentElement, 'dark');
      } else {
        if (this._document.documentElement.className.includes('dark')) {
          this._renderer.removeClass(this._document.documentElement, 'dark');
        }
      }
    });
  }

  // Public method to toggle theme
  public toggleDarkMode(): void {
    const newTheme =
      localStorage.getItem('theme') === 'dark' ? 'light' : 'dark';
    localStorage.setItem('theme', newTheme);
    this._theme$.next(newTheme);
  }

  // Cleanup on service destroy
  public ngOnDestroy(): void {
    this._destroyed$.next();
    this._destroyed$.complete();
  }
}
```

### 4. Add a Theme Toggle to Your Application

Use the service to add a toggle button in your application:

```typescript
import { Component, inject } from '@angular/core';
import { AsyncPipe } from '@angular/common';
import { ThemeService } from './path-to-theme-service';

@Component({
  selector: 'app-theme-toggle',
  standalone: true,
  imports: [AsyncPipe],
  template: `
    <button 
      hlmBtn
      variant="ghost"
      size="icon"
      (click)="toggleTheme()" 
      aria-label="Toggle theme"
    >
      <span *ngIf="(theme$ | async) === 'light'" class="icon">🌙</span>
      <span *ngIf="(theme$ | async) === 'dark'" class="icon">☀️</span>
    </button>
  `
})
export class ThemeToggleComponent {
  private _themeService = inject(ThemeService);
  public theme$ = this._themeService.theme$;

  public toggleTheme(): void {
    this._themeService.toggleDarkMode();
  }
}
```

## Using Dark Mode Classes

With dark mode configured, you can use Tailwind's dark mode variants in your templates:

```html
<div class="bg-white dark:bg-slate-800 text-black dark:text-white">
  This content adapts to light/dark mode
</div>
```

Spartan UI components automatically adapt to dark mode when the `dark` class is present.

## Testing Dark Mode

To test your dark mode implementation:

1. Toggle dark mode with your theme switch
2. Check system preference changes (in browser devtools or OS settings)
3. Test persistence by reloading the page
4. Check initial load with both preferences

## Advanced Customization

### Theme Transition Animations

Add smooth transitions between themes:

```css
/* In your global CSS */
html {
  transition: background-color 0.3s ease, color 0.3s ease;
}

* {
  transition: background-color 0.3s ease, border-color 0.3s ease, color 0.3s ease;
}
```

### Adding Additional Themes

You can extend the service to support more than just light and dark:

```typescript
// In ThemeService
private _theme$ = new ReplaySubject<'light' | 'dark' | 'system' | 'custom'>(1);

// Method to set specific theme
public setTheme(theme: 'light' | 'dark' | 'system' | 'custom'): void {
  localStorage.setItem('theme', theme);
  
  if (theme === 'system') {
    const systemPrefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    this._theme$.next(systemPrefersDark ? 'dark' : 'light');
    // Apply appropriate class
    if (systemPrefersDark) {
      this._renderer.addClass(this._document.documentElement, 'dark');
      this._renderer.removeClass(this._document.documentElement, 'custom');
    } else {
      this._renderer.removeClass(this._document.documentElement, 'dark');
      this._renderer.removeClass(this._document.documentElement, 'custom');
    }
  } else {
    this._theme$.next(theme);
    
    // Apply appropriate classes
    if (theme === 'dark') {
      this._renderer.addClass(this._document.documentElement, 'dark');
      this._renderer.removeClass(this._document.documentElement, 'custom');
    } else if (theme === 'custom') {
      this._renderer.removeClass(this._document.documentElement, 'dark');
      this._renderer.addClass(this._document.documentElement, 'custom');
    } else {
      this._renderer.removeClass(this._document.documentElement, 'dark');
      this._renderer.removeClass(this._document.documentElement, 'custom');
    }
  }
}
```

### Listening to System Preference Changes

To react to system theme changes while the app is running:

```typescript
// In ThemeService constructor
constructor() {
  this.syncThemeFromLocalStorage();
  this.toggleClassOnThemeChanges();
  this.listenForSystemPreferenceChanges();
}

private listenForSystemPreferenceChanges(): void {
  if (isPlatformBrowser(this._platformId)) {
    const mediaQuery = window.matchMedia('(prefers-color-scheme: dark)');
    
    const listener = (event: MediaQueryListEvent) => {
      if (localStorage.getItem('theme') === 'system') {
        const newTheme = event.matches ? 'dark' : 'light';
        this._theme$.next(newTheme);
      }
    };
    
    mediaQuery.addEventListener('change', listener);
    
    // Clean up listener on destroy
    this._destroyed$.subscribe(() => {
      mediaQuery.removeEventListener('change', listener);
    });
  }
}
```

## Troubleshooting

### Theme Flash on Page Load

If you experience a flash of the wrong theme when loading the page:

1. Make sure the script is placed in the `<head>` before any stylesheets
2. Check that the theme variables are properly defined in your CSS

### Theme Not Persisting

If theme preferences aren't persisting:

1. Check localStorage access (private browsing may block it)
2. Ensure localStorage keys match between script and service
3. Verify the theme toggle is updating localStorage correctly

### Dark Mode Styles Not Applied

If dark mode styles aren't being applied:

1. Check that Tailwind is configured with `darkMode: 'class'`
2. Ensure the `dark` class is being added to the HTML element
3. Verify dark mode variants are correctly defined in your templates

## Additional Resources

- [Spartan UI Documentation](https://www.spartan.ng/documentation/dark-mode)
- [Dark Mode with Analog & Tailwind](https://dev.to/this-is-angular/dark-mode-with-analog-tailwind-4049)
- [Tailwind Dark Mode Documentation](https://tailwindcss.com/docs/dark-mode)