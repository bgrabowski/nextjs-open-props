# Semantic Tokens Reference

This file contains the complete token system for an Open Props + CSS Modules project. Copy the `globals.css` template when scaffolding a new project, and reference this file when adding new semantic tokens.

## Table of Contents

1. [Open Props Imports](#open-props-imports)
2. [Light Mode Tokens (`:root`)](#light-mode-tokens)
3. [Dark Mode Tokens (`[data-theme='dark']`)](#dark-mode-tokens)
4. [Base Styles](#base-styles)
5. [Typography Scale](#typography-scale)
6. [Spacing Scale](#spacing-scale)
7. [Adding New Tokens](#adding-new-tokens)

---

## Open Props Imports

```css
/* Full bundle (~1.5KB gzipped) */
@import "open-props/style";
@import "open-props/normalize";

/* OR selective imports for smaller bundle */
@import "open-props/colors";
@import "open-props/sizes";
@import "open-props/fonts";
@import "open-props/shadows";
@import "open-props/easings";
@import "open-props/borders";
@import "open-props/animations";
@import "open-props/aspects";
```

Use the full bundle unless you're optimizing aggressively. Open Props is already tiny.

---

## Light Mode Tokens

These go on `:root` in `globals.css`. Every component references these semantic names — never raw Open Props tokens.

```css
:root {
  /* ==========================================
     BRAND COLORS
     ========================================== */
  --color-primary: var(--blue-7);
  --color-primary-hover: var(--blue-8);
  --color-primary-active: var(--blue-9);
  --color-primary-subtle: var(--blue-2);     /* Light tint for backgrounds */
  --color-primary-text: var(--gray-0);       /* Text on primary backgrounds */

  /* ==========================================
     TEXT
     ========================================== */
  --color-text-1: var(--gray-9);             /* Headlines, high emphasis */
  --color-text-2: var(--gray-7);             /* Body text, medium emphasis */
  --color-text-3: var(--gray-5);             /* Captions, low emphasis */
  --color-text-inverse: var(--gray-0);       /* Text on dark backgrounds */

  /* ==========================================
     SURFACES & BACKGROUNDS
     ========================================== */
  --color-bg: var(--gray-0);                 /* Page background */
  --color-surface-1: var(--gray-1);          /* Cards, alternate sections */
  --color-surface-2: var(--gray-2);          /* Nested surfaces, hover states */
  --color-surface-3: var(--gray-3);          /* Deeper nesting */
  --color-border: var(--gray-3);             /* Dividers, card borders */
  --color-border-subtle: var(--gray-2);      /* Subtle separators */

  /* ==========================================
     SEMANTIC / STATUS COLORS
     ========================================== */
  --color-accent: var(--orange-7);
  --color-accent-hover: var(--orange-8);
  --color-accent-subtle: var(--orange-2);

  --color-destructive: var(--red-7);
  --color-destructive-hover: var(--red-8);
  --color-destructive-subtle: var(--red-2);

  --color-success: var(--green-7);
  --color-success-hover: var(--green-8);
  --color-success-subtle: var(--green-2);

  --color-warning: var(--yellow-6);
  --color-warning-subtle: var(--yellow-2);

  --color-info: var(--blue-6);
  --color-info-subtle: var(--blue-1);

  /* ==========================================
     LINKS
     ========================================== */
  --color-link: var(--blue-7);
  --color-link-hover: var(--blue-8);
  --color-link-visited: var(--indigo-7);

  /* ==========================================
     FOCUS
     ========================================== */
  --color-focus-ring: var(--blue-6);

  /* ==========================================
     LAYOUT
     ========================================== */
  --layout-max-width: 1280px;
  --layout-content-padding: var(--size-4);

  /* ==========================================
     SHADOWS (semantic wrappers)
     ========================================== */
  --shadow-card: var(--shadow-2);
  --shadow-card-hover: var(--shadow-3);
  --shadow-dropdown: var(--shadow-4);
  --shadow-modal: var(--shadow-5);
}
```

---

## Dark Mode Tokens

These override `:root` when `data-theme="dark"` is set on `<html>`. The pattern is to flip to the lighter end of each color scale so elements maintain contrast against dark backgrounds.

```css
[data-theme='dark'] {
  /* ==========================================
     BRAND COLORS
     ========================================== */
  --color-primary: var(--blue-4);
  --color-primary-hover: var(--blue-3);
  --color-primary-active: var(--blue-2);
  --color-primary-subtle: var(--blue-9);
  --color-primary-text: var(--gray-9);

  /* ==========================================
     TEXT
     ========================================== */
  --color-text-1: var(--gray-1);
  --color-text-2: var(--gray-3);
  --color-text-3: var(--gray-5);
  --color-text-inverse: var(--gray-9);

  /* ==========================================
     SURFACES & BACKGROUNDS
     ========================================== */
  --color-bg: var(--gray-9);
  --color-surface-1: var(--gray-8);
  --color-surface-2: var(--gray-7);
  --color-surface-3: var(--gray-6);
  --color-border: var(--gray-7);
  --color-border-subtle: var(--gray-8);

  /* ==========================================
     SEMANTIC / STATUS COLORS
     ========================================== */
  --color-accent: var(--orange-4);
  --color-accent-hover: var(--orange-3);
  --color-accent-subtle: var(--orange-9);

  --color-destructive: var(--red-4);
  --color-destructive-hover: var(--red-3);
  --color-destructive-subtle: var(--red-9);

  --color-success: var(--green-4);
  --color-success-hover: var(--green-3);
  --color-success-subtle: var(--green-9);

  --color-warning: var(--yellow-4);
  --color-warning-subtle: var(--yellow-9);

  --color-info: var(--blue-4);
  --color-info-subtle: var(--blue-9);

  /* ==========================================
     LINKS
     ========================================== */
  --color-link: var(--blue-4);
  --color-link-hover: var(--blue-3);
  --color-link-visited: var(--indigo-4);

  /* ==========================================
     FOCUS
     ========================================== */
  --color-focus-ring: var(--blue-4);

  /* ==========================================
     SHADOWS
     ========================================== */
  --shadow-card: var(--shadow-2);
  --shadow-card-hover: var(--shadow-3);
  --shadow-dropdown: var(--shadow-4);
  --shadow-modal: var(--shadow-5);
}
```

---

## Base Styles

Add these after the token definitions in `globals.css`:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
}

html {
  color-scheme: light;
  scroll-behavior: smooth;
}

html[data-theme='dark'] {
  color-scheme: dark;
}

body {
  font-family: var(--font-sans), system-ui, sans-serif;
  font-size: var(--font-size-1);
  font-weight: var(--font-weight-4);
  line-height: var(--font-lineheight-3);
  color: var(--color-text-2);
  background: var(--color-bg);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

h1, h2, h3, h4, h5, h6 {
  color: var(--color-text-1);
  line-height: var(--font-lineheight-1);
  text-wrap: balance;
}

h1 { font-size: var(--font-size-6); font-weight: var(--font-weight-7); }
h2 { font-size: var(--font-size-5); font-weight: var(--font-weight-7); }
h3 { font-size: var(--font-size-3); font-weight: var(--font-weight-6); }
h4 { font-size: var(--font-size-2); font-weight: var(--font-weight-6); }

p {
  max-width: var(--size-content-3);
  text-wrap: pretty;
}

a {
  color: var(--color-link);
  text-decoration: none;
  transition: color 0.15s var(--ease-3);

  &:hover {
    color: var(--color-link-hover);
  }

  &:focus-visible {
    outline: 2px solid var(--color-focus-ring);
    outline-offset: 2px;
    border-radius: var(--radius-1);
  }
}

img, picture, video, canvas, svg {
  display: block;
  max-width: 100%;
}

code {
  font-family: var(--font-mono), monospace;
  font-size: var(--font-size-0);
}

@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

## Typography Scale

Open Props font-size tokens map to these approximate values:

| Token | ~Size | Use For |
|---|---|---|
| `--font-size-00` | 12px | Captions, fine print |
| `--font-size-0` | 14px | Small text, labels |
| `--font-size-1` | 16px | Body text (base) |
| `--font-size-2` | 18px | Large body, lead text |
| `--font-size-3` | 20px | H4, subheadings |
| `--font-size-4` | 24px | H3 |
| `--font-size-5` | 32px | H2 |
| `--font-size-6` | 48px | H1 |
| `--font-size-7` | 64px | Display / hero |
| `--font-size-8` | 96px | Jumbo display |

Font weight tokens:

| Token | Weight | Use For |
|---|---|---|
| `--font-weight-3` | 300 | Light |
| `--font-weight-4` | 400 | Regular / body |
| `--font-weight-5` | 500 | Medium / labels |
| `--font-weight-6` | 600 | Semibold / subheadings |
| `--font-weight-7` | 700 | Bold / headings |
| `--font-weight-9` | 900 | Black / display |

---

## Spacing Scale

Open Props size tokens for reference:

| Token | ~Size | Common Use |
|---|---|---|
| `--size-1` | 4px | Tight gaps, icon padding |
| `--size-2` | 8px | Button padding (block), small gaps |
| `--size-3` | 12px | Component gaps |
| `--size-4` | 16px | Card padding, section side padding |
| `--size-5` | 20px | Card padding (generous) |
| `--size-6` | 24px | Section side padding (md+) |
| `--size-7` | 32px | Large button inline padding |
| `--size-8` | 40px | Content edge margins |
| `--size-9` | 48px | Mid-size section gaps |
| `--size-10` | 56px | Section vertical padding |
| `--size-11` | 64px | Large section gaps |
| `--size-12` | 80px | Hero-level spacing |

---

## Adding New Tokens

When a component needs a semantic value that doesn't exist yet, add it to both `:root` and `[data-theme='dark']` in `globals.css`. Follow these conventions:

- Prefix with `--color-` for colors, `--shadow-` for shadows, `--layout-` for layout values
- Reference an Open Props token, don't hardcode a hex value
- For dark mode, flip to the appropriate end of the scale (lighter numbers for colors that need to be bright on dark backgrounds)

**Example — adding a new "highlight" color:**

```css
:root {
  --color-highlight: var(--yellow-3);
  --color-highlight-text: var(--gray-9);
}

[data-theme='dark'] {
  --color-highlight: var(--yellow-7);
  --color-highlight-text: var(--gray-0);
}
```
