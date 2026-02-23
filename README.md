# nextjs-open-props

A Claude skill that builds Next.js websites using [Open Props](https://open-props.style/) design tokens and CSS Modules — no Tailwind, no utility classes. It walks you through an interactive setup, learns your design preferences, then generates production-ready components, pages, and layouts that follow a consistent architecture.

## What This Skill Does

This skill teaches Claude how to scaffold and build Next.js projects using Open Props instead of Tailwind CSS. Open Props is a lightweight set of CSS custom properties (colors, spacing, typography, shadows, easing) that you use in regular CSS. Combined with CSS Modules for scoping, it gives you full global style control through variables — changing your entire site's color palette means editing a few lines in one file.

The skill enforces a **semantic token layer** on top of Open Props. Instead of using raw tokens like `var(--blue-7)` in components, everything references semantic names like `var(--color-primary)`. This means dark mode, theming, and brand updates propagate automatically across your entire project.

### What Gets Generated

- **Complete project scaffolding** — `app/` routing structure, `globals.css` with your design tokens, root layout with fonts and metadata
- **UI components** — Button (5 variants, 3 sizes), Card, Input, Textarea, Badge, Modal, Accordion, each with a `.tsx` file and co-located `.module.css`
- **Layout components** — Responsive header with navigation, mobile slide-out menu, multi-column footer
- **Page sections** — Hero, features grid, testimonials, CTA band, pricing table, stats, logo cloud, contact form
- **Dark mode system** — Full light/dark token set with optional toggle component or system-preference-only mode
- **Utility helpers** — `cx()` classname helper, constants file, TypeScript interfaces

Every generated file follows the same conventions: semantic tokens only, mobile-first responsive, CSS nesting, Server Components by default, proper accessibility (focus states, ARIA attributes, semantic HTML, reduced motion support).

## How It Works

When the skill triggers, it runs an **interactive setup flow** before writing any code. It asks you questions in four stages, uses your answers to configure the design system, then generates everything to match.

### Stage 1: Project Foundation

The skill asks three questions to understand what you're building:

| Prompt | What It Determines |
|---|---|
| **"What is this website for?"** | The overall project context — marketing site, portfolio, SaaS landing page, blog, etc. Shapes all downstream decisions. |
| **"What pages do you need?"** | The site structure. Offers suggestions (Home, About, Services, Pricing, Blog, Contact, FAQ, Portfolio, Careers, Terms/Privacy) and asks about subpages. Directly defines the Next.js `app/` routing. |
| **"What sections should your homepage include?"** | Which page section patterns to use. Options include Hero, Features grid, Testimonials, Stats, Logo cloud, Pricing preview, CTA band, FAQ, and Contact form. |

### Stage 2: Visual Identity

Four questions that define the look and feel:

| Prompt | What It Determines |
|---|---|
| **"What's your primary brand color?"** | The main color for buttons, links, focus rings, and accents. Accepts hex values, color names, or references. Maps to the closest Open Props color scale (e.g., "blue" → `--blue-7` for base, `--blue-8` hover, `--blue-9` active, `--blue-4` dark mode). |
| **"What's your secondary or accent color?"** | Used for highlights, badges, and visual variety. If you don't have one, it suggests complementary pairings (blue primary → orange accent, purple primary → teal accent, etc.). |
| **"Do you want dark mode support?"** | Three options: **toggle** (generates a ThemeToggle component), **system preference only** (uses `prefers-color-scheme`), or **light only** (skips dark tokens entirely). |
| **"What's the overall design vibe?"** | Anchors the aesthetic direction. Five options — Clean & minimal, Bold & modern, Warm & friendly, Professional & corporate, Editorial / magazine. This adjusts border radius, shadow intensity, heading weight, spacing scale, and line height defaults. |

### Stage 3: Content & Typography

Three questions about type and content:

| Prompt | What It Determines |
|---|---|
| **"Do you have a preferred font, or should I pick one?"** | If you have a brand font, it uses that. If not, it suggests options matched to your design vibe — e.g., Clean & minimal → Inter, DM Sans, Outfit; Editorial → Playfair Display + Source Serif 4. Asks whether you want the same font for headings and body or a display/body pairing. |
| **"What's your brand name and tagline?"** | Used for site metadata, header logo text, SEO title/description defaults, and Open Graph tags. |
| **"Do you have existing content ready?"** | Determines whether to use your real copy/images or generate realistic placeholder content matched to your brand voice. |

### Stage 4: Functional Requirements

Three questions about features and integrations:

| Prompt | What It Determines |
|---|---|
| **"Do you need a contact form? If so, what fields?"** | Defaults to Name, Email, Subject, Message. Asks about additional fields like phone, company, budget range, or file upload. |
| **"Will you have a blog or news section?"** | If yes, asks about the content source (Markdown files, CMS like Sanity/Contentful, or static data), categories/tags, and author pages. |
| **"Any third-party integrations you already know you'll need?"** | Covers analytics (GA, Plausible, Vercel Analytics), email/newsletter (Mailchimp, ConvertKit, Resend), CMS, form services, and payments (Stripe). |

### Confirmation Before Building

After gathering your answers, the skill presents a summary:

```
Here's what I'll build:

- Project: Acme Corp — SaaS landing page for project management tool
- Pages: Home, Features, Pricing, Blog, Contact, Terms, Privacy
- Homepage sections: Hero, Features grid, Testimonials, Stats, CTA band
- Primary color: Indigo (--indigo-7)
- Accent color: Orange (--orange-7)
- Dark mode: Yes, with toggle
- Design vibe: Clean & minimal
- Fonts: Inter (headings + body)
- Contact form: Yes — Name, Email, Company, Message
- Blog: Yes, Markdown files, with categories
- Integrations: Vercel Analytics, Resend

Does this look right? Anything you want to change before I start building?
```

You confirm or adjust, then it scaffolds the entire project.

### For Existing Projects

If you already have a project set up and just need to add a component or page, the skill skips the full setup. It checks your existing `globals.css` for the token system already in place and only asks what's needed for the specific task.

## Architecture

```
src/
├── app/                      # Next.js App Router pages
│   ├── layout.tsx            # Root layout with fonts, metadata, globals.css
│   ├── page.tsx              # Homepage
│   └── [your-pages]/         # Generated from your page list
├── components/
│   ├── ui/                   # Primitives — each with .tsx + .module.css
│   │   ├── button.tsx
│   │   ├── button.module.css
│   │   ├── card.tsx
│   │   ├── card.module.css
│   │   └── ...
│   ├── layout/               # Header, Footer, Nav, MobileNav
│   ├── sections/             # Hero, Features, Testimonials, CTA, etc.
│   └── shared/               # Logo, ThemeToggle, SocialLinks
├── lib/
│   ├── classnames.ts         # cx() helper for conditional CSS classes
│   └── constants.ts          # Nav links, social URLs, site metadata
├── styles/
│   └── globals.css           # Open Props imports + semantic token system
└── content/                  # Static data files
```

### Key Principles

- **Semantic tokens only** — Components never use raw Open Props tokens (`--blue-7`). They use semantic names (`--color-primary`) defined in `globals.css`. This is what makes global theming and dark mode work.
- **Co-located CSS Modules** — Every component has a matching `.module.css` file in the same directory. No inline styles, no utility classes in JSX.
- **Mobile-first responsive** — Base styles target small screens. Breakpoints layer on with `min-width` media queries.
- **Server Components by default** — Only components that need interactivity (state, effects, event handlers) get `"use client"`.
- **Accessibility built in** — Focus-visible states, ARIA attributes, semantic HTML, `prefers-reduced-motion` support.

## Installation

### Option 1: Drag and drop

Download `nextjs-open-props.skill` from the [Releases](../../releases) page and drag it into a Claude conversation.

### Option 2: Manual install

Clone this repo and copy the `nextjs-open-props` folder to your skills directory:

```bash
# Claude Code (local skills)
cp -r nextjs-open-props ~/.claude/skills/

# Or wherever your Claude skills are configured
cp -r nextjs-open-props /path/to/your/skills/user/
```

### Option 3: From source

```bash
git clone https://github.com/[your-username]/nextjs-open-props.git
cd nextjs-open-props
```

The skill folder is ready to use as-is — no build step required.

## Usage

Once installed, the skill triggers automatically when you ask Claude to build Next.js pages or components with Open Props. You don't need to invoke it manually.

**Trigger phrases that activate the skill:**

- "Build me a Next.js site with Open Props"
- "Create a landing page using CSS Modules"
- "Add a pricing section to my Open Props project"
- "Set up dark mode with CSS custom properties"
- "Convert this Tailwind component to CSS Modules"
- "Scaffold a new Next.js project without Tailwind"

**For a new project**, it runs the full interactive setup flow (4 stages, ~13 questions). **For an existing project**, it asks only what's needed for the specific task.

## What's Inside

| File | Size | Purpose |
|---|---|---|
| `SKILL.md` | ~400 lines | Main skill — setup flow, architecture rules, color/vibe mapping tables, code patterns |
| `references/semantic-tokens.md` | ~250 lines | Complete `globals.css` template with light/dark tokens, base styles, typography and spacing scale reference |
| `references/component-patterns.md` | ~550 lines | 11 UI components with full TypeScript + CSS Module code |
| `references/page-sections.md` | ~500 lines | 10 landing page section patterns with full code |

Total packaged size: **~20 KB**

## Open Props Color Scale Reference

The skill maps your color choices to Open Props token scales. Here's how the states work:

| State | Scale Position | Example (Blue) |
|---|---|---|
| Subtle / tint backgrounds | 2 | `--blue-2` |
| Dark mode primary | 4 | `--blue-4` |
| Light mode primary | 7 | `--blue-7` |
| Hover | 8 | `--blue-8` |
| Active / pressed | 9 | `--blue-9` |

This pattern is consistent across all colors. Changing your primary from blue to indigo means swapping `--blue-*` for `--indigo-*` in the semantic token definitions — one file, six lines.

## Requirements

- **Next.js 14+** with App Router
- **Open Props** (`npm install open-props`)
- **TypeScript** (recommended)
- No other CSS dependencies needed — CSS Modules are built into Next.js

## License

MIT
