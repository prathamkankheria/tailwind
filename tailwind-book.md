# The Complete Tailwind CSS Reference Book
### A Production-Grade Guide for Frontend Developers (v3 & v4)

> **Who this is for:** A self-taught frontend developer who knows React, Vite, TypeScript, and basic CSS. This book teaches you to *master* Tailwind — not just use it. Every chapter has real, copy-paste-ready code.

---

## Table of Contents

**Foundations**
1. [What Is Tailwind CSS & Why It Exists](#chapter-1--what-is-tailwind-css--why-it-exists)
2. [Installation & Setup](#chapter-2--installation--setup)
3. [Core Concepts (Deep Dive)](#chapter-3--core-concepts-deep-dive)

**Layout**

4. [Layout Utilities](#chapter-4--layout-utilities)
5. [Flexbox (Complete)](#chapter-5--flexbox-complete)
6. [Grid (Complete)](#chapter-6--grid-complete)
7. [Spacing](#chapter-7--spacing)
8. [Sizing](#chapter-8--sizing)

**Styling**

9. [Typography (Complete)](#chapter-9--typography-complete)
10. [Colors & Backgrounds](#chapter-10--colors--backgrounds)
11. [Borders](#chapter-11--borders)
12. [Shadows & Effects](#chapter-12--shadows--effects)
13. [Filters & Backdrop Filters](#chapter-13--filters--backdrop-filters)

**Motion & Interaction**

14. [Transitions & Animations](#chapter-14--transitions--animations)
15. [Transforms](#chapter-15--transforms)
16. [Interactivity](#chapter-16--interactivity)

**Advanced**

17. [Tailwind v4 — New Features](#chapter-17--tailwind-v4--new-features-critical-chapter)
18. [Customization](#chapter-18--customization)
19. [Tailwind with React (Practical Patterns)](#chapter-19--tailwind-with-react-practical-patterns)
20. [Performance & Production](#chapter-20--performance--production)
21. [Accessibility with Tailwind](#chapter-21--accessibility-with-tailwind)

**Reference**

22. [Real-World Mini Projects (Code Complete)](#chapter-22--real-world-mini-projects-code-complete)
23. [Tips, Tricks & Gotchas](#chapter-23--tips-tricks--gotchas)
24. [Tailwind Cheat Sheet (Quick Reference)](#chapter-24--tailwind-cheat-sheet-quick-reference)

---

---

## Chapter 1 — What Is Tailwind CSS & Why It Exists

### The Problem with Traditional CSS

At small scale, CSS is fine. But when a project grows — more pages, more components, a team of three developers — these problems emerge fast:

**1. Global scope by default**
Every CSS rule you write is global. A `.button` class in `checkout.css` silently overrides the one in `header.css`. There's no module boundary.

**2. Naming wars (the BEM trap)**
Teams spend hours debating naming conventions. Should it be `.card__header--active` or `.card-header.is-active`? Neither is wrong, both are painful, and new devs never follow the convention consistently.

**3. Specificity hell**
You write `.nav a { color: blue }`. A third-party library writes `.header .nav a { color: red }`. Now you need `.header .nav a.primary { color: blue !important }`. The arms race never ends.

**4. Dead CSS accumulates**
After six months, nobody knows if removing `.legacy-widget` will break the forgotten `/admin/old-dashboard` route. You leave it in. The stylesheet grows forever.

**5. Context switching overhead**
Write a div, switch to the CSS file, write a class, switch back to HTML to verify the class name is spelled correctly. Multiply by 100 components.

```css
/* Traditional CSS — you're managing this complexity manually */
.card { ... }
.card--featured { ... }
.card__header { ... }
.card__header--dark { ... }
.card__body { ... }
.card__footer { ... }
.card__footer--sticky { ... }
/* 3 months later: which of these are still used? */
```

---

### What "Utility-First" Means

Utility-first means: instead of writing semantic class names that map to bundles of CSS properties, you compose small single-purpose classes directly in your HTML.

| Approach | How you style | Class example |
|---|---|---|
| **Traditional CSS** | Write a class, define its properties in a .css file | `.card-header { padding: 1rem; background: white; }` |
| **Bootstrap (component)** | Use pre-built component classes | `<div class="card"><div class="card-header">` |
| **Tailwind (utility)** | Compose atomic classes inline | `<div class="p-4 bg-white rounded-lg">` |

**Bootstrap vs Tailwind — the key philosophical difference:**

Bootstrap gives you a `.btn-primary`. You can't easily make it have `14px` padding without overriding CSS. You're constrained to Bootstrap's design decisions.

Tailwind gives you `px-3.5 py-2 text-sm font-medium bg-blue-600 text-white rounded-lg hover:bg-blue-700`. You own every pixel. No overrides needed — ever.

```jsx
{/* Bootstrap — fighting the framework to deviate from defaults */}
<button className="btn btn-primary btn-custom-size">Click</button>
{/* .btn-custom-size { padding: 0.6rem 1.4rem !important; } */}

{/* Tailwind — you ARE the CSS, no fights */}
<button className="px-5 py-2.5 text-sm font-semibold bg-blue-600 text-white rounded-lg hover:bg-blue-700 transition-colors">
  Click
</button>
```

---

### Tailwind's Philosophy: Constraint-Based Design System

Tailwind doesn't give you arbitrary freedom — it gives you a *constrained* set of values. Spacing goes: 0, 1, 2, 3, 4, 5, 6, 8, 10, 12... not every possible pixel value. Colors are named scales (blue-50 through blue-950), not hex codes.

This is the key insight: **constraints produce consistency**. When every developer on your team picks padding values from the same scale, the UI naturally looks cohesive without a design system meeting.

> 💡 **Tip:** The spacing scale (`1 = 4px`, `2 = 8px`, `4 = 16px`) maps to a 4px grid system — the same grid that design tools like Figma use by default. This is not a coincidence.

---

### When to Use Tailwind vs. When NOT To

**Use Tailwind when:**
- Building a new app with React, Vue, or any JS framework where components are your abstraction layer
- You want consistency without writing a design system from scratch
- Your team has varying CSS skill levels — Tailwind's constraints prevent bad CSS decisions
- You need fast iteration — changing a design means editing JSX, not hunting through CSS files
- You want tiny production CSS bundles (Tailwind ships only classes you actually use)

**Avoid Tailwind when:**
- You're adding it to an existing large codebase with established CSS conventions — the migration cost is high
- Your project has very custom, unique visuals that don't fit the default scale — you'll be writing arbitrary values everywhere, defeating the purpose
- The team strongly prefers CSS Modules or CSS-in-JS and has an existing component library — don't force the change
- You're building a static documentation site or a WordPress theme — the setup overhead isn't worth it for simple projects
- You need to support IE11 — Tailwind v3+ drops IE support

**⚠️ Honest tradeoff:** Tailwind HTML *does* look verbose. `className="flex items-center gap-3 p-4 bg-white rounded-xl shadow-sm border border-gray-100 hover:shadow-md transition-shadow"` is longer than `className="card"`. The tradeoff is that you never have to look at a separate file to understand what this element looks like. For most modern React apps, this is the right trade.

---

### v3 vs v4 — What Changed Fundamentally

**Tailwind v3 (current stable as of 2024):**
- Configuration lives in `tailwind.config.js` — a JavaScript file
- Uses PostCSS as the transform engine
- Requires three CSS directives: `@tailwind base; @tailwind components; @tailwind utilities;`
- Custom utilities need JavaScript plugin API
- Colors use sRGB hex values

**Tailwind v4 (new architecture):**
- **CSS-first configuration** — `tailwind.config.js` is gone; you configure everything in your CSS file using `@theme`
- Uses **Lightning CSS** instead of PostCSS — significantly faster builds (up to 10x)
- Single import: `@import "tailwindcss"` replaces all three directives
- Custom utilities defined with `@utility` — pure CSS, no JavaScript plugin needed
- Custom variants defined with `@variant` — pure CSS
- **OKLCH color system** — perceptually uniform colors (more on this in Chapter 10)
- Dynamic utilities — `mt-17`, `text-11xl` work without any config
- `@source` replaces the `content` array

```css
/* v3 — main CSS file */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* v4 — main CSS file */
@import "tailwindcss";

@theme {
  --color-brand: oklch(60% 0.2 250);
  --font-display: "Inter", sans-serif;
}
```

---

### JIT (Just-In-Time) — How It Works Under the Hood

Before JIT (Tailwind v2 and earlier), Tailwind pre-generated ALL possible utility classes at build time. The full stylesheet was 3MB+ of CSS. PurgeCSS was run in production to remove unused classes.

**The problem:** PurgeCSS was a separate, dumb tool that scanned for class name strings. It was slow, and if you dynamically generated class names it would remove classes you needed.

**JIT engine (introduced v2.1, default in v3):** Instead of generating everything upfront, JIT watches your files and generates CSS **on-demand** as you write class names. 

How it works:
1. Tailwind scans your `content` files using fast regex/string scanning (not AST parsing)
2. When it sees `bg-blue-500`, it generates exactly that CSS rule — nothing else
3. This happens in milliseconds, not seconds
4. In development, the CSS is regenerated on save
5. In production, the output is already minimal — no PurgeCSS needed

```
You type: bg-blue-500
Tailwind generates: .bg-blue-500 { background-color: rgb(59 130 246); }

You type: bg-blue-500/75
Tailwind generates: .bg-blue-500\/75 { background-color: rgb(59 130 246 / 0.75); }
```

**⚠️ Gotcha:** Because JIT uses string scanning (not code execution), class names must appear as complete strings in your source files. This is why `text-${color}-500` breaks — the scanner never sees `text-red-500` or `text-blue-500` as a literal string.

```jsx
// ❌ BROKEN — JIT scanner never sees "text-red-500" as a complete string
const color = "red";
<p className={`text-${color}-500`}>Hello</p>

// ✅ CORRECT — use a lookup map with complete class strings
const colorMap = {
  red: "text-red-500",
  blue: "text-blue-500",
  green: "text-green-500",
};
<p className={colorMap[color]}>Hello</p>
```

---

---

## Chapter 2 — Installation & Setup

### Install with Vite + React (Step by Step)

```bash
# 1. Create a new Vite + React + TypeScript project
npm create vite@latest my-app -- --template react-ts
cd my-app

# 2. Install Tailwind and its peer dependencies
npm install -D tailwindcss postcss autoprefixer

# 3. Generate config files
npx tailwindcss init -p
# This creates tailwind.config.js and postcss.config.js
```

**`tailwind.config.ts`** — configure content paths so Tailwind knows what to scan:

```ts
// tailwind.config.ts
import type { Config } from "tailwindcss";

export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // your customizations here
    },
  },
  plugins: [],
} satisfies Config;
```

**`postcss.config.js`** — generated automatically, should look like:

```js
// postcss.config.js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

**`src/index.css`** — add the Tailwind directives:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

**`src/main.tsx`** — import the CSS:

```tsx
import "./index.css";
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

**`vite.config.ts`** — no special configuration needed for Tailwind with Vite, but here's the default:

```ts
// vite.config.ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";

export default defineConfig({
  plugins: [react()],
});
```

---

### Install with Next.js 14/15 App Router

```bash
# Create Next.js app (14/15 with App Router)
npx create-next-app@latest my-app
# Answer prompts: TypeScript ✓, Tailwind CSS ✓, App Router ✓
# Tailwind is configured automatically!
```

If you're adding Tailwind to an existing Next.js app:

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

**`tailwind.config.ts`** for Next.js App Router:

```ts
import type { Config } from "tailwindcss";

export default {
  content: [
    "./app/**/*.{js,ts,jsx,tsx,mdx}",
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
    // For src/ directory layout:
    "./src/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
} satisfies Config;
```

**`app/globals.css`** (App Router):

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

### Install with Plain HTML (CDN — for Prototyping)

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- Tailwind v3 CDN play — includes all utilities, not for production -->
  <script src="https://cdn.tailwindcss.com"></script>
  <title>Tailwind Prototype</title>
</head>
<body class="bg-gray-100 p-8">
  <div class="max-w-md mx-auto bg-white rounded-xl shadow-md p-6">
    <h1 class="text-2xl font-bold text-gray-900">Hello Tailwind</h1>
    <p class="mt-2 text-gray-600">This is a CDN prototype.</p>
  </div>
</body>
</html>
```

**⚠️ Gotcha:** The CDN version is 300KB+ and includes all Tailwind classes. Never use this in production — it's purely for prototyping and demos.

---

### Tailwind v4 Setup — CSS-First Approach

```bash
# Install Tailwind v4
npm install tailwindcss@next @tailwindcss/vite@next

# No tailwind.config.js needed!
```

**`vite.config.ts`** — use the Vite plugin instead of PostCSS:

```ts
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [
    react(),
    tailwindcss(), // ← replaces PostCSS config entirely
  ],
});
```

**`src/index.css`** — single import, no directives:

```css
@import "tailwindcss";

/* Your customizations go here using @theme */
@theme {
  --color-brand: oklch(60% 0.2 250);
  --font-display: "Inter", sans-serif;
  --spacing-section: 5rem;
}
```

That's it. No `tailwind.config.js`, no `postcss.config.js`, no three directives.

---

### PostCSS Setup — What Each Plugin Does

```js
// postcss.config.js
export default {
  plugins: {
    tailwindcss: {},     // Core: processes Tailwind's directives (@tailwind, @apply, etc.)
    autoprefixer: {},    // Adds vendor prefixes: -webkit-, -moz- where needed
  },
};
```

**`tailwindcss`** — reads your CSS, finds `@tailwind` directives, and generates utility classes based on your config and content scan.

**`autoprefixer`** — takes the generated CSS and automatically adds vendor-prefixed versions of properties that need them. For example, `display: grid` becomes `display: -ms-grid; display: grid;` for older browsers. With Lightning CSS in v4, autoprefixer is no longer needed.

---

### The `content` Array — What It Is & Why Wrong Config Breaks Production

The `content` array tells Tailwind **where to look** for class names. It uses glob patterns to scan files.

```ts
content: [
  "./index.html",           // root HTML
  "./src/**/*.{js,ts,jsx,tsx}", // all JS/TS/JSX/TSX files
]
```

**How scanning works:** Tailwind reads every file matching these patterns and looks for class name strings. It does NOT execute JavaScript — it does simple string/regex scanning. That's why complete class name strings must exist as literals.

**What happens with wrong config:**
- If you forget `./index.html`, classes used only in `index.html` get purged in production
- If your components live in `./components/` but you only specify `./src/`, those component classes get purged
- In development with `vite dev`, this doesn't matter (all classes are available). Only in production builds (`vite build`) are unused classes removed.

```ts
// ❌ BAD — won't scan files in ./components/ or ./pages/
content: ["./src/App.tsx"]

// ✅ GOOD — scans everything in src/ recursively
content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"]
```

> 💡 **Tip:** If styles appear in development but disappear in production, wrong `content` config is almost always the cause. Check that your glob patterns actually match your file locations.

---

### Prettier Plugin for Tailwind — Auto Class Sorting

The `prettier-plugin-tailwindcss` plugin automatically sorts your Tailwind classes in the recommended order (layout → spacing → typography → colors → etc.). This makes class lists consistent and scannable across your whole team.

```bash
npm install -D prettier prettier-plugin-tailwindcss
```

**`prettier.config.js`:**

```js
// prettier.config.js
export default {
  plugins: ["prettier-plugin-tailwindcss"],
  // optional: specify custom tailwind config location
  // tailwindConfig: "./tailwind.config.ts",
};
```

Now Prettier will reorder your classes on save:

```jsx
// Before Prettier save:
<div className="text-white p-4 flex bg-blue-500 rounded-lg items-center gap-2">

// After Prettier save (sorted by recommended order):
<div className="flex items-center gap-2 rounded-lg bg-blue-500 p-4 text-white">
```

---

### VS Code: Tailwind CSS IntelliSense Setup

Install the **Tailwind CSS IntelliSense** extension by Tailwind Labs (ID: `bradlc.vscode-tailwindcss`).

**What it gives you:**
- Autocomplete for utility class names as you type
- Hover documentation showing the exact CSS a class generates
- Linting for unknown or deprecated class names
- Color preview swatches inline in your editor

**`.vscode/settings.json`** — recommended settings:

```json
{
  "editor.quickSuggestions": {
    "strings": "on"
  },
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"],
    ["cx\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"],
    ["twMerge\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"]
  ],
  "tailwindCSS.includeLanguages": {
    "plaintext": "html"
  }
}
```

The `classRegex` config tells IntelliSense to provide autocomplete inside `cva()`, `cx()`, and `twMerge()` calls — essential for component-based Tailwind patterns (Chapter 19).

> 💡 **Tip:** Hover over any Tailwind class in VS Code to see the exact CSS it generates. This is the fastest way to learn what classes do without reading docs.

---

---

## Chapter 3 — Core Concepts (Deep Dive)

### 3.1 — How Utility Classes Work

Every Tailwind class maps to one or a small group of CSS properties. The mapping is intentional, predictable, and consistent.

**10+ Core examples:**

| Tailwind class | Generated CSS |
|---|---|
| `p-4` | `padding: 1rem;` |
| `mt-2` | `margin-top: 0.5rem;` |
| `text-lg` | `font-size: 1.125rem; line-height: 1.75rem;` |
| `font-bold` | `font-weight: 700;` |
| `bg-blue-500` | `background-color: rgb(59 130 246);` |
| `text-white` | `color: rgb(255 255 255);` |
| `rounded-lg` | `border-radius: 0.5rem;` |
| `flex` | `display: flex;` |
| `hidden` | `display: none;` |
| `w-full` | `width: 100%;` |
| `border` | `border-width: 1px;` |
| `shadow-md` | `box-shadow: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);` |

**Naming conventions:**

- `{property}-{value}`: `p-4`, `m-2`, `text-sm`
- `{property}-{side}-{value}`: `pt-4` (padding-top), `mx-8` (margin x-axis)
- `{property}-{modifier}`: `font-bold`, `text-center`

```jsx
{/* A complete card using composed utilities */}
<div className="bg-white rounded-xl shadow-md p-6 max-w-sm w-full">
  <h2 className="text-xl font-bold text-gray-900">Card Title</h2>
  <p className="mt-2 text-sm text-gray-600 leading-relaxed">
    Card description text here.
  </p>
  <button className="mt-4 w-full bg-blue-600 text-white font-medium py-2 px-4 rounded-lg hover:bg-blue-700 transition-colors">
    Action
  </button>
</div>
```

---

#### Negative Values

Prefix any class with `-` to use the negative version. Works for margins, translate, top/right/bottom/left, and more.

```jsx
<div className="-mt-4">Pulls up by 16px</div>
<div className="-translate-x-2">Shifts left by 8px</div>
<div className="-rotate-6">Tilted counter-clockwise</div>
<div className="-z-10">Behind other content</div>
```

---

#### Arbitrary Values

When you need a value that's not in the default scale, use square brackets:

```jsx
{/* Arbitrary dimension */}
<div className="w-[347px]">Exactly 347px wide</div>

{/* Arbitrary color */}
<div className="bg-[#1a1a1a] text-[#f0f0f0]">Custom colors</div>

{/* Arbitrary clamp() for fluid typography */}
<h1 className="text-[clamp(1.5rem,5vw,3rem)]">Fluid heading</h1>

{/* Arbitrary calc() */}
<div className="w-[calc(100%-2rem)]">Full width minus 2rem</div>

{/* Arbitrary CSS variable */}
<div className="bg-[var(--brand-color)]">Uses CSS variable</div>

{/* Arbitrary grid template */}
<div className="grid-cols-[200px_1fr_auto]">Custom grid</div>
```

> 💡 **Tip:** Arbitrary values are escape hatches, not primary tools. If you're using `w-[347px]` in ten different places, add a custom spacing token to your config instead.

---

### 3.2 — Responsive Design

#### Mobile-First Philosophy

Tailwind is **mobile-first**. Unprefixed utilities apply to ALL screen sizes. Prefixed utilities apply from that breakpoint **upward**.

```jsx
{/* 
  text-sm   → mobile: small text
  md:text-base → medium screens and up: base size
  lg:text-xl   → large screens and up: xl size
*/}
<p className="text-sm md:text-base lg:text-xl">Responsive text</p>
```

This means: start with your mobile design, then add `md:`, `lg:`, etc. to override for larger screens.

---

#### Default Breakpoints

| Prefix | Min-width | Typical device |
|---|---|---|
| *(none)* | 0px | All devices (mobile-first base) |
| `sm` | 640px | Large phones, small tablets |
| `md` | 768px | Tablets |
| `lg` | 1024px | Laptops |
| `xl` | 1280px | Desktops |
| `2xl` | 1536px | Large monitors |

```jsx
{/* Shows on mobile, hidden on md and up */}
<div className="block md:hidden">Mobile menu</div>

{/* Hidden on mobile, shows on md and up */}
<div className="hidden md:flex">Desktop nav</div>
```

---

#### Custom Breakpoints (v3)

```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      screens: {
        "xs": "475px",  // extra small
        "3xl": "1920px", // ultra-wide
      },
    },
  },
};
```

```jsx
<div className="xs:text-sm 3xl:text-2xl">Custom breakpoints</div>
```

#### v4 Custom Responsive Variants

```css
/* In your CSS with v4 */
@custom-variant tablet (@media (min-width: 800px) and (max-width: 1199px));
```

```jsx
<div className="tablet:bg-yellow-100">Only on tablets</div>
```

---

#### The `container` Class

```jsx
{/* Centers content with max-width */}
<div className="container mx-auto px-4">
  {/* page content */}
</div>
```

By default, `container` sets `max-width` to the current breakpoint. It does NOT center itself — you need `mx-auto`. Customize in config:

```ts
// tailwind.config.ts
export default {
  theme: {
    container: {
      center: true,   // adds mx-auto automatically
      padding: "1rem", // adds horizontal padding
    },
  },
};
```

---

#### Real Example: Responsive Navbar + Grid

```jsx
{/* Responsive Navbar */}
function Navbar() {
  return (
    <nav className="bg-white border-b border-gray-200 px-4 py-3">
      <div className="max-w-7xl mx-auto flex items-center justify-between">
        <span className="text-xl font-bold text-gray-900">Logo</span>

        {/* Desktop links — hidden on mobile */}
        <div className="hidden md:flex items-center gap-6">
          <a href="#" className="text-sm font-medium text-gray-600 hover:text-gray-900">Home</a>
          <a href="#" className="text-sm font-medium text-gray-600 hover:text-gray-900">About</a>
          <a href="#" className="text-sm font-medium text-gray-600 hover:text-gray-900">Contact</a>
        </div>

        {/* Mobile hamburger — hidden on desktop */}
        <button className="md:hidden p-2 rounded-lg hover:bg-gray-100">
          <svg className="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M4 6h16M4 12h16M4 18h16" />
          </svg>
        </button>
      </div>
    </nav>
  );
}

{/* Responsive Grid Layout */}
function ProductGrid() {
  return (
    <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6 p-6">
      {products.map((product) => (
        <ProductCard key={product.id} {...product} />
      ))}
    </div>
  );
}
```

---

### 3.3 — Dark Mode

#### `media` Strategy vs `class` Strategy

| Strategy | How it activates | Use when |
|---|---|---|
| `media` | Follows OS `prefers-color-scheme` | You want automatic OS-based dark mode, no toggle |
| `class` | When `.dark` class is on `<html>` | You want a user-controlled toggle in the UI |

**v3 config:**

```ts
// tailwind.config.ts
export default {
  darkMode: "class", // or "media"
  // ...
};
```

---

#### Implementation with `class` Strategy in React

```tsx
// hooks/useDarkMode.ts
import { useState, useEffect } from "react";

export function useDarkMode() {
  const [isDark, setIsDark] = useState(() => {
    // Check localStorage first, then system preference
    const stored = localStorage.getItem("theme");
    if (stored) return stored === "dark";
    return window.matchMedia("(prefers-color-scheme: dark)").matches;
  });

  useEffect(() => {
    const root = document.documentElement;
    if (isDark) {
      root.classList.add("dark");
      localStorage.setItem("theme", "dark");
    } else {
      root.classList.remove("dark");
      localStorage.setItem("theme", "light");
    }
  }, [isDark]);

  return { isDark, toggle: () => setIsDark((d) => !d) };
}
```

```tsx
// components/DarkModeToggle.tsx
import { useDarkMode } from "../hooks/useDarkMode";

export function DarkModeToggle() {
  const { isDark, toggle } = useDarkMode();

  return (
    <button
      onClick={toggle}
      className="p-2 rounded-lg bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700 transition-colors"
      aria-label="Toggle dark mode"
    >
      {isDark ? (
        <SunIcon className="w-5 h-5 text-yellow-500" />
      ) : (
        <MoonIcon className="w-5 h-5 text-gray-600" />
      )}
    </button>
  );
}
```

---

#### `dark:` Prefix — Full Examples

```jsx
{/* Background and text */}
<div className="bg-white dark:bg-gray-900 text-gray-900 dark:text-gray-100">
  Content
</div>

{/* Borders */}
<div className="border border-gray-200 dark:border-gray-700">Card</div>

{/* Shadows */}
<div className="shadow-lg dark:shadow-gray-900/50">Elevated card</div>

{/* Buttons */}
<button className="bg-blue-600 dark:bg-blue-500 hover:bg-blue-700 dark:hover:bg-blue-400 text-white">
  Button
</button>

{/* Complete dark-mode-aware card */}
<div className="bg-white dark:bg-gray-800 rounded-xl border border-gray-100 dark:border-gray-700 p-6 shadow-sm">
  <h2 className="text-gray-900 dark:text-white font-bold">Title</h2>
  <p className="text-gray-600 dark:text-gray-400 mt-1">Description</p>
</div>
```

#### v4 Dark Mode

In v4, define your dark variant in CSS:

```css
/* index.css */
@import "tailwindcss";

@variant dark (&:where(.dark *));
```

The `dark:` prefix then works identically. You can also do media-based:

```css
@variant dark (@media (prefers-color-scheme: dark));
```

---

### 3.4 — State Modifiers (Pseudo-classes & Pseudo-elements)

#### All Core State Modifiers

```jsx
{/* hover, focus, active */}
<button className="bg-blue-600 hover:bg-blue-700 focus:bg-blue-800 active:bg-blue-900">
  Click me
</button>

{/* disabled */}
<button className="bg-blue-600 disabled:opacity-50 disabled:cursor-not-allowed" disabled>
  Disabled
</button>

{/* visited (links) */}
<a href="#" className="text-blue-600 visited:text-purple-600">Link</a>

{/* checked (checkbox/radio) */}
<input type="checkbox" className="accent-blue-600 checked:ring-2 checked:ring-blue-600" />

{/* required, placeholder */}
<input
  className="border border-gray-300 required:border-red-300 placeholder:text-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500"
  placeholder="Required field"
  required
/>

{/* first, last, odd, even */}
<ul>
  {items.map((item, i) => (
    <li key={i} className="py-2 px-4 first:pt-0 last:pb-0 odd:bg-gray-50 even:bg-white">
      {item}
    </li>
  ))}
</ul>

{/* empty — when element has no children */}
<div className="empty:hidden">Only visible if it has content</div>

{/* focus-within — parent when any child is focused */}
<div className="border border-gray-300 focus-within:border-blue-500 focus-within:ring-1 focus-within:ring-blue-500 rounded-lg p-3">
  <label className="text-sm text-gray-600">Email</label>
  <input className="w-full outline-none mt-1" type="email" />
</div>

{/* focus-visible — keyboard focus only, not mouse */}
<button className="focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2">
  Keyboard-accessible button
</button>
```

---

#### `aria-*` and `data-*` Variants

```jsx
{/* aria-* — style based on ARIA state */}
<button
  aria-expanded={isOpen}
  className="aria-expanded:bg-gray-100 aria-expanded:text-gray-900"
>
  Toggle
</button>

<li
  aria-selected={isActive}
  className="aria-selected:bg-blue-50 aria-selected:text-blue-700 px-3 py-2 rounded"
>
  Nav item
</li>

{/* data-* — style based on data attributes (great for component state) */}
<div
  data-state={isOpen ? "open" : "closed"}
  className="data-[state=open]:rotate-180 transition-transform"
>
  <ChevronDown />
</div>
```

---

#### `before:` and `after:` Pseudo-elements

```jsx
{/* Required field asterisk using before: */}
<label className="before:content-['*'] before:mr-0.5 before:text-red-500 font-medium">
  Name
</label>

{/* Decorative line using after: */}
<h2 className="relative after:content-[''] after:absolute after:bottom-0 after:left-0 after:w-full after:h-0.5 after:bg-blue-500 pb-2">
  Section Title
</h2>
```

**⚠️ Gotcha:** `before:` and `after:` require `content-['']` (or some content value) to be visible. They also need to be positioned if you're using them for decorative lines.

---

#### `group` and `group-hover:` — Parent-Controlled Child Styling

The most powerful pattern for hover interactions on cards and list items:

```jsx
{/* Add "group" to parent, use "group-hover:" on children */}
<div className="group relative bg-white rounded-xl border border-gray-200 p-6 hover:shadow-lg transition-shadow cursor-pointer">
  <h3 className="text-gray-900 group-hover:text-blue-600 font-semibold transition-colors">
    Card Title
  </h3>
  <p className="text-gray-500 mt-1 text-sm">Description text</p>
  
  {/* Arrow that slides in on hover */}
  <span className="absolute right-4 top-1/2 -translate-y-1/2 opacity-0 group-hover:opacity-100 group-hover:translate-x-1 transition-all text-blue-600">
    →
  </span>
</div>
```

**Named groups** — when you have nested groups:

```jsx
<div className="group/card">
  <div className="group/inner">
    <p className="group-hover/card:text-blue-600 group-hover/inner:font-bold">
      Responds to both parent groups independently
    </p>
  </div>
</div>
```

---

#### `peer` and `peer-*:` — Sibling-Controlled Styling

`peer` marks an element, and `peer-*:` on a *subsequent sibling* responds to its state. Perfect for form validation:

```jsx
{/* Real form validation example */}
<div className="flex flex-col gap-1">
  <label className="text-sm font-medium text-gray-700">Email</label>
  
  <input
    type="email"
    placeholder="you@example.com"
    className="peer border border-gray-300 rounded-lg px-3 py-2 
               focus:outline-none focus:ring-2 focus:ring-blue-500
               invalid:border-red-400 invalid:focus:ring-red-400"
  />
  
  {/* This message only shows when input is invalid AND not empty */}
  <p className="hidden text-xs text-red-500 peer-invalid:block">
    Please enter a valid email address.
  </p>
</div>
```

**⚠️ Gotcha:** `peer` must come *before* the element using `peer-*:` in the DOM. The peer variant uses the CSS `~` sibling combinator which only works forward in the DOM.

---

#### `has-*:` Modifier (Parent Selector)

CSS `:has()` lets you style a parent based on its children. Now available in Tailwind:

```jsx
{/* Card that changes style when its checkbox is checked */}
<label className="flex items-center gap-3 p-4 border rounded-lg cursor-pointer has-[:checked]:bg-blue-50 has-[:checked]:border-blue-400">
  <input type="checkbox" className="rounded" />
  <span>Select this option</span>
</label>

{/* Form group with error */}
<div className="has-[:invalid]:text-red-600 has-[:invalid]:border-red-400">
  <label>Username</label>
  <input type="text" minLength={3} />
</div>
```

---

#### `not-*:` Modifier

```jsx
{/* Apply style to elements that don't match a condition */}
<button className="not-disabled:hover:bg-blue-700 bg-blue-600 text-white px-4 py-2 rounded">
  Smart button
</button>

<li className="not-last:border-b border-gray-200 py-3 px-4">
  List item with divider except last
</li>
```

---

### 3.5 — Stacking Modifiers

Modifiers can be combined — Tailwind processes them from right to left:

```jsx
{/* dark mode + hover + md breakpoint */}
<button className="bg-blue-500 md:hover:bg-blue-600 dark:bg-blue-700 dark:hover:bg-blue-800">
  Stacked modifiers
</button>

{/* hover + focus-within stacked */}
<div className="bg-gray-50 hover:bg-white focus-within:ring-2 focus-within:ring-blue-500 rounded-lg p-4">
  <input className="bg-transparent outline-none w-full" />
</div>
```

**Does order matter?** Between responsive prefixes and state modifiers, yes — the responsive prefix should come last in your reading order but it's the outermost media query. Best practice (enforced by Prettier plugin):

1. Responsive prefix (`sm:`, `md:`, etc.)
2. State modifier (`hover:`, `focus:`, etc.)
3. Dark mode (`dark:`)
4. Utility class

The Prettier plugin enforces this order automatically.

---

#### `[&]:` Arbitrary Variant Syntax

When you need a CSS selector that Tailwind doesn't have a modifier for:

```jsx
{/* Style direct children */}
<div className="[&>*]:border-b [&>*]:border-gray-100 [&>*:last-child]:border-0">
  <div>Row 1</div>
  <div>Row 2</div>
  <div>Row 3</div>
</div>

{/* nth-child */}
<div className="[&:nth-child(3)]:bg-yellow-100">Third item</div>

{/* Complex selector */}
<div className="[&_.active]:font-bold [&_.active]:text-blue-600">
  <span className="active">Active item</span>
  <span>Inactive item</span>
</div>
```

---

#### `supports-*:` — Progressive Enhancement

```jsx
{/* Use CSS grid only if supported, fall back to flex */}
<div className="flex flex-wrap gap-4 supports-[display:grid]:grid supports-[display:grid]:grid-cols-3">
  {items.map((item) => <Card key={item.id} {...item} />)}
</div>

{/* Use backdrop-blur only if supported */}
<nav className="bg-white/50 supports-[backdrop-filter]:backdrop-blur-md">
  Nav content
</nav>
```

---

---

## Chapter 4 — Layout Utilities

### 4.1 — Display

```jsx
{/* Block — full width, stacks vertically */}
<div className="block">Full width block</div>

{/* Inline — flows with text, no width/height control */}
<span className="inline">Inline element</span>

{/* Inline-block — flows with text, but respects width/height */}
<span className="inline-block w-4 h-4 bg-blue-500 rounded-full"></span>

{/* Flex — block-level flex container */}
<div className="flex items-center gap-4">...</div>

{/* Inline-flex — inline flex container */}
<span className="inline-flex items-center gap-1 text-sm">
  <CheckIcon className="w-4 h-4" /> Verified
</span>

{/* Grid */}
<div className="grid grid-cols-3 gap-4">...</div>

{/* Hidden — display: none */}
<div className="hidden md:block">Shows on desktop only</div>

{/* Table display utilities */}
<div className="table w-full">
  <div className="table-header-group">...</div>
  <div className="table-row-group">
    <div className="table-row">
      <div className="table-cell px-4 py-2">Cell</div>
    </div>
  </div>
</div>
```

**Mental model for choosing display type:**

| Need | Use |
|---|---|
| Full-width stacking layout | `block` |
| Flow with text, no sizing | `inline` |
| Flow with text, need sizing | `inline-block` |
| Row/column layout with alignment | `flex` |
| Row/column layout + grid alignment | `grid` |
| Hide completely | `hidden` |

---

### 4.2 — Position

```jsx
{/* static — default, not positioned */}
<div className="static">Normal flow</div>

{/* relative — positioned relative to itself; reference for absolute children */}
<div className="relative">
  Parent (establishes positioning context)
  
  {/* absolute — removed from flow, positioned relative to nearest positioned ancestor */}
  <span className="absolute top-2 right-2 bg-red-500 text-white text-xs px-2 py-1 rounded-full">
    Badge
  </span>
</div>

{/* fixed — removed from flow, positioned relative to viewport */}
<button className="fixed bottom-6 right-6 bg-blue-600 text-white p-4 rounded-full shadow-lg hover:bg-blue-700 z-50">
  <PlusIcon className="w-6 h-6" />
</button>

{/* sticky — sticks at scroll position */}
<thead className="sticky top-0 bg-white z-10 border-b border-gray-200">
  <tr>
    <th className="px-4 py-3 text-left text-sm font-semibold text-gray-900">Name</th>
    <th className="px-4 py-3 text-left text-sm font-semibold text-gray-900">Status</th>
  </tr>
</thead>
```

**`inset-*` utilities** — shorthand for top/right/bottom/left:

```jsx
{/* inset-0 = top:0 right:0 bottom:0 left:0 — full overlay */}
<div className="absolute inset-0 bg-black/50 rounded-xl" />

{/* inset-x-0 = left:0 right:0 */}
<div className="absolute inset-x-0 bottom-0 h-16 bg-gradient-to-t from-black/50 to-transparent" />

{/* inset-y-4 = top:1rem bottom:1rem */}
<div className="absolute inset-y-4 left-0 w-px bg-gray-200" />
```

---

#### Real-World Position Examples

```jsx
{/* Tooltip */}
<div className="relative group">
  <button className="text-gray-600 hover:text-gray-900">Hover me</button>
  <div className="absolute bottom-full left-1/2 -translate-x-1/2 mb-2 px-3 py-1.5 bg-gray-900 text-white text-xs rounded-lg whitespace-nowrap opacity-0 group-hover:opacity-100 pointer-events-none transition-opacity z-50">
    This is a tooltip
    <div className="absolute top-full left-1/2 -translate-x-1/2 border-4 border-transparent border-t-gray-900" />
  </div>
</div>

{/* Dropdown */}
<div className="relative">
  <button className="flex items-center gap-2 px-4 py-2 border border-gray-300 rounded-lg">
    Options <ChevronDown className="w-4 h-4" />
  </button>
  <div className="absolute top-full left-0 mt-1 w-48 bg-white border border-gray-200 rounded-xl shadow-lg z-50 py-1">
    <a href="#" className="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50">Option 1</a>
    <a href="#" className="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-50">Option 2</a>
  </div>
</div>

{/* Sticky header */}
<header className="sticky top-0 z-40 bg-white/90 backdrop-blur-sm border-b border-gray-200">
  <div className="max-w-7xl mx-auto px-4 py-4 flex items-center justify-between">
    <Logo />
    <Nav />
  </div>
</header>
```

---

### 4.3 — Z-Index

```jsx
{/* Z-index scale: z-0, z-10, z-20, z-30, z-40, z-50, z-auto */}
<div className="relative z-0">Base layer</div>
<div className="relative z-10">Above base</div>
<div className="fixed z-50">Topmost (modals, toasts)</div>

{/* Negative z-index */}
<div className="relative -z-10">Behind siblings</div>

{/* Arbitrary z-index */}
<div className="z-[9999]">Custom z-index</div>
```

**Stacking context gotcha:** `z-index` only works within the same stacking context. If a parent has `transform`, `opacity < 1`, `filter`, or `isolation: isolate`, it creates a new stacking context and your `z-50` inside it won't stack above elements outside it.

```jsx
{/* ❌ BROKEN — transform creates new stacking context */}
<div className="transform translate-x-0">
  <div className="z-50">This won't be above the parent's stacking context</div>
</div>

{/* ✅ FIX — use isolate on the container to intentionally create context */}
<div className="isolate">
  <div className="relative z-10">Layer 1</div>
  <div className="relative z-20">Layer 2</div>
</div>
```

---

### 4.4 — Overflow & Overscroll

```jsx
{/* overflow-hidden — clips content, use for rounded corners with images */}
<div className="rounded-xl overflow-hidden">
  <img src="hero.jpg" className="w-full h-48 object-cover" />
</div>

{/* overflow-auto — scrollbar appears only when content overflows */}
<div className="max-h-64 overflow-auto">
  {longContent}
</div>

{/* overflow-scroll — always shows scrollbar */}
<div className="overflow-scroll h-48">Always scrollable</div>

{/* overflow-clip — clips without scroll (no scrollbar at all) */}
<div className="overflow-clip">Clipped, no scroll</div>

{/* X/Y axis control */}
<div className="overflow-x-auto overflow-y-hidden">
  Horizontal scroll only (table, code block)
</div>

{/* overscroll-contain — prevent scroll chaining (body doesn't scroll when inner scrolls to end) */}
<div className="overflow-auto overscroll-contain h-64">
  Scrolls independently from page
</div>

{/* overscroll-none — prevent all overscroll effects including pull-to-refresh */}
<div className="overflow-auto overscroll-none">No bounce effect</div>
```

> 💡 **Tip:** `overscroll-contain` is essential for modals and mobile drawers. Without it, when the user scrolls to the end of the modal, the page behind it starts scrolling — a jarring UX.

---

### 4.5 — Visibility & Opacity

**The three ways to "hide" an element — and when to use each:**

| Class | CSS | In document flow? | Accessible? | Animatable? |
|---|---|---|---|---|
| `hidden` | `display: none` | No — takes no space | No | No (abrupt) |
| `invisible` | `visibility: hidden` | **Yes — still takes space** | No | No |
| `opacity-0` | `opacity: 0` | **Yes — still takes space** | Yes (still focusable!) | **Yes** |
| `sr-only` | Clip + position | No visible space | Yes (screen readers) | No |

```jsx
{/* hidden — element removed from layout */}
<div className="hidden">Gone completely</div>
<div className="hidden md:block">Shows on desktop</div>

{/* invisible — takes space but not visible */}
<div className="invisible group-hover:visible transition-all">
  Appears on hover without layout shift
</div>

{/* opacity — use for transitions */}
<div className={`transition-opacity duration-300 ${isVisible ? "opacity-100" : "opacity-0 pointer-events-none"}`}>
  Fades in/out
</div>
```

**Opacity scale:** `opacity-0`, `opacity-5`, `opacity-10`, `opacity-25`, `opacity-50`, `opacity-75`, `opacity-90`, `opacity-95`, `opacity-100`

---

### 4.6 — Box Sizing & Isolation

```jsx
{/* box-border (default) — padding/border included in width calculation */}
<div className="box-border w-48 p-4 border-2">
  Width stays 192px regardless of padding
</div>

{/* box-content — padding/border added on top of width */}
<div className="box-content w-48 p-4 border-2">
  Width is 192px + 32px padding + 4px border = 228px
</div>
```

> 💡 **Tip:** `box-border` is the default in Tailwind (via the preflight reset) and is the correct model for 95% of use cases. You rarely need `box-content`.

**`isolate` — fixing z-index stacking context bugs:**

```jsx
{/* isolate creates a new stacking context without any visual change */}
<div className="isolate">
  {/* All z-index inside here is relative to this container */}
  <div className="relative z-10">Modal backdrop</div>
  <div className="relative z-20">Modal content</div>
</div>

{/* isolation-auto — resets isolation */}
<div className="isolation-auto">Back to normal</div>
```

Use `isolate` when you have a section of UI that should manage its own stacking context — for example, a complex card with overlapping elements that shouldn't interact with z-indexes outside the card.

---

---

## Chapter 5 — Flexbox (Complete)

Flexbox is a one-dimensional layout system. Use it for rows and columns of items that need alignment, distribution, and wrapping.

### Flex Container Setup

```jsx
{/* Enable flex on the parent */}
<div className="flex">                   {/* row (default direction) */}
<div className="flex flex-col">          {/* column */}
<div className="flex flex-row-reverse">  {/* row, reversed */}
<div className="flex flex-col-reverse">  {/* column, reversed */}
```

---

### Flex Wrap

```jsx
{/* flex-nowrap (default) — items stay in one line, may overflow */}
<div className="flex flex-nowrap">...</div>

{/* flex-wrap — items wrap to next line when out of space */}
<div className="flex flex-wrap gap-3">
  {tags.map((tag) => <Tag key={tag} label={tag} />)}
</div>

{/* flex-wrap-reverse — items wrap upward */}
<div className="flex flex-wrap-reverse">...</div>
```

---

### Flex Item Sizing

These go on the **child** elements:

| Class | CSS | Behavior |
|---|---|---|
| `flex-1` | `flex: 1 1 0%` | Grows and shrinks, ignores content size — takes equal share |
| `flex-auto` | `flex: 1 1 auto` | Grows and shrinks, starts from content size |
| `flex-initial` | `flex: 0 1 auto` | Doesn't grow, can shrink, starts from content size (default) |
| `flex-none` | `flex: none` | Fixed size, won't grow or shrink |

```jsx
{/* flex-1 — equal width columns */}
<div className="flex gap-4">
  <div className="flex-1 bg-blue-100 p-4">Column 1 (equal)</div>
  <div className="flex-1 bg-green-100 p-4">Column 2 (equal)</div>
  <div className="flex-1 bg-red-100 p-4">Column 3 (equal)</div>
</div>

{/* flex-none — fixed sidebar, flex-1 main content */}
<div className="flex h-screen">
  <aside className="flex-none w-64 bg-gray-900 text-white p-4">Sidebar</aside>
  <main className="flex-1 overflow-auto p-6">Main content fills remaining space</main>
</div>
```

---

### `grow` and `shrink`

```jsx
{/* grow — item can grow to fill available space */}
<div className="flex gap-2">
  <input className="grow px-3 py-2 border rounded-lg" placeholder="Search..." />
  <button className="flex-none px-4 py-2 bg-blue-600 text-white rounded-lg">Go</button>
</div>

{/* grow-0 — explicitly prevent growing */}
<div className="flex-1 grow-0">Won't grow beyond its content</div>

{/* shrink-0 — prevent item from shrinking (important for icons, avatars) */}
<div className="flex items-center gap-3">
  <img src="avatar.jpg" className="shrink-0 w-10 h-10 rounded-full object-cover" />
  <div className="min-w-0">  {/* min-w-0 needed for text truncation inside flex */}
    <p className="font-medium truncate">Very Long User Name Here</p>
    <p className="text-sm text-gray-500 truncate">very.long.email@example.com</p>
  </div>
</div>
```

---

### `basis-*` — Flex Basis

Sets the initial size before growing/shrinking:

```jsx
{/* basis controls starting size */}
<div className="flex gap-4">
  <div className="basis-64 shrink-0 bg-gray-100 p-4">Always 256px wide start</div>
  <div className="grow bg-blue-50 p-4">Fills rest</div>
</div>

{/* fractional basis */}
<div className="flex gap-4">
  <div className="basis-1/3 bg-red-100 p-4">1/3</div>
  <div className="basis-2/3 bg-blue-100 p-4">2/3</div>
</div>
```

---

### `justify-*` — Main Axis Alignment

Controls how items are distributed along the **main axis** (horizontal in `flex-row`, vertical in `flex-col`):

```jsx
<div className="flex justify-start">   {/* [item] [item] [item]               */}
<div className="flex justify-end">     {/*               [item] [item] [item] */}
<div className="flex justify-center">  {/*       [item] [item] [item]         */}
<div className="flex justify-between"> {/* [item]       [item]       [item]   */}
<div className="flex justify-around">  {/*  _[item]_   _[item]_   _[item]_   */}
<div className="flex justify-evenly">  {/* __[item]___[item]___[item]__       */}
<div className="flex justify-stretch"> {/* items stretch to fill              */}
```

---

### `items-*` — Cross Axis Alignment

Controls alignment on the **cross axis** (vertical in `flex-row`, horizontal in `flex-col`):

```jsx
<div className="flex items-start">    {/* aligned to top */}
<div className="flex items-end">      {/* aligned to bottom */}
<div className="flex items-center">   {/* centered vertically */}
<div className="flex items-stretch">  {/* stretch to fill container height (default) */}
<div className="flex items-baseline"> {/* align text baselines */}
```

---

### `self-*` — Individual Item Override

```jsx
<div className="flex items-center h-24">
  <div className="self-start">Top</div>
  <div className="self-center">Center (inherits)</div>
  <div className="self-end">Bottom</div>
  <div className="self-stretch">Full height</div>
</div>
```

---

### `gap-*` — Gaps Between Items

```jsx
{/* Uniform gap */}
<div className="flex gap-4">                  {/* 16px all sides */}
<div className="flex gap-x-4 gap-y-2">        {/* 16px horizontal, 8px vertical */}
<div className="flex flex-wrap gap-3">         {/* 12px gap with wrapping */}
```

**`gap-*` vs `space-x-*` / `space-y-*`:**

| | `gap-*` | `space-x-*` / `space-y-*` |
|---|---|---|
| Works with | `flex`, `grid` | `flex` only (adds margin to children) |
| Works with wrap | ✅ Yes | ❌ Issues with wrapped items |
| Inspectable in DevTools | Cleaner | Adds margin to DOM elements |
| **Recommendation** | ✅ Prefer this | Only for legacy or specific cases |

```jsx
{/* ✅ Prefer gap */}
<div className="flex flex-wrap gap-4">
  {items.map(item => <Item key={item.id} />)}
</div>

{/* ⚠️ space-x — avoid with flex-wrap */}
<div className="flex space-x-4">
  <Item /><Item /><Item />
</div>
```

---

### `order-*` — Reordering Without Changing DOM

```jsx
{/* Reorder visually without changing HTML order (important for accessibility) */}
<div className="flex">
  <div className="order-3">Third visually, first in DOM</div>
  <div className="order-1">First visually</div>
  <div className="order-2">Second visually</div>
</div>

{/* Mobile: image below text; Desktop: image first */}
<div className="flex flex-col md:flex-row gap-8">
  <div className="order-2 md:order-1 md:flex-1">
    <h1>Heading</h1>
    <p>Description</p>
  </div>
  <div className="order-1 md:order-2 md:w-96">
    <img src="hero.jpg" className="w-full rounded-xl" />
  </div>
</div>
```

**⚠️ Gotcha:** Changing visual order with `order-*` without changing DOM order can confuse screen readers and keyboard navigation. Only use this when the visual reorder doesn't change the logical reading order.

---

### Real Flexbox Examples

#### 1. Navbar with Logo Left + Links Right

```jsx
function Navbar() {
  return (
    <nav className="flex items-center justify-between px-6 py-4 bg-white border-b border-gray-200">
      {/* Logo — fixed left */}
      <a href="/" className="flex items-center gap-2 text-xl font-bold text-gray-900">
        <div className="w-8 h-8 bg-blue-600 rounded-lg" />
        Acme
      </a>

      {/* Links — center */}
      <div className="hidden md:flex items-center gap-6">
        <a href="#" className="text-sm font-medium text-gray-600 hover:text-gray-900 transition-colors">Features</a>
        <a href="#" className="text-sm font-medium text-gray-600 hover:text-gray-900 transition-colors">Pricing</a>
        <a href="#" className="text-sm font-medium text-gray-600 hover:text-gray-900 transition-colors">Docs</a>
      </div>

      {/* CTA — right */}
      <div className="flex items-center gap-3">
        <a href="/login" className="hidden md:block text-sm font-medium text-gray-600 hover:text-gray-900">
          Sign in
        </a>
        <a href="/signup" className="px-4 py-2 bg-blue-600 text-white text-sm font-medium rounded-lg hover:bg-blue-700 transition-colors">
          Get started
        </a>
      </div>
    </nav>
  );
}
```

#### 2. Card Footer Pushed to Bottom

```jsx
{/* Card where footer always sticks to bottom regardless of content height */}
<div className="flex flex-col h-full bg-white rounded-xl border border-gray-200 p-6">
  <div className="flex items-center gap-3 mb-4">
    <img src={author.avatar} className="w-10 h-10 rounded-full shrink-0" alt={author.name} />
    <div className="min-w-0">
      <p className="font-semibold text-gray-900 truncate">{author.name}</p>
      <p className="text-sm text-gray-500">{author.role}</p>
    </div>
  </div>

  {/* Content grows to fill space */}
  <div className="grow">
    <h3 className="font-bold text-gray-900">{post.title}</h3>
    <p className="mt-2 text-sm text-gray-600 line-clamp-3">{post.excerpt}</p>
  </div>

  {/* Footer always at bottom */}
  <div className="flex items-center justify-between mt-4 pt-4 border-t border-gray-100">
    <span className="text-xs text-gray-400">{post.date}</span>
    <a href={post.url} className="text-xs font-medium text-blue-600 hover:text-blue-700">
      Read more →
    </a>
  </div>
</div>
```

#### 3. Centered Hero Content

```jsx
function Hero() {
  return (
    <section className="flex flex-col items-center justify-center min-h-[80vh] text-center px-4">
      <span className="inline-flex items-center gap-2 px-3 py-1 bg-blue-50 text-blue-700 text-sm font-medium rounded-full mb-6">
        <span className="w-2 h-2 bg-blue-500 rounded-full animate-pulse" />
        New: v2.0 is here
      </span>

      <h1 className="text-4xl sm:text-5xl lg:text-6xl font-bold text-gray-900 max-w-4xl leading-tight">
        Build faster with{" "}
        <span className="bg-clip-text text-transparent bg-gradient-to-r from-blue-600 to-cyan-500">
          modern tooling
        </span>
      </h1>

      <p className="mt-6 text-lg text-gray-600 max-w-2xl leading-relaxed">
        Stop fighting your tools. Start shipping products. Our platform gives you everything you need to go from idea to production.
      </p>

      <div className="flex flex-col sm:flex-row items-center gap-4 mt-10">
        <a href="/signup" className="px-8 py-3 bg-blue-600 text-white font-semibold rounded-xl hover:bg-blue-700 transition-colors shadow-lg shadow-blue-500/25">
          Start for free
        </a>
        <a href="/demo" className="flex items-center gap-2 px-8 py-3 text-gray-700 font-semibold hover:text-gray-900 transition-colors">
          <PlayCircle className="w-5 h-5" />
          Watch demo
        </a>
      </div>
    </section>
  );
}
```

#### 4. Sidebar Layout

```jsx
function AppLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen bg-gray-50">
      {/* Sidebar */}
      <aside className="flex-none w-64 bg-white border-r border-gray-200 flex flex-col">
        {/* Logo */}
        <div className="flex items-center gap-2 px-6 py-4 border-b border-gray-200">
          <div className="w-8 h-8 bg-blue-600 rounded-lg" />
          <span className="font-bold text-gray-900">Dashboard</span>
        </div>

        {/* Nav links — grows */}
        <nav className="grow px-3 py-4 flex flex-col gap-1">
          {navItems.map((item) => (
            <a
              key={item.href}
              href={item.href}
              className="flex items-center gap-3 px-3 py-2 rounded-lg text-sm font-medium text-gray-600 hover:bg-gray-50 hover:text-gray-900 transition-colors"
            >
              <item.Icon className="w-5 h-5 shrink-0" />
              {item.label}
            </a>
          ))}
        </nav>

        {/* User profile — pinned to bottom */}
        <div className="flex-none border-t border-gray-200 px-4 py-4">
          <div className="flex items-center gap-3">
            <img src={user.avatar} className="w-8 h-8 rounded-full shrink-0" />
            <div className="min-w-0 grow">
              <p className="text-sm font-medium text-gray-900 truncate">{user.name}</p>
              <p className="text-xs text-gray-500 truncate">{user.email}</p>
            </div>
            <button className="flex-none p-1 hover:bg-gray-100 rounded">
              <MoreHorizontal className="w-4 h-4 text-gray-500" />
            </button>
          </div>
        </div>
      </aside>

      {/* Main content */}
      <main className="flex-1 overflow-auto">
        <div className="max-w-6xl mx-auto p-8">
          {children}
        </div>
      </main>
    </div>
  );
}
```

---

> 💡 **Tip — The `min-w-0` trick:** When you have text truncation (`truncate`) inside a flex child, the text often refuses to truncate because flex items have `min-width: auto` by default. Adding `min-w-0` to the flex child overrides this, allowing the text to shrink properly.

```jsx
{/* ❌ Text won't truncate — min-width: auto prevents it */}
<div className="flex items-center gap-3">
  <Avatar />
  <p className="truncate">Very long name that should truncate</p>
</div>

{/* ✅ Works — min-w-0 allows shrinking */}
<div className="flex items-center gap-3">
  <Avatar className="shrink-0" />
  <p className="min-w-0 truncate">Very long name that truncates correctly</p>
</div>
```

---


---

---

## Chapter 6 — Grid (Complete)

CSS Grid is a two-dimensional layout system. Use it when you need to control both rows AND columns simultaneously — dashboards, page layouts, card grids, and complex UI structures.

### Grid Basics

```jsx
{/* Enable grid on the parent */}
<div className="grid grid-cols-3 gap-6">
  <div>Column 1</div>
  <div>Column 2</div>
  <div>Column 3</div>
</div>
```

---

### `grid-cols-{n}` and `grid-rows-{n}`

```jsx
{/* Fixed column counts */}
<div className="grid grid-cols-1">  {/* 1 column */}
<div className="grid grid-cols-2">  {/* 2 equal columns */}
<div className="grid grid-cols-3">  {/* 3 equal columns */}
<div className="grid grid-cols-4">  {/* 4 equal columns */}
<div className="grid grid-cols-12"> {/* 12-column grid */}

{/* Fixed row heights */}
<div className="grid grid-rows-3">  {/* 3 equal rows */}
<div className="grid grid-rows-[auto_1fr_auto]"> {/* header + main + footer */}
```

---

### `col-span-*` and `row-span-*`

Items can span multiple columns or rows:

```jsx
<div className="grid grid-cols-4 gap-4">
  {/* Spans all 4 columns */}
  <div className="col-span-4 bg-blue-100 p-4 rounded">Full width header</div>

  {/* Spans 3 columns */}
  <div className="col-span-3 bg-green-100 p-4 rounded">Main content</div>

  {/* Single column (sidebar) */}
  <div className="col-span-1 bg-gray-100 p-4 rounded">Sidebar</div>

  {/* Row spanning */}
  <div className="row-span-2 col-span-2 bg-purple-100 p-4 rounded">
    Spans 2 rows and 2 columns
  </div>
  <div className="col-span-2 bg-yellow-100 p-4 rounded">Top right</div>
  <div className="col-span-2 bg-pink-100 p-4 rounded">Bottom right</div>
</div>
```

---

### `col-start-*` / `col-end-*` / `row-start-*` / `row-end-*`

Precisely place items on specific grid lines (lines are numbered from 1):

```jsx
<div className="grid grid-cols-6 gap-4">
  {/* Starts at column line 2, ends at line 4 (spans 2 columns) */}
  <div className="col-start-2 col-end-4 bg-blue-100 p-4 rounded">Columns 2–3</div>

  {/* Starts at column line 1, ends at line -1 (last line = full width) */}
  <div className="col-start-1 col-end-7 bg-green-100 p-4 rounded">Full width</div>

  {/* Place in specific row */}
  <div className="row-start-2 col-start-3 bg-red-100 p-4 rounded">Row 2, Col 3</div>
</div>
```

---

### Arbitrary Grid Templates

When you need non-equal columns, use arbitrary values:

```jsx
{/* 200px sidebar + 1fr main + 300px panel */}
<div className="grid grid-cols-[200px_1fr_300px] gap-6 h-screen">
  <aside className="bg-gray-900 text-white p-4">Sidebar</aside>
  <main className="p-6">Main content</main>
  <aside className="bg-gray-50 p-4">Right panel</aside>
</div>

{/* Min-max column sizing */}
<div className="grid grid-cols-[minmax(200px,1fr)_2fr] gap-4">
  <div>Flexible sidebar (200px min)</div>
  <div>Main content (2x wider)</div>
</div>

{/* Auto-fit responsive grid — no media queries needed! */}
<div className="grid grid-cols-[repeat(auto-fit,minmax(280px,1fr))] gap-6">
  {cards.map(card => <Card key={card.id} {...card} />)}
</div>
```

> 💡 **Tip:** `grid-cols-[repeat(auto-fit,minmax(280px,1fr))]` is one of the most powerful CSS tricks. It creates as many columns as fit in the container, each at least 280px wide. Fully responsive with zero media queries.

---

### `auto-cols-*` / `auto-rows-*` / `grid-flow-*`

Controls how implicitly created tracks are sized and how items flow:

```jsx
{/* Auto rows with fixed height */}
<div className="grid auto-rows-[200px] grid-cols-3 gap-4">
  {/* All rows will be exactly 200px tall */}
  {items.map(item => <div key={item.id}>{item.name}</div>)}
</div>

{/* auto-rows-min — row height = tallest item */}
<div className="grid auto-rows-min grid-cols-3 gap-4">...</div>

{/* grid-flow-col — fill columns first, then new row */}
<div className="grid grid-flow-col auto-cols-fr gap-4">
  <div>A</div><div>B</div><div>C</div>
  {/* Lays out horizontally */}
</div>

{/* grid-flow-dense — fill gaps left by spanning items */}
<div className="grid grid-cols-3 grid-flow-dense gap-4">
  <div className="col-span-2">Wide item</div>
  <div>Short item fills the gap</div>
</div>
```

---

### Alignment in Grid

```jsx
{/* place-items — shorthand for align-items + justify-items */}
<div className="grid grid-cols-3 place-items-center gap-4 h-64">
  {/* All items centered in their grid cells */}
  <div className="w-16 h-16 bg-blue-500 rounded"></div>
  <div className="w-24 h-8 bg-green-500 rounded"></div>
  <div className="w-8 h-24 bg-red-500 rounded"></div>
</div>

{/* place-content — align the entire grid within its container */}
<div className="grid grid-cols-2 place-content-center gap-4 h-screen">
  <div>Item 1</div>
  <div>Item 2</div>
</div>

{/* place-self — override alignment for individual items */}
<div className="grid grid-cols-3 gap-4 h-32">
  <div className="place-self-start">Top-left</div>
  <div className="place-self-center">Center</div>
  <div className="place-self-end">Bottom-right</div>
</div>

{/* Separate justify and align */}
<div className="grid grid-cols-3 items-start justify-items-center gap-4">
  {/* Cross-axis: top-aligned, Main-axis: centered */}
</div>
```

---

### Real Grid Examples

#### 1. Responsive 12-Column Layout

```jsx
function PageLayout() {
  return (
    <div className="grid grid-cols-12 gap-6 max-w-7xl mx-auto px-4 py-8">
      {/* Full width hero */}
      <div className="col-span-12">
        <Hero />
      </div>

      {/* Feature cards: 4 cols on lg, 6 on md, 12 on mobile */}
      {features.map((feature) => (
        <div key={feature.id} className="col-span-12 md:col-span-6 lg:col-span-3">
          <FeatureCard {...feature} />
        </div>
      ))}

      {/* Main content: 8/12 cols; sidebar: 4/12 */}
      <div className="col-span-12 lg:col-span-8">
        <MainContent />
      </div>
      <div className="col-span-12 lg:col-span-4">
        <Sidebar />
      </div>
    </div>
  );
}
```

#### 2. Dashboard Grid with Spanning Cards

```jsx
function Dashboard() {
  return (
    <div className="grid grid-cols-12 gap-4 p-6">
      {/* Stats row */}
      <div className="col-span-12 sm:col-span-6 xl:col-span-3 bg-white rounded-xl border border-gray-200 p-6">
        <StatCard label="Revenue" value="$24,512" change="+12%" />
      </div>
      <div className="col-span-12 sm:col-span-6 xl:col-span-3 bg-white rounded-xl border border-gray-200 p-6">
        <StatCard label="Users" value="8,234" change="+5%" />
      </div>
      <div className="col-span-12 sm:col-span-6 xl:col-span-3 bg-white rounded-xl border border-gray-200 p-6">
        <StatCard label="Orders" value="1,429" change="+3%" />
      </div>
      <div className="col-span-12 sm:col-span-6 xl:col-span-3 bg-white rounded-xl border border-gray-200 p-6">
        <StatCard label="Conversion" value="3.24%" change="-0.1%" />
      </div>

      {/* Large chart — spans 8 columns */}
      <div className="col-span-12 xl:col-span-8 bg-white rounded-xl border border-gray-200 p-6">
        <h3 className="font-semibold text-gray-900 mb-4">Revenue Chart</h3>
        <RevenueChart />
      </div>

      {/* Activity feed — spans 4 columns */}
      <div className="col-span-12 xl:col-span-4 bg-white rounded-xl border border-gray-200 p-6">
        <h3 className="font-semibold text-gray-900 mb-4">Recent Activity</h3>
        <ActivityFeed />
      </div>

      {/* Full width table */}
      <div className="col-span-12 bg-white rounded-xl border border-gray-200">
        <DataTable />
      </div>
    </div>
  );
}
```

#### 3. Holy Grail Layout (Header + Sidebar + Main + Footer)

```jsx
function HolyGrail({ children }: { children: React.ReactNode }) {
  return (
    <div className="grid grid-rows-[auto_1fr_auto] grid-cols-[240px_1fr] min-h-screen">
      {/* Header — spans all columns */}
      <header className="col-span-2 bg-white border-b border-gray-200 px-6 py-4 flex items-center justify-between">
        <Logo />
        <UserMenu />
      </header>

      {/* Sidebar */}
      <aside className="bg-gray-900 text-white overflow-y-auto">
        <SidebarNav />
      </aside>

      {/* Main content */}
      <main className="overflow-auto bg-gray-50 p-8">
        {children}
      </main>

      {/* Footer — spans all columns */}
      <footer className="col-span-2 bg-white border-t border-gray-200 px-6 py-4 text-center text-sm text-gray-500">
        © 2025 Acme Inc.
      </footer>
    </div>
  );
}
```

#### 4. Masonry-Like Layout Trick

True masonry requires JavaScript, but CSS Grid `grid-flow-dense` gives an approximation:

```jsx
{/* Items flow into gaps, creating a denser layout */}
<div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 grid-flow-dense gap-4">
  {photos.map((photo, i) => (
    <div
      key={photo.id}
      className={`rounded-xl overflow-hidden ${
        // Every 5th item is wide, every 7th is tall
        i % 5 === 0 ? "col-span-2" :
        i % 7 === 0 ? "row-span-2" : ""
      }`}
    >
      <img src={photo.src} className="w-full h-full object-cover" alt={photo.alt} />
    </div>
  ))}
</div>
```

---

### CSS Grid Named Areas with Arbitrary Values

```jsx
{/* Using grid-template-areas via arbitrary values */}
<div className="grid [grid-template-areas:'header_header''sidebar_main''footer_footer'] grid-cols-[200px_1fr] grid-rows-[64px_1fr_48px] min-h-screen gap-0">
  <header className="[grid-area:header] bg-white border-b flex items-center px-6">Header</header>
  <nav className="[grid-area:sidebar] bg-gray-900 text-white">Sidebar</nav>
  <main className="[grid-area:main] p-8 bg-gray-50">Main</main>
  <footer className="[grid-area:footer] bg-white border-t flex items-center justify-center text-sm text-gray-500">Footer</footer>
</div>
```

---

---

## Chapter 7 — Spacing

Tailwind's spacing scale is based on a **4px unit** (`1 = 4px`). This maps directly to common design tool grids (Figma, Sketch). Padding and margin share the same scale.

### 7.1 — Padding

| Class | Property | CSS output |
|---|---|---|
| `p-{n}` | All sides | `padding: {n * 4}px` |
| `px-{n}` | Left + Right | `padding-left: ...; padding-right: ...` |
| `py-{n}` | Top + Bottom | `padding-top: ...; padding-bottom: ...` |
| `pt-{n}` | Top | `padding-top: ...` |
| `pr-{n}` | Right | `padding-right: ...` |
| `pb-{n}` | Bottom | `padding-bottom: ...` |
| `pl-{n}` | Left | `padding-left: ...` |
| `ps-{n}` | Start (logical) | `padding-inline-start: ...` |
| `pe-{n}` | End (logical) | `padding-inline-end: ...` |

**Full spacing scale with pixel equivalents:**

| Value | px | rem |
|---|---|---|
| `0` | 0px | 0 |
| `0.5` | 2px | 0.125rem |
| `1` | 4px | 0.25rem |
| `1.5` | 6px | 0.375rem |
| `2` | 8px | 0.5rem |
| `2.5` | 10px | 0.625rem |
| `3` | 12px | 0.75rem |
| `3.5` | 14px | 0.875rem |
| `4` | 16px | 1rem |
| `5` | 20px | 1.25rem |
| `6` | 24px | 1.5rem |
| `7` | 28px | 1.75rem |
| `8` | 32px | 2rem |
| `9` | 36px | 2.25rem |
| `10` | 40px | 2.5rem |
| `11` | 44px | 2.75rem |
| `12` | 48px | 3rem |
| `14` | 56px | 3.5rem |
| `16` | 64px | 4rem |
| `20` | 80px | 5rem |
| `24` | 96px | 6rem |
| `28` | 112px | 7rem |
| `32` | 128px | 8rem |
| `36` | 144px | 9rem |
| `40` | 160px | 10rem |
| `44` | 176px | 11rem |
| `48` | 192px | 12rem |
| `52` | 208px | 13rem |
| `56` | 224px | 14rem |
| `60` | 240px | 15rem |
| `64` | 256px | 16rem |
| `72` | 288px | 18rem |
| `80` | 320px | 20rem |
| `96` | 384px | 24rem |

```jsx
{/* Padding examples */}
<div className="p-4">16px all sides</div>
<div className="px-6 py-3">24px horizontal, 12px vertical</div>
<div className="pt-8 pb-4 px-6">32px top, 16px bottom, 24px sides</div>
<div className="p-[18px]">Arbitrary: exactly 18px</div>

{/* Real button padding */}
<button className="px-4 py-2 text-sm">Small button</button>
<button className="px-6 py-3 text-base">Medium button</button>
<button className="px-8 py-4 text-lg">Large button</button>
```

---

#### `ps-*` / `pe-*` — Logical Properties for RTL Support

Physical properties (`pl`, `pr`) always mean left and right. Logical properties (`ps`, `pe`) mean "start" and "end" — which flip in RTL languages (Arabic, Hebrew):

```jsx
{/* In LTR: ps-4 = padding-left: 1rem */}
{/* In RTL: ps-4 = padding-right: 1rem (start is right in RTL) */}
<div className="ps-4 pe-2" dir="auto">
  Supports both LTR and RTL automatically
</div>
```

---

### 7.2 — Margin

All the same directional variants as padding: `m-*`, `mx-*`, `my-*`, `mt-*`, `mr-*`, `mb-*`, `ml-*`.

```jsx
{/* Centering with mx-auto */}
<div className="max-w-3xl mx-auto px-4">
  {/* Centers the block horizontally */}
  Content
</div>

{/* Negative margins — pull elements closer or create overlaps */}
<div className="mt-8">
  <div className="-mt-4 bg-white shadow rounded-xl p-6">
    This card overlaps the section above by 16px
  </div>
</div>

{/* Negative margin for overlapping avatars */}
<div className="flex">
  {avatars.map((avatar, i) => (
    <img
      key={i}
      src={avatar}
      className="w-8 h-8 rounded-full border-2 border-white -ml-2 first:ml-0"
    />
  ))}
</div>
```

---

#### `space-x-*` / `space-y-*` vs `gap-*`

`space-x-*` adds `margin-left` to all children except the first. It's a flex-only pattern that predates `gap` support in browsers.

```jsx
{/* space-x — adds margin between flex children */}
<div className="flex space-x-4">
  <button>One</button>
  <button>Two</button>
  <button>Three</button>
</div>

{/* gap — modern, works with flex AND grid, works with flex-wrap */}
<div className="flex gap-4">
  <button>One</button>
  <button>Two</button>
  <button>Three</button>
</div>
```

**✅ Best Practice:** Use `gap-*` everywhere. Use `space-x-*` only if you need to support older browsers or have a specific case where gap causes issues.

**⚠️ Gotcha:** `space-x-*` breaks with `flex-wrap` because the first item in a new row still gets `margin-left: 0`, causing misalignment. `gap` handles this correctly.

---

---

## Chapter 8 — Sizing

### 8.1 — Width

**Fixed widths** (same spacing scale as padding/margin):

```jsx
<div className="w-4">16px</div>
<div className="w-16">64px</div>
<div className="w-64">256px</div>
<div className="w-96">384px</div>
```

**Fractional widths** (percentage-based):

```jsx
<div className="w-1/2">50%</div>
<div className="w-1/3">33.33%</div>
<div className="w-2/3">66.67%</div>
<div className="w-1/4">25%</div>
<div className="w-3/4">75%</div>
<div className="w-1/5">20%</div>
<div className="w-2/5">40%</div>
<div className="w-3/5">60%</div>
<div className="w-4/5">80%</div>
<div className="w-1/6">16.67%</div>
<div className="w-5/6">83.33%</div>
```

**Special widths:**

```jsx
<div className="w-full">width: 100%</div>
<div className="w-screen">width: 100vw</div>
<div className="w-svw">width: 100svw (small viewport width)</div>
<div className="w-lvw">width: 100lvw (large viewport width)</div>
<div className="w-dvw">width: 100dvw (dynamic viewport width)</div>
<div className="w-min">width: min-content</div>
<div className="w-max">width: max-content</div>
<div className="w-fit">width: fit-content</div>
<div className="w-auto">width: auto</div>
```

**Arbitrary widths:**

```jsx
<div className="w-[347px]">Exactly 347px</div>
<div className="w-[calc(100%-2rem)]">Full width minus 2rem</div>
<div className="w-[min(100%,48rem)]">Min of 100% and 48rem</div>
```

---

### 8.2 — Height

Same pattern as width with `h-*`. Key additions:

```jsx
<div className="h-screen">height: 100vh</div>
<div className="h-svh">height: 100svh (small viewport height)</div>
<div className="h-lvh">height: 100lvh (large viewport height)</div>
<div className="h-dvh">height: 100dvh (dynamic viewport height)</div>
<div className="h-full">height: 100%</div>
<div className="h-auto">height: auto</div>
```

**The `dvh` trick for mobile viewports:**

The classic `100vh` bug: on mobile browsers, `100vh` includes the browser's address bar, so your "full screen" section is actually taller than the visible viewport. When the address bar hides on scroll, content gets cut off.

```jsx
{/* ❌ Old approach — too tall on mobile when address bar is visible */}
<section className="min-h-screen">Full page section</section>

{/* ✅ Modern approach — exactly the visible viewport height */}
<section className="min-h-dvh">True full screen on mobile too</section>

{/* Progressive enhancement — dvh with fallback */}
<section className="min-h-screen min-h-dvh">
  {/* min-h-screen is the fallback, min-h-dvh overrides if supported */}
</section>
```

---

### 8.3 — Min/Max Width and Height

```jsx
{/* Max width — constraining content width */}
<div className="max-w-xs">320px</div>
<div className="max-w-sm">384px</div>
<div className="max-w-md">448px</div>
<div className="max-w-lg">512px</div>
<div className="max-w-xl">576px</div>
<div className="max-w-2xl">672px</div>
<div className="max-w-3xl">768px</div>
<div className="max-w-4xl">896px</div>
<div className="max-w-5xl">1024px</div>
<div className="max-w-6xl">1152px</div>
<div className="max-w-7xl">1280px</div>
<div className="max-w-full">100%</div>
<div className="max-w-none">none</div>

{/* max-w-prose — optimal reading width (~65 characters) */}
<article className="max-w-prose mx-auto px-4">
  <p>This text will never be wider than ~65 characters, which is the optimal reading line length for prose.</p>
</article>

{/* max-w-screen-* — breakpoint-aligned max widths */}
<div className="max-w-screen-sm">640px</div>
<div className="max-w-screen-md">768px</div>
<div className="max-w-screen-lg">1024px</div>
<div className="max-w-screen-xl">1280px</div>

{/* Min width */}
<button className="min-w-[120px]">Button (never narrower than 120px)</button>

{/* Max height — scrollable containers */}
<div className="max-h-64 overflow-auto">
  Scrolls when content exceeds 256px tall
</div>
<div className="max-h-[80vh] overflow-y-auto">
  Scrolls at 80% of viewport height
</div>
```

---

### 8.4 — Aspect Ratio

Controls the ratio between width and height:

```jsx
{/* Built-in ratios */}
<div className="aspect-square">1:1 square</div>
<div className="aspect-video">16:9 (video)</div>

{/* Arbitrary ratios */}
<div className="aspect-[4/3]">4:3 (traditional monitor)</div>
<div className="aspect-[3/4]">Portrait 3:4</div>
<div className="aspect-[21/9]">Ultrawide 21:9</div>

{/* Responsive video embed */}
<div className="w-full aspect-video rounded-xl overflow-hidden">
  <iframe
    src="https://youtube.com/embed/..."
    className="w-full h-full"
    allowFullScreen
  />
</div>

{/* Responsive image card */}
<div className="aspect-[4/3] overflow-hidden rounded-xl bg-gray-100">
  <img
    src={photo.url}
    alt={photo.alt}
    className="w-full h-full object-cover hover:scale-105 transition-transform duration-300"
  />
</div>

{/* Profile avatar — always square */}
<img
  src={user.avatar}
  alt={user.name}
  className="w-16 aspect-square rounded-full object-cover"
/>
```

---

---

## Chapter 9 — Typography (Complete)

### Font Family

```jsx
<p className="font-sans">   {/* ui-sans-serif, system-ui, sans-serif */}
<p className="font-serif">  {/* ui-serif, Georgia, serif */}
<p className="font-mono">   {/* ui-monospace, SFMono-Regular, monospace */}
```

---

### Custom Fonts — Adding Google Fonts + Tailwind Config

**Step 1 — Add to your HTML `<head>` or import in CSS:**

```html
<!-- In index.html -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
```

Or in CSS (v4 preferred):

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
```

**Step 2 — Configure in Tailwind:**

```ts
// tailwind.config.ts (v3)
import { fontFamily } from "tailwindcss/defaultTheme";

export default {
  theme: {
    extend: {
      fontFamily: {
        sans: ["Inter", ...fontFamily.sans], // replaces default sans
        display: ["Cal Sans", ...fontFamily.sans], // new font family
        code: ["JetBrains Mono", ...fontFamily.mono],
      },
    },
  },
};
```

```css
/* index.css (v4) */
@import "tailwindcss";

@theme {
  --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;
  --font-display: "Cal Sans", ui-sans-serif, sans-serif;
  --font-mono: "JetBrains Mono", ui-monospace, monospace;
}
```

```jsx
<h1 className="font-display text-4xl font-bold">Display heading</h1>
<p className="font-sans text-base">Body text</p>
<code className="font-mono text-sm">Code snippet</code>
```

---

### Font Size Scale

| Class | Font size | Line height |
|---|---|---|
| `text-xs` | 0.75rem (12px) | 1rem (16px) |
| `text-sm` | 0.875rem (14px) | 1.25rem (20px) |
| `text-base` | 1rem (16px) | 1.5rem (24px) |
| `text-lg` | 1.125rem (18px) | 1.75rem (28px) |
| `text-xl` | 1.25rem (20px) | 1.75rem (28px) |
| `text-2xl` | 1.5rem (24px) | 2rem (32px) |
| `text-3xl` | 1.875rem (30px) | 2.25rem (36px) |
| `text-4xl` | 2.25rem (36px) | 2.5rem (40px) |
| `text-5xl` | 3rem (48px) | 1 |
| `text-6xl` | 3.75rem (60px) | 1 |
| `text-7xl` | 4.5rem (72px) | 1 |
| `text-8xl` | 6rem (96px) | 1 |
| `text-9xl` | 8rem (128px) | 1 |

```jsx
{/* Type scale in action */}
<h1 className="text-5xl font-bold tracking-tight text-gray-900">Hero Heading</h1>
<h2 className="text-3xl font-bold text-gray-900">Section Title</h2>
<h3 className="text-xl font-semibold text-gray-800">Subsection</h3>
<p className="text-base text-gray-600 leading-relaxed">Body paragraph text.</p>
<span className="text-sm text-gray-500">Caption or helper text</span>
<span className="text-xs text-gray-400 uppercase tracking-wider">Label</span>
```

---

### Font Weight

| Class | `font-weight` |
|---|---|
| `font-thin` | 100 |
| `font-extralight` | 200 |
| `font-light` | 300 |
| `font-normal` | 400 |
| `font-medium` | 500 |
| `font-semibold` | 600 |
| `font-bold` | 700 |
| `font-extrabold` | 800 |
| `font-black` | 900 |

---

### Italic

```jsx
<p className="italic">Italic text</p>
<p className="not-italic">Reset to normal (useful inside italic parent)</p>
```

---

### Letter Spacing (`tracking-*`)

| Class | CSS |
|---|---|
| `tracking-tighter` | -0.05em |
| `tracking-tight` | -0.025em |
| `tracking-normal` | 0 |
| `tracking-wide` | 0.025em |
| `tracking-wider` | 0.05em |
| `tracking-widest` | 0.1em |

```jsx
<h1 className="text-6xl font-black tracking-tighter">Tight heading</h1>
<span className="text-xs uppercase tracking-widest font-medium text-gray-500">Label Text</span>
```

---

### Line Height (`leading-*`)

| Class | CSS |
|---|---|
| `leading-none` | 1 |
| `leading-tight` | 1.25 |
| `leading-snug` | 1.375 |
| `leading-normal` | 1.5 |
| `leading-relaxed` | 1.625 |
| `leading-loose` | 2 |
| `leading-3` | 0.75rem |
| `leading-4` | 1rem |
| ... | (follows spacing scale) |
| `leading-10` | 2.5rem |

```jsx
{/* Headlines need tight line height */}
<h1 className="text-5xl font-bold leading-tight">
  Multi-line<br />headline here
</h1>

{/* Body text needs relaxed line height */}
<p className="text-base leading-relaxed text-gray-600">
  Body text reads much better with relaxed line height.
</p>
```

---

### Text Alignment

```jsx
<p className="text-left">Left aligned</p>
<p className="text-center">Centered</p>
<p className="text-right">Right aligned</p>
<p className="text-justify">Justified — spaces words to fill full width of container</p>
<p className="text-start">Start (LTR: left, RTL: right)</p>
<p className="text-end">End (LTR: right, RTL: left)</p>
```

---

### Text Decoration

```jsx
<a className="underline hover:no-underline">Link</a>
<del className="line-through text-gray-400">Deleted price</del>
<span className="overline">Overline</span>

{/* decoration-* — color, style, thickness, offset */}
<a className="underline decoration-blue-500 decoration-2 underline-offset-4 hover:decoration-blue-700">
  Styled underline link
</a>

<span className="underline decoration-wavy decoration-red-500">Wavy underline</span>
<span className="underline decoration-dotted decoration-gray-400">Dotted</span>
<span className="underline decoration-dashed decoration-green-500">Dashed</span>
```

---

### Text Transform

```jsx
<span className="uppercase">all caps</span>
<span className="lowercase">ALL LOWER</span>
<span className="capitalize">first letter of each Word</span>
<span className="normal-case">Resets transform</span>
```

---

### Text Overflow & Truncation

```jsx
{/* Single line truncation */}
<p className="truncate max-w-xs">This very long text will be truncated with an ellipsis</p>

{/* Multi-line truncation with line-clamp */}
<p className="line-clamp-2">
  Two lines of text then ellipsis. This is the most useful text utility for
  card descriptions, feed items, and any content that needs to fit a fixed space.
</p>
<p className="line-clamp-3">Three lines</p>
<p className="line-clamp-4">Four lines</p>
<p className="line-clamp-none">Remove clamping</p>

{/* Text overflow options */}
<p className="text-ellipsis overflow-hidden whitespace-nowrap">Manual truncation</p>
<p className="text-clip overflow-hidden whitespace-nowrap">Clips without ellipsis</p>
```

> 💡 **Tip:** `line-clamp-3` is one of the most-used utilities in production. Every product card, article preview, and feed item needs it. Add `line-clamp-none` on hover to reveal the full text.

---

### Whitespace

```jsx
<pre className="whitespace-pre">Preserves all whitespace and newlines</pre>
<p className="whitespace-pre-wrap">Preserves whitespace, wraps when needed</p>
<p className="whitespace-pre-line">Collapses spaces, preserves newlines</p>
<p className="whitespace-nowrap">Never wraps — stays on one line</p>
<p className="whitespace-normal">Default browser behavior</p>
<p className="whitespace-break-spaces">Preserves sequences of spaces</p>
```

---

### Word Break & Overflow Wrap

```jsx
{/* break-words — breaks long words/URLs to prevent overflow */}
<p className="break-words">
  https://this-is-a-very-long-url-that-would-break-the-layout.example.com/some/deep/path
</p>

{/* break-all — break at any character (avoid for prose) */}
<p className="break-all">Breaks at any character boundary</p>

{/* break-keep — don't break between CJK characters */}
<p className="break-keep">中文文本 不在中间换行</p>
```

---

### Text Wrapping (New CSS)

```jsx
{/* text-balance — evenly distributes text across lines (great for headings) */}
<h2 className="text-balance text-3xl font-bold max-w-md">
  A Nicely Balanced Heading That Wraps Evenly
</h2>

{/* text-pretty — prevents orphans (single words on last line) */}
<p className="text-pretty max-w-prose leading-relaxed">
  This paragraph will avoid leaving a single word alone on the last line.
</p>
```

---

### Lists

```jsx
<ul className="list-disc list-inside space-y-1 text-gray-700">
  <li>Bullet item</li>
  <li>Another item</li>
</ul>

<ol className="list-decimal list-inside space-y-1 text-gray-700">
  <li>First step</li>
  <li>Second step</li>
</ol>

{/* list-outside — marker outside the text box (default browser behavior) */}
<ul className="list-disc list-outside pl-5 space-y-1">
  <li>Item with outside marker</li>
</ul>

{/* list-none — removes markers */}
<ul className="list-none space-y-1">
  <li>No bullet</li>
</ul>
```

---

### Real Example: Blog Post Typography System

```jsx
function BlogPost({ post }: { post: Post }) {
  return (
    <article className="max-w-prose mx-auto px-4 py-12">
      {/* Category label */}
      <span className="text-xs uppercase tracking-widest font-semibold text-blue-600">
        {post.category}
      </span>

      {/* Title */}
      <h1 className="mt-3 text-4xl font-bold text-gray-900 leading-tight text-balance">
        {post.title}
      </h1>

      {/* Meta */}
      <div className="mt-4 flex items-center gap-4 text-sm text-gray-500">
        <span>{post.author}</span>
        <span>·</span>
        <time>{post.date}</time>
        <span>·</span>
        <span>{post.readTime} min read</span>
      </div>

      {/* Hero image */}
      <div className="mt-8 aspect-video rounded-2xl overflow-hidden bg-gray-100">
        <img src={post.heroImage} alt={post.title} className="w-full h-full object-cover" />
      </div>

      {/* Body */}
      <div className="mt-10 space-y-6 text-gray-700 text-lg leading-relaxed">
        <p className="text-xl text-gray-600 leading-relaxed font-light">
          {post.excerpt}
        </p>

        {/* Use @tailwindcss/typography prose class for rendered HTML content */}
        <div
          className="prose prose-lg prose-gray max-w-none prose-headings:font-bold prose-a:text-blue-600 prose-a:no-underline hover:prose-a:underline"
          dangerouslySetInnerHTML={{ __html: post.content }}
        />
      </div>
    </article>
  );
}
```

---

---

## Chapter 10 — Colors & Backgrounds

### 10.1 — The Color System

Tailwind ships a full palette of named colors, each with 11 shades (50–950). Understanding the naming system is essential.

**Full default palette:**

| Family | Description |
|---|---|
| `slate` | Blue-tinted gray — most popular for UIs |
| `gray` | Neutral gray |
| `zinc` | Slightly warm gray |
| `neutral` | Pure neutral gray |
| `stone` | Warm, brownish gray |
| `red` | Pure red |
| `orange` | Orange |
| `amber` | Yellow-orange |
| `yellow` | Yellow |
| `lime` | Yellow-green |
| `green` | Green |
| `emerald` | Rich green (like cash 💚) |
| `teal` | Blue-green |
| `cyan` | Light blue-green |
| `sky` | Light blue |
| `blue` | Classic blue |
| `indigo` | Blue-violet |
| `violet` | Violet |
| `purple` | Purple |
| `fuchsia` | Pink-purple |
| `pink` | Pink |
| `rose` | Red-pink |

**Shade scale logic:**

| Shade | Purpose |
|---|---|
| `50` | Lightest tint — backgrounds, subtle fills |
| `100` | Light tint — hover backgrounds |
| `200` | Light — borders on light backgrounds |
| `300` | Medium-light |
| `400` | Medium — disabled states, icons |
| `500` | Base color — your reference point |
| `600` | Slightly dark — primary buttons |
| `700` | Dark — hover states for buttons |
| `800` | Very dark |
| `900` | Near-black — headings, body text |
| `950` | Near-black, darkest tint |

```jsx
{/* A complete button using the color scale */}
<button className="
  bg-blue-600          /* base state */
  hover:bg-blue-700    /* hover: one shade darker */
  active:bg-blue-800   /* active: two shades darker */
  text-white
  focus:ring-2 focus:ring-blue-500 focus:ring-offset-2
  disabled:bg-blue-300 disabled:cursor-not-allowed
">
  Button
</button>

{/* Using different properties with same color */}
<div className="bg-blue-50 border border-blue-200 text-blue-900 rounded-lg p-4">
  <p className="font-semibold text-blue-800">Info</p>
  <p className="text-blue-700 mt-1">This is an informational message.</p>
</div>
```

---

#### Color with Opacity Modifier

Tailwind v3 uses the slash syntax for color opacity:

```jsx
{/* bg-{color}/{opacity-percentage} */}
<div className="bg-blue-500/50">50% transparent blue background</div>
<div className="bg-blue-500/10">10% — very subtle tint</div>
<div className="bg-blue-500/75">75% opacity</div>

{/* Works for all color utilities */}
<p className="text-red-600/75">75% opacity red text</p>
<div className="border border-gray-900/10">10% opacity border</div>
<div className="shadow-blue-500/25 shadow-lg">25% opacity colored shadow</div>

{/* Backdrop overlay */}
<div className="fixed inset-0 bg-black/50 backdrop-blur-sm z-40">
  Modal overlay
</div>
```

**How it works under the hood:** Tailwind generates CSS with the `rgb(r g b / alpha)` syntax:

```css
.bg-blue-500\/50 {
  background-color: rgb(59 130 246 / 0.5);
}
```

---

#### v4 OKLCH Colors

Tailwind v4 switches from sRGB hex to the **OKLCH** color space. OKLCH (Lightness, Chroma, Hue) is perceptually uniform — meaning equal numeric changes in lightness *look* equal to the human eye. sRGB hex doesn't have this property.

```css
/* v4 theme — colors in OKLCH */
@theme {
  --color-blue-500: oklch(62.3% 0.214 259.8);
  --color-brand: oklch(60% 0.2 250);        /* custom brand color */
  --color-brand-light: oklch(90% 0.1 250);  /* easily derived lighter version */
}
```

**Benefits:**
- Better gradients — no "muddy middle" when interpolating between colors
- Easier to create consistent color palettes (tweak L, keep C and H)
- P3 wide gamut support for vibrant colors on modern displays

---

#### Customizing Colors

**v3 config — extend palette (keep defaults):**

```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        brand: {
          50:  "#eff6ff",
          100: "#dbeafe",
          500: "#3b82f6",
          600: "#2563eb",
          700: "#1d4ed8",
          900: "#1e3a8a",
        },
        // Single flat color
        "primary": "#6366f1",
      },
    },
  },
};
```

**v4 `@theme` — define custom colors:**

```css
@theme {
  /* Flat color */
  --color-primary: oklch(60% 0.2 280);

  /* Color scale */
  --color-brand-50: oklch(97% 0.02 250);
  --color-brand-100: oklch(93% 0.05 250);
  --color-brand-500: oklch(62% 0.18 250);
  --color-brand-700: oklch(45% 0.18 250);
  --color-brand-900: oklch(28% 0.12 250);
}
```

Usage is identical regardless of v3 or v4: `bg-brand-500`, `text-primary`, etc.

---

### 10.2 — Background Utilities

#### Background Color

```jsx
<div className="bg-white">White</div>
<div className="bg-black">Black</div>
<div className="bg-transparent">Transparent</div>
<div className="bg-current">Uses current text color</div>
<div className="bg-inherit">Inherits from parent</div>
```

---

#### Gradients

```jsx
{/* Direction variants */}
<div className="bg-gradient-to-r from-blue-500 to-purple-600">Left to right →</div>
<div className="bg-gradient-to-b from-blue-500 to-purple-600">Top to bottom ↓</div>
<div className="bg-gradient-to-br from-blue-500 to-purple-600">Diagonal ↘</div>
<div className="bg-gradient-to-t from-blue-500 to-transparent">Bottom to top ↑</div>

{/* Directions: t, tr, r, br, b, bl, l, tl */}

{/* Three stops with via */}
<div className="bg-gradient-to-r from-pink-500 via-purple-500 to-blue-500">
  Pink → Purple → Blue
</div>

{/* Transparent gradient stops — fade in/out from a color */}
<div className="bg-gradient-to-b from-blue-600/0 via-blue-600/50 to-blue-600">
  Fades from transparent to blue
</div>

{/* Transparent fade for "more content" overlay */}
<div className="relative">
  <div className="max-h-48 overflow-hidden">
    {longContent}
  </div>
  <div className="absolute inset-x-0 bottom-0 h-16 bg-gradient-to-t from-white to-transparent" />
</div>

{/* Gradient text — THE most-used gradient trick */}
<h1 className="text-5xl font-black bg-clip-text text-transparent bg-gradient-to-r from-purple-600 to-pink-600">
  Gradient Text
</h1>
```

---

#### Background Size, Position, Repeat

```jsx
{/* Background size */}
<div className="bg-cover">Scales to fill container (may crop)</div>
<div className="bg-contain">Scales to fit container (may leave space)</div>
<div className="bg-auto">Natural image size</div>

{/* Background position */}
<div className="bg-center">Centered</div>
<div className="bg-top">Top edge</div>
<div className="bg-bottom">Bottom edge</div>
<div className="bg-left">Left edge</div>
<div className="bg-right">Right edge</div>
<div className="bg-left-top">Top-left corner</div>

{/* Background repeat */}
<div className="bg-no-repeat">No repeat</div>
<div className="bg-repeat">Tile in both directions</div>
<div className="bg-repeat-x">Tile horizontally only</div>
<div className="bg-repeat-y">Tile vertically only</div>

{/* Common pattern: hero background */}
<section
  className="bg-cover bg-center bg-no-repeat min-h-screen"
  style={{ backgroundImage: "url('/hero-bg.jpg')" }}
>
  Content
</section>

{/* In Tailwind (arbitrary) */}
<section className="bg-[url('/hero-bg.jpg')] bg-cover bg-center bg-no-repeat min-h-screen">
  Content
</section>
```

---

#### Background Attachment — Parallax Effect

```jsx
{/* bg-fixed creates a parallax effect — background stays fixed while content scrolls */}
<section
  className="bg-[url('/parallax.jpg')] bg-fixed bg-cover bg-center min-h-screen flex items-center justify-center"
>
  <h1 className="text-6xl font-black text-white drop-shadow-2xl">Parallax Hero</h1>
</section>
```

---

#### `bg-clip-text` + `text-transparent` — Full Gradient Text Example

```jsx
{/* Gradient text — works with any gradient or background */}
<h1 className="text-6xl font-black bg-clip-text text-transparent bg-gradient-to-r from-violet-600 via-pink-600 to-orange-500">
  Colorful Heading
</h1>

{/* Animated gradient text */}
<h1 className="text-6xl font-black bg-clip-text text-transparent bg-gradient-to-r from-blue-600 via-purple-600 to-pink-600 bg-[length:200%] animate-[gradient_3s_linear_infinite]">
  Animated Gradient
</h1>

{/* With custom keyframe in config */}
```

```ts
// tailwind.config.ts — for animated gradient
export default {
  theme: {
    extend: {
      keyframes: {
        gradient: {
          "0%, 100%": { backgroundPosition: "0% 50%" },
          "50%": { backgroundPosition: "100% 50%" },
        },
      },
      animation: {
        gradient: "gradient 4s ease infinite",
      },
    },
  },
};
```

---

#### Glassmorphism Card

The classic "frosted glass" effect combines backdrop blur, semi-transparent background, and subtle border:

```jsx
{/* Full glassmorphism card */}
function GlassCard({ children }: { children: React.ReactNode }) {
  return (
    <div className="relative overflow-hidden rounded-2xl p-6
                    bg-white/10 backdrop-blur-md
                    border border-white/20
                    shadow-xl shadow-black/10">
      {children}
    </div>
  );
}

{/* Usage — needs a colorful background behind it */}
<div className="min-h-screen bg-gradient-to-br from-blue-600 via-purple-600 to-pink-500 flex items-center justify-center p-8">
  <GlassCard>
    <h2 className="text-2xl font-bold text-white">Glass Card</h2>
    <p className="mt-2 text-white/80">Frosted glass effect with backdrop-blur.</p>
    <button className="mt-6 px-6 py-2.5 bg-white/20 hover:bg-white/30 text-white font-medium rounded-xl border border-white/30 transition-colors backdrop-blur-sm">
      Get Started
    </button>
  </GlassCard>
</div>
```

---

#### Mesh Gradient (Advanced)

A mesh gradient is a multi-point gradient using layered radial gradients:

```jsx
{/* Mesh gradient via arbitrary CSS in Tailwind */}
<div className="min-h-screen bg-[radial-gradient(ellipse_at_top_left,_var(--tw-gradient-stops))] from-violet-400 via-transparent to-transparent relative">
  {/* Layer multiple gradients using pseudo-elements */}
  <div className="absolute inset-0 bg-[radial-gradient(ellipse_at_bottom_right,_var(--tw-gradient-stops))] from-pink-400 via-transparent to-transparent" />
  <div className="absolute inset-0 bg-[radial-gradient(ellipse_at_center,_var(--tw-gradient-stops))] from-blue-400/50 via-transparent to-transparent" />
  <div className="relative z-10">
    Content over mesh gradient
  </div>
</div>
```

---


---

---

## Chapter 11 — Borders

### Border Width

```jsx
<div className="border">1px border (all sides)</div>
<div className="border-0">No border</div>
<div className="border-2">2px border</div>
<div className="border-4">4px border</div>
<div className="border-8">8px border</div>

{/* Individual sides */}
<div className="border-t">Top only</div>
<div className="border-r">Right only</div>
<div className="border-b">Bottom only</div>
<div className="border-l">Left only</div>
<div className="border-x">Left + Right</div>
<div className="border-y">Top + Bottom</div>

{/* Side + width */}
<div className="border-b-2 border-b-blue-500">Bottom 2px blue border</div>
<div className="border-t-4 border-t-gray-200">Top 4px gray border</div>
```

---

### Border Color & Opacity

```jsx
<div className="border border-gray-200">Light gray border</div>
<div className="border border-gray-900/10">10% opacity border</div>
<div className="border border-blue-500">Blue border</div>
<div className="border-2 border-red-400/50">50% transparent red</div>

{/* Dark mode border */}
<div className="border border-gray-200 dark:border-gray-700">Adapts to dark mode</div>
```

---

### Border Style

```jsx
<div className="border border-solid">Solid (default)</div>
<div className="border border-dashed">Dashed</div>
<div className="border border-dotted">Dotted</div>
<div className="border-4 border-double">Double (needs border-4+)</div>
<div className="border-none">No border</div>
```

---

### Border Radius

```jsx
{/* Scale */}
<div className="rounded-none">0px</div>
<div className="rounded-sm">2px</div>
<div className="rounded">4px</div>
<div className="rounded-md">6px</div>
<div className="rounded-lg">8px</div>
<div className="rounded-xl">12px</div>
<div className="rounded-2xl">16px</div>
<div className="rounded-3xl">24px</div>
<div className="rounded-full">9999px — pill shape</div>

{/* Per-side */}
<div className="rounded-t-xl">Top corners only</div>
<div className="rounded-b-lg">Bottom corners only</div>
<div className="rounded-l-full">Left side pill</div>
<div className="rounded-r-xl">Right corners</div>

{/* Per-corner */}
<div className="rounded-tl-xl rounded-tr-xl rounded-bl-none rounded-br-none">
  Rounded top, sharp bottom
</div>
<div className="rounded-tl-3xl rounded-br-3xl">Diagonal rounding</div>
```

---

### Outline

```jsx
{/* Outline — renders outside the element, doesn't affect layout */}
<button className="outline outline-2 outline-blue-500 outline-offset-2">
  Outlined button
</button>

{/* Removing default focus outline */}
<input className="focus:outline-none focus:ring-2 focus:ring-blue-500" />

{/* Custom outline on focus */}
<button className="focus:outline focus:outline-2 focus:outline-blue-500 focus:outline-offset-2">
  Accessible focus outline
</button>
```

---

### `ring-*` — Focus Rings

Rings are box-shadows that look like borders. They're the Tailwind-preferred way to show focus:

```jsx
{/* Basic ring */}
<button className="focus:ring-2 focus:ring-blue-500">With focus ring</button>

{/* Ring variants: 1, 2, 4, 8 */}
<button className="ring-1 ring-gray-300">Always visible ring (like border)</button>

{/* Ring color */}
<button className="focus:ring-2 focus:ring-blue-500 focus:ring-offset-2">
  Blue ring with 2px offset
</button>

{/* Ring offset — space between element and ring */}
<button className="focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-gray-100">
  Ring offset with custom color (for non-white backgrounds)
</button>

{/* Dark mode ring */}
<button className="focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-white dark:focus:ring-offset-gray-900">
  Works in both modes
</button>

{/* Complete accessible button */}
<button className="px-4 py-2 bg-blue-600 text-white font-medium rounded-lg
                   hover:bg-blue-700 active:bg-blue-800
                   focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2
                   disabled:opacity-50 disabled:cursor-not-allowed
                   transition-colors">
  Accessible Button
</button>
```

---

### `divide-*` — Borders Between Children

Instead of adding `border-b` to every child except the last, use `divide-*` on the parent:

```jsx
{/* Horizontal dividers between rows */}
<div className="divide-y divide-gray-100">
  {items.map((item) => (
    <div key={item.id} className="py-3 px-4 flex items-center justify-between">
      <span>{item.name}</span>
      <span className="text-sm text-gray-500">{item.value}</span>
    </div>
  ))}
</div>

{/* Vertical dividers between columns */}
<div className="flex divide-x divide-gray-200">
  <div className="px-6 py-4">Section A</div>
  <div className="px-6 py-4">Section B</div>
  <div className="px-6 py-4">Section C</div>
</div>

{/* Colored dividers */}
<div className="divide-y divide-blue-100">
  <div className="py-4">Row 1</div>
  <div className="py-4">Row 2</div>
</div>
```

> 💡 **Tip:** `divide-y divide-gray-200` is one of the cleanest ways to build lists and tables. Far better than adding `border-b last:border-0` to each row.

---

---

## Chapter 12 — Shadows & Effects

### Box Shadow Scale

```jsx
<div className="shadow-sm">Subtle shadow (1px blur)</div>
<div className="shadow">Default shadow</div>
<div className="shadow-md">Medium shadow</div>
<div className="shadow-lg">Large shadow</div>
<div className="shadow-xl">Extra large shadow</div>
<div className="shadow-2xl">2XL shadow</div>
<div className="shadow-inner">Inset shadow</div>
<div className="shadow-none">No shadow</div>
```

---

### Colored Shadows

```jsx
{/* shadow-{color} colors the shadow — great for colored buttons */}
<button className="bg-blue-600 text-white px-6 py-3 rounded-xl shadow-lg shadow-blue-500/40 hover:shadow-blue-500/60 transition-shadow">
  Blue glow button
</button>

<button className="bg-purple-600 text-white px-6 py-3 rounded-xl shadow-lg shadow-purple-500/40 hover:shadow-xl hover:shadow-purple-500/60 transition-all">
  Purple glow button
</button>

{/* Card with subtle shadow */}
<div className="bg-white rounded-2xl shadow-lg shadow-gray-200/80 border border-gray-100 p-6">
  Card with realistic shadow
</div>
```

---

### Custom Shadows with Arbitrary Values

```jsx
{/* Full control over shadow via arbitrary */}
<div className="shadow-[0_4px_24px_rgba(0,0,0,0.12)]">Soft far shadow</div>
<div className="shadow-[0_1px_3px_rgba(0,0,0,0.08),_0_8px_24px_rgba(0,0,0,0.06)]">
  Layered shadow
</div>
<div className="shadow-[inset_0_2px_4px_rgba(0,0,0,0.06)]">Inner shadow</div>

{/* Multi-layer elevated card shadow */}
<div className="rounded-2xl bg-white p-6
                shadow-[0_1px_2px_rgba(0,0,0,0.04),_0_4px_8px_rgba(0,0,0,0.04),_0_12px_24px_rgba(0,0,0,0.08)]">
  Deeply elevated card
</div>
```

---

### Opacity

```jsx
{/* Opacity scale */}
<div className="opacity-0">Invisible (0%)</div>
<div className="opacity-5">5%</div>
<div className="opacity-10">10%</div>
<div className="opacity-25">25%</div>
<div className="opacity-50">50%</div>
<div className="opacity-75">75%</div>
<div className="opacity-90">90%</div>
<div className="opacity-95">95%</div>
<div className="opacity-100">Fully visible (100%)</div>

{/* Arbitrary opacity */}
<div className="opacity-[0.67]">67%</div>

{/* Hover reveal */}
<div className="opacity-0 hover:opacity-100 transition-opacity duration-200">
  Reveals on hover
</div>

{/* Disabled state */}
<button disabled className="disabled:opacity-40 disabled:cursor-not-allowed">
  Disabled button
</button>
```

---

### Mix Blend Modes

```jsx
{/* mix-blend-* — controls how an element blends with what's behind it */}
<div className="relative">
  <img src="photo.jpg" className="w-full" />
  <div className="absolute inset-0 bg-blue-500 mix-blend-multiply" />
</div>

{/* Common blend modes */}
<div className="mix-blend-multiply">Multiply — darkens</div>
<div className="mix-blend-screen">Screen — lightens</div>
<div className="mix-blend-overlay">Overlay — contrast</div>
<div className="mix-blend-darken">Darken</div>
<div className="mix-blend-lighten">Lighten</div>
<div className="mix-blend-color-dodge">Color dodge</div>
<div className="mix-blend-color-burn">Color burn</div>
<div className="mix-blend-hard-light">Hard light</div>
<div className="mix-blend-soft-light">Soft light</div>
<div className="mix-blend-difference">Difference — inverts</div>
<div className="mix-blend-exclusion">Exclusion</div>
<div className="mix-blend-hue">Hue</div>
<div className="mix-blend-saturation">Saturation</div>
<div className="mix-blend-color">Color</div>
<div className="mix-blend-luminosity">Luminosity</div>
<div className="mix-blend-normal">Normal (default)</div>
```

---

### Glassmorphism Card (Complete Example)

```jsx
function GlassmorphCard() {
  return (
    <div className="min-h-screen flex items-center justify-center p-8
                    bg-gradient-to-br from-violet-500 via-purple-500 to-pink-500">
      {/* Background decorative blobs */}
      <div className="absolute top-20 left-20 w-72 h-72 bg-pink-400 rounded-full mix-blend-multiply filter blur-3xl opacity-30 animate-pulse" />
      <div className="absolute bottom-20 right-20 w-72 h-72 bg-blue-400 rounded-full mix-blend-multiply filter blur-3xl opacity-30 animate-pulse" />

      {/* Glass card */}
      <div className="relative z-10 w-full max-w-md rounded-3xl p-8
                      bg-white/15 backdrop-blur-xl
                      border border-white/25
                      shadow-2xl shadow-black/20">
        <div className="flex items-center gap-4 mb-6">
          <div className="w-14 h-14 rounded-2xl bg-white/20 backdrop-blur-sm flex items-center justify-center">
            <span className="text-2xl">🚀</span>
          </div>
          <div>
            <h2 className="text-xl font-bold text-white">Launch Ready</h2>
            <p className="text-white/70 text-sm">Your app is live</p>
          </div>
        </div>

        <div className="space-y-3 mb-6">
          {[["Status", "Deployed"], ["Region", "us-east-1"], ["Version", "2.4.1"]].map(([k, v]) => (
            <div key={k} className="flex justify-between py-2 border-b border-white/10">
              <span className="text-white/60 text-sm">{k}</span>
              <span className="text-white text-sm font-medium">{v}</span>
            </div>
          ))}
        </div>

        <button className="w-full py-3 bg-white/20 hover:bg-white/30 backdrop-blur-sm
                           text-white font-semibold rounded-2xl
                           border border-white/30
                           transition-all duration-200 hover:scale-[1.02] active:scale-[0.98]">
          View Dashboard
        </button>
      </div>
    </div>
  );
}
```

---

---

## Chapter 13 — Filters & Backdrop Filters

### Element Filters

Applied to the element itself (including its children):

```jsx
{/* blur */}
<img className="blur-none" src="photo.jpg" />
<img className="blur-sm" src="photo.jpg" />   {/* 4px */}
<img className="blur" src="photo.jpg" />      {/* 8px */}
<img className="blur-md" src="photo.jpg" />   {/* 12px */}
<img className="blur-lg" src="photo.jpg" />   {/* 16px */}
<img className="blur-xl" src="photo.jpg" />   {/* 24px */}
<img className="blur-2xl" src="photo.jpg" />  {/* 40px */}
<img className="blur-3xl" src="photo.jpg" />  {/* 64px */}
<img className="blur-[2px]" src="photo.jpg" /> {/* arbitrary */}

{/* brightness */}
<img className="brightness-50" src="photo.jpg" />   {/* Darkened */}
<img className="brightness-75" src="photo.jpg" />
<img className="brightness-100" src="photo.jpg" />  {/* Normal */}
<img className="brightness-125" src="photo.jpg" />  {/* Brighter */}
<img className="brightness-150" src="photo.jpg" />
<img className="brightness-200" src="photo.jpg" />  {/* Very bright */}

{/* contrast */}
<img className="contrast-0" src="photo.jpg" />    {/* Gray */}
<img className="contrast-50" src="photo.jpg" />
<img className="contrast-100" src="photo.jpg" /> {/* Normal */}
<img className="contrast-150" src="photo.jpg" />
<img className="contrast-200" src="photo.jpg" /> {/* High contrast */}

{/* grayscale */}
<img className="grayscale" src="photo.jpg" />     {/* Full grayscale */}
<img className="grayscale-0" src="photo.jpg" />   {/* Color (default) */}

{/* hue-rotate */}
<img className="hue-rotate-0" src="photo.jpg" />
<img className="hue-rotate-15" src="photo.jpg" />
<img className="hue-rotate-30" src="photo.jpg" />
<img className="hue-rotate-60" src="photo.jpg" />
<img className="hue-rotate-90" src="photo.jpg" />
<img className="hue-rotate-180" src="photo.jpg" /> {/* Colors inverted by hue */}
<img className="-hue-rotate-30" src="photo.jpg" /> {/* Negative hue rotate */}

{/* invert */}
<img className="invert" src="photo.jpg" />     {/* Fully inverted */}
<img className="invert-0" src="photo.jpg" />   {/* Normal */}

{/* saturate */}
<img className="saturate-0" src="photo.jpg" />    {/* Grayscale */}
<img className="saturate-50" src="photo.jpg" />   {/* Desaturated */}
<img className="saturate-100" src="photo.jpg" />  {/* Normal */}
<img className="saturate-150" src="photo.jpg" />  {/* Vibrant */}
<img className="saturate-200" src="photo.jpg" />  {/* Very vibrant */}

{/* sepia */}
<img className="sepia" src="photo.jpg" />    {/* Full sepia */}
<img className="sepia-0" src="photo.jpg" />  {/* Normal */}
```

---

### `drop-shadow-*` — Filter-Based Shadow

Unlike `box-shadow`, `drop-shadow` follows the alpha channel of the element. It renders on the actual visible shape, not the bounding box — essential for PNGs with transparent backgrounds:

```jsx
{/* Regular box-shadow — renders as rectangle */}
<img src="logo.png" className="shadow-lg" />
{/* ❌ Shadow appears as rectangle, even for transparent PNG */}

{/* drop-shadow — follows the actual shape */}
<img src="logo.png" className="drop-shadow-lg" />
{/* ✅ Shadow follows the logo's actual shape */}

{/* Scale */}
<img src="icon.png" className="drop-shadow-sm" />
<img src="icon.png" className="drop-shadow" />
<img src="icon.png" className="drop-shadow-md" />
<img src="icon.png" className="drop-shadow-lg" />
<img src="icon.png" className="drop-shadow-xl" />
<img src="icon.png" className="drop-shadow-2xl" />
<img src="icon.png" className="drop-shadow-none" />

{/* Colored drop shadow */}
<img src="logo.png" className="drop-shadow-[0_4px_12px_rgba(99,102,241,0.5)]" />
```

---

### Backdrop Filters

`backdrop-*` filters affect everything *behind* the element (the "backdrop"), not the element itself. This is how glassmorphism works.

```jsx
{/* backdrop-blur — THE most used backdrop filter */}
<div className="backdrop-blur-none">No blur</div>
<div className="backdrop-blur-sm">4px blur</div>
<div className="backdrop-blur">8px blur</div>
<div className="backdrop-blur-md">12px blur</div>
<div className="backdrop-blur-lg">16px blur</div>
<div className="backdrop-blur-xl">24px blur</div>
<div className="backdrop-blur-2xl">40px blur</div>
<div className="backdrop-blur-3xl">64px blur</div>

{/* backdrop-brightness */}
<div className="backdrop-brightness-50">Darkens backdrop</div>
<div className="backdrop-brightness-150">Lightens backdrop</div>

{/* backdrop-contrast, backdrop-grayscale, backdrop-invert, etc. */}
<div className="backdrop-grayscale">Grayscale backdrop</div>
<div className="backdrop-invert">Inverted backdrop</div>
<div className="backdrop-saturate-200">Vibrant backdrop</div>
<div className="backdrop-opacity-50">50% opacity backdrop</div>
<div className="backdrop-sepia">Sepia backdrop</div>
<div className="backdrop-hue-rotate-90">Hue rotated backdrop</div>
```

---

### Real Example: Frosted Glass Navbar

```jsx
function FrostedNavbar() {
  return (
    <header className="sticky top-0 z-40
                       bg-white/70 dark:bg-gray-900/70
                       backdrop-blur-xl backdrop-saturate-150
                       border-b border-gray-200/50 dark:border-gray-700/50">
      <nav className="max-w-7xl mx-auto px-4 sm:px-6 py-4 flex items-center justify-between">
        <a href="/" className="flex items-center gap-2 font-bold text-gray-900 dark:text-white">
          <div className="w-8 h-8 rounded-lg bg-gradient-to-br from-violet-500 to-purple-600" />
          Acme
        </a>

        <div className="hidden md:flex items-center gap-1">
          {["Features", "Pricing", "Blog", "Docs"].map((item) => (
            <a
              key={item}
              href="#"
              className="px-4 py-2 text-sm font-medium text-gray-600 dark:text-gray-400
                         hover:text-gray-900 dark:hover:text-white
                         hover:bg-gray-100/80 dark:hover:bg-gray-800/80
                         rounded-lg transition-colors"
            >
              {item}
            </a>
          ))}
        </div>

        <div className="flex items-center gap-3">
          <a href="/login" className="text-sm font-medium text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white">
            Sign in
          </a>
          <a href="/signup" className="px-4 py-2 bg-gray-900 dark:bg-white text-white dark:text-gray-900 text-sm font-semibold rounded-xl hover:bg-gray-700 dark:hover:bg-gray-100 transition-colors">
            Get started
          </a>
        </div>
      </nav>
    </header>
  );
}
```

---

---

## Chapter 14 — Transitions & Animations

### 14.1 — Transitions

#### Transition Property

```jsx
{/* transition — applies to color, background, border, box-shadow, opacity, transform */}
<button className="bg-blue-600 hover:bg-blue-700 transition">Default transition</button>

{/* transition-all — all properties */}
<div className="transition-all hover:scale-105 hover:shadow-lg">All at once</div>

{/* transition-colors — color, background-color, border-color, text-decoration-color, fill, stroke */}
<button className="bg-blue-600 hover:bg-blue-700 text-white transition-colors">Color only</button>

{/* transition-opacity */}
<div className="opacity-0 hover:opacity-100 transition-opacity">Fade</div>

{/* transition-shadow */}
<div className="shadow hover:shadow-xl transition-shadow">Shadow only</div>

{/* transition-transform */}
<div className="hover:scale-105 transition-transform">Scale only</div>

{/* transition-none — disable all transitions */}
<div className="transition-none">No transition</div>
```

---

#### Duration

```jsx
<div className="transition duration-75">75ms</div>
<div className="transition duration-100">100ms</div>
<div className="transition duration-150">150ms (default)</div>
<div className="transition duration-200">200ms</div>
<div className="transition duration-300">300ms</div>
<div className="transition duration-500">500ms</div>
<div className="transition duration-700">700ms</div>
<div className="transition duration-1000">1000ms</div>
<div className="transition duration-[400ms]">400ms (arbitrary)</div>
```

---

#### Easing

```jsx
<div className="transition ease-linear">Constant speed</div>
<div className="transition ease-in">Starts slow, ends fast</div>
<div className="transition ease-out">Starts fast, ends slow (most natural)</div>
<div className="transition ease-in-out">Slow start and end (default)</div>
<div className="transition ease-[cubic-bezier(0.34,1.56,0.64,1)]">Custom bounce easing</div>
```

---

#### Delay

```jsx
<div className="transition delay-75">75ms delay</div>
<div className="transition delay-100">100ms delay</div>
<div className="transition delay-150">150ms delay</div>
<div className="transition delay-200">200ms delay</div>
<div className="transition delay-300">300ms delay</div>
<div className="transition delay-500">500ms delay</div>
<div className="transition delay-700">700ms delay</div>
<div className="transition delay-1000">1000ms delay</div>
```

---

#### Real Example: Button Hover with Color + Shadow Transition

```jsx
{/* Professional button with smooth all-state transitions */}
<button className="
  relative inline-flex items-center gap-2
  px-6 py-3 rounded-xl
  bg-blue-600 hover:bg-blue-700 active:bg-blue-800
  text-white font-semibold text-sm
  shadow-md shadow-blue-500/25
  hover:shadow-lg hover:shadow-blue-500/40
  hover:-translate-y-0.5
  active:translate-y-0 active:shadow-sm
  focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2
  transition-all duration-150 ease-out
  disabled:opacity-50 disabled:cursor-not-allowed disabled:hover:translate-y-0
">
  <span>Get Started</span>
  <ArrowRight className="w-4 h-4" />
</button>

{/* Card hover lift */}
<div className="
  bg-white rounded-2xl border border-gray-100 p-6
  shadow-sm hover:shadow-xl hover:shadow-gray-200/80
  hover:-translate-y-1
  transition-all duration-200 ease-out
  cursor-pointer
">
  <h3 className="font-bold text-gray-900">Card title</h3>
  <p className="text-gray-500 mt-1 text-sm">Lifts on hover with a smooth shadow transition.</p>
</div>
```

---

### 14.2 — Animations

#### Built-in Animations

```jsx
{/* spin — continuous 360° rotation */}
<svg className="animate-spin h-5 w-5 text-blue-600" viewBox="0 0 24 24">
  <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" fill="none" />
  <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.37 0 0 5.37 0 12h4z" />
</svg>

{/* ping — ripple/pulse that expands and fades — great for notification badges */}
<span className="relative inline-flex">
  <button className="px-4 py-2 bg-blue-600 text-white rounded-lg">Messages</button>
  <span className="absolute -top-1 -right-1 flex h-4 w-4">
    <span className="animate-ping absolute inline-flex h-full w-full rounded-full bg-red-400 opacity-75"></span>
    <span className="relative inline-flex rounded-full h-4 w-4 bg-red-500 text-white text-[10px] items-center justify-center font-bold">3</span>
  </span>
</span>

{/* pulse — gentle opacity pulse — skeleton loading */}
<div className="animate-pulse space-y-4">
  <div className="h-6 bg-gray-200 rounded w-3/4"></div>
  <div className="h-4 bg-gray-200 rounded"></div>
  <div className="h-4 bg-gray-200 rounded w-5/6"></div>
</div>

{/* bounce — vertical bounce */}
<svg className="animate-bounce w-6 h-6 text-blue-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 9l-7 7-7-7" />
</svg>

{/* none — stops animation */}
<div className="animate-none">Static</div>
```

---

#### Custom Animations

```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      keyframes: {
        // Fade in from below
        "fade-up": {
          "0%": { opacity: "0", transform: "translateY(16px)" },
          "100%": { opacity: "1", transform: "translateY(0)" },
        },
        // Shimmer for skeleton
        shimmer: {
          "0%": { backgroundPosition: "-200% 0" },
          "100%": { backgroundPosition: "200% 0" },
        },
        // Float
        float: {
          "0%, 100%": { transform: "translateY(0)" },
          "50%": { transform: "translateY(-12px)" },
        },
        // Slide in from right
        "slide-in": {
          "0%": { opacity: "0", transform: "translateX(24px)" },
          "100%": { opacity: "1", transform: "translateX(0)" },
        },
      },
      animation: {
        "fade-up": "fade-up 0.4s ease-out both",
        shimmer: "shimmer 2s linear infinite",
        float: "float 3s ease-in-out infinite",
        "slide-in": "slide-in 0.3s ease-out both",
      },
    },
  },
};
```

```jsx
{/* Loading spinner with custom keyframe */}
<div className="animate-spin w-8 h-8 rounded-full border-4 border-gray-200 border-t-blue-600" />

{/* Shimmer skeleton */}
<div className="animate-shimmer bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] rounded-lg h-6 w-3/4" />

{/* Floating hero image */}
<img src="hero.png" className="animate-float drop-shadow-2xl" alt="Hero" />

{/* Staggered fade-up list items */}
<ul>
  {features.map((feature, i) => (
    <li
      key={feature.id}
      className="animate-fade-up"
      style={{ animationDelay: `${i * 100}ms` }}
    >
      {feature.title}
    </li>
  ))}
</ul>
```

---

#### `motion-safe:` and `motion-reduce:`

**Always** wrap heavy animations in `motion-safe:` or disable them with `motion-reduce:`. Some users have vestibular disorders and experience nausea from animations:

```jsx
{/* ✅ Best Practice: use motion-safe for non-essential animations */}
<div className="motion-safe:animate-bounce">Only bounces if user allows motion</div>
<div className="motion-safe:hover:-translate-y-1 motion-safe:transition-transform">
  Card lift — disabled for reduced-motion users
</div>

{/* Reduce motion for those who prefer it */}
<div className="animate-spin motion-reduce:animate-none">
  Spins, but stops for reduced-motion preference
</div>

{/* The essential loading spinner — always show it, just don't spin */}
<div className="w-8 h-8 rounded-full border-4 border-gray-200 border-t-blue-600
                animate-spin motion-reduce:animate-none motion-reduce:opacity-50">
</div>
```

---

---

## Chapter 15 — Transforms

### Scale

```jsx
{/* scale-{amount} — 0 to 150 in steps */}
<div className="scale-0">scale(0) — invisible</div>
<div className="scale-50">scale(0.5) — half size</div>
<div className="scale-75">scale(0.75)</div>
<div className="scale-90">scale(0.9)</div>
<div className="scale-95">scale(0.95)</div>
<div className="scale-100">scale(1) — normal</div>
<div className="scale-105">scale(1.05)</div>
<div className="scale-110">scale(1.1)</div>
<div className="scale-125">scale(1.25)</div>
<div className="scale-150">scale(1.5)</div>

{/* Axis-specific */}
<div className="scale-x-75">Compress horizontally</div>
<div className="scale-y-125">Stretch vertically</div>

{/* Negative scale = flip */}
<div className="-scale-x-100">Flip horizontally (mirror)</div>

{/* Hover scale — most common use case */}
<img className="hover:scale-105 transition-transform duration-200 cursor-pointer" src="photo.jpg" />
```

---

### Rotate

```jsx
<div className="rotate-0">No rotation</div>
<div className="rotate-1">1deg</div>
<div className="rotate-2">2deg</div>
<div className="rotate-3">3deg</div>
<div className="rotate-6">6deg</div>
<div className="rotate-12">12deg</div>
<div className="rotate-45">45deg</div>
<div className="rotate-90">90deg</div>
<div className="rotate-180">180deg</div>
<div className="-rotate-6">-6deg</div>
<div className="-rotate-45">-45deg</div>
<div className="rotate-[17deg]">17deg (arbitrary)</div>

{/* Animated dropdown arrow */}
<button
  onClick={() => setOpen(!open)}
  className="flex items-center gap-2"
>
  Dropdown
  <ChevronDown className={`w-4 h-4 transition-transform duration-200 ${open ? "rotate-180" : "rotate-0"}`} />
</button>
```

---

### Translate

```jsx
{/* translate-x/y use the spacing scale */}
<div className="translate-x-4">Move right 16px</div>
<div className="-translate-x-4">Move left 16px</div>
<div className="translate-y-2">Move down 8px</div>
<div className="-translate-y-1">Move up 4px</div>

{/* 50% = center relative to own size */}
<div className="translate-x-1/2">Move right by own half-width</div>
<div className="-translate-x-1/2">Move left by own half-width</div>
<div className="-translate-y-1/2">Move up by own half-height</div>

{/* THE centering trick with absolute position */}
<div className="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2">
  Perfectly centered in parent
</div>

{/* Slide-in animation */}
<div className={`transition-transform duration-300 ${isOpen ? "translate-x-0" : "-translate-x-full"}`}>
  Slide-in drawer
</div>
```

---

### Skew

```jsx
<div className="skew-x-3">Slight horizontal skew</div>
<div className="skew-x-6">6deg horizontal skew</div>
<div className="-skew-x-6">Negative skew</div>
<div className="skew-y-3">Vertical skew</div>

{/* Decorative skewed section divider */}
<div className="relative overflow-hidden">
  <div className="bg-blue-600 py-24 -skew-y-3 -mt-6 -mb-6">
    <div className="skew-y-3 max-w-7xl mx-auto px-4">
      Skewed section with counter-skewed content
    </div>
  </div>
</div>
```

---

### Transform Origin

```jsx
{/* Default is center — transform-origin: center */}
<div className="origin-center rotate-45">Rotates around center (default)</div>
<div className="origin-top rotate-45">Rotates around top edge</div>
<div className="origin-top-right rotate-45">Rotates around top-right corner</div>
<div className="origin-bottom-left rotate-45">Rotates around bottom-left</div>

{/* Useful for dropdown arrows that "pivot" from a point */}
<ChevronDown className="origin-center rotate-0 group-open:rotate-180 transition-transform" />
```

---

### `transform-gpu` — GPU Acceleration

Adds `translateZ(0)` to force the browser to use GPU compositing layer, resulting in smoother animations:

```jsx
{/* Use on heavily animated elements */}
<div className="transform-gpu animate-float hover:scale-105 transition-transform">
  GPU-accelerated animation
</div>

{/* Always use transform-gpu on full-screen overlays/modals */}
<div className="fixed inset-0 transform-gpu transition-opacity duration-300">
  Modal backdrop
</div>
```

---

### Real Example: 3D Card Flip Effect

```jsx
function FlipCard({ front, back }: { front: React.ReactNode; back: React.ReactNode }) {
  const [flipped, setFlipped] = useState(false);

  return (
    {/* perspective wrapper */}
    <div
      className="w-64 h-40 cursor-pointer [perspective:1000px]"
      onClick={() => setFlipped(!flipped)}
    >
      {/* card inner — does the rotation */}
      <div
        className={`relative w-full h-full transition-transform duration-500 ease-in-out [transform-style:preserve-3d] ${
          flipped ? "[transform:rotateY(180deg)]" : ""
        }`}
      >
        {/* Front face */}
        <div className="absolute inset-0 bg-blue-600 text-white rounded-2xl flex items-center justify-center [backface-visibility:hidden]">
          {front}
        </div>

        {/* Back face — pre-rotated 180deg */}
        <div className="absolute inset-0 bg-purple-600 text-white rounded-2xl flex items-center justify-center [backface-visibility:hidden] [transform:rotateY(180deg)]">
          {back}
        </div>
      </div>
    </div>
  );
}
```

---

---

## Chapter 16 — Interactivity

### Cursor

```jsx
<div className="cursor-auto">Auto (default)</div>
<div className="cursor-default">Default arrow</div>
<div className="cursor-pointer">Pointer (hand)</div>
<div className="cursor-wait">Spinner/wait</div>
<div className="cursor-text">Text I-beam</div>
<div className="cursor-move">Move crosshair</div>
<div className="cursor-help">Help (question mark)</div>
<div className="cursor-not-allowed">No/disabled circle</div>
<div className="cursor-none">No cursor (custom cursor UIs)</div>
<div className="cursor-crosshair">Crosshair</div>
<div className="cursor-grab">Grab (open hand)</div>
<div className="cursor-grabbing">Grabbing (closed hand)</div>
<div className="cursor-zoom-in">Zoom in</div>
<div className="cursor-zoom-out">Zoom out</div>
<div className="cursor-col-resize">Column resize</div>
<div className="cursor-row-resize">Row resize</div>
```

---

### Pointer Events

```jsx
{/* pointer-events-none — element doesn't receive mouse events (clicks pass through) */}
<div className="relative">
  <input className="w-full px-3 py-2 border rounded-lg" />
  {/* Decorative icon that won't block input clicks */}
  <SearchIcon className="absolute right-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400 pointer-events-none" />
</div>

{/* Use on overlays that should be "invisible" to mouse */}
<div className="absolute inset-0 bg-gradient-to-b from-black/30 to-transparent pointer-events-none rounded-xl" />

{/* pointer-events-auto — re-enable (use to undo pointer-events-none from parent) */}
<a href="#" className="pointer-events-auto">Clickable inside pointer-events-none parent</a>
```

---

### User Select

```jsx
<p className="select-none">Can't be selected (good for UI labels, buttons)</p>
<p className="select-text">Normal text selection</p>
<p className="select-all">Click to select all text at once</p>
<p className="select-auto">Browser default behavior</p>

{/* Common: prevent accidental text selection on interactive UI */}
<button className="select-none">Button label</button>
<div className="select-none" role="button">Custom interactive element</div>
```

---

### Scroll Behavior & Snap

```jsx
{/* scroll-smooth — smooth scrolling for anchor links */}
<html className="scroll-smooth">

{/* scroll-mt-* — scroll margin top — accounts for sticky headers on anchor jumps */}
<section id="features" className="scroll-mt-20">
  {/* When linked to via #features, page scrolls 80px (5rem) above this section */}
</section>

{/* scroll-snap — CSS scroll snap for carousels */}
<div className="flex overflow-x-auto snap-x snap-mandatory gap-4 p-4">
  {slides.map((slide) => (
    <div key={slide.id} className="snap-start shrink-0 w-80 h-48 rounded-xl bg-gray-100 flex items-center justify-center">
      {slide.content}
    </div>
  ))}
</div>

{/* snap-center — snaps to center of container */}
<div className="flex overflow-x-auto snap-x snap-mandatory">
  {slides.map((slide) => (
    <div key={slide.id} className="snap-center shrink-0 w-full h-screen">
      {slide.content}
    </div>
  ))}
</div>
```

---

### Appearance & Form Resets

```jsx
{/* appearance-none — removes native OS/browser styling for form elements */}
<select className="appearance-none w-full px-3 py-2 border border-gray-300 rounded-lg pr-10 bg-white focus:outline-none focus:ring-2 focus:ring-blue-500">
  <option>Option 1</option>
</select>
{/* ✅ Now style it completely yourself */}

{/* Custom checkbox using appearance-none */}
<input
  type="checkbox"
  className="appearance-none w-5 h-5 border-2 border-gray-300 rounded
             checked:bg-blue-600 checked:border-blue-600
             focus:ring-2 focus:ring-blue-500 focus:ring-offset-1
             transition-colors cursor-pointer"
/>
```

---

### Accent Color & Caret Color

```jsx
{/* accent-* — colors checkbox, radio, range, progress natively */}
<input type="checkbox" className="accent-blue-600" />
<input type="radio" className="accent-purple-600" />
<input type="range" className="accent-green-500" />
<progress className="accent-blue-600" value="60" max="100" />

{/* caret-* — cursor caret color in inputs */}
<input
  type="text"
  className="caret-blue-600 border border-gray-300 rounded-lg px-3 py-2 focus:outline-none focus:ring-2 focus:ring-blue-500"
  placeholder="Blue caret"
/>
```

---

### Resize

```jsx
<textarea className="resize-none">Can't resize</textarea>
<textarea className="resize">Both directions</textarea>
<textarea className="resize-x">Horizontal only</textarea>
<textarea className="resize-y">Vertical only (most common)</textarea>
```

---

### Will Change

```jsx
{/* Performance hint to browser — creates a new compositing layer */}
<div className="will-change-transform hover:scale-105 transition-transform">
  GPU-optimized scale animation
</div>
<div className="will-change-opacity transition-opacity hover:opacity-50">
  GPU-optimized opacity
</div>
<div className="will-change-auto">Removes the hint (default)</div>
```

**⚠️ Gotcha:** Don't use `will-change-transform` on everything — it consumes GPU memory. Only use it on elements that are about to animate.

---

### Touch Action

```jsx
{/* touch-none — disable all touch events (custom gesture handling) */}
<div className="touch-none" onPointerDown={handleDrag}>Draggable area</div>

{/* touch-pan-y — allow vertical scroll but capture horizontal for swipe */}
<div className="touch-pan-y overflow-x-auto">Horizontal swipeable</div>

{/* touch-manipulation — disable double-tap zoom (reduces 300ms click delay) */}
<button className="touch-manipulation">Fast mobile tap</button>
```

---

---

## Chapter 17 — Tailwind v4 — New Features

This is a critical chapter. Tailwind v4 represents a fundamental architectural shift. If you're starting a new project in 2025+, you should use v4. If you're on v3, this chapter tells you what's coming.

### CSS-First Configuration — No `tailwind.config.js`

In v3, all configuration lived in a JS file. In v4, your CSS **is** your configuration. The `@theme` directive replaces `tailwind.config.js` entirely.

```css
/* v4: index.css — this IS your config */
@import "tailwindcss";

@theme {
  /* Colors */
  --color-brand: oklch(60% 0.2 250);
  --color-brand-light: oklch(85% 0.12 250);
  --color-brand-dark: oklch(40% 0.2 250);

  /* Typography */
  --font-sans: "Inter", ui-sans-serif, system-ui, sans-serif;
  --font-display: "Cal Sans", ui-sans-serif, sans-serif;
  --font-mono: "JetBrains Mono", ui-monospace, monospace;

  /* Spacing */
  --spacing-section: 5rem;
  --spacing-container: 1280px;

  /* Border radius */
  --radius-brand: 0.75rem;

  /* Shadows */
  --shadow-card: 0 1px 3px oklch(0% 0 0 / 0.08), 0 8px 24px oklch(0% 0 0 / 0.06);
}
```

These tokens become usable as utility classes automatically:
- `--color-brand` → `bg-brand`, `text-brand`, `border-brand`
- `--font-display` → `font-display`
- `--spacing-section` → `p-section`, `mt-section`, etc.

---

### `@import "tailwindcss"` — Single Import

```css
/* v3 — three directives required */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* v4 — single import, includes everything */
@import "tailwindcss";
```

You can also selectively import layers in v4:

```css
@import "tailwindcss/preflight";   /* just the base reset */
@import "tailwindcss/utilities";   /* just utilities */
```

---

### `@theme` — Defining Design Tokens

The `@theme` directive is the single place for all design tokens. Variable naming follows a strict convention: `--{category}-{name}`.

```css
@theme {
  /* ── Colors ── */
  --color-primary: oklch(55% 0.22 260);
  --color-primary-50: oklch(97% 0.02 260);
  --color-primary-500: oklch(55% 0.22 260);
  --color-primary-900: oklch(25% 0.12 260);

  /* ── Spacing ── */
  --spacing-*: initial; /* reset default spacing scale */
  --spacing-1: 4px;
  --spacing-2: 8px;
  --spacing-4: 16px;
  --spacing-8: 32px;
  --spacing-16: 64px;

  /* ── Fonts ── */
  --font-sans: "Inter Variable", system-ui, sans-serif;
  --font-mono: "Fira Code", monospace;

  /* ── Breakpoints ── */
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
  --breakpoint-2xl: 1536px;
  --breakpoint-3xl: 1920px; /* custom! */

  /* ── Animations ── */
  --animate-fade-up: fade-up 0.4s ease-out both;

  /* ── Border radius ── */
  --radius-card: 1rem;
  --radius-button: 0.5rem;
}

@keyframes fade-up {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

---

### `@utility` — Custom Utilities (No Plugin Needed)

In v4, define custom utilities directly in CSS. No JavaScript plugin API required.

```css
/* Scrollbar hide */
@utility scrollbar-hide {
  scrollbar-width: none;
  &::-webkit-scrollbar {
    display: none;
  }
}

/* Text gradient */
@utility text-gradient {
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
}

/* Safe-area padding for mobile notches */
@utility pb-safe {
  padding-bottom: env(safe-area-inset-bottom);
}

/* Full bleed — break out of container */
@utility full-bleed {
  width: 100vw;
  margin-left: calc(50% - 50vw);
}

/* Truncate at N lines — parametric */
@utility line-clamp-* {
  overflow: hidden;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: --value;
}
```

Usage:

```jsx
<div className="scrollbar-hide overflow-auto h-64">Scrolls without scrollbar</div>
<h1 className="text-gradient bg-gradient-to-r from-violet-600 to-pink-600 text-5xl font-black">
  Gradient Heading
</h1>
```

---

### `@variant` — Custom Variants

```css
/* Dark mode using class strategy */
@variant dark (&:where(.dark *));

/* Hover + focus combined (hocus) */
@variant hocus (&:hover, &:focus);

/* Specific parent state */
@variant sidebar-open (.sidebar-open &);

/* Custom data attribute variant */
@variant loading ([data-loading] &);

/* Touch device variant */
@variant touch (@media (hover: none) and (pointer: coarse));
```

```jsx
{/* hocus variant — applies on both hover and focus */}
<a href="#" className="hocus:text-blue-600 hocus:underline">Link</a>

{/* loading variant — applies when ancestor has data-loading */}
<div data-loading>
  <button className="loading:opacity-50 loading:cursor-wait">Submit</button>
</div>
```

---

### OKLCH Color System

OKLCH stands for **OK (perceptual) Lightness, Chroma, Hue**. It's the future of web color.

**Why it's better than hex:**

```
sRGB hex:  #3b82f6  →  rgb(59, 130, 246)  — hard to reason about, sRGB only
OKLCH:     oklch(60% 0.2 260)  — L=lightness(%), C=chroma, H=hue(deg)
```

- **Perceptually uniform:** `oklch(60% 0.2 260)` and `oklch(60% 0.2 30)` have the *same perceived brightness* — sRGB doesn't guarantee this
- **Wide gamut:** Can express P3 colors that sRGB can't
- **Predictable manipulation:** Increase L by 10% and the color gets visually ~10% lighter every time

```css
@theme {
  /* Easy to create a consistent scale — just change Lightness */
  --color-blue-50:  oklch(97% 0.02 260);
  --color-blue-100: oklch(93% 0.04 260);
  --color-blue-200: oklch(87% 0.07 260);
  --color-blue-300: oklch(78% 0.12 260);
  --color-blue-400: oklch(68% 0.17 260);
  --color-blue-500: oklch(60% 0.20 260); /* base */
  --color-blue-600: oklch(52% 0.20 260);
  --color-blue-700: oklch(44% 0.18 260);
  --color-blue-800: oklch(36% 0.14 260);
  --color-blue-900: oklch(27% 0.10 260);
  --color-blue-950: oklch(20% 0.07 260);
}
```

---

### `@source` — Replace the `content` Array

In v4, `@source` replaces the `content` configuration in `tailwind.config.js`:

```css
/* index.css */
@import "tailwindcss";

/* Explicitly scan these paths */
@source "./src/**/*.{js,ts,jsx,tsx}";
@source "./components/**/*.{js,ts,jsx,tsx}";
@source "../../packages/ui/src/**/*.{js,ts,jsx,tsx}"; /* monorepo packages */
```

---

### Dynamic Utility Values

In v4, the engine is smarter. Values like `mt-17`, `text-11xl`, `p-13` work without any config — the engine generates them dynamically:

```jsx
{/* v3 — these don't exist without config */}
<div className="mt-17">❌ Nothing generated in v3</div>

{/* v4 — any number works */}
<div className="mt-17">✅ margin-top: calc(17 * 0.25rem)</div>
<div className="text-11xl">✅ font-size: 11rem</div>
<div className="w-13">✅ width: 3.25rem</div>
```

---

### New `not-*` Modifier

```jsx
{/* not-hover, not-focus, not-disabled, etc. */}
<li className="not-last:border-b border-gray-100 py-3">
  Has border except on last item
</li>

<button className="not-disabled:hover:bg-blue-700 bg-blue-600 text-white">
  Hover darkens unless disabled
</button>
```

---

### New `in-*` Modifier (Ancestor Targeting)

Target elements based on an ancestor's state without `group`:

```jsx
{/* Respond to ancestor state without adding "group" class */}
<div data-theme="dark">
  <p className="in-[data-theme=dark]:text-white text-gray-900">
    White in dark theme, gray otherwise
  </p>
</div>

{/* Respond to form state */}
<form data-submitting="true">
  <button className="in-[data-submitting]:opacity-50 in-[data-submitting]:cursor-wait">
    Submit
  </button>
</form>
```

---

### New `starting:` Variant — Enter Animations

CSS `@starting-style` enables enter animations without JavaScript. The `starting:` variant targets these styles:

```jsx
{/* Element animates in when it first appears */}
<dialog className="
  open:opacity-100 open:translate-y-0
  starting:open:opacity-0 starting:open:translate-y-4
  transition-all duration-200
">
  Dialog content
</dialog>

{/* Popover enter animation */}
<div
  popover
  className="
    opacity-100 scale-100
    starting:opacity-0 starting:scale-95
    transition-all duration-150 ease-out
  "
>
  Popover
</div>
```

---

### Migration Guide: v3 → v4

**Breaking changes and what to do:**

| v3 | v4 | Notes |
|---|---|---|
| `tailwind.config.js` | `@theme {}` in CSS | Run official migration tool |
| `@tailwind base/components/utilities` | `@import "tailwindcss"` | Single import |
| `content: [...]` | `@source "..."` | In CSS file |
| `darkMode: "class"` | `@variant dark` | In CSS file |
| `theme.extend.colors` | `--color-*` in `@theme` | CSS variables |
| `theme.extend.fontFamily` | `--font-*` in `@theme` | CSS variables |
| `plugins: [addUtilities]` | `@utility` in CSS | Pure CSS |
| `postcss.config.js` + `tailwindcss` plugin | `@tailwindcss/vite` | Vite plugin |
| `bg-opacity-50` (old) | `bg-blue-500/50` | Opacity modifier |
| `text-opacity-75` (old) | `text-blue-500/75` | Opacity modifier |
| `flex-shrink-0` | `shrink-0` | Renamed |
| `flex-grow` | `grow` | Renamed |
| `overflow-ellipsis` | `text-ellipsis` | Renamed |
| `decoration-slice` | `box-decoration-slice` | Renamed |

**Run the official migration tool:**

```bash
npx @tailwindcss/upgrade@next
```

This handles most renames automatically.

---

---

## Chapter 18 — Customization

### 18.1 — Tailwind Config (v3)

#### `theme` vs `theme.extend` — Critical Difference

```ts
export default {
  theme: {
    // ⚠️ REPLACES the entire default scale — use with caution
    colors: {
      brand: "#6366f1",
      // Now ONLY "brand" exists — gray, blue, red, etc. are all gone
    },

    extend: {
      // ✅ ADDS to defaults — existing colors remain
      colors: {
        brand: {
          50:  "#eef2ff",
          500: "#6366f1",
          900: "#312e81",
        },
      },
    },
  },
};
```

**✅ Best Practice:** Almost always use `theme.extend` unless you explicitly want to replace the entire default scale.

---

#### Custom Colors

```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        // Full scale (recommended for consistent dark mode support)
        brand: {
          50:  "#f0f9ff",
          100: "#e0f2fe",
          200: "#bae6fd",
          300: "#7dd3fc",
          400: "#38bdf8",
          500: "#0ea5e9",
          600: "#0284c7",
          700: "#0369a1",
          800: "#075985",
          900: "#0c4a6e",
          950: "#082f49",
        },
        // Semantic colors
        success: "#22c55e",
        warning: "#f59e0b",
        error:   "#ef4444",
        info:    "#3b82f6",
      },
    },
  },
};
```

---

#### Custom Spacing

```ts
export default {
  theme: {
    extend: {
      spacing: {
        "13": "3.25rem",    // fills gap in default scale
        "15": "3.75rem",
        "18": "4.5rem",
        "88": "22rem",
        "128": "32rem",
        "section": "6rem",  // semantic name
        "gutter": "1.5rem",
      },
    },
  },
};
```

---

#### Custom Breakpoints

```ts
export default {
  theme: {
    extend: {
      screens: {
        "xs": "475px",   // extra small phones
        "3xl": "1920px", // ultra-wide monitors
        "4xl": "2560px", // 4K monitors
        "tall": { "raw": "(min-height: 800px)" }, // height-based breakpoint!
      },
    },
  },
};
```

```jsx
<div className="hidden tall:block">Only on tall screens</div>
```

---

#### Custom Fonts & Animations in Config

```ts
import { fontFamily } from "tailwindcss/defaultTheme";

export default {
  theme: {
    extend: {
      fontFamily: {
        sans: ["Inter Variable", ...fontFamily.sans],
        display: ["Bricolage Grotesque", ...fontFamily.sans],
        mono: ["JetBrains Mono", ...fontFamily.mono],
      },
      keyframes: {
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--radix-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: "0" },
        },
        wiggle: {
          "0%, 100%": { transform: "rotate(-3deg)" },
          "50%": { transform: "rotate(3deg)" },
        },
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
        wiggle: "wiggle 1s ease-in-out infinite",
      },
    },
  },
};
```

---

### 18.2 — v4 `@theme`

The v3 config options map directly to `@theme` variables:

| v3 config key | v4 `@theme` variable prefix |
|---|---|
| `colors.*` | `--color-*` |
| `fontFamily.*` | `--font-*` |
| `fontSize.*` | `--text-*` |
| `fontWeight.*` | `--font-weight-*` |
| `spacing.*` | `--spacing-*` |
| `borderRadius.*` | `--radius-*` |
| `screens.*` | `--breakpoint-*` |
| `boxShadow.*` | `--shadow-*` |
| `keyframes.*` | (direct `@keyframes` in CSS) |
| `animation.*` | `--animate-*` |

```css
@theme {
  /* Equivalent to theme.extend.colors.brand */
  --color-brand-50: #f0f9ff;
  --color-brand-500: #0ea5e9;
  --color-brand-900: #0c4a6e;

  /* Equivalent to theme.extend.fontFamily.display */
  --font-display: "Bricolage Grotesque", ui-sans-serif, sans-serif;

  /* Equivalent to theme.extend.borderRadius.card */
  --radius-card: 1rem;

  /* Equivalent to theme.extend.animation.fade-up */
  --animate-fade-up: fade-up 0.4s ease-out both;
}

@keyframes fade-up {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

---

### 18.3 — Plugins

#### Official Plugins

```bash
npm install -D @tailwindcss/typography @tailwindcss/forms @tailwindcss/container-queries
```

**`@tailwindcss/typography`** — The `prose` class applies beautiful typographic styles to server-rendered HTML (blog posts, markdown, CMS content):

```ts
// tailwind.config.ts
import typography from "@tailwindcss/typography";
export default {
  plugins: [typography],
};
```

```jsx
{/* Renders beautiful markdown/HTML content */}
<article
  className="prose prose-lg prose-gray max-w-none
             prose-headings:font-bold prose-headings:text-gray-900
             prose-a:text-blue-600 prose-a:no-underline hover:prose-a:underline
             prose-code:bg-gray-100 prose-code:rounded prose-code:text-sm prose-code:px-1
             prose-img:rounded-xl prose-img:shadow-md
             dark:prose-invert"
  dangerouslySetInnerHTML={{ __html: post.content }}
/>

{/* not-prose — opt a child out of prose styles */}
<article className="prose">
  <p>Normal prose paragraph.</p>
  <div className="not-prose">
    <MyCustomComponent /> {/* Styled by your own classes, not prose */}
  </div>
</article>
```

**`@tailwindcss/forms`** — Resets form element styles to a sensible baseline that's easy to style with Tailwind:

```ts
import forms from "@tailwindcss/forms";
export default {
  plugins: [forms],
};
```

**`@tailwindcss/container-queries`** — Adds container query support with `@` prefix:

```ts
import containerQueries from "@tailwindcss/container-queries";
export default {
  plugins: [containerQueries],
};
```

```jsx
<div className="@container">
  <div className="@sm:grid-cols-2 @lg:grid-cols-3 grid grid-cols-1 gap-4">
    {/* Responds to container width, not viewport! */}
  </div>
</div>
```

---

#### Writing a Custom Plugin (v3)

```ts
// tailwind.config.ts
import plugin from "tailwindcss/plugin";

export default {
  plugins: [
    plugin(function ({ addUtilities, addComponents, addBase, theme, e }) {
      // Add base styles (like @tailwind base)
      addBase({
        "html": { scrollBehavior: "smooth" },
        "body": { fontFamily: theme("fontFamily.sans") },
      });

      // Add custom utilities
      addUtilities({
        ".scrollbar-hide": {
          scrollbarWidth: "none",
          "&::-webkit-scrollbar": { display: "none" },
        },
        ".text-gradient": {
          backgroundClip: "text",
          WebkitBackgroundClip: "text",
          color: "transparent",
        },
      });

      // Add component classes (appear before utilities in cascade)
      addComponents({
        ".btn": {
          display: "inline-flex",
          alignItems: "center",
          justifyContent: "center",
          padding: `${theme("spacing.2")} ${theme("spacing.4")}`,
          borderRadius: theme("borderRadius.lg"),
          fontWeight: theme("fontWeight.semibold"),
          fontSize: theme("fontSize.sm"),
          transition: "all 150ms ease",
        },
        ".btn-primary": {
          backgroundColor: theme("colors.blue.600"),
          color: "white",
          "&:hover": { backgroundColor: theme("colors.blue.700") },
        },
      });
    }),
  ],
};
```

---

---

## Chapter 19 — Tailwind with React (Practical Patterns)

### 19.1 — Class Management

#### The `clsx` / `cx` Pattern

```bash
npm install clsx
```

```tsx
import clsx from "clsx";
// or: import { clsx } from "clsx";

function Button({ variant = "primary", disabled, className, children }) {
  return (
    <button
      disabled={disabled}
      className={clsx(
        // Base classes — always applied
        "inline-flex items-center justify-center px-4 py-2 rounded-lg font-semibold text-sm transition-colors",
        // Variant classes — conditionally applied
        {
          "bg-blue-600 text-white hover:bg-blue-700": variant === "primary",
          "bg-gray-100 text-gray-900 hover:bg-gray-200": variant === "secondary",
          "border border-gray-300 text-gray-700 hover:bg-gray-50": variant === "outline",
          "text-red-600 hover:bg-red-50": variant === "danger",
        },
        // Disabled state
        disabled && "opacity-50 cursor-not-allowed pointer-events-none",
        // Allow caller to inject additional classes
        className,
      )}
    >
      {children}
    </button>
  );
}
```

---

#### `tailwind-merge` — Resolving Class Conflicts

When you merge Tailwind classes from props and defaults, conflicts arise. `tailwind-merge` resolves them intelligently:

```bash
npm install tailwind-merge
```

```tsx
import { twMerge } from "tailwind-merge";

// Without twMerge — BROKEN: both p-4 and p-8 are in the string, browser picks last
<Button className="p-8" />
// className="... p-4 ... p-8" — conflict!

// With twMerge — CORRECT: p-8 wins (later one wins, duplicates removed)
function Button({ className, children }) {
  return (
    <button className={twMerge("px-4 py-2 bg-blue-600 text-white rounded-lg", className)}>
      {children}
    </button>
  );
}

<Button className="px-8 bg-red-500">Override padding and color</Button>
// Result: "py-2 text-white rounded-lg px-8 bg-red-500" — px and bg correctly overridden
```

---

#### The `cva` (Class Variance Authority) Pattern

`cva` is the gold standard for building variant-based Tailwind components. It's type-safe, composable, and works perfectly with `twMerge`:

```bash
npm install class-variance-authority tailwind-merge clsx
```

```tsx
// lib/utils.ts — create a combined utility
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

```tsx
// components/Button.tsx
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/utils";

const buttonVariants = cva(
  // Base classes (always applied)
  "inline-flex items-center justify-center gap-2 whitespace-nowrap rounded-lg text-sm font-semibold transition-all duration-150 focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-offset-2 disabled:pointer-events-none disabled:opacity-50 active:scale-[0.98]",
  {
    variants: {
      variant: {
        primary:   "bg-blue-600 text-white hover:bg-blue-700 focus-visible:ring-blue-500 shadow-sm",
        secondary: "bg-gray-100 text-gray-900 hover:bg-gray-200 focus-visible:ring-gray-500",
        outline:   "border border-gray-300 bg-white text-gray-700 hover:bg-gray-50 focus-visible:ring-gray-500",
        ghost:     "text-gray-700 hover:bg-gray-100 hover:text-gray-900 focus-visible:ring-gray-500",
        danger:    "bg-red-600 text-white hover:bg-red-700 focus-visible:ring-red-500 shadow-sm",
        success:   "bg-green-600 text-white hover:bg-green-700 focus-visible:ring-green-500 shadow-sm",
      },
      size: {
        sm:   "h-8 px-3 text-xs",
        md:   "h-10 px-4 text-sm",
        lg:   "h-12 px-6 text-base",
        icon: "h-10 w-10",
      },
    },
    defaultVariants: {
      variant: "primary",
      size: "md",
    },
  }
);

export interface ButtonProps
  extends React.ButtonHTMLAttributes<HTMLButtonElement>,
    VariantProps<typeof buttonVariants> {
  asChild?: boolean;
}

export function Button({ className, variant, size, ...props }: ButtonProps) {
  return (
    <button
      className={cn(buttonVariants({ variant, size }), className)}
      {...props}
    />
  );
}

// Export for use with Link components etc.
export { buttonVariants };
```

```tsx
{/* Usage */}
<Button>Default (primary, md)</Button>
<Button variant="secondary" size="sm">Small secondary</Button>
<Button variant="danger" size="lg">Large danger</Button>
<Button variant="outline" className="mt-4">With extra class</Button>
<Button variant="icon" size="icon"><PlusIcon className="w-4 h-4" /></Button>
```

---

### 19.2 — Component Patterns

#### Reusable `Card` Component

```tsx
// components/Card.tsx
import { cn } from "@/lib/utils";

interface CardProps extends React.HTMLAttributes<HTMLDivElement> {
  hover?: boolean;
}

export function Card({ className, hover = false, ...props }: CardProps) {
  return (
    <div
      className={cn(
        "bg-white rounded-2xl border border-gray-100 shadow-sm",
        hover && "hover:shadow-lg hover:-translate-y-0.5 transition-all duration-200 cursor-pointer",
        className
      )}
      {...props}
    />
  );
}

export function CardHeader({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) {
  return <div className={cn("px-6 pt-6 pb-4", className)} {...props} />;
}

export function CardContent({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) {
  return <div className={cn("px-6 pb-6", className)} {...props} />;
}

export function CardFooter({ className, ...props }: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div className={cn("px-6 py-4 border-t border-gray-100 flex items-center", className)} {...props} />
  );
}
```

---

#### `Badge` Component with Multiple Variants

```tsx
import { cva, type VariantProps } from "class-variance-authority";
import { cn } from "@/lib/utils";

const badgeVariants = cva(
  "inline-flex items-center gap-1 rounded-full px-2.5 py-0.5 text-xs font-semibold",
  {
    variants: {
      variant: {
        default:  "bg-gray-100 text-gray-800",
        primary:  "bg-blue-100 text-blue-800",
        success:  "bg-green-100 text-green-800",
        warning:  "bg-amber-100 text-amber-800",
        danger:   "bg-red-100 text-red-800",
        outline:  "border border-current text-gray-600",
      },
    },
    defaultVariants: { variant: "default" },
  }
);

interface BadgeProps extends React.HTMLAttributes<HTMLSpanElement>, VariantProps<typeof badgeVariants> {}

export function Badge({ className, variant, ...props }: BadgeProps) {
  return <span className={cn(badgeVariants({ variant }), className)} {...props} />;
}
```

---

#### `Input` with Error/Success States Using `peer`

```tsx
import { cn } from "@/lib/utils";
import { forwardRef } from "react";

interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label: string;
  error?: string;
  hint?: string;
}

export const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ label, error, hint, className, id, ...props }, ref) => {
    const inputId = id ?? label.toLowerCase().replace(/\s+/g, "-");

    return (
      <div className="flex flex-col gap-1.5">
        <label htmlFor={inputId} className="text-sm font-medium text-gray-700">
          {label}
          {props.required && <span className="text-red-500 ml-1">*</span>}
        </label>

        <input
          ref={ref}
          id={inputId}
          className={cn(
            "peer w-full px-3 py-2 rounded-lg border text-sm text-gray-900",
            "placeholder:text-gray-400",
            "transition-colors duration-150",
            "focus:outline-none focus:ring-2 focus:ring-offset-0",
            // Normal state
            !error && "border-gray-300 focus:border-blue-500 focus:ring-blue-500/20",
            // Error state
            error && "border-red-400 focus:border-red-500 focus:ring-red-500/20 bg-red-50/30",
            // Disabled
            "disabled:opacity-50 disabled:cursor-not-allowed disabled:bg-gray-50",
            className,
          )}
          {...props}
        />

        {(error || hint) && (
          <p className={cn("text-xs", error ? "text-red-500" : "text-gray-500")}>
            {error ?? hint}
          </p>
        )}
      </div>
    );
  }
);
Input.displayName = "Input";
```

---

### 19.3 — Using `@apply` (and When NOT To)

```css
/* ✅ When @apply is acceptable */

/* 1. Styling server-rendered HTML you can't add classes to (CMS, markdown) */
.prose h1 { @apply text-3xl font-bold text-gray-900 mt-8 mb-4; }
.prose p  { @apply text-gray-700 leading-relaxed; }

/* 2. Third-party component overrides */
.react-select__control {
  @apply border border-gray-300 rounded-lg shadow-none;
}

/* 3. Base layer resets */
@layer base {
  * { @apply border-border; }
  body { @apply bg-background text-foreground; }
}
```

```tsx
{/* ❌ When @apply is WRONG in React */}

{/* Bad: creating semantic class names defeats Tailwind's purpose */}
{/* In CSS: */}
// .card-header { @apply bg-white rounded-t-xl p-6 border-b; }
{/* In JSX: */}
<div className="card-header">...

{/* ✅ Correct: extract a React component instead */}
function CardHeader({ children }) {
  return <div className="bg-white rounded-t-xl p-6 border-b">{children}</div>;
}
```

**Why `@apply` in React components is wrong:**
1. You lose the colocation of styles and markup — one of Tailwind's key benefits
2. Dead CSS can't be detected (the class is always "used")
3. You can't use responsive/state modifiers in `@apply` easily
4. `twMerge` and `clsx` don't work with it
5. You're back to naming problems

---

### 19.4 — Avoiding Class String Explosion

When a component accumulates 50+ utility classes, use these patterns:

```tsx
{/* ✅ Pattern 1: Extract to a component */}
function PrimaryButton({ children, ...props }) {
  return (
    <button
      className="inline-flex items-center justify-center px-6 py-3 bg-blue-600 hover:bg-blue-700 active:bg-blue-800 text-white font-semibold text-sm rounded-xl shadow-sm shadow-blue-500/25 hover:shadow-blue-500/40 hover:-translate-y-px active:translate-y-0 transition-all duration-150 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed"
      {...props}
    >
      {children}
    </button>
  );
}

{/* ✅ Pattern 2: Use cva for variant logic (covered in 19.1) */}

{/* ✅ Pattern 3: Map pattern for dynamic class lists */}
const STATUS_CLASSES = {
  active:   "bg-green-100 text-green-800 border-green-200",
  inactive: "bg-gray-100 text-gray-600 border-gray-200",
  pending:  "bg-amber-100 text-amber-800 border-amber-200",
  error:    "bg-red-100 text-red-800 border-red-200",
} as const;

function StatusBadge({ status }: { status: keyof typeof STATUS_CLASSES }) {
  return (
    <span className={cn(
      "inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-semibold border",
      STATUS_CLASSES[status]  // ← full class string, never interpolated
    )}>
      {status}
    </span>
  );
}
```

---

---

## Chapter 20 — Performance & Production

### How PurgeCSS / Content Scanning Works (v3)

Tailwind v3 uses a custom scanner (not PurgeCSS) built into the JIT engine. It:
1. Reads all files matching your `content` glob patterns
2. Uses a fast regex that looks for class-shaped strings
3. Generates **only** those classes in the output CSS
4. Does NOT execute JavaScript — it's static string analysis

The scanner is intentionally simple. It looks for any string that matches a potential Tailwind class, regardless of whether it's in a JSX expression, a comment, or a string literal.

---

### How `@source` Works (v4)

Same concept, different configuration. In v4, `@source` directives in your CSS file tell the scanner what to scan:

```css
@import "tailwindcss";
@source "./src/**/*.{js,ts,jsx,tsx}";
```

v4's scanner is faster (integrated with Lightning CSS) and more accurate.

---

### Why You Must NEVER Dynamically Construct Class Names

```tsx
// ❌ BROKEN — the scanner never sees "text-red-500" as a complete string
const color = "red";
<p className={`text-${color}-500`}>Hello</p>

// ❌ BROKEN — same problem
const size = "lg";
<div className={`p-${size}`}>Content</div>

// ❌ BROKEN — even with a lookup, if you compute the key dynamically
const prefix = "bg-";
const suffix = "-500";
const cls = prefix + color + suffix; // "bg-red-500" — scanner can't see it
```

**The scanner must find the exact complete class string in your source code:**

```tsx
// ✅ CORRECT — use a lookup object with complete class strings
const colorClasses = {
  red:   "text-red-500",
  blue:  "text-blue-500",
  green: "text-green-500",
} as const;

<p className={colorClasses[color]}>Hello</p>

// ✅ CORRECT — ternary with complete strings
<div className={isLarge ? "p-8" : "p-4"}>Content</div>

// ✅ CORRECT — cva with full strings defined up-front
const variants = cva("", {
  variants: {
    color: {
      red:   "text-red-500",
      blue:  "text-blue-500",
      green: "text-green-500",
    },
  },
});
```

---

### Safelist (v3) — When and How to Use It

The safelist forces Tailwind to include specific classes even if they don't appear in your content files:

```ts
// tailwind.config.ts
export default {
  safelist: [
    // Exact classes
    "bg-red-500",
    "text-center",
    // Pattern matching (for dynamic content like user-provided colors)
    {
      pattern: /bg-(red|blue|green|yellow)-(100|500|900)/,
      variants: ["hover", "dark"],
    },
    // Safelist for all sizes of a component
    {
      pattern: /^(p|m|gap)-(1|2|3|4|6|8)$/,
    },
  ],
};
```

**When to safelist:**
- Classes applied via `dangerouslySetInnerHTML` or dynamic HTML from a CMS
- Classes stored in a database and applied at runtime
- Third-party component libraries that inject classes dynamically
- When you receive a color value from an API and need all shade variants

---

### Build Output Sizes

| Setup | CSS size (gzipped) |
|---|---|
| Tailwind v3 (prod build, typical app) | 5–15 KB |
| Tailwind v4 (prod build) | 3–10 KB |
| Bootstrap 5 | ~30 KB |
| Bulma | ~28 KB |
| Full Tailwind without purge | ~3.5 MB |

---

### `NODE_ENV=production` Effect

In development (`NODE_ENV=development`), Tailwind includes a comment in the CSS showing the source of each class (for debugging). In production:
- All comments stripped
- CSS minified
- Only used classes included

```bash
# Development build (larger, with source comments)
vite dev

# Production build (minified, purged)
NODE_ENV=production vite build
# Or simply:
vite build  # vite sets NODE_ENV=production automatically
```

---

### Analyzing Your Bundle

```bash
# See what's in your final CSS
vite build
# Check dist/assets/*.css for the compiled stylesheet

# For bundle analysis
npm install -D rollup-plugin-visualizer
```

```ts
// vite.config.ts
import { visualizer } from "rollup-plugin-visualizer";

export default {
  plugins: [
    visualizer({ open: true, gzipSize: true }),
  ],
};
```

> 💡 **Tip:** The fastest way to reduce your CSS bundle is ensuring your `content` paths don't scan `node_modules` or test files. Also, if you use a CDN, set long `Cache-Control` headers — your CSS file rarely changes between deploys if hashed correctly.

---

---

## Chapter 21 — Accessibility with Tailwind

### `sr-only` and `not-sr-only`

```jsx
{/* sr-only — visually hidden but read by screen readers */}
<span className="sr-only">Loading, please wait...</span>

{/* Icon button — always needs sr-only label */}
<button className="p-2 rounded-lg hover:bg-gray-100">
  <SearchIcon className="w-5 h-5 text-gray-600" aria-hidden="true" />
  <span className="sr-only">Search</span>
</button>

{/* Skip navigation link */}
<a
  href="#main-content"
  className="sr-only focus:not-sr-only focus:fixed focus:top-4 focus:left-4 focus:z-50 focus:px-4 focus:py-2 focus:bg-blue-600 focus:text-white focus:rounded-lg"
>
  Skip to main content
</a>

{/* not-sr-only — reveal element that was sr-only (useful for mobile menus) */}
<nav className="sr-only md:not-sr-only md:flex md:items-center md:gap-6">
  Mobile: screen-reader only | Desktop: visible
</nav>
```

---

### `focus-visible:` vs `focus:`

```jsx
{/* ❌ focus: — shows ring on mouse click too (ugly but not a11y wrong) */}
<button className="focus:ring-2 focus:ring-blue-500">
  Ring appears on both keyboard AND mouse click
</button>

{/* ✅ focus-visible: — ring only on keyboard navigation (modern a11y standard) */}
<button className="focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2">
  Ring ONLY on keyboard tab focus — clean for mouse users
</button>
```

**⚠️ Gotcha:** Always include `focus:outline-none` when using `focus-visible:ring-*` — otherwise browsers will show both the ring AND the default outline for keyboard users.

---

### `aria-*:` Variants

```jsx
{/* Style based on ARIA attributes */}
<button
  aria-expanded={isOpen}
  className="flex items-center gap-2 px-4 py-2 rounded-lg
             aria-expanded:bg-gray-100 aria-expanded:text-gray-900
             hover:bg-gray-50 transition-colors"
>
  Menu
  <ChevronDown className="w-4 h-4 aria-expanded:rotate-180 transition-transform" />
</button>

{/* Selected state in a listbox/tabs */}
<li
  role="option"
  aria-selected={isSelected}
  className="px-4 py-2 cursor-pointer rounded-lg
             aria-selected:bg-blue-600 aria-selected:text-white
             hover:bg-gray-100 aria-selected:hover:bg-blue-700
             transition-colors"
>
  Option
</li>

{/* Disabled state */}
<a
  href={isDisabled ? undefined : href}
  aria-disabled={isDisabled}
  className="px-4 py-2 text-sm font-medium
             aria-disabled:opacity-50 aria-disabled:cursor-not-allowed aria-disabled:pointer-events-none
             hover:bg-gray-100 rounded-lg transition-colors"
>
  Link
</a>
```

---

### Color Contrast — Checking Compliance

WCAG 2.1 requires:
- **AA (minimum):** 4.5:1 for normal text, 3:1 for large text (18px+ or 14px+ bold)
- **AAA (enhanced):** 7:1 for normal text, 4.5:1 for large text

**Tailwind palette contrast guide:**

| Background | Safe text colors | Notes |
|---|---|---|
| `bg-white` | `text-gray-900`, `text-gray-800` | Always safe |
| `bg-gray-50` | `text-gray-900`, `text-gray-700` | Safe |
| `bg-blue-600` | `text-white` | Safe |
| `bg-blue-100` | `text-blue-900`, `text-blue-800` | Safe |
| `bg-yellow-300` | `text-gray-900` | ⚠️ Light colors need dark text |
| `bg-red-500` | `text-white` | Safe |
| `bg-gray-300` | `text-gray-900` | ✅ But avoid `text-gray-500` (fails AA) |

> 💡 **Tip:** Use [Tailwind's official contrast checker](https://tailwindcss.com) or install the `axe` accessibility browser extension to audit your deployed app. The "Tailwind CSS IntelliSense" extension shows contrast ratio on hover.

---

### `prefers-reduced-motion`

```jsx
{/* motion-reduce: — apply when user prefers reduced motion */}
<div className="animate-spin motion-reduce:animate-none">
  Loader (stops animating for reduced-motion users)
</div>

{/* motion-safe: — ONLY apply when motion is OK */}
<div className="motion-safe:transition-transform motion-safe:hover:scale-105">
  Scale on hover (disabled for reduced-motion users)
</div>

{/* ✅ Complete accessible loading spinner */}
<div
  role="status"
  aria-label="Loading"
  className="w-8 h-8 rounded-full border-4 border-gray-200 border-t-blue-600 animate-spin motion-reduce:animate-none"
>
  <span className="sr-only">Loading...</span>
</div>
```

---

### Real Accessible Button Example

```tsx
import { forwardRef } from "react";
import { cn } from "@/lib/utils";

interface AccessibleButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  isLoading?: boolean;
  loadingText?: string;
}

export const AccessibleButton = forwardRef<HTMLButtonElement, AccessibleButtonProps>(
  ({ children, isLoading, loadingText = "Loading...", disabled, className, ...props }, ref) => {
    return (
      <button
        ref={ref}
        disabled={disabled || isLoading}
        aria-busy={isLoading}
        aria-disabled={disabled || isLoading}
        className={cn(
          // Layout
          "inline-flex items-center justify-center gap-2",
          // Size
          "px-5 py-2.5 text-sm font-semibold",
          // Colors
          "bg-blue-600 text-white",
          // Hover (only when interactive)
          "hover:bg-blue-700 active:bg-blue-800",
          // Focus — visible ring for keyboard, clean for mouse
          "focus:outline-none focus-visible:ring-2 focus-visible:ring-blue-500 focus-visible:ring-offset-2",
          // Disabled
          "disabled:opacity-50 disabled:cursor-not-allowed",
          // Shape
          "rounded-xl",
          // Motion-safe transition
          "motion-safe:transition-all motion-safe:duration-150",
          className,
        )}
        {...props}
      >
        {isLoading ? (
          <>
            <svg
              className="w-4 h-4 animate-spin motion-reduce:animate-none"
              fill="none"
              viewBox="0 0 24 24"
              aria-hidden="true"
            >
              <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" />
              <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.37 0 0 5.37 0 12h4z" />
            </svg>
            <span className="sr-only">{loadingText}</span>
            <span aria-hidden="true">{loadingText}</span>
          </>
        ) : (
          children
        )}
      </button>
    );
  }
);
AccessibleButton.displayName = "AccessibleButton";
```

---

---

## Chapter 22 — Real-World Mini Projects (Code Complete)

### Project 1: Landing Page Hero Section

```jsx
function HeroSection() {
  return (
    <section className="relative overflow-hidden bg-white">
      {/* Background decoration */}
      <div className="absolute inset-0 -z-10">
        <div className="absolute top-0 right-0 w-[600px] h-[600px] bg-gradient-to-bl from-blue-50 via-indigo-50 to-transparent rounded-full -translate-y-1/2 translate-x-1/4" />
        <div className="absolute bottom-0 left-0 w-[400px] h-[400px] bg-gradient-to-tr from-pink-50 to-transparent rounded-full translate-y-1/2 -translate-x-1/4" />
      </div>

      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pt-20 pb-28">
        <div className="flex flex-col lg:flex-row items-center gap-12 lg:gap-16">
          {/* Left: text content */}
          <div className="flex-1 text-center lg:text-left">
            {/* Eyebrow */}
            <div className="inline-flex items-center gap-2 px-3 py-1.5 bg-blue-50 border border-blue-100 text-blue-700 text-sm font-medium rounded-full mb-6">
              <span className="w-2 h-2 rounded-full bg-blue-500 animate-pulse" />
              Just launched v2.0
            </div>

            {/* Heading */}
            <h1 className="text-5xl sm:text-6xl lg:text-7xl font-black text-gray-900 leading-[1.05] tracking-tight text-balance">
              Ship products{" "}
              <span className="bg-clip-text text-transparent bg-gradient-to-r from-blue-600 to-violet-600">
                10x faster
              </span>
            </h1>

            {/* Subheading */}
            <p className="mt-6 text-xl text-gray-500 leading-relaxed max-w-xl mx-auto lg:mx-0 text-pretty">
              Stop reinventing the wheel. Our production-ready components, design system, and starter kits let you focus on what matters — your product.
            </p>

            {/* CTAs */}
            <div className="mt-10 flex flex-col sm:flex-row items-center justify-center lg:justify-start gap-4">
              <a
                href="/signup"
                className="w-full sm:w-auto px-8 py-4 bg-blue-600 hover:bg-blue-700 text-white font-bold text-base rounded-2xl shadow-lg shadow-blue-500/30 hover:shadow-xl hover:shadow-blue-500/40 hover:-translate-y-0.5 transition-all duration-150 text-center"
              >
                Start for free →
              </a>
              <a
                href="/demo"
                className="w-full sm:w-auto px-8 py-4 bg-white hover:bg-gray-50 text-gray-900 font-bold text-base rounded-2xl border-2 border-gray-200 hover:border-gray-300 transition-all duration-150 text-center"
              >
                View demo
              </a>
            </div>

            {/* Social proof */}
            <div className="mt-10 flex items-center justify-center lg:justify-start gap-4">
              <div className="flex -space-x-2">
                {[1, 2, 3, 4, 5].map((i) => (
                  <img
                    key={i}
                    src={`/avatars/${i}.jpg`}
                    alt=""
                    className="w-8 h-8 rounded-full border-2 border-white object-cover"
                  />
                ))}
              </div>
              <div className="text-sm text-gray-500">
                <span className="font-semibold text-gray-900">4,200+</span> developers trust us
              </div>
            </div>
          </div>

          {/* Right: product screenshot */}
          <div className="flex-1 lg:flex-none lg:w-[520px]">
            <div className="relative">
              <div className="absolute inset-0 bg-gradient-to-r from-blue-500 to-violet-500 rounded-3xl blur-3xl opacity-20 scale-95" />
              <div className="relative rounded-3xl overflow-hidden border border-gray-200 shadow-2xl">
                <img src="/screenshot.png" alt="Product screenshot" className="w-full" />
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  );
}
```

---

### Project 2: Responsive Navbar with Dark Mode Toggle

```jsx
import { useState } from "react";

function Navbar() {
  const [mobileOpen, setMobileOpen] = useState(false);
  const { isDark, toggle } = useDarkMode();

  const links = [
    { href: "/features", label: "Features" },
    { href: "/pricing", label: "Pricing" },
    { href: "/blog", label: "Blog" },
    { href: "/docs", label: "Docs" },
  ];

  return (
    <header className="sticky top-0 z-50 bg-white/80 dark:bg-gray-900/80 backdrop-blur-xl border-b border-gray-200/50 dark:border-gray-700/50">
      <nav className="max-w-7xl mx-auto px-4 sm:px-6">
        <div className="flex items-center justify-between h-16">
          {/* Logo */}
          <a href="/" className="flex items-center gap-2.5">
            <div className="w-8 h-8 rounded-xl bg-gradient-to-br from-blue-500 to-violet-600 flex items-center justify-center">
              <span className="text-white font-black text-sm">A</span>
            </div>
            <span className="font-bold text-gray-900 dark:text-white text-lg">Acme</span>
          </a>

          {/* Desktop nav */}
          <div className="hidden md:flex items-center gap-1">
            {links.map((link) => (
              <a
                key={link.href}
                href={link.href}
                className="px-4 py-2 text-sm font-medium text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800 rounded-lg transition-colors"
              >
                {link.label}
              </a>
            ))}
          </div>

          {/* Right actions */}
          <div className="flex items-center gap-3">
            {/* Dark mode toggle */}
            <button
              onClick={toggle}
              className="p-2 rounded-lg text-gray-500 hover:text-gray-900 dark:text-gray-400 dark:hover:text-white hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors"
              aria-label="Toggle dark mode"
            >
              {isDark ? (
                <svg className="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"><path fillRule="evenodd" d="M10 2a1 1 0 011 1v1a1 1 0 11-2 0V3a1 1 0 011-1zm4 8a4 4 0 11-8 0 4 4 0 018 0zm-.464 4.95l.707.707a1 1 0 001.414-1.414l-.707-.707a1 1 0 00-1.414 1.414zm2.12-10.607a1 1 0 010 1.414l-.706.707a1 1 0 11-1.414-1.414l.707-.707a1 1 0 011.414 0zM17 11a1 1 0 100-2h-1a1 1 0 100 2h1zm-7 4a1 1 0 011 1v1a1 1 0 11-2 0v-1a1 1 0 011-1zM5.05 6.464A1 1 0 106.465 5.05l-.708-.707a1 1 0 00-1.414 1.414l.707.707zm1.414 8.486l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM4 11a1 1 0 100-2H3a1 1 0 000 2h1z" clipRule="evenodd" /></svg>
              ) : (
                <svg className="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"><path d="M17.293 13.293A8 8 0 016.707 2.707a8.001 8.001 0 1010.586 10.586z" /></svg>
              )}
            </button>

            <div className="hidden md:flex items-center gap-3">
              <a href="/login" className="text-sm font-medium text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-white transition-colors">
                Sign in
              </a>
              <a href="/signup" className="px-4 py-2 bg-gray-900 dark:bg-white text-white dark:text-gray-900 text-sm font-semibold rounded-xl hover:bg-gray-700 dark:hover:bg-gray-100 transition-colors">
                Get started
              </a>
            </div>

            {/* Mobile hamburger */}
            <button
              onClick={() => setMobileOpen(!mobileOpen)}
              className="md:hidden p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors"
              aria-label="Toggle menu"
              aria-expanded={mobileOpen}
            >
              <div className="w-5 h-4 flex flex-col justify-between">
                <span className={`block h-0.5 bg-gray-600 dark:bg-gray-300 transition-transform duration-200 ${mobileOpen ? "rotate-45 translate-y-[7.5px]" : ""}`} />
                <span className={`block h-0.5 bg-gray-600 dark:bg-gray-300 transition-opacity duration-200 ${mobileOpen ? "opacity-0" : ""}`} />
                <span className={`block h-0.5 bg-gray-600 dark:bg-gray-300 transition-transform duration-200 ${mobileOpen ? "-rotate-45 -translate-y-[7.5px]" : ""}`} />
              </div>
            </button>
          </div>
        </div>

        {/* Mobile menu */}
        <div className={`md:hidden overflow-hidden transition-all duration-200 ${mobileOpen ? "max-h-96 pb-4" : "max-h-0"}`}>
          <div className="flex flex-col gap-1 pt-2">
            {links.map((link) => (
              <a
                key={link.href}
                href={link.href}
                className="px-4 py-3 text-sm font-medium text-gray-700 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-800 rounded-xl transition-colors"
              >
                {link.label}
              </a>
            ))}
            <div className="h-px bg-gray-100 dark:bg-gray-800 my-2" />
            <a href="/signup" className="px-4 py-3 text-sm font-semibold text-center bg-gray-900 dark:bg-white text-white dark:text-gray-900 rounded-xl">
              Get started
            </a>
          </div>
        </div>
      </nav>
    </header>
  );
}
```

---

### Project 3: Pricing Cards

```jsx
const plans = [
  {
    id: "starter",
    name: "Starter",
    price: "$0",
    description: "Perfect for side projects",
    featured: false,
    cta: "Start for free",
    features: ["5 projects", "10GB storage", "Basic analytics", "Email support"],
  },
  {
    id: "pro",
    name: "Pro",
    price: "$29",
    description: "For growing teams",
    featured: true,
    cta: "Get started",
    features: ["Unlimited projects", "100GB storage", "Advanced analytics", "Priority support", "Custom domains", "Team collaboration"],
  },
  {
    id: "enterprise",
    name: "Enterprise",
    price: "$99",
    description: "For large organizations",
    featured: false,
    cta: "Contact sales",
    features: ["Everything in Pro", "Unlimited storage", "SLA guarantee", "Dedicated support", "SSO/SAML", "Audit logs"],
  },
];

function PricingCards() {
  return (
    <section className="py-24 bg-gray-50">
      <div className="max-w-6xl mx-auto px-4">
        <div className="text-center mb-16">
          <h2 className="text-4xl font-black text-gray-900">Simple, transparent pricing</h2>
          <p className="mt-4 text-xl text-gray-500 max-w-2xl mx-auto">
            No hidden fees. Cancel anytime. All plans include a 14-day free trial.
          </p>
        </div>

        <div className="grid grid-cols-1 md:grid-cols-3 gap-8 items-stretch">
          {plans.map((plan) => (
            <div
              key={plan.id}
              className={`relative flex flex-col rounded-3xl p-8 ${
                plan.featured
                  ? "bg-blue-600 text-white shadow-2xl shadow-blue-500/30 scale-105"
                  : "bg-white border border-gray-200 shadow-sm"
              }`}
            >
              {plan.featured && (
                <div className="absolute -top-4 left-1/2 -translate-x-1/2">
                  <span className="inline-flex items-center px-4 py-1.5 bg-gradient-to-r from-amber-400 to-orange-400 text-white text-xs font-black rounded-full shadow-lg">
                    ⭐ MOST POPULAR
                  </span>
                </div>
              )}

              <div className="mb-6">
                <h3 className={`text-lg font-bold ${plan.featured ? "text-blue-100" : "text-gray-900"}`}>
                  {plan.name}
                </h3>
                <div className="mt-3 flex items-baseline gap-1">
                  <span className={`text-5xl font-black ${plan.featured ? "text-white" : "text-gray-900"}`}>
                    {plan.price}
                  </span>
                  <span className={`text-sm ${plan.featured ? "text-blue-200" : "text-gray-500"}`}>/month</span>
                </div>
                <p className={`mt-2 text-sm ${plan.featured ? "text-blue-200" : "text-gray-500"}`}>
                  {plan.description}
                </p>
              </div>

              <ul className="flex-1 space-y-3 mb-8">
                {plan.features.map((feature) => (
                  <li key={feature} className="flex items-center gap-3 text-sm">
                    <svg
                      className={`w-5 h-5 shrink-0 ${plan.featured ? "text-blue-200" : "text-green-500"}`}
                      fill="currentColor" viewBox="0 0 20 20"
                    >
                      <path fillRule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clipRule="evenodd" />
                    </svg>
                    <span className={plan.featured ? "text-blue-50" : "text-gray-700"}>
                      {feature}
                    </span>
                  </li>
                ))}
              </ul>

              <a
                href="/signup"
                className={`block text-center py-3.5 px-6 rounded-2xl font-bold text-sm transition-all duration-150 hover:-translate-y-0.5 ${
                  plan.featured
                    ? "bg-white text-blue-600 hover:bg-blue-50 shadow-lg"
                    : "bg-gray-900 text-white hover:bg-gray-700"
                }`}
              >
                {plan.cta}
              </a>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

### Project 4: Dashboard Layout

```jsx
function DashboardLayout({ children }: { children: React.ReactNode }) {
  const [sidebarOpen, setSidebarOpen] = useState(true);

  return (
    <div className="flex h-screen bg-gray-50 overflow-hidden">
      {/* Sidebar */}
      <aside
        className={`${sidebarOpen ? "w-64" : "w-16"} flex-none flex flex-col bg-gray-900 text-white transition-[width] duration-200 ease-in-out overflow-hidden`}
      >
        {/* Logo */}
        <div className="flex items-center h-16 px-4 border-b border-gray-800 shrink-0">
          <div className="w-8 h-8 rounded-xl bg-blue-500 flex items-center justify-center shrink-0">
            <span className="font-black text-sm">A</span>
          </div>
          {sidebarOpen && <span className="ml-3 font-bold text-lg truncate">Acme</span>}
        </div>

        {/* Nav */}
        <nav className="flex-1 px-2 py-4 space-y-1 overflow-y-auto">
          {navItems.map((item) => (
            <a
              key={item.href}
              href={item.href}
              className={`flex items-center gap-3 px-2 py-2.5 rounded-xl font-medium text-sm transition-colors ${
                item.active
                  ? "bg-blue-600 text-white"
                  : "text-gray-400 hover:text-white hover:bg-gray-800"
              }`}
            >
              <item.Icon className="w-5 h-5 shrink-0" />
              {sidebarOpen && <span className="truncate">{item.label}</span>}
            </a>
          ))}
        </nav>

        {/* User */}
        <div className="shrink-0 border-t border-gray-800 p-3">
          <div className="flex items-center gap-3">
            <img src="/avatar.jpg" className="w-8 h-8 rounded-full shrink-0 object-cover" alt="" />
            {sidebarOpen && (
              <div className="min-w-0">
                <p className="text-sm font-medium truncate">Jane Smith</p>
                <p className="text-xs text-gray-500 truncate">jane@acme.com</p>
              </div>
            )}
          </div>
        </div>
      </aside>

      {/* Main */}
      <div className="flex-1 flex flex-col overflow-hidden">
        {/* Topbar */}
        <header className="flex items-center justify-between h-16 px-6 bg-white border-b border-gray-200 shrink-0">
          <button
            onClick={() => setSidebarOpen(!sidebarOpen)}
            className="p-2 rounded-lg hover:bg-gray-100 text-gray-500 hover:text-gray-900 transition-colors"
          >
            <MenuIcon className="w-5 h-5" />
          </button>
          <div className="flex items-center gap-3">
            <button className="relative p-2 rounded-lg hover:bg-gray-100 text-gray-500">
              <BellIcon className="w-5 h-5" />
              <span className="absolute top-1.5 right-1.5 w-2 h-2 bg-red-500 rounded-full" />
            </button>
          </div>
        </header>

        {/* Content */}
        <main className="flex-1 overflow-y-auto p-6">
          <div className="max-w-7xl mx-auto">
            {children}
          </div>
        </main>
      </div>
    </div>
  );
}
```

---

### Project 5: Form with Validation States

```jsx
import { useState } from "react";

function ContactForm() {
  const [errors, setErrors] = useState<Record<string, string>>({});
  const [success, setSuccess] = useState(false);

  return (
    <form
      className="max-w-xl mx-auto bg-white rounded-3xl border border-gray-200 shadow-sm p-8 space-y-6"
      onSubmit={handleSubmit}
      noValidate
    >
      <div>
        <h2 className="text-2xl font-bold text-gray-900">Contact us</h2>
        <p className="text-gray-500 mt-1">We'll get back to you within 24 hours.</p>
      </div>

      {/* Name and Email row */}
      <div className="grid grid-cols-1 sm:grid-cols-2 gap-4">
        <div className="space-y-1.5">
          <label className="text-sm font-medium text-gray-700">
            First name <span className="text-red-500">*</span>
          </label>
          <input
            type="text"
            className={`w-full px-3 py-2.5 rounded-xl border text-sm transition-colors focus:outline-none focus:ring-2 ${
              errors.firstName
                ? "border-red-400 focus:ring-red-500/20 focus:border-red-500 bg-red-50/30"
                : "border-gray-300 focus:ring-blue-500/20 focus:border-blue-500"
            }`}
            placeholder="Jane"
          />
          {errors.firstName && <p className="text-xs text-red-500">{errors.firstName}</p>}
        </div>
        <div className="space-y-1.5">
          <label className="text-sm font-medium text-gray-700">
            Last name <span className="text-red-500">*</span>
          </label>
          <input
            type="text"
            className={`w-full px-3 py-2.5 rounded-xl border text-sm transition-colors focus:outline-none focus:ring-2 ${
              errors.lastName
                ? "border-red-400 focus:ring-red-500/20 focus:border-red-500 bg-red-50/30"
                : "border-gray-300 focus:ring-blue-500/20 focus:border-blue-500"
            }`}
            placeholder="Smith"
          />
          {errors.lastName && <p className="text-xs text-red-500">{errors.lastName}</p>}
        </div>
      </div>

      {/* Email with peer validation */}
      <div className="space-y-1.5">
        <label className="text-sm font-medium text-gray-700">
          Email address <span className="text-red-500">*</span>
        </label>
        <input
          type="email"
          name="email"
          required
          className="peer w-full px-3 py-2.5 rounded-xl border border-gray-300 text-sm
                     focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-500
                     invalid:border-red-400 invalid:focus:ring-red-500/20 invalid:focus:border-red-500
                     transition-colors"
          placeholder="jane@example.com"
        />
        <p className="hidden text-xs text-red-500 peer-[&:not(:placeholder-shown)]:peer-invalid:block">
          Please enter a valid email address.
        </p>
      </div>

      {/* Topic select */}
      <div className="space-y-1.5">
        <label className="text-sm font-medium text-gray-700">Topic</label>
        <div className="relative">
          <select
            className="w-full appearance-none px-3 py-2.5 pr-10 rounded-xl border border-gray-300 text-sm bg-white
                       focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-500
                       transition-colors text-gray-900"
          >
            <option value="">Select a topic...</option>
            <option>General inquiry</option>
            <option>Sales</option>
            <option>Technical support</option>
            <option>Billing</option>
          </select>
          <div className="absolute right-3 top-1/2 -translate-y-1/2 pointer-events-none text-gray-400">
            <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M19 9l-7 7-7-7" />
            </svg>
          </div>
        </div>
      </div>

      {/* Message textarea */}
      <div className="space-y-1.5">
        <label className="text-sm font-medium text-gray-700">
          Message <span className="text-red-500">*</span>
        </label>
        <textarea
          rows={4}
          required
          className="w-full px-3 py-2.5 rounded-xl border border-gray-300 text-sm resize-y
                     focus:outline-none focus:ring-2 focus:ring-blue-500/20 focus:border-blue-500
                     placeholder:text-gray-400 transition-colors"
          placeholder="Tell us how we can help..."
        />
      </div>

      {/* Checkbox */}
      <label className="flex items-start gap-3 cursor-pointer group">
        <input
          type="checkbox"
          required
          className="mt-0.5 w-4 h-4 rounded border-gray-300 accent-blue-600
                     focus:ring-2 focus:ring-blue-500 focus:ring-offset-1"
        />
        <span className="text-sm text-gray-600 group-hover:text-gray-900 transition-colors">
          I agree to the{" "}
          <a href="/terms" className="text-blue-600 hover:underline">Terms of Service</a>{" "}
          and{" "}
          <a href="/privacy" className="text-blue-600 hover:underline">Privacy Policy</a>
        </span>
      </label>

      {/* Success message */}
      {success && (
        <div className="flex items-center gap-3 p-4 bg-green-50 border border-green-200 rounded-xl">
          <svg className="w-5 h-5 text-green-500 shrink-0" fill="currentColor" viewBox="0 0 20 20">
            <path fillRule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clipRule="evenodd" />
          </svg>
          <p className="text-sm font-medium text-green-800">
            Message sent! We'll get back to you within 24 hours.
          </p>
        </div>
      )}

      {/* Submit */}
      <button
        type="submit"
        className="w-full py-3 px-6 bg-blue-600 hover:bg-blue-700 active:bg-blue-800 text-white font-bold text-sm rounded-2xl
                   shadow-lg shadow-blue-500/25 hover:shadow-xl hover:shadow-blue-500/30 hover:-translate-y-0.5
                   focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2
                   transition-all duration-150"
      >
        Send message
      </button>
    </form>
  );
}
```

---

---

## Chapter 23 — Tips, Tricks & Gotchas

### 1. `min-h-0` Trick in Flex Children to Prevent Overflow

```jsx
{/* ❌ The inner content overflows — flex children have min-height: auto */}
<div className="flex flex-col h-screen">
  <header className="h-16">Header</header>
  <main className="flex-1 overflow-auto">  {/* overflow-auto doesn't work! */}
    {longContent}
  </main>
</div>

{/* ✅ Add min-h-0 to the flex child that should scroll */}
<div className="flex flex-col h-screen">
  <header className="h-16">Header</header>
  <main className="flex-1 min-h-0 overflow-auto">  {/* Now it scrolls correctly */}
    {longContent}
  </main>
</div>
```

---

### 2. `flex-1 min-w-0` for Text Truncation Inside Flex

```jsx
{/* ❌ Text won't truncate — flex item's min-width is auto */}
<div className="flex items-center gap-3">
  <Avatar src={user.avatar} />
  <span className="truncate">{user.longName}</span>  {/* Doesn't truncate */}
</div>

{/* ✅ min-w-0 allows the text container to shrink */}
<div className="flex items-center gap-3">
  <Avatar src={user.avatar} className="shrink-0" />
  <span className="flex-1 min-w-0 truncate">{user.longName}</span>  {/* Truncates correctly */}
</div>
```

---

### 3. `grid place-items-center` — Simplest Centering

```jsx
{/* Center anything — works for single or multiple items */}
<div className="grid place-items-center min-h-screen">
  <Card>Perfectly centered</Card>
</div>

{/* Center within a fixed area */}
<div className="grid place-items-center w-16 h-16 bg-blue-100 rounded-full">
  <Icon className="w-6 h-6 text-blue-600" />
</div>
```

---

### 4. `absolute inset-0` — Full Overlay Pattern

```jsx
{/* Full-size overlay on a positioned parent */}
<div className="relative rounded-xl overflow-hidden">
  <img src="photo.jpg" className="w-full h-64 object-cover" />
  {/* Dark overlay */}
  <div className="absolute inset-0 bg-gradient-to-t from-black/60 to-transparent" />
  {/* Text on top of overlay */}
  <div className="absolute inset-0 flex items-end p-4">
    <h3 className="text-white font-bold text-lg">Photo caption</h3>
  </div>
</div>
```

---

### 5. `w-full aspect-video object-cover` — Responsive Video/Image

```jsx
{/* Responsive video embed */}
<div className="w-full aspect-video rounded-xl overflow-hidden">
  <iframe src="https://youtube.com/embed/..." className="w-full h-full" />
</div>

{/* Responsive image that fills and crops */}
<div className="w-full aspect-[4/3] overflow-hidden rounded-xl">
  <img src="photo.jpg" alt="" className="w-full h-full object-cover object-center" />
</div>
```

---

### 6. `pointer-events-none` on Decorative Elements

```jsx
{/* Decorative elements that shouldn't block clicks */}
<div className="relative">
  <input className="w-full pl-10 pr-4 py-2 border rounded-lg focus:ring-2 focus:ring-blue-500 focus:outline-none" />
  {/* Icon doesn't block the input */}
  <SearchIcon className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400 pointer-events-none" />
</div>

{/* Background decoration doesn't block page content */}
<div className="absolute top-0 left-0 w-full h-full overflow-hidden pointer-events-none -z-10">
  <div className="absolute top-20 left-20 w-96 h-96 bg-blue-100 rounded-full blur-3xl opacity-40" />
</div>
```

---

### 7. `isolate` to Fix Z-Index Stacking Context Bugs

```jsx
{/* Problem: a transform or filter on a parent creates a new stacking context,
    making z-50 inside it not stack above z-10 outside it */}

{/* ✅ Use isolate to intentionally contain z-index within the component */}
<div className="isolate relative">
  <div className="relative z-10 bg-white p-4 rounded">Low layer</div>
  <div className="relative z-20 bg-blue-600 p-4 rounded -mt-2">High layer</div>
</div>
```

---

### 8. `transform-gpu` for Smoother Animations

```jsx
{/* Add to any element with frequent transforms/animations */}
<div className="transform-gpu hover:scale-105 hover:-translate-y-1 transition-transform duration-200">
  GPU-composited element — silky smooth on mobile
</div>
```

---

### 9. `scroll-mt-*` for Anchor Link Offsets with Sticky Headers

```jsx
{/* Your sticky header is 64px (4rem = 16 units) tall */}
<header className="sticky top-0 h-16 z-50 bg-white">...</header>

{/* Add scroll-mt-16 to every section target */}
<section id="features" className="scroll-mt-20 py-24">
  {/* scroll-mt-20 = 80px — adds extra breathing room above the header */}
</section>

{/* In-page anchor link */}
<a href="#features" className="scroll-smooth">See features ↓</a>
{/* Also add scroll-smooth to <html> or the scroll container */}
```

---

### 10. `line-clamp-3` for Card Description Truncation

```jsx
{/* Perfect for blog cards, product cards, user bios */}
<p className="text-sm text-gray-600 line-clamp-3 leading-relaxed">
  {post.description}
  {/* Only 3 lines shown, rest is "..." — consistent card heights */}
</p>

{/* Expandable truncation */}
const [expanded, setExpanded] = useState(false);
<p className={`text-sm text-gray-600 leading-relaxed ${expanded ? "" : "line-clamp-3"}`}>
  {post.description}
</p>
<button onClick={() => setExpanded(!expanded)} className="text-xs text-blue-600 mt-1">
  {expanded ? "Show less" : "Show more"}
</button>
```

---

### 11. Gradient Text

```jsx
{/* Classic gradient text */}
<h1 className="bg-clip-text text-transparent bg-gradient-to-r from-purple-600 to-pink-600 font-black text-5xl">
  Gradient Heading
</h1>

{/* With hover color change */}
<h2 className="bg-clip-text text-transparent bg-gradient-to-r from-blue-600 to-cyan-500 hover:from-violet-600 hover:to-pink-500 transition-all duration-300 font-bold text-3xl cursor-default">
  Hover to change gradient
</h2>
```

---

### 12. `divide-y divide-gray-200` Instead of Border on Each Row

```jsx
{/* ❌ Verbose — border on every item */}
<ul>
  {items.map((item, i) => (
    <li key={i} className={`py-3 ${i !== items.length - 1 ? "border-b border-gray-200" : ""}`}>
      {item.name}
    </li>
  ))}
</ul>

{/* ✅ Clean — divide-y handles it automatically */}
<ul className="divide-y divide-gray-200">
  {items.map((item) => (
    <li key={item.id} className="py-3 first:pt-0 last:pb-0">
      {item.name}
    </li>
  ))}
</ul>
```

---

### 13. `group/name` — Named Groups for Nested Interactions

```jsx
{/* Two levels of group-hover without conflict */}
<div className="group/card hover:shadow-lg transition-shadow">
  <div className="group/inner p-4">
    <h3 className="font-bold group-hover/card:text-blue-600 transition-colors">
      Title changes on card hover
    </h3>
    <button className="group-hover/inner:visible invisible opacity-0 group-hover/inner:opacity-100 transition-all text-sm text-blue-600">
      Action appears on inner hover
    </button>
  </div>
</div>
```

---

### 14. `peer-checked:` for Custom Toggle/Checkbox UI

```jsx
{/* Custom toggle switch using peer */}
<label className="relative inline-flex items-center cursor-pointer">
  <input type="checkbox" className="sr-only peer" />
  {/* Track */}
  <div className="w-11 h-6 bg-gray-200 rounded-full
                  peer-checked:bg-blue-600
                  after:content-[''] after:absolute after:top-0.5 after:left-0.5
                  after:bg-white after:rounded-full after:h-5 after:w-5
                  after:transition-transform after:duration-200
                  peer-checked:after:translate-x-5" />
  <span className="ml-3 text-sm font-medium text-gray-700 peer-checked:text-blue-700">
    Enable notifications
  </span>
</label>
```

---

### 15. `[&>*]:` — Arbitrary Child Selector

```jsx
{/* Apply styles to all direct children */}
<div className="[&>*]:border-b [&>*]:border-gray-100 [&>*:last-child]:border-0 [&>*]:py-3 [&>*]:px-4">
  <div>Row 1</div>
  <div>Row 2</div>
  <div>Row 3</div>
</div>

{/* Apply to all paragraphs inside */}
<div className="[&_p]:text-gray-700 [&_p]:leading-relaxed [&_p]:mb-4">
  <div dangerouslySetInnerHTML={{ __html: content }} />
</div>
```

---

### 16. `@apply` in Base Layer for Typography Resets

```css
/* In your CSS file — using @apply in @layer base */
@layer base {
  h1 { @apply text-4xl font-bold text-gray-900 leading-tight; }
  h2 { @apply text-2xl font-bold text-gray-900; }
  h3 { @apply text-xl font-semibold text-gray-800; }
  p  { @apply text-gray-700 leading-relaxed; }
  a  { @apply text-blue-600 hover:text-blue-700 hover:underline transition-colors; }
  code { @apply font-mono text-sm bg-gray-100 text-gray-800 px-1.5 py-0.5 rounded; }
}
```

---

### 17. Transparent Gradient Fade for "More Content" Hint

```jsx
{/* Fade out to hint there's more content below */}
<div className="relative">
  <div className="max-h-48 overflow-hidden">
    {longContent}
  </div>
  {/* Gradient from transparent to white */}
  <div className="absolute inset-x-0 bottom-0 h-24 bg-gradient-to-t from-white from-10% to-transparent pointer-events-none" />
  <button className="mt-2 text-sm text-blue-600 hover:text-blue-700 font-medium">
    Show more
  </button>
</div>
```

---

### 18. `bg-[url('/img.png')]` — Background Image from Arbitrary Value

```jsx
{/* Hero with background image */}
<section className="bg-[url('/hero-bg.jpg')] bg-cover bg-center bg-no-repeat min-h-screen">
  <div className="bg-black/40 min-h-screen flex items-center justify-center">
    <h1 className="text-white text-6xl font-black">Hero Text</h1>
  </div>
</section>

{/* Pattern background */}
<div className="bg-[url('/grid-pattern.svg')] bg-repeat bg-[length:40px_40px]">
  Content on pattern
</div>
```

---

### 19. Container Query Pattern

```bash
npm install @tailwindcss/container-queries
```

```jsx
{/* The container responds to ITS OWN width, not the viewport */}
<div className="@container">
  <div className="grid grid-cols-1 @sm:grid-cols-2 @lg:grid-cols-3 gap-4">
    {/* Becomes 2-col when container is ≥640px, 3-col when ≥1024px */}
    {items.map((item) => <Card key={item.id} {...item} />)}
  </div>
</div>

{/* This is powerful for reusable components — the same card component
    adapts to wherever it's placed (narrow sidebar vs wide main area) */}
```

---

### 20. `overscroll-contain` to Prevent Body Scroll in Modal

```jsx
{/* Prevent the page from scrolling when modal is scrolled to the end */}
<div
  className="fixed inset-0 z-50 overflow-y-auto overscroll-contain bg-black/50"
  onClick={closeModal}
>
  <div
    className="min-h-screen flex items-center justify-center p-4"
    onClick={(e) => e.stopPropagation()}
  >
    <div className="bg-white rounded-3xl max-w-lg w-full max-h-[90vh] overflow-y-auto overscroll-contain p-8">
      {/* Modal content */}
    </div>
  </div>
</div>
```

---

### 21. The `not-prose` Class Inside `prose` Blocks

```jsx
{/* Inside a prose article, opt specific elements out of prose styles */}
<article className="prose prose-lg">
  <h1>Article Title</h1>
  <p>Normal prose paragraph with prose styles applied.</p>

  {/* This component won't inherit prose styles */}
  <div className="not-prose">
    <CodeBlock language="tsx" code={snippet} />
  </div>

  <p>Back to normal prose after the code block.</p>
</article>
```

---

### 22. Using `data-*` Attributes with Tailwind Variants

```jsx
{/* Control visual state via data attributes — great for headless UI components */}
<button
  data-state={isOpen ? "open" : "closed"}
  data-variant="primary"
  className="data-[state=open]:rotate-180 data-[state=open]:bg-blue-50 data-[variant=primary]:text-blue-600 transition-all"
>
  Toggle
</button>

{/* Radix UI / Headless UI integration */}
<AccordionTrigger
  className="data-[state=open]:text-blue-600 data-[state=open]:font-semibold flex items-center justify-between w-full py-4 text-left transition-colors"
>
  Question
  <ChevronDown className="w-4 h-4 data-[state=open]:rotate-180 transition-transform" />
</AccordionTrigger>
```

---

### 23. Hover Card Lift

```jsx
{/* Smooth lift and shadow on hover */}
<div className="bg-white rounded-2xl border border-gray-100 p-6 shadow-sm
                hover:-translate-y-1 hover:shadow-xl hover:shadow-gray-200/80
                transition-all duration-200 ease-out cursor-pointer">
  <h3 className="font-bold text-gray-900">Feature card</h3>
  <p className="text-gray-500 text-sm mt-1">Lifts on hover.</p>
</div>
```

---

### 24. Shimmer/Skeleton Animation

```ts
// tailwind.config.ts
export default {
  theme: {
    extend: {
      keyframes: {
        shimmer: {
          "0%": { backgroundPosition: "-200% 0" },
          "100%": { backgroundPosition: "200% 0" },
        },
      },
      animation: {
        shimmer: "shimmer 1.5s linear infinite",
      },
    },
  },
};
```

```jsx
{/* Skeleton loader card */}
function SkeletonCard() {
  return (
    <div className="bg-white rounded-2xl border border-gray-100 p-6 space-y-4">
      <div className="flex items-center gap-3">
        <div className="w-10 h-10 rounded-full bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] animate-shimmer" />
        <div className="space-y-2">
          <div className="h-3 w-32 rounded bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] animate-shimmer" />
          <div className="h-2.5 w-24 rounded bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] animate-shimmer" />
        </div>
      </div>
      <div className="space-y-2">
        <div className="h-3 w-full rounded bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] animate-shimmer" />
        <div className="h-3 w-5/6 rounded bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] animate-shimmer" />
        <div className="h-3 w-4/6 rounded bg-gradient-to-r from-gray-200 via-gray-100 to-gray-200 bg-[length:200%_100%] animate-shimmer" />
      </div>
    </div>
  );
}
```

---

### 25. Dynamic Class Gotcha — Never String-Interpolate Class Names

```tsx
// ❌ ALL of these break in production (JIT scanner never sees complete class strings):
const size = "lg";
<div className={`text-${size}`} />            // "text-lg" — NOT seen by scanner

const color = props.color;
<div className={`bg-${color}-500`} />          // "bg-blue-500" — NOT seen

const prefix = "mt-";
<div className={prefix + props.spacing} />      // "mt-4" — NOT seen

// ✅ ALL of these work — complete class strings exist as literals:
const sizeMap = { sm: "text-sm", md: "text-base", lg: "text-lg" };
<div className={sizeMap[size]} />               // scanner sees all 3 strings

const colorMap = { red: "bg-red-500", blue: "bg-blue-500" };
<div className={colorMap[color]} />             // scanner sees all strings

<div className={spacing === 4 ? "mt-4" : "mt-8"} />  // both strings are literals
```

---

---

## Chapter 24 — Tailwind Cheat Sheet (Quick Reference)

### Spacing Scale (0–96)

| Scale | px | rem | Scale | px | rem |
|---|---|---|---|---|---|
| `0` | 0 | 0 | `20` | 80px | 5rem |
| `0.5` | 2px | 0.125rem | `24` | 96px | 6rem |
| `1` | 4px | 0.25rem | `28` | 112px | 7rem |
| `1.5` | 6px | 0.375rem | `32` | 128px | 8rem |
| `2` | 8px | 0.5rem | `36` | 144px | 9rem |
| `2.5` | 10px | 0.625rem | `40` | 160px | 10rem |
| `3` | 12px | 0.75rem | `44` | 176px | 11rem |
| `3.5` | 14px | 0.875rem | `48` | 192px | 12rem |
| `4` | 16px | 1rem | `52` | 208px | 13rem |
| `5` | 20px | 1.25rem | `56` | 224px | 14rem |
| `6` | 24px | 1.5rem | `60` | 240px | 15rem |
| `7` | 28px | 1.75rem | `64` | 256px | 16rem |
| `8` | 32px | 2rem | `72` | 288px | 18rem |
| `9` | 36px | 2.25rem | `80` | 320px | 20rem |
| `10` | 40px | 2.5rem | `96` | 384px | 24rem |
| `12` | 48px | 3rem | | | |
| `14` | 56px | 3.5rem | | | |
| `16` | 64px | 4rem | | | |

---

### Default Color Palette

```
Grayscale:  slate · gray · zinc · neutral · stone
Red family: red · orange · amber · yellow
Green:      lime · green · emerald · teal
Blue:       cyan · sky · blue · indigo
Purple:     violet · purple · fuchsia · pink · rose

Shades: 50 · 100 · 200 · 300 · 400 · 500 · 600 · 700 · 800 · 900 · 950
```

---

### Default Breakpoints

| Prefix | Min-width | Target |
|---|---|---|
| *(none)* | 0px | Mobile (base) |
| `sm:` | 640px | Large phones |
| `md:` | 768px | Tablets |
| `lg:` | 1024px | Laptops |
| `xl:` | 1280px | Desktops |
| `2xl:` | 1536px | Wide monitors |

---

### Most Common Layout Combos

```jsx
{/* Full-screen center */}
<div className="flex items-center justify-center min-h-screen">
<div className="grid place-items-center min-h-screen">

{/* Sticky header + scrolling content */}
<div className="flex flex-col h-screen">
  <header className="shrink-0">
  <main className="flex-1 min-h-0 overflow-auto">

{/* Sidebar + main */}
<div className="flex h-screen">
  <aside className="w-64 shrink-0">
  <main className="flex-1 min-h-0 overflow-auto">

{/* Centered constrained content */}
<div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">

{/* Card grid */}
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">

{/* Absolute fill overlay */}
<div className="absolute inset-0">
```

---

### Most Common Flex Combos

```jsx
{/* Navbar: logo left, nav center, actions right */}
<div className="flex items-center justify-between">

{/* Centered row */}
<div className="flex items-center justify-center gap-4">

{/* Grow input + fixed button */}
<div className="flex gap-2">
  <input className="grow" />
  <button className="shrink-0">

{/* Card: equal columns */}
<div className="flex gap-4">
  <div className="flex-1">  {/* each takes equal space */}

{/* Footer pushed down */}
<div className="flex flex-col h-full">
  <div className="grow">  {/* content grows */}
  <footer>  {/* footer stays at bottom */}

{/* Avatar + text, text truncates */}
<div className="flex items-center gap-3">
  <img className="shrink-0 w-10 h-10 rounded-full" />
  <div className="min-w-0">
    <p className="truncate">
```

---

### Most Common Grid Combos

```jsx
{/* Responsive auto-fit grid */}
<div className="grid grid-cols-[repeat(auto-fit,minmax(280px,1fr))] gap-6">

{/* 12-col layout */}
<div className="grid grid-cols-12 gap-6">
  <div className="col-span-12 md:col-span-8">main
  <div className="col-span-12 md:col-span-4">sidebar

{/* Holy grail */}
<div className="grid grid-cols-[240px_1fr] grid-rows-[auto_1fr_auto] min-h-screen">

{/* Two equal columns */}
<div className="grid grid-cols-2 gap-4">

{/* Custom columns */}
<div className="grid grid-cols-[200px_1fr_auto] gap-4">
```

---

### Most Common Text Combos

```jsx
{/* Hero heading */}
<h1 className="text-5xl font-black tracking-tight text-gray-900 leading-tight text-balance">

{/* Section heading */}
<h2 className="text-3xl font-bold text-gray-900">

{/* Card title */}
<h3 className="text-lg font-semibold text-gray-900">

{/* Body text */}
<p className="text-base text-gray-600 leading-relaxed">

{/* Caption / meta */}
<span className="text-sm text-gray-500">

{/* Label */}
<span className="text-xs font-medium uppercase tracking-widest text-gray-500">

{/* Gradient heading */}
<h1 className="bg-clip-text text-transparent bg-gradient-to-r from-blue-600 to-violet-600 font-black">

{/* Truncated card text */}
<p className="text-sm text-gray-600 line-clamp-3 leading-relaxed">
```

---

### Most Useful Modifier Prefixes

| Modifier | When it applies |
|---|---|
| `hover:` | Mouse hover |
| `focus:` | Focus (keyboard + mouse) |
| `focus-visible:` | Keyboard focus only |
| `active:` | Click/press state |
| `disabled:` | `disabled` attribute |
| `checked:` | Checked input |
| `group-hover:` | Parent with `group` is hovered |
| `peer-checked:` | Sibling with `peer` is checked |
| `dark:` | Dark mode active |
| `sm:` `md:` `lg:` `xl:` `2xl:` | Responsive breakpoints |
| `first:` `last:` | First/last child |
| `odd:` `even:` | Alternating rows |
| `aria-expanded:` | ARIA expanded state |
| `data-[key=val]:` | Custom data attribute |
| `motion-safe:` | User allows motion |
| `motion-reduce:` | User prefers reduced motion |
| `before:` `after:` | Pseudo-elements |
| `placeholder:` | Input placeholder |
| `selection:` | Selected text |
| `has-[selector]:` | Contains matching descendant |
| `not-[selector]:` | Doesn't match selector |
| `supports-[prop]:` | Browser supports CSS property |
| `[&>*]:` | Arbitrary child selector |
| `starting:` | `@starting-style` enter animations |

---

### Quick Utility Reference Card

```
DISPLAY:        block · inline-block · inline · flex · grid · hidden
POSITION:       static · relative · absolute · fixed · sticky
OVERFLOW:       visible · hidden · auto · scroll · clip
Z-INDEX:        z-0 · z-10 · z-20 · z-30 · z-40 · z-50 · z-auto · -z-10

FLEXBOX:
  Direction:    flex-row · flex-col · flex-row-reverse · flex-col-reverse
  Wrap:         flex-wrap · flex-nowrap
  Justify:      justify-start · center · end · between · around · evenly
  Align:        items-start · center · end · stretch · baseline
  Grow/Shrink:  flex-1 · flex-auto · flex-none · grow · shrink-0
  Gap:          gap-{n} · gap-x-{n} · gap-y-{n}

GRID:
  Cols:         grid-cols-{1-12} · grid-cols-[custom]
  Span:         col-span-{1-12} · row-span-{1-6}
  Place:        place-items-center · place-content-center

SIZING:
  Width:        w-full · w-screen · w-fit · w-min · w-max · w-1/2 · w-{n}
  Height:       h-full · h-screen · h-dvh · h-auto · h-{n}
  Max-w:        max-w-xs···7xl · max-w-prose · max-w-screen-{bp}

TYPOGRAPHY:
  Size:         text-xs · sm · base · lg · xl · 2xl···9xl
  Weight:       font-thin · light · normal · medium · semibold · bold · black
  Align:        text-left · center · right · justify
  Color:        text-{color}-{shade}
  Overflow:     truncate · line-clamp-{1-6}
  Transform:    uppercase · lowercase · capitalize
  Spacing:      tracking-tight · normal · wide · widest
  Height:       leading-tight · normal · relaxed · loose

COLORS:
  Text:         text-{color}-{shade} · text-{color}-{shade}/{opacity}
  Background:   bg-{color}-{shade} · bg-{color}-{shade}/{opacity}
  Border:       border-{color}-{shade}
  Ring:         ring-{color}-{shade}

BORDERS:
  Width:        border · border-{0,2,4,8} · border-{t,r,b,l,x,y}
  Radius:       rounded-{none,sm,,md,lg,xl,2xl,3xl,full}
  Style:        border-solid · dashed · dotted · double · none
  Ring:         ring-{1,2,4,8} · ring-offset-{1,2,4}

EFFECTS:
  Shadow:       shadow-{sm,,md,lg,xl,2xl,inner,none}
  Opacity:      opacity-{0,5,10,25,50,75,90,95,100}
  Blur:         blur-{sm,,md,lg,xl,2xl,3xl}
  Backdrop:     backdrop-blur-{sm,,md,lg,xl,2xl,3xl}

TRANSITIONS:
  Property:     transition · transition-all · transition-colors · transition-transform
  Duration:     duration-{75,100,150,200,300,500,700,1000}
  Easing:       ease-linear · ease-in · ease-out · ease-in-out
  Delay:        delay-{75,100,150,200,300,500,700,1000}

TRANSFORMS:
  Scale:        scale-{0,50,75,90,95,100,105,110,125,150}
  Rotate:       rotate-{0,1,2,3,6,12,45,90,180} · -rotate-*
  Translate:    translate-x-{n} · -translate-x-{n} · translate-y-{n}
  Origin:       origin-{center,top,top-right,right,...}
```

---

*End of The Complete Tailwind CSS Reference Book*

*Total chapters: 24 | Covers Tailwind v3 and v4*


