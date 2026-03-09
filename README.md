# AITOOLS.PRO — 2026 Field Guide for Technical Professionals

> The definitive technical reference for engineers and architects building with AI in 2026.  
> 142+ tools reviewed, benchmarked, and ranked — no hype, no ads, no sponsorships.

---

## Table of Contents

- [Overview](#overview)
- [Live Demo](#live-demo)
- [Features](#features)
- [Project Structure](#project-structure)
- [Sections](#sections)
- [Tool Data Schema](#tool-data-schema)
- [Guides Data Schema](#guides-data-schema)
- [UI Components](#ui-components)
- [JavaScript API](#javascript-api)
- [Design System](#design-system)
- [Typography](#typography)
- [Keyboard Shortcuts](#keyboard-shortcuts)
- [Browser Support](#browser-support)
- [Adding New Tools](#adding-new-tools)
- [Adding New Guides](#adding-new-guides)
- [Customization](#customization)
- [Performance Notes](#performance-notes)
- [Roadmap](#roadmap)
- [License](#license)

---

## Overview

**AITOOLS.PRO** is a fully self-contained, single-file SPA (Single Page Application) built with vanilla HTML, CSS, and JavaScript — zero dependencies, zero frameworks, zero build tools required. Open `index.html` in any modern browser and it runs.

The design aesthetic is intentionally bold: dark terminal theme (Bloomberg Terminal meets editorial magazine), lime-green and cyan accent colors, JetBrains Mono monospaced type, CRT scan-line texture, and a custom dual-ring cursor. It is purpose-built to feel like a professional technical tool, not a marketing brochure.

**Edition:** Q1 2026  
**Last Updated:** February 2026  
**Tools Indexed:** 24 curated (142+ referenced)  
**Guides:** 5 in-depth technical guides  

---

## Live Demo

Since this is a single static HTML file, there is nothing to install or deploy. Just open it:

```bash
# Option 1 — Open directly
open index.html

# Option 2 — Serve locally with Python
python3 -m http.server 8080
# Then visit http://localhost:8080

# Option 3 — Serve locally with Node
npx serve .
# Then visit http://localhost:3000
```

No internet connection is required except for loading Google Fonts (JetBrains Mono, DM Serif Display, Outfit). If offline, the browser will fall back to system monospace and serif fonts gracefully.

---

## Features

### Navigation
- Fixed topbar with smooth-scroll navigation links
- Active section auto-highlighting as you scroll via `IntersectionObserver`
- Reading progress bar (1px lime line) pinned below the topbar
- Back-to-top button that appears after scrolling 400px

### Global Search (`⌘K`)
- Full-text search modal triggered by `⌘K` / `Ctrl+K` or the Search button
- Searches across tool names, descriptions, and categories in real time
- Keyboard navigable (`↑↓` to navigate, `↵` to open, `ESC` to close)
- Click outside the modal to dismiss

### Live Ticker
- Auto-scrolling ticker bar showing all tools with their category and rating
- Pure CSS animation (`@keyframes tickScroll`), infinitely looping
- Duplicated set of items ensures seamless infinite scroll

### Hero Section
- Full-viewport landing with serif + ghost typography
- "Hot Right Now" leaderboard sidebar (top 5 by rating, clickable)
- Category quick-filter pills with tool counts, color-coded by category
- Four key stats: total tools, guides, categories, edition

### Tools Index
- **24 curated tools** rendered as interactive table rows
- **Live search** filter — type to filter by name, description, or category keyword
- **Category tabs**: All / LLMs / Code / Image / Data / Agents / DevOps
- **Sort toggle**: by rating ascending/descending
- **Tool count** updates live in the section bar
- **Click any row** → opens full detail modal (overview, category, score, best-for)
- Right sidebar with:
  - Live index stats (total, avg rating, free/open %, edition)
  - Category bar chart (relative distribution)
  - Top Rated quick list (top 4 by score)

### Guides & Tutorials
- 5 complete technical guides with full written content
- Featured guide (RAG Pipelines) displayed prominently
- 4 secondary guide cards, each color-coded by discipline
- Click "Read →" or any guide card → opens rich content modal

### Side-by-Side Comparison Tables
- 3 tabbed comparison tables:
  1. **LLM Platforms** — 6 models compared across context window, API, fine-tuning, functions, vision, open source, pricing
  2. **Code Assistants** — 6 tools compared across IDE support, codebase context, multi-file edit, test gen, PR review, free tier
  3. **Agent Frameworks** — 5 frameworks compared across language, multi-agent, memory, tool use, observability, license
- Visual indicators: ✓ (yes / green), ~ (partial / yellow), ✗ (no / red)

### Newsletter
- Two-column layout with headline and subscription form
- Email validation (must contain `@`)
- Success/error feedback via toast notification
- Feature list showing newsletter contents

### Footer
- Brand column + 4 link columns (Tools, Guides, Compare, About)
- All footer links are functional (scroll to section, open guide modal, or show toast)
- Copyright and policy links

### Modals
- Single shared modal used for both tool details and guide content
- Animated entry (`scale + translateY` → normal)
- Close via: ✕ button, `ESC` key, or clicking the backdrop overlay
- Body scroll locked while modal is open

### Toast Notifications
- Bottom-right toast with lime accent border
- 3.2 second auto-dismiss
- Used for: newsletter subscription, "Visit Website", "Add to Toolkit", footer links, Pro CTA

### Custom Cursor
- Dual-ring cursor: small lime dot + larger ghost ring
- On `mousedown`: dot expands to cyan, ring enlarges
- Uses `mix-blend-mode: screen` for neon glow effect
- The native cursor is hidden (`cursor: none`) site-wide

---

## Project Structure

```
aitools-pro/
│
├── index.html          ← The entire application (single file)
└── README.md           ← This file
```

All HTML, CSS, and JavaScript live in `index.html`. There are no external dependencies, no build steps, no package.json, no node_modules. The entire application is approximately 1,745 lines.

**External resources loaded (CDN):**
- Google Fonts: `JetBrains Mono`, `DM Serif Display`, `Outfit`

That's it.

---

## Sections

The page is divided into the following named sections, each with an `id` for anchor navigation:

| ID | Label | Description |
|---|---|---|
| `#home` | Hero | Landing section with headline, stats, hot list, category pills |
| `#tools` | Tools Index | Filterable, searchable table of all 24 tools |
| `#guides` | Guides | Featured guide + 4 guide cards |
| `#compare` | Compare | Tabbed comparison tables |
| `#newsletter` | Newsletter | Email subscription form |
| `footer` | Footer | Links, brand, copyright |

---

## Tool Data Schema

All tools are stored as plain JavaScript objects in the `TOOLS` array inside `<script>`:

```js
{
  id:     Number,    // Unique integer ID
  n:      String,    // Tool name
  cat:    String,    // Category: 'llm' | 'code' | 'image' | 'data' | 'agent' | 'devops'
  icon:   String,    // Emoji icon displayed in the row
  d:      String,    // Short description (1–2 sentences, shown in row and modal)
  b:      Array,     // Badge array: any combination of 'free' | 'paid' | 'open' | 'new' | 'hot'
  r:      Number,    // Rating (e.g. 4.7) — used for sorting and color coding
}
```

**Example:**
```js
{
  id: 1,
  n: "Claude 4 Sonnet",
  cat: "llm",
  icon: "🧠",
  d: "Anthropic's flagship. 200K context, exceptional reasoning, strong coding & analysis.",
  b: ["paid", "hot"],
  r: 4.9
}
```

**Badge Colors:**

| Badge | Background | Text |
|---|---|---|
| `free` | green-tinted | `#3dd68c` |
| `paid` | orange-tinted | `#ff8c69` |
| `open` | blue-tinted | `#4d9fff` |
| `new` | lime-tinted | `#c8f135` |
| `hot` | red-tinted | `#ff6b7a` |

**Rating Color Coding:**

| Score | Color | Variable |
|---|---|---|
| ≥ 4.7 | Green | `r-high` |
| ≥ 4.4 | Lime | `r-med` |
| < 4.4 | Orange | `r-low` |

---

## Guides Data Schema

All guides are stored as an object map in `GUIDES`:

```js
{
  [key: String]: {
    title: String,   // Full guide title (shown in modal header)
    sup:   String,   // Supertitle metadata line (e.g. "Featured Guide · 45 min · Advanced")
    body:  String,   // Full HTML content rendered in modal body
  }
}
```

**Available Guide Keys:**

| Key | Title |
|---|---|
| `RAG` | Building a Production RAG Pipeline in 2026 |
| `CodeReview` | LLM-Assisted Code Review: The 2026 Workflow |
| `TextSQL` | Text-to-SQL with LLMs: Benchmarks & Best Practices |
| `FineTune` | Fine-tuning vs. RAG vs. Prompt Engineering |
| `MultiAgent` | Multi-Agent Systems: Architecture Patterns for Production |

Guide body HTML supports: `<h4>`, `<p>`, `<ul>`, `<li>`, `<strong>`, `<code>`.

---

## UI Components

### Buttons

```html
<!-- Primary CTA (lime filled) -->
<button class="btn-lime">Browse All Tools →</button>

<!-- Ghost / secondary -->
<button class="btn-ghost">Read Guides</button>
```

### Badges

```html
<span class="tbdg tbdg-free">free</span>
<span class="tbdg tbdg-paid">paid</span>
<span class="tbdg tbdg-open">open</span>
<span class="tbdg tbdg-new">new</span>
<span class="tbdg tbdg-hot">hot</span>
```

### Section Bar

```html
<div class="section-bar">
  <div class="section-bar-title">Section Title</div>
  <div class="section-bar-meta">Metadata / count</div>
</div>
```

### Toast (programmatic)

```js
showToast('Your message here');
// Auto-dismisses after 3.2 seconds
```

### Modal (programmatic)

```js
// Open a tool by ID
openTool(1);

// Open a guide by key
openGuide('RAG');

// Close modal
closeModal();
```

### Reveal Animation

Add `.reveal` to any element and it will fade up when scrolled into view:

```html
<div class="reveal">
  <!-- Content animates in on scroll -->
</div>
```

---

## JavaScript API

All functions are global (attached to `window` implicitly). Key functions:

| Function | Description |
|---|---|
| `renderTools(q)` | Render tools list filtered by query string `q` and current category |
| `filterCat(cat, btn)` | Set active category filter, re-render tools |
| `filterTools()` | Read search input and re-render (called on `oninput`) |
| `toggleSort()` | Toggle sort direction (rating ↑ / ↓), re-render |
| `renderSidebar()` | Render hot list, category pills, bar chart, top rated |
| `openTool(id)` | Open tool detail modal by tool ID |
| `openGuide(key)` | Open guide modal by guide key string |
| `closeModal()` | Close the main modal |
| `openSearch()` | Open the global search modal |
| `closeSearch()` | Close the global search modal |
| `renderSearch(q)` | Render search results for query `q` |
| `switchCmpTab(id, btn)` | Switch comparison table tab |
| `subscribe()` | Validate and handle newsletter subscription |
| `showToast(msg)` | Show a bottom-right toast notification |
| `setNav(el)` | Manually set active nav link |
| `scrollTo(hash)` | Smooth scroll to a CSS selector |
| `hexToRgb(hex)` | Convert `#rrggbb` to `r,g,b` string for CSS `rgba()` |

---

## Design System

All design tokens are CSS custom properties on `:root`:

```css
:root {
  /* Backgrounds (dark to light) */
  --bg:   #080c0f;   /* Page background */
  --bg1:  #0d1318;   /* Topbar, sidebar panels */
  --bg2:  #111820;   /* Table headers, hover states */
  --bg3:  #16202a;   /* Unused reserve */

  /* Accent Colors */
  --lime:   #c8f135;  /* Primary accent — CTA, active states, highlights */
  --cyan:   #00e5cc;  /* Secondary accent — hover states, guide borders */
  --orange: #ff6b35;  /* Tertiary — guide card accent, low ratings */
  --blue:   #4d9fff;  /* Quaternary — guide card accent, badges */
  --purple: #b06cff;  /* Guide card accent */
  --red:    #ff3d57;  /* Negative — ✗ symbols, error states */

  /* Neutral Text */
  --text:  #c8d8e8;   /* Primary body text */
  --text2: #7a9bb5;   /* Secondary / dimmed text */
  --text3: #3f5568;   /* Tertiary / very dimmed labels */
  --white: #eef4fa;   /* Headings, high-emphasis text */

  /* Structure */
  --muted: #4a5568;   /* Muted text */
  --dim:   #2a3540;   /* Subtle borders, bar backgrounds */
  --border: 1px solid #1e2f3d;  /* Standard border shorthand */

  /* Glows */
  --glow-lime: 0 0 20px rgba(200,241,53,0.25);
  --glow-cyan: 0 0 20px rgba(0,229,204,0.25);
}
```

---

## Typography

Three typefaces are used, each with a distinct role:

| Font | Source | Role | Weights Used |
|---|---|---|---|
| **JetBrains Mono** | Google Fonts | UI chrome: nav, labels, badges, data, metadata, buttons | 300, 400, 500, 700, 800 |
| **DM Serif Display** | Google Fonts | Display headings: hero title, section headings, guide titles | Regular, Italic |
| **Outfit** | Google Fonts | Body copy: descriptions, guide prose, paragraph text | 300, 400, 500, 600 |

CSS variables:
```css
--mono:  'JetBrains Mono', monospace;
--serif: 'DM Serif Display', serif;
--body:  'Outfit', sans-serif;
```

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `⌘K` / `Ctrl+K` | Open global search modal |
| `ESC` | Close any open modal or search |
| `↑` / `↓` | Navigate search results (visual, click to select) |

---

## Browser Support

Tested and functional in:

| Browser | Version | Status |
|---|---|---|
| Chrome | 110+ | ✓ Full support |
| Firefox | 110+ | ✓ Full support |
| Safari | 16+ | ✓ Full support |
| Edge | 110+ | ✓ Full support |
| Safari iOS | 16+ | ✓ Full support (cursor hidden on touch) |
| Chrome Android | 110+ | ✓ Full support (cursor hidden on touch) |

**Features used:**
- CSS Custom Properties (variables)
- CSS `mix-blend-mode`
- CSS `backdrop-filter` (modal blur)
- CSS `clip-path`
- `IntersectionObserver` API
- CSS `@keyframes` animations
- ES6+ JavaScript (arrow functions, template literals, destructuring, spread, `const`/`let`)
- CSS Grid and Flexbox

No polyfills are included. IE11 is not supported.

---

## Adding New Tools

Open `index.html` and find the `TOOLS` array inside the `<script>` tag (approximately line 1460). Add a new object following the schema:

```js
{
  id: 25,                          // Next sequential integer
  n: "Your Tool Name",             // Tool name
  cat: "llm",                      // Category (see valid values below)
  icon: "🔮",                      // Emoji
  d: "One to two sentence description of what the tool does and who it's for.",
  b: ["paid", "new"],              // Badge array
  r: 4.5                           // Rating out of 5.0
},
```

**Valid `cat` values:**

| Value | Label | Color |
|---|---|---|
| `llm` | LLMs | `#e8380d` |
| `code` | Code | `#1a6bff` |
| `image` | Image | `#8b2be2` |
| `data` | Data | `#0da86e` |
| `agent` | Agents | `#e87d0d` |
| `devops` | DevOps | `#d4a017` |

The tool will automatically appear in:
- The tools table (sorted by rating by default)
- The global search
- The category filter tabs
- The sidebar stats and bar chart
- The ticker (if added to the ticker HTML)
- The top-rated / hot lists if rating is high enough

To add to the **ticker**, find the `<div id="ticker">` block and add:
```html
<div class="ticker-item">
  <span class="ti-name">Your Tool Name</span>
  <span class="ti-val">LLM</span>
  <span class="ti-up">▲ 4.5</span>
</div>
```
Add it in both the first and second halves of the ticker (the second half is the duplicate for seamless looping).

---

## Adding New Guides

Find the `GUIDES` object in the `<script>` tag (approximately line 1480) and add a new key:

```js
GUIDES.MyGuide = {
  title: "Your Guide Title Here",
  sup: "Category · X min read · Difficulty",
  body: `
    <h4>Section Heading</h4>
    <p>Paragraph content here.</p>
    <h4>Another Section</h4>
    <ul>
      <li>List item one</li>
      <li>List item two</li>
    </ul>
    <h4>Code Example</h4>
    <p>Use <code>inline code</code> for technical terms.</p>
  `
};
```

Then add a card in the HTML guides grid:

```html
<div class="guide-card gc-cyan">
  <div class="gc-tag" style="color:var(--cyan)">Category · X min</div>
  <div class="gc-title">Your Guide Title Here</div>
  <div class="gc-desc">Short one-line description of the guide.</div>
  <div class="gc-foot">
    <span>Difficulty</span>
    <button class="gc-read" onclick="openGuide('MyGuide')">Read →</button>
  </div>
</div>
```

**Available guide card accent classes:**

| Class | Color |
|---|---|
| `gc-cyan` | `--cyan` (#00e5cc) |
| `gc-orange` | `--orange` (#ff6b35) |
| `gc-purple` | `--purple` (#b06cff) |
| `gc-blue` | `--blue` (#4d9fff) |

---

## Customization

### Change the Brand Name

Find and replace all instances of `AITOOLS.PRO` in the HTML (topbar logo, footer logo, `<title>` tag, footer tagline).

### Change Accent Color

Replace `--lime: #c8f135` in `:root` with your desired hex color. All primary accent elements (CTA buttons, active states, progress bar, cursor dot, section bar indicators, glow effects) will update automatically.

### Disable the Custom Cursor

Remove `cursor: none` from the `body` rule and delete the `#cur` and `#cur2` elements from the HTML, along with their associated CSS and JavaScript.

### Disable the Scan-Line Overlay

Remove the `body::before` CSS rule block.

### Change Fonts

Replace the Google Fonts `<link>` tag with your desired fonts and update the three CSS variables: `--mono`, `--serif`, `--body`.

### Add a New Comparison Tab

1. Add a new `<button class="cmp-tab">` element in `.cmp-tabs`
2. Add a new `<div class="cmp-pane" id="cmp-yourkey">` with your table
3. Call `switchCmpTab('yourkey', this)` in the button's `onclick`

---

## Performance Notes

- **Single HTTP request** for the HTML (plus Google Fonts, which are cached)
- No JavaScript framework — no React, Vue, Angular, Svelte overhead
- No CSS framework — no Tailwind or Bootstrap downloaded
- All animations are CSS-driven (`@keyframes`, `transition`) — zero JS animation loops
- `IntersectionObserver` used for scroll reveals (not scroll event listeners)
- Scroll progress bar uses a passive scroll event listener
- The `renderTools()` function does a full re-render on every filter/search change. For 24 tools this is imperceptibly fast. For 1000+ tools, consider virtualizing the list.
- Fonts load asynchronously; fallback fonts are defined in the CSS stack

**Approximate file size:** ~85KB unminified HTML (including all CSS and JS inline).  
Minified estimate: ~42KB. Gzipped estimate: ~12KB.

---

## Roadmap

Features planned for future editions:

- [ ] **Persistent toolkit** — save tools to a local "My Toolkit" list via `localStorage`
- [ ] **Tool submission form** — GitHub-integrated form for community submissions
- [ ] **Expanded database** — all 142+ referenced tools fully reviewed and indexed
- [ ] **Version history** — changelog modal showing what changed each week
- [ ] **RSS feed** — auto-generated feed for new tool additions
- [ ] **Dark/light theme toggle** — optional light editorial mode
- [ ] **Printer-friendly CSS** — clean `@media print` stylesheet
- [ ] **Vector DB comparison tab** — dedicated table for Pinecone, Weaviate, Qdrant, pgvector
- [ ] **API access** — public JSON endpoint for the tools database
- [ ] **Pro tier** — advanced filters, export to CSV, email alerts for new tools in category

---

## License

MIT License

*Built with zero AI hype. Just HTML, CSS, and JavaScript.*  
*AITOOLS.PRO — Q1 2026 Edition*
