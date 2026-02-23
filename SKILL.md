---
name: nextjs-open-props
description: Build Next.js websites and components using Open Props design tokens and CSS Modules. Use this skill whenever the user wants to create, scaffold, or build pages, components, layouts, or sections for a Next.js project that uses Open Props instead of Tailwind CSS. Also trigger when the user asks to add a new component, page, or section to an existing Open Props + CSS Modules project, when they mention Open Props by name, when they want to set up dark mode theming with CSS custom properties, or when they want to convert Tailwind-based components to CSS Modules. Do NOT use this skill for projects that explicitly use Tailwind CSS or other utility-class frameworks.
---

# Next.js + Open Props Component Builder

This skill generates production-quality Next.js components, pages, and layouts using Open Props for design tokens and CSS Modules for scoped styling. Every output follows a consistent architecture: semantic CSS custom properties, co-located `.module.css` files, mobile-first responsive design, and built-in dark mode support.

## Interactive Setup Flow

Before generating any code, walk the user through these setup questions to gather the information needed to build their project correctly. Ask these in a conversational way — don't dump all questions at once. Group them into natural stages and confirm answers before moving on.

### Stage 1: Project Foundation

Ask the user:

1. **"What is this website for?"**
   Get a brief description of the project — who it's for, what it does, and the general purpose (marketing site, portfolio, SaaS landing page, blog, agency site, etc.). This shapes every decision that follows.

2. **"What pages do you need?"**
   Get a list of pages. Offer common suggestions if the user isn't sure:
   - Home
   - About
   - Services / Features
   - Pricing
   - Blog (listing + posts)
   - Contact
   - FAQ
   - Portfolio / Work
   - Careers
   - Terms / Privacy

   For each page, ask if there are subpages (e.g., individual service pages under Services). This directly defines the `app/` routing structure.

3. **"What sections should your homepage include?"**
   Walk through common homepage sections and let them pick:
   - Hero (headline + CTA)
   - Features / benefits grid
   - Testimonials / social proof
   - Stats / numbers
   - Logo cloud / trusted by
   - Pricing preview
   - CTA band
   - FAQ
   - Contact form

   This determines which patterns from `references/page-sections.md` to use.

### Stage 2: Visual Identity

Ask the user:

4. **"What's your primary brand color?"**
   Get a specific color — hex value, color name, or a reference ("the blue from our logo"). Map it to the closest Open Props color scale. If they say "blue," ask if it's a bright/vibrant blue (like `--blue-7`) or a deeper/darker blue (like `--blue-9`) or a softer blue (like `--blue-5`).

   The primary color is used for: buttons, links, focus rings, key accents, and active states.

5. **"What's your secondary or accent color?"**
   This is used for highlights, badges, secondary actions, and visual variety. If the user doesn't have one, suggest a complementary color from Open Props that pairs well with their primary choice. Common good pairings:
   - Blue primary → Orange or Amber accent
   - Green primary → Blue or Purple accent
   - Purple primary → Orange or Teal accent
   - Red/Pink primary → Blue or Cyan accent

6. **"Do you want dark mode support?"**
   Three options:
   - **Yes, with a toggle** — user can switch between light and dark (generates ThemeToggle component)
   - **Yes, system preference only** — follows the user's OS setting automatically (uses `prefers-color-scheme` media query)
   - **No, light mode only** — skip dark mode token definitions

7. **"What's the overall design vibe?"**
   Give them options to anchor the aesthetic direction:
   - **Clean & minimal** — lots of whitespace, subtle shadows, understated
   - **Bold & modern** — strong colors, larger typography, more contrast
   - **Warm & friendly** — rounded corners, softer colors, approachable
   - **Professional & corporate** — structured, conservative, trustworthy
   - **Editorial / magazine** — strong typography hierarchy, asymmetric layouts

   This influences spacing scale choices, border radius defaults, shadow intensity, and typography weight.

### Stage 3: Content & Typography

Ask the user:

8. **"Do you have a preferred font, or should I pick one?"**
   If they have a brand font, use it. If not, suggest options based on the design vibe:
   - Clean & minimal → Inter, DM Sans, or Outfit
   - Bold & modern → Space Grotesk, Sora, or Manrope
   - Warm & friendly → Nunito, Quicksand, or Plus Jakarta Sans
   - Professional → Source Sans 3, Lato, or IBM Plex Sans
   - Editorial → Playfair Display (headings) + Source Serif 4 (body), or Fraunces + Inter

   Ask if they want the same font for headings and body, or a display/body pairing.

9. **"What's your brand name and tagline?"**
   Used for the site metadata, header logo text, and SEO defaults. Also ask for a short meta description (or offer to write one based on what they've described).

10. **"Do you have any existing content ready — copy, images, logos?"**
    This determines whether to use placeholder content or real content. If they have content, ask them to share it or describe what they have. If not, let them know you'll use realistic placeholder text that matches their brand voice, which they can replace later.

### Stage 4: Functional Requirements

Ask the user:

11. **"Do you need a contact form? If so, what fields?"**
    Default suggestion: Name, Email, Subject, Message. Ask if they need additional fields (phone, company, budget range, file upload, etc.).

12. **"Will you have a blog or news section?"**
    If yes, ask:
    - Where will content come from? (Markdown files, a CMS like Sanity/Contentful, or just static data for now?)
    - Do they need categories or tags?
    - Author pages?

13. **"Any third-party integrations you already know you'll need?"**
    Common ones to ask about:
    - Analytics (Google Analytics, Plausible, Vercel Analytics)
    - Email/newsletter (Mailchimp, ConvertKit, Resend)
    - CMS (Sanity, Contentful, Keystatic)
    - Forms (Formspree, Basin, custom API)
    - Payments (Stripe)

### After Gathering Answers

Once the user has answered (they don't need to answer everything — use sensible defaults for anything they skip), summarize back to them:

**"Here's what I'll build:"**

- Project: [name] — [description]
- Pages: [list with any subpages]
- Homepage sections: [list]
- Primary color: [color] (`--[color]-[number]`)
- Accent color: [color] (`--[color]-[number]`)
- Dark mode: [yes with toggle / yes system only / no]
- Design vibe: [choice]
- Fonts: [heading font] / [body font]
- Contact form: [yes/no, fields]
- Blog: [yes/no, content source]
- Integrations: [list or "none yet"]

Ask: **"Does this look right? Anything you want to change before I start building?"**

Wait for confirmation, then proceed to scaffolding. Apply their color and font choices to the semantic tokens in `globals.css`, generate the page structure, and build out components using the reference files.

### For Returning Users / Adding to Existing Projects

If the user already has a project set up and is asking to add a specific component or page, skip the full setup flow. Instead:

1. Check if `globals.css` already has semantic tokens defined — if so, use those colors and conventions
2. Ask only what's needed for the specific task ("What should this page include?" or "Any specific requirements for this component?")
3. Generate the new files following the established patterns

The full setup flow is for new projects. For existing projects, be efficient and only ask what's necessary.

---

## Architecture Overview

### Styling Stack
- **Open Props** — design token foundation (installed via `npm install open-props`)
- **CSS Modules** — scoped component styles (built into Next.js, zero config)
- **CSS Custom Properties** — semantic layer bridging tokens to components
- **No Tailwind, no shadcn/ui, no utility classes in JSX**

### Key Rules

1. **Every component gets a co-located `.module.css` file.** A `Button` component lives as `button.tsx` + `button.module.css` in the same directory.

2. **Components use semantic tokens only.** Write `var(--color-primary)`, never `var(--blue-7)`. Raw Open Props tokens are only referenced in `globals.css` where semantic tokens are defined.

3. **Mobile-first responsive.** Base styles target small screens. Layer on `@media (min-width: 768px)` and `@media (min-width: 1024px)` as needed.

4. **CSS nesting is allowed.** Use `&:hover`, `&:focus-visible`, `&.active` — supported in modern CSS and Next.js.

5. **Server Components by default.** Only add `"use client"` when the component needs state, effects, event handlers, or browser APIs.

6. **Use `next/image` for all images and `next/link` for all internal links.**

## When Generating Components

For every component, follow this sequence:

1. Read `references/semantic-tokens.md` to understand the available token system
2. Read `references/component-patterns.md` for the standard patterns
3. Generate the `.tsx` file importing its `.module.css`
4. Generate the `.module.css` file using only semantic tokens — **customized with the user's chosen colors and fonts from the setup flow**
5. Ensure the component uses the `cx()` helper for conditional classes (see below)

### The `cx()` Helper

Every project should have this in `lib/classnames.ts`:

```ts
export function cx(...classes: (string | false | undefined | null)[]): string {
  return classes.filter(Boolean).join(' ');
}
```

Use it for conditional class composition:

```tsx
import { cx } from '@/lib/classnames';
import styles from './card.module.css';

<div className={cx(styles.card, isInteractive && styles.interactive, className)} />
```

Do NOT install `clsx`, `classnames`, or `tailwind-merge`. This lightweight helper is all that's needed.

### Component TypeScript Pattern

All components follow this structure:

```tsx
import styles from './component-name.module.css';
import { cx } from '@/lib/classnames';

interface ComponentNameProps {
  // Use interface, not type
  // Define explicit props — avoid `any`
  className?: string; // Always accept className for composition
  children?: React.ReactNode;
}

export function ComponentName({ className, children, ...props }: ComponentNameProps) {
  return (
    <div className={cx(styles.wrapper, className)} {...props}>
      {children}
    </div>
  );
}
```

### CSS Module Pattern

All `.module.css` files follow these conventions:

```css
/* component-name.module.css */

.wrapper {
  /* Use semantic tokens from globals.css */
  color: var(--color-text-1);
  background: var(--color-bg);

  /* Use Open Props for spacing, radius, shadows, easing */
  padding: var(--size-4);
  border-radius: var(--radius-2);
  box-shadow: var(--shadow-2);
  transition: box-shadow 0.2s var(--ease-3);

  /* Nest pseudo-selectors and states */
  &:hover {
    box-shadow: var(--shadow-3);
  }

  &:focus-visible {
    outline: 2px solid var(--color-primary);
    outline-offset: 2px;
  }
}

/* Responsive — mobile-first */
@media (min-width: 768px) {
  .wrapper {
    padding: var(--size-6);
  }
}
```

## Project Scaffolding

When setting up a new project or generating the initial structure, create these files:

### File Structure

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   └── [pages as needed]/
├── components/
│   ├── ui/          # Button, Card, Input, Badge, etc.
│   ├── layout/      # Header, Footer, Nav, MobileNav
│   ├── sections/    # Hero, Features, CTA, Testimonials
│   └── shared/      # Logo, ThemeToggle, SocialLinks
├── lib/
│   ├── classnames.ts
│   └── constants.ts
├── content/         # Static data files
├── public/
│   ├── images/
│   └── icons/
└── styles/
    └── globals.css
```

### globals.css Setup

Read `references/semantic-tokens.md` for the complete token definitions to include in `globals.css`. The file should:

1. Import Open Props
2. Define `:root` semantic tokens referencing Open Props — **using the user's chosen colors from the setup flow**
3. Define `[data-theme='dark']` overrides (if dark mode was requested)
4. Set base typography and body styles — **using the user's chosen fonts**

### Mapping User Color Choices to Open Props

When the user picks a primary or accent color, map it to the appropriate Open Props scale:

| User says | Open Props scale | Primary | Hover | Active | Subtle | Dark mode primary |
|---|---|---|---|---|---|---|
| Blue | `--blue-*` | `--blue-7` | `--blue-8` | `--blue-9` | `--blue-2` | `--blue-4` |
| Indigo | `--indigo-*` | `--indigo-7` | `--indigo-8` | `--indigo-9` | `--indigo-2` | `--indigo-4` |
| Green | `--green-*` | `--green-7` | `--green-8` | `--green-9` | `--green-2` | `--green-4` |
| Teal | `--teal-*` | `--teal-7` | `--teal-8` | `--teal-9` | `--teal-2` | `--teal-4` |
| Purple / Violet | `--violet-*` | `--violet-7` | `--violet-8` | `--violet-9` | `--violet-2` | `--violet-4` |
| Red | `--red-*` | `--red-7` | `--red-8` | `--red-9` | `--red-2` | `--red-4` |
| Orange | `--orange-*` | `--orange-7` | `--orange-8` | `--orange-9` | `--orange-2` | `--orange-4` |
| Pink | `--pink-*` | `--pink-7` | `--pink-8` | `--pink-9` | `--pink-2` | `--pink-4` |
| Cyan | `--cyan-*` | `--cyan-7` | `--cyan-8` | `--cyan-9` | `--cyan-2` | `--cyan-4` |

The pattern is consistent: **base at 7, hover at 8, active at 9, subtle at 2, dark mode flips to 4**. If the user provides a specific hex value, find the closest Open Props color scale and use that.

### Mapping Design Vibe to Defaults

Adjust these defaults based on the user's chosen vibe:

| Setting | Clean & Minimal | Bold & Modern | Warm & Friendly | Professional | Editorial |
|---|---|---|---|---|---|
| Border radius | `--radius-2` | `--radius-2` | `--radius-3` | `--radius-1` | `--radius-1` |
| Card shadow | `--shadow-2` | `--shadow-3` | `--shadow-2` | `--shadow-2` | `none` |
| Heading weight | `--font-weight-7` | `--font-weight-9` | `--font-weight-6` | `--font-weight-7` | `--font-weight-7` |
| Section spacing | `--size-10` | `--size-12` | `--size-10` | `--size-10` | `--size-12` |
| Body line-height | `--font-lineheight-3` | `--font-lineheight-2` | `--font-lineheight-4` | `--font-lineheight-3` | `--font-lineheight-4` |

### Root Layout

```tsx
// app/layout.tsx
import '@/styles/globals.css';
import { Inter, JetBrains_Mono } from 'next/font/google';
import type { Metadata } from 'next';

const inter = Inter({ subsets: ['latin'], variable: '--font-sans' });
const jetbrainsMono = JetBrains_Mono({ subsets: ['latin'], variable: '--font-mono' });

export const metadata: Metadata = {
  title: { default: '[Site Name]', template: '%s | [Site Name]' },
  description: '[Description]',
};

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={`${inter.variable} ${jetbrainsMono.variable}`}>
      <body>{children}</body>
    </html>
  );
}
```

## Reference Files

For detailed token definitions, component templates, and page section patterns, consult these references:

- **`references/semantic-tokens.md`** — Complete `:root` and dark mode token definitions, Open Props imports, globals.css template. Read this when setting up a new project or adding new semantic tokens.

- **`references/component-patterns.md`** — Ready-to-use patterns for common UI components (Button, Card, Input, Badge, Modal, Accordion, Form, Navigation). Read this when generating any UI component.

- **`references/page-sections.md`** — Patterns for full page sections (Hero, Features grid, Testimonials, CTA band, Pricing table, FAQ). Read this when building page layouts or landing pages.

## Naming Conventions

| What | Convention | Example |
|---|---|---|
| Files & folders | kebab-case | `feature-card.tsx` |
| CSS Module files | match component | `feature-card.module.css` |
| Components | PascalCase | `FeatureCard` |
| CSS classes | camelCase | `.sectionHeader` |
| Props interfaces | PascalCase + Props | `FeatureCardProps` |
| Hooks | camelCase with use | `useMediaQuery` |
| Constants | UPPER_SNAKE_CASE | `NAV_LINKS` |

## Common Mistakes to Avoid

- **Using raw Open Props tokens in components.** Always use semantic tokens. If `var(--color-primary)` doesn't exist for what you need, define a new semantic token in `globals.css` first, then use it.
- **Writing Tailwind classes.** No `className="flex items-center gap-4"`. That's CSS Module territory: `.wrapper { display: flex; align-items: center; gap: var(--size-3); }`
- **Hardcoding pixel values.** Use `var(--size-*)` for spacing, `var(--font-size-*)` for type, `var(--radius-*)` for corners.
- **Forgetting `prefers-reduced-motion`.** Wrap non-essential animations in a media query.
- **Making everything a client component.** Default to Server Components. Only add `"use client"` when you actually need interactivity.
- **Skipping the `className` prop.** Every component should accept an optional `className` for composition flexibility.
