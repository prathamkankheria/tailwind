# Tailwind CSS v4 — Complete Notes (React Edition)
> **Version covered:** v4.3.1 (latest stable, released June 12, 2026)  
> **Stack assumed:** Vite + React + JavaScript (or TypeScript)  
> **Official Docs:** https://tailwindcss.com/docs  
> **Tailwind Play (browser sandbox):** https://play.tailwindcss.com

---

## Table of Contents
1. [What is Tailwind CSS?](#1-what-is-tailwind-css)
2. [Why Tailwind with React?](#2-why-tailwind-with-react)
3. [v4 vs v3 — What Changed?](#3-v4-vs-v3--what-changed)
4. [Setup — Vite + React + Tailwind v4](#4-setup--vite--react--tailwind-v4)
5. [The Mental Model — Utility-First](#5-the-mental-model--utility-first)
6. [Core Utility Categories](#6-core-utility-categories)
7. [Responsive Design](#7-responsive-design)
8. [Dark Mode](#8-dark-mode)
9. [State Variants (hover, focus, active...)](#9-state-variants-hover-focus-active)
10. [Flexbox Utilities](#10-flexbox-utilities)
11. [Grid Utilities](#11-grid-utilities)
12. [Typography Utilities](#12-typography-utilities)
13. [Colors & Opacity](#13-colors--opacity)
14. [Spacing (Padding & Margin)](#14-spacing-padding--margin)
15. [Sizing (Width & Height)](#15-sizing-width--height)
16. [Borders & Rings](#16-borders--rings)
17. [Shadows & Effects](#17-shadows--effects)
18. [Arbitrary Values](#18-arbitrary-values)
19. [Custom Theme with @theme](#19-custom-theme-with-theme)
20. [@apply — Extracting Repeated Patterns](#20-apply--extracting-repeated-patterns)
21. [Group & Peer Variants](#21-group--peer-variants)
22. [Container Queries](#22-container-queries)
23. [v4.3 New Features](#23-v43-new-features)
24. [React-Specific Patterns](#24-react-specific-patterns)
25. [Tools, Extensions & Resources](#25-tools-extensions--resources)

---

## 1. What is Tailwind CSS?

Tailwind CSS is a **utility-first CSS framework**. Instead of writing CSS files, you apply pre-built utility classes directly in your HTML/JSX.

**Traditional CSS approach:**
```css
/* styles.css */
.card {
  padding: 16px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}
```
```jsx
<div className="card">...</div>
```

**Tailwind approach:**
```jsx
<div className="p-4 bg-white rounded-lg shadow-md">...</div>
```

You are composing styles **inline**, directly in your markup using small single-purpose classes.

### The Kitchen Analogy
- **Bootstrap / MUI / Chakra** = Fast food menu (pre-made components, everything looks similar)
- **Tailwind** = A fully stocked kitchen (raw ingredients — you cook what you want)

Tailwind gives you the tools, not the finished product.

---

## 2. Why Tailwind with React?

React is component-based — you already isolate markup, logic, and now styles in one file. Tailwind fits perfectly because:

- **No context switching** — CSS lives next to the JSX that uses it
- **No naming fatigue** — You never invent class names like `.card-wrapper-inner-left`
- **Design consistency** — All spacing, colors, and typography come from the same design scale
- **Dead code elimination** — Tailwind v4 generates only the CSS classes you actually use
- **Co-location** — A `<Button />` component carries its own styles, no external CSS file needed

```jsx
// Button.jsx — self-contained, no CSS file required
export function Button({ children, variant = 'primary' }) {
  const base = "px-4 py-2 rounded-lg font-semibold text-sm transition-colors";
  const variants = {
    primary: "bg-indigo-600 text-white hover:bg-indigo-700",
    ghost:   "bg-transparent text-indigo-600 hover:bg-indigo-50 border border-indigo-600",
  };
  return (
    <button className={`${base} ${variants[variant]}`}>
      {children}
    </button>
  );
}
```

---

## 3. v4 vs v3 — What Changed?

Understanding the shift from v3 → v4 is essential because most tutorials online are still v3.

### Key Differences

| Feature | v3 | v4 |
|---|---|---|
| Config file | `tailwind.config.js` | No config file by default |
| CSS setup | `@tailwind base/components/utilities` | `@import "tailwindcss"` (one line) |
| Custom theme | JS object in config | `@theme` directive in CSS |
| Dark mode setup | `darkMode: 'class'` in config | `@custom-variant dark (...)` in CSS |
| PostCSS | Required separately | Handled internally |
| Build speed | Baseline | Up to **5× faster** full builds, **100× faster** incremental |
| Engine | Node.js | Rust-based core |
| Color format | HSL | **OKLCH** (wider gamut, more vivid) |
| Gradient syntax | `bg-gradient-to-r` | `bg-linear-to-r` (CSS-native naming) |

### v4 Setup Philosophy
> "CSS-first configuration" — Everything is configured in CSS, not JavaScript.

The `tailwind.config.js` file is **still supported for backwards compatibility** but is no longer the recommended approach. v4 encourages you to do all customization in your CSS file using new directives like `@theme`, `@custom-variant`, and `@utility`.

---

## 4. Setup — Vite + React + Tailwind v4

### Step 1: Create Vite + React project
```bash
npm create vite@latest my-app -- --template react
cd my-app
npm install
```

### Step 2: Install Tailwind and the Vite plugin
```bash
npm install -D tailwindcss @tailwindcss/vite
```

> **Note:** In v4 you do NOT need to install `autoprefixer` or `postcss` separately. The Vite plugin handles everything.

### Step 3: Add the plugin to `vite.config.js`
```js
// vite.config.js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import tailwindcss from '@tailwindcss/vite'   // ← add this

export default defineConfig({
  plugins: [
    react(),
    tailwindcss(),   // ← add this
  ],
})
```

### Step 4: Import Tailwind in your CSS
```css
/* src/index.css */
@import "tailwindcss";
```

That single line is the entire setup. No `@tailwind base/components/utilities` like in v3.

### Step 5: Import the CSS in `main.jsx`
```jsx
// src/main.jsx
import './index.css'
```

### Step 6: Verify it works
```jsx
// src/App.jsx
function App() {
  return (
    <h1 className="text-3xl font-bold text-indigo-600">
      Tailwind v4 is working!
    </h1>
  );
}
export default App;
```

Run `npm run dev` and you should see styled text.

---

## 5. The Mental Model — Utility-First

### How classes are generated
Tailwind does NOT ship a static CSS file with thousands of classes. It **scans your files** and generates only the CSS for classes you actually use. This keeps bundle size tiny.

### Naming pattern
Most utility classes follow a predictable pattern:

```
[property]-[value]
  p-4        → padding: 1rem
  m-2        → margin: 0.5rem
  text-xl    → font-size: 1.25rem
  bg-red-500 → background-color: red (500 shade)
  rounded-lg → border-radius: 0.5rem

[variant]:[property]-[value]
  hover:bg-blue-600   → on hover, change background
  md:flex             → at md breakpoint, display flex
  dark:text-white     → in dark mode, white text
```

### Modifiers stack left to right
```jsx
// "On medium screens, when dark mode, on hover → white text"
<p className="md:dark:hover:text-white">...</p>
```

---

## 6. Core Utility Categories

Quick reference map of what utilities exist and what they control.

```
LAYOUT         → display, position, overflow, z-index, float
FLEXBOX        → flex, justify, items, gap, flex-wrap, flex-direction
GRID           → grid-cols, grid-rows, col-span, row-span, gap
SPACING        → p, px, py, pt, pr, pb, pl, m, mx, my, mt, mr, mb, ml, gap
SIZING         → w, h, min-w, max-w, min-h, max-h, size (w+h together)
TYPOGRAPHY     → text, font, leading, tracking, truncate, line-clamp
COLORS         → text-*, bg-*, border-*, ring-*, shadow-*
BACKGROUNDS    → bg-*, bg-gradient, from-*, to-*, via-*
BORDERS        → border, border-*, rounded-*
EFFECTS        → shadow, ring, opacity, blur, backdrop-blur
TRANSITIONS    → transition, duration, ease, delay
TRANSFORMS     → scale, rotate, translate, skew
FILTERS        → blur, brightness, contrast, grayscale, saturate
INTERACTIVITY  → cursor, pointer-events, select, resize, scroll-*
ACCESSIBILITY  → sr-only, not-sr-only, focus-visible
```

---

## 7. Responsive Design

Tailwind is **mobile-first**. Styles without a prefix apply to ALL screen sizes. A breakpoint prefix means "at this size AND ABOVE".

### Default Breakpoints

| Prefix | Min Width | Equivalent |
|--------|-----------|-----------|
| *(none)* | 0px | Mobile |
| `sm:` | 40rem (640px) | Small tablets |
| `md:` | 48rem (768px) | Tablets |
| `lg:` | 64rem (1024px) | Laptops |
| `xl:` | 80rem (1280px) | Desktops |
| `2xl:` | 96rem (1536px) | Large screens |

### Example — Responsive Grid
```jsx
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
  <div className="bg-white rounded-xl p-4 shadow">Card 1</div>
  <div className="bg-white rounded-xl p-4 shadow">Card 2</div>
  <div className="bg-white rounded-xl p-4 shadow">Card 3</div>
</div>
```
- Mobile: 1 column
- Small screen: 2 columns
- Large screen: 3 columns

### Example — Responsive Typography
```jsx
<h1 className="text-2xl md:text-4xl xl:text-6xl font-bold">
  Hello, Pratham
</h1>
```

### Targeting Ranges (v4 feature)
In v4 you can use `max-*` prefixes to target BELOW a breakpoint:
```jsx
<div className="max-md:hidden">Only visible on md and above</div>
<div className="md:max-lg:block">Only visible between md and lg</div>
```

---

## 8. Dark Mode

### Default behavior (v4)
In v4, dark mode works automatically using the OS `prefers-color-scheme` media query — **no configuration needed** out of the box.

```jsx
<div className="bg-white dark:bg-gray-900">
  <p className="text-gray-900 dark:text-gray-100">
    Auto-switches based on OS dark mode setting
  </p>
</div>
```

### Manual Toggle (class-based dark mode)
If you want a button that toggles dark mode regardless of OS setting, you need to configure it. Add this to your `index.css`:

```css
/* index.css */
@import "tailwindcss";

@custom-variant dark (&:where(.dark, .dark *));
```

Then in React, toggle the `dark` class on the `<html>` element:
```jsx
function DarkModeToggle() {
  const toggle = () => {
    document.documentElement.classList.toggle('dark');
  };
  return <button onClick={toggle}>Toggle Dark Mode</button>;
}
```

### Common Dark Mode Pattern
```jsx
<div className="
  min-h-screen
  bg-white dark:bg-gray-950
  text-gray-900 dark:text-gray-100
  transition-colors duration-300
">
  <header className="border-b border-gray-200 dark:border-gray-800 bg-white dark:bg-gray-900">
    ...
  </header>
</div>
```

---

## 9. State Variants (hover, focus, active...)

State variants let you apply styles conditionally based on user interaction.

### Common State Variants

```jsx
// Hover
<button className="bg-indigo-600 hover:bg-indigo-700">Click me</button>

// Focus
<input className="border focus:outline-none focus:ring-2 focus:ring-indigo-500" />

// Focus-visible (keyboard-only focus ring — better UX)
<button className="focus-visible:ring-2 focus-visible:ring-indigo-500">...</button>

// Active (while clicking)
<button className="active:scale-95">...</button>

// Disabled
<button className="disabled:opacity-50 disabled:cursor-not-allowed" disabled>...</button>

// Placeholder text
<input className="placeholder:text-gray-400" placeholder="Search..." />

// First/Last child
<li className="border-b last:border-b-0">Item</li>

// Odd/Even rows
<tr className="odd:bg-white even:bg-gray-50">...</tr>

// Required / Invalid (form states)
<input className="invalid:border-red-500 required:border-yellow-400" />
```

### Stacking Variants
```jsx
// Hover + Dark mode
<div className="bg-white hover:bg-gray-50 dark:bg-gray-800 dark:hover:bg-gray-700">...</div>

// Responsive + Hover
<button className="md:hover:scale-105 transition-transform">...</button>
```

---

## 10. Flexbox Utilities

```jsx
// Basic flex container
<div className="flex">...</div>

// Direction
<div className="flex flex-row">        // default, horizontal
<div className="flex flex-col">        // vertical stack
<div className="flex flex-row-reverse">
<div className="flex flex-col-reverse">

// Justify content (main axis)
<div className="flex justify-start">    // default
<div className="flex justify-center">
<div className="flex justify-end">
<div className="flex justify-between">  // space between items
<div className="flex justify-around">
<div className="flex justify-evenly">

// Align items (cross axis)
<div className="flex items-start">
<div className="flex items-center">
<div className="flex items-end">
<div className="flex items-stretch">   // default

// Wrapping
<div className="flex flex-wrap">
<div className="flex flex-nowrap">

// Gap between items
<div className="flex gap-4">           // gap in both directions
<div className="flex gap-x-4 gap-y-2">

// Flex item properties
<div className="flex">
  <div className="flex-1">  // grow and fill available space
  <div className="flex-none">  // don't grow or shrink
  <div className="shrink-0">  // don't shrink
  <div className="grow">      // grow to fill
</div>
```

### Real Example — Navbar
```jsx
<nav className="flex items-center justify-between px-6 py-4 bg-white shadow">
  <span className="text-xl font-bold text-indigo-600">Pxtitan Studio</span>
  <ul className="flex items-center gap-6 text-sm font-medium text-gray-600">
    <li className="hover:text-indigo-600 cursor-pointer">Work</li>
    <li className="hover:text-indigo-600 cursor-pointer">About</li>
    <li>
      <button className="bg-indigo-600 text-white px-4 py-2 rounded-lg hover:bg-indigo-700">
        Contact
      </button>
    </li>
  </ul>
</nav>
```

---

## 11. Grid Utilities

```jsx
// Basic grid
<div className="grid grid-cols-3 gap-4">...</div>

// Auto columns (auto-fill / auto-fit)
<div className="grid grid-cols-[repeat(auto-fill,minmax(200px,1fr))] gap-4">

// Column spanning
<div className="grid grid-cols-4 gap-4">
  <div className="col-span-2">Takes 2 columns</div>
  <div className="col-span-1">Takes 1 column</div>
  <div className="col-span-1">Takes 1 column</div>
</div>

// Row spanning
<div className="row-span-2">Spans 2 rows</div>

// Grid template areas — use arbitrary values
<div className="grid [grid-template-areas:'header_header''sidebar_main']">

// Place items
<div className="grid place-items-center h-screen">   // center everything
  <div>Centered</div>
</div>
```

### Real Example — Card Grid
```jsx
<section className="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-3 gap-6 p-6">
  {projects.map(project => (
    <div key={project.id} className="bg-white rounded-2xl shadow-md overflow-hidden hover:shadow-lg transition-shadow">
      <img src={project.image} className="w-full h-48 object-cover" />
      <div className="p-4">
        <h3 className="font-semibold text-gray-900">{project.title}</h3>
        <p className="text-sm text-gray-500 mt-1">{project.description}</p>
      </div>
    </div>
  ))}
</section>
```

---

## 12. Typography Utilities

```jsx
// Font size (these also set line-height automatically)
text-xs      // 0.75rem
text-sm      // 0.875rem
text-base    // 1rem (default)
text-lg      // 1.125rem
text-xl      // 1.25rem
text-2xl     // 1.5rem
text-3xl     // 1.875rem
text-4xl     // 2.25rem
text-5xl     // 3rem
text-6xl     // 3.75rem

// Font weight
font-thin         // 100
font-light        // 300
font-normal       // 400
font-medium       // 500
font-semibold     // 600
font-bold         // 700
font-extrabold    // 800
font-black        // 900

// Font family
font-sans    // system UI sans-serif stack
font-serif   // Georgia etc.
font-mono    // monospace

// Line height (leading)
leading-none     // 1
leading-tight    // 1.25
leading-snug     // 1.375
leading-normal   // 1.5
leading-relaxed  // 1.625
leading-loose    // 2

// Letter spacing (tracking)
tracking-tighter  // -0.05em
tracking-tight    // -0.025em
tracking-normal   // 0em
tracking-wide     // 0.025em
tracking-wider    // 0.05em
tracking-widest   // 0.1em

// Text alignment
text-left  text-center  text-right  text-justify

// Text decoration
underline  line-through  no-underline

// Text transform
uppercase  lowercase  capitalize  normal-case

// Text overflow
truncate          // overflow hidden + ellipsis
text-ellipsis     // ellipsis on overflow
line-clamp-2      // clamp to 2 lines with ellipsis
line-clamp-3      // clamp to 3 lines

// Text shadow (added in v4.1!)
text-shadow-sm
text-shadow-md
text-shadow-lg
```

### Example — Hero Typography
```jsx
<div className="space-y-4">
  <p className="text-sm tracking-widest uppercase text-indigo-500 font-semibold">
    UI Design & Development
  </p>
  <h1 className="text-5xl xl:text-7xl font-black leading-none tracking-tight text-gray-900 dark:text-white">
    Building the web,<br />pixel by pixel.
  </h1>
  <p className="text-lg text-gray-600 dark:text-gray-400 max-w-xl leading-relaxed">
    Freelance frontend developer and UI designer based in Jaipur.
  </p>
</div>
```

---

## 13. Colors & Opacity

### Color Scale
Tailwind includes a full color palette. Each color has shades from 50 (lightest) to 950 (darkest).

```
slate  gray  zinc  neutral  stone       ← neutral grays
red  orange  amber  yellow  lime        ← warm
green  emerald  teal  cyan              ← cool greens
sky  blue  indigo  violet               ← blues/purples
purple  fuchsia  pink  rose             ← pinks/purples
```

Usage pattern: `{property}-{color}-{shade}`
```jsx
bg-indigo-500       // background
text-gray-700       // text color
border-red-300      // border
ring-blue-500       // focus ring
shadow-indigo-200   // colored shadow
from-violet-500     // gradient start
```

### Opacity Modifier
You can adjust opacity of any color directly with `/`:
```jsx
bg-indigo-500/50      // 50% opacity background
text-white/80         // 80% opacity text
border-black/10       // 10% opacity border
ring-indigo-500/30    // 30% opacity ring

// Using arbitrary opacity
bg-indigo-500/[0.35]
```

This uses `color-mix()` under the hood in v4 — no need for separate opacity utilities.

### v4 Color Format: OKLCH
v4 ships its built-in palette in OKLCH format, which is perceptually uniform and produces more vivid colors across the scale. You don't need to do anything special — this just means the 500 shades are more vibrant than they were in v3.

---

## 14. Spacing (Padding & Margin)

Tailwind uses a spacing scale where `1 unit = 0.25rem = 4px`.

```
0 → 0px       1 → 4px       2 → 8px       3 → 12px
4 → 16px      5 → 20px      6 → 24px      8 → 32px
10 → 40px     12 → 48px     16 → 64px     20 → 80px
24 → 96px     32 → 128px    40 → 160px    48 → 192px
```

### Padding
```jsx
p-4         // all sides
px-4        // left + right
py-4        // top + bottom
pt-4 pr-4 pb-4 pl-4  // individual sides
ps-4 pe-4   // logical: inline-start, inline-end (RTL-safe)
```

### Margin
```jsx
m-4   mx-4   my-4
mt-4  mr-4   mb-4  ml-4
ms-4  me-4   // logical (RTL-safe)

// Auto margin — great for centering
mx-auto     // center block horizontally
ml-auto     // push to the right
```

### Space Between Children
```jsx
// Add margin between children automatically
<div className="flex flex-col space-y-4">
  <div>Item 1</div>
  <div>Item 2</div>  // gets margin-top: 1rem
  <div>Item 3</div>  // gets margin-top: 1rem
</div>

// Prefer gap-* when using flex or grid
<div className="flex gap-4">...</div>
```

---

## 15. Sizing (Width & Height)

```jsx
// Fixed sizes (same spacing scale)
w-4  w-8  w-16  w-32  w-64

// Percentage-based
w-1/2    // 50%
w-1/3    // 33.33%
w-2/3    // 66.66%
w-3/4    // 75%
w-full   // 100%
w-screen // 100vw

// Height
h-4  h-8  h-16  h-32  h-64
h-full   h-screen  h-svh  h-dvh  h-lvh

// Min / Max sizing
min-w-0  min-w-full  min-w-screen
max-w-sm  max-w-md  max-w-lg  max-w-xl  max-w-2xl
max-w-screen-lg  max-w-full  max-w-none

// size-* (sets BOTH width and height — v4)
size-4      // w-4 h-4 in one
size-10     // w-10 h-10
size-full   // w-full h-full
```

### Centering a container
```jsx
<div className="max-w-5xl mx-auto px-4">
  {/* Content centered with max width */}
</div>
```

---

## 16. Borders & Rings

### Borders
```jsx
border          // 1px border (uses currentColor)
border-2        // 2px
border-4        // 4px

border-gray-200         // border color
border-indigo-500

border-solid            // default
border-dashed
border-dotted
border-none

rounded          // small border-radius
rounded-md       // medium
rounded-lg       // large
rounded-xl       // extra large
rounded-2xl
rounded-full     // circle / pill

// Individual sides
rounded-t-lg     // top
rounded-b-xl     // bottom
rounded-l-full   // left
rounded-r-2xl    // right

// Individual border sides
border-t border-r border-b border-l
border-t-2 border-b-4
```

### Rings (focus outlines)
Rings sit OUTSIDE the element and don't affect layout:
```jsx
ring            // 1px ring
ring-2          // 2px ring
ring-4
ring-inset      // ring on the inside

ring-indigo-500    // ring color
ring-white

ring-offset-2      // gap between element and ring
ring-offset-white
```

### Common Input Pattern
```jsx
<input
  className="
    w-full px-4 py-2
    border border-gray-300 rounded-lg
    focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent
    placeholder:text-gray-400
    text-gray-900
  "
  placeholder="Enter email"
/>
```

---

## 17. Shadows & Effects

### Box Shadow
```jsx
shadow-sm       // very subtle
shadow          // default
shadow-md       // medium
shadow-lg       // large
shadow-xl       // extra large
shadow-2xl      // dramatic
shadow-inner    // inward shadow
shadow-none

// Colored shadows (v4)
shadow-indigo-500/50   // indigo shadow at 50% opacity
```

### Opacity
```jsx
opacity-0     // fully transparent
opacity-25
opacity-50
opacity-75
opacity-100   // fully visible

// Using hover
hover:opacity-80
```

### Blur & Backdrop Blur
```jsx
blur-sm  blur  blur-md  blur-lg  blur-xl  blur-2xl

// Blur the background behind an element (frosted glass)
backdrop-blur-sm  backdrop-blur  backdrop-blur-md  backdrop-blur-xl
```

### Frosted Glass Card
```jsx
<div className="
  backdrop-blur-md
  bg-white/20
  border border-white/30
  rounded-2xl
  p-6
  shadow-xl
">
  Glassmorphism card
</div>
```

---

## 18. Arbitrary Values

When Tailwind's built-in scale doesn't have the exact value you need, use square bracket syntax:

```jsx
// Arbitrary size
w-[350px]   h-[72px]

// Arbitrary color
bg-[#ff6b6b]   text-[#1a1a2e]

// Arbitrary spacing
mt-[13px]   px-[22px]

// Arbitrary font size
text-[15px]  text-[2.3rem]

// Arbitrary line height
leading-[1.8]

// Arbitrary z-index
z-[999]

// CSS variables as values
bg-[var(--brand-color)]

// Complex values with spaces → use underscores
bg-[linear-gradient(to_right,#667eea,#764ba2)]
grid-cols-[1fr_2fr_1fr]

// Arbitrary properties (for things Tailwind doesn't cover)
[mask-image:linear-gradient(to_bottom,black,transparent)]
[content-visibility:auto]
```

---

## 19. Custom Theme with @theme

This is the v4 way to define your design tokens — all inside your CSS file.

```css
/* src/index.css */
@import "tailwindcss";

@theme {
  /* Custom colors */
  --color-brand:        #6366f1;
  --color-brand-dark:   #4f46e5;
  --color-surface:      #0f0f0f;
  --color-accent:       #f472b6;

  /* Custom fonts */
  --font-sans: "Inter", sans-serif;
  --font-display: "Cal Sans", "Inter", sans-serif;
  --font-mono: "JetBrains Mono", monospace;

  /* Custom spacing */
  --spacing-18: 4.5rem;
  --spacing-22: 5.5rem;

  /* Custom border radius */
  --radius-4xl: 2rem;

  /* Custom breakpoints */
  --breakpoint-xs: 30rem;   /* 480px */
  --breakpoint-3xl: 110rem; /* 1760px */
}
```

Once defined, use them exactly like built-in values:
```jsx
<div className="bg-brand text-white rounded-4xl px-18">
  Custom branded section
</div>
```

### Dark Mode Token Overrides
```css
@import "tailwindcss";

@custom-variant dark (&:where(.dark, .dark *));

@theme {
  --color-bg: #ffffff;
  --color-text: #111827;
  --color-card: #f9fafb;
}

.dark {
  --color-bg: #0a0a0a;
  --color-text: #f9fafb;
  --color-card: #1a1a1a;
}
```

```jsx
// Tokens auto-switch between light/dark
<div className="bg-[var(--color-bg)] text-[var(--color-text)]">...</div>
```

---

## 20. @apply — Extracting Repeated Patterns

When you find yourself copying the same set of classes everywhere, extract them with `@apply`:

```css
/* src/index.css */
@import "tailwindcss";

@layer components {
  .btn {
    @apply px-4 py-2 rounded-lg font-semibold text-sm transition-colors duration-200 cursor-pointer;
  }

  .btn-primary {
    @apply bg-indigo-600 text-white hover:bg-indigo-700 active:bg-indigo-800;
  }

  .btn-ghost {
    @apply bg-transparent text-indigo-600 border border-indigo-600 hover:bg-indigo-50;
  }

  .input-base {
    @apply w-full px-4 py-2 border border-gray-300 rounded-lg
           focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:border-transparent
           text-gray-900 placeholder:text-gray-400;
  }

  .card {
    @apply bg-white rounded-2xl shadow-md overflow-hidden border border-gray-100;
  }
}
```

```jsx
// Now use the clean class names
<button className="btn btn-primary">Save</button>
<button className="btn btn-ghost">Cancel</button>
<input className="input-base" placeholder="Email" />
<div className="card p-6">...</div>
```

> **Use `@apply` sparingly.** Prefer React components for reusable UI. Only use `@apply` for truly low-level patterns like base input/button styles that apply to plain HTML elements.

---

## 21. Group & Peer Variants

### Group — Style children based on parent state
Apply the `group` class to the parent, then use `group-hover:`, `group-focus:` etc. on children:

```jsx
<div className="group relative flex items-center gap-3 p-4 rounded-xl hover:bg-indigo-50 cursor-pointer">
  {/* Icon changes color when parent is hovered */}
  <div className="w-10 h-10 bg-indigo-100 group-hover:bg-indigo-500 rounded-lg transition-colors flex items-center justify-center">
    <Icon className="text-indigo-500 group-hover:text-white" />
  </div>

  {/* Text changes when parent is hovered */}
  <div>
    <p className="font-medium text-gray-900 group-hover:text-indigo-600">Project Name</p>
    <p className="text-sm text-gray-500">3 files updated</p>
  </div>

  {/* Arrow only appears on hover */}
  <span className="ml-auto opacity-0 group-hover:opacity-100 transition-opacity text-indigo-500">→</span>
</div>
```

### Named Groups (when you have nested groups)
```jsx
<div className="group/card hover:shadow-xl">
  <div className="group/inner p-4">
    <span className="group-hover/card:text-indigo-600">
      Changes on card hover
    </span>
    <span className="group-hover/inner:underline">
      Changes on inner hover
    </span>
  </div>
</div>
```

### Peer — Style siblings based on another sibling's state
```jsx
<div>
  <input
    id="email"
    className="peer w-full border border-gray-300 rounded-lg px-4 py-2
               focus:ring-2 focus:ring-indigo-500 focus:border-transparent
               invalid:border-red-500"
    type="email"
    required
  />
  {/* Error message — only visible when input is invalid */}
  <p className="hidden peer-invalid:block text-sm text-red-500 mt-1">
    Please enter a valid email address.
  </p>
</div>
```

---

## 22. Container Queries

Unlike media queries (viewport-based), container queries let you style an element based on its **parent container's** size. Perfect for reusable React components.

```css
/* index.css */
@import "tailwindcss";
```

```jsx
{/* Mark a parent as a container */}
<div className="@container">
  <div className="
    flex flex-col
    @md:flex-row       // when container ≥ 28rem, switch to row
    @lg:gap-8          // when container ≥ 32rem, increase gap
    gap-4
  ">
    <img className="w-full @md:w-48 rounded-xl" src={...} />
    <div>
      <h3 className="text-base @lg:text-xl font-bold">Title</h3>
      <p className="text-sm @lg:text-base text-gray-600">Description</p>
    </div>
  </div>
</div>
```

This component will reflow based on where it's placed in the layout, not the viewport. A sidebar card and a full-width card can both use this same component.

### Named Containers (v4.3)
```jsx
<div className="@container/card h-[300px]">
  {/* Responds to card container's HEIGHT (new in v4.3) */}
  <p className="text-slate-600 @[block-size>200px]/card:text-emerald-600">
    Color changes when card is tall enough
  </p>
</div>
```

---

## 23. v4.3 New Features

Here's what's new in **v4.3.x** (the version family you're using at v4.3.1):

### 1. Scrollbar Styling (native, no plugin!)
Previously you needed the `tailwind-scrollbar` plugin. Now it's built-in:

```jsx
<div className="
  h-64 overflow-y-scroll
  scrollbar-thin
  scrollbar-thumb-indigo-500
  scrollbar-track-gray-100
  dark:scrollbar-thumb-indigo-400
  dark:scrollbar-track-gray-800
">
  Long content here...
</div>
```

### 2. More Logical Property Utilities
RTL-safe utilities that work for both left-to-right and right-to-left layouts:

```jsx
// Padding (block-start, block-end, inline-start, inline-end)
<div className="pbs-4 pbe-8 pis-6 pie-6">...</div>
// = padding-block-start: 1rem, padding-block-end: 2rem, padding-inline: 1.5rem

// Margin
<div className="mbs-4 mbe-8">...</div>

// Logical inset (positioning — replaces start-* and end-*)
<div className="absolute inset-s-0 inset-e-4 inset-bs-2 inset-be-8">...</div>
// Note: start-* / end-* still work but are deprecated in favor of inset-s-* / inset-e-*
```

### 3. Zoom Utilities
```jsx
<div className="zoom-50">   Zoomed out to 50%</div>
<div className="zoom-100">  Normal</div>
<div className="zoom-125">  Zoomed in to 125%</div>
<div className="zoom-150">  Zoomed in to 150%</div>

// Arbitrary
<div className="zoom-[1.35]">...</div>
```
`zoom` scales the element visually without affecting layout flow (unlike `scale`).

### 4. Tab-Size Utilities
Controls the rendered width of tab characters (useful in `<pre>` code blocks):
```jsx
<pre className="tab-2 bg-gray-900 text-white p-4 rounded-lg">
  {code}
</pre>

// Available: tab-2, tab-4, tab-8 or tab-[n]
```

### 5. Font Feature Utilities
Control OpenType font features:
```jsx
// Enable tabular numbers (all digits same width — great for data)
<span className="tabular-nums">12,345.67</span>

// Low-level OpenType features
<span className="font-features-['ss01']">Stylistic set 1</span>
<span className="font-features-['liga','calt']">Ligatures</span>
```

### 6. Stacked & Compound @variant Support
You can now write stacked variants directly in CSS:
```css
@utility btn-highlight {
  /* Applies when hovered AND focused */
  @variant hover:focus {
    background-color: var(--color-indigo-600);
  }

  /* Applies when hovered OR focused */
  @variant hover, focus {
    outline: 2px solid var(--color-indigo-500);
  }
}
```

---

## 24. React-Specific Patterns

### Pattern 1 — Conditional Classes with Template Literals
```jsx
function Badge({ status }) {
  return (
    <span
      className={`
        inline-flex items-center px-2.5 py-1 rounded-full text-xs font-medium
        ${status === 'active'  ? 'bg-green-100 text-green-700'  : ''}
        ${status === 'pending' ? 'bg-yellow-100 text-yellow-700' : ''}
        ${status === 'error'   ? 'bg-red-100 text-red-700'       : ''}
      `}
    >
      {status}
    </span>
  );
}
```

### Pattern 2 — clsx (Recommended Library)
Template literals get messy fast. Use `clsx` for cleaner conditional classes:

```bash
npm install clsx
```

```jsx
import clsx from 'clsx';

function Button({ children, variant, size, disabled }) {
  return (
    <button
      className={clsx(
        // Always applied
        'inline-flex items-center justify-center rounded-lg font-semibold transition-colors',
        // Size variants
        size === 'sm' && 'px-3 py-1.5 text-sm',
        size === 'md' && 'px-4 py-2 text-sm',
        size === 'lg' && 'px-6 py-3 text-base',
        // Color variants
        variant === 'primary' && 'bg-indigo-600 text-white hover:bg-indigo-700',
        variant === 'danger'  && 'bg-red-600 text-white hover:bg-red-700',
        variant === 'ghost'   && 'bg-transparent border border-gray-300 text-gray-700 hover:bg-gray-50',
        // State
        disabled && 'opacity-50 cursor-not-allowed pointer-events-none'
      )}
      disabled={disabled}
    >
      {children}
    </button>
  );
}
```

### Pattern 3 — Variants Object Map
Good for components with multiple style variants:
```jsx
const buttonVariants = {
  solid:   'bg-indigo-600 text-white hover:bg-indigo-700',
  outline: 'border-2 border-indigo-600 text-indigo-600 hover:bg-indigo-50',
  ghost:   'text-indigo-600 hover:bg-indigo-50',
  danger:  'bg-red-500 text-white hover:bg-red-600',
};

const buttonSizes = {
  sm: 'px-3 py-1.5 text-xs',
  md: 'px-4 py-2 text-sm',
  lg: 'px-6 py-3 text-base',
};

function Button({ variant = 'solid', size = 'md', children, ...props }) {
  return (
    <button
      className={`rounded-lg font-semibold transition-colors ${buttonVariants[variant]} ${buttonSizes[size]}`}
      {...props}
    >
      {children}
    </button>
  );
}
```

### Pattern 4 — twMerge (Handling Conflicting Classes)
When you need to override Tailwind classes from the outside:

```bash
npm install tailwind-merge
```

```jsx
import { twMerge } from 'tailwind-merge';
import clsx from 'clsx';

// Combine clsx + twMerge (common pattern)
function cn(...inputs) {
  return twMerge(clsx(inputs));
}

// Usage — parent can override child classes cleanly
function Card({ className, children }) {
  return (
    <div className={cn('bg-white rounded-xl p-4 shadow', className)}>
      {children}
    </div>
  );
}

// Parent overrides bg-white with bg-gray-900 — twMerge handles the conflict
<Card className="bg-gray-900 text-white">Dark card</Card>
```

### Pattern 5 — Tailwind + CSS Modules (optional)
If you want to scope some complex styles, you can mix Tailwind with CSS Modules:
```jsx
import styles from './Hero.module.css';

function Hero() {
  return (
    <section className={`${styles.hero} flex items-center justify-center min-h-screen`}>
      <h1 className="text-6xl font-black text-white">Hello</h1>
    </section>
  );
}
```

```css
/* Hero.module.css */
.hero {
  background: radial-gradient(ellipse at 50% 0%, #6366f1 0%, #0f0f0f 70%);
}
```

---

## 25. Tools, Extensions & Resources

### Must-Have VS Code Extensions

| Extension | Purpose |
|---|---|
| **Tailwind CSS IntelliSense** (official) | Autocomplete, hover preview, linting |
| **Prettier - Code formatter** | Auto-sort Tailwind classes with `prettier-plugin-tailwindcss` |

#### Setup Prettier class sorting
```bash
npm install -D prettier prettier-plugin-tailwindcss
```
```json
// .prettierrc
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```
This auto-sorts your classes into a consistent order on save — essential for teamwork and readability.

### Browser Tools

| Tool | Link |
|---|---|
| **Tailwind Play** — browser sandbox, instant preview | https://play.tailwindcss.com |
| **Tailwind CSS Cheat Sheet** — searchable reference | https://nerdcave.com/tailwind-cheat-sheet |
| **Tailwind Components** — free component collection | https://tailwindcomponents.com |

### Official Documentation Pages You'll Visit Often

| Page | Link |
|---|---|
| Docs home | https://tailwindcss.com/docs |
| Installation (Vite) | https://tailwindcss.com/docs/installation/using-vite |
| Responsive design | https://tailwindcss.com/docs/responsive-design |
| Dark mode | https://tailwindcss.com/docs/dark-mode |
| Customizing theme | https://tailwindcss.com/docs/theme |
| Arbitrary values | https://tailwindcss.com/docs/adding-custom-styles |
| Container queries | https://tailwindcss.com/docs/container-queries |
| Upgrade guide (v3→v4) | https://tailwindcss.com/docs/upgrade-guide |

### Component Libraries Built on Tailwind

| Library | What it is |
|---|---|
| **shadcn/ui** | Copy-paste components (React), v4 supported | https://ui.shadcn.com |
| **daisyUI** | Plugin that adds component classes | https://daisyui.com |
| **Headless UI** | Unstyled accessible components from Tailwind team | https://headlessui.com |
| **Tailwind Plus** | Official premium UI blocks | https://tailwindcss.com/plus |
| **Flowbite** | Tailwind components + React library | https://flowbite.com |
| **HyperUI** | Free open-source components | https://www.hyperui.dev |

### Utility Libraries for React

| Package | Use case |
|---|---|
| `clsx` | Conditional class joining |
| `tailwind-merge` | Merging/overriding conflicting classes |
| `class-variance-authority` (CVA) | Type-safe variant management |

---

## Quick Reference Cheat Sheet

```
RESPONSIVE   sm: md: lg: xl: 2xl: max-md: md:max-lg:
DARK MODE    dark:
HOVER        hover:    FOCUS focus:    FOCUS-VISIBLE focus-visible:
ACTIVE       active:   DISABLED disabled:   CHECKED checked:
GROUP        group / group-hover:  group-focus:  group-[named]/name:
PEER         peer / peer-hover:   peer-invalid:  peer-checked:
NTH          nth-[2n+1]: nth-child-* first: last: odd: even:

DISPLAY      block inline-block inline flex inline-flex grid hidden

FLEX         flex flex-col flex-row flex-wrap gap-*
             justify-* items-* flex-1 flex-none shrink-0 grow

GRID         grid grid-cols-* grid-rows-* col-span-* row-span-* gap-*

SPACING      p-* px-* py-* pt-* pr-* pb-* pl-*  m-* mx-* my-* mt-* ...

SIZING       w-* h-* size-* min-w-* max-w-* min-h-* max-h-*

TYPOGRAPHY   text-* font-* leading-* tracking-* line-clamp-* truncate

COLORS       text-{color}-{shade}  bg-{color}-{shade}  border-{color}-{shade}
             Opacity: bg-indigo-500/50   Arbitrary: bg-[#ff6b6b]

BORDERS      border border-* border-{side}-* rounded-*

SHADOWS      shadow shadow-sm shadow-md shadow-lg shadow-xl shadow-inner

EFFECTS      opacity-* blur-* backdrop-blur-* scale-* rotate-*

TRANSITIONS  transition transition-all duration-* ease-* delay-*

ARBITRARY    w-[350px] text-[15px] bg-[#ff6b6b] z-[999]
```

---

*Notes compiled for Pxtitan Studio | Tailwind CSS v4.3.1 | React + Vite stack*
