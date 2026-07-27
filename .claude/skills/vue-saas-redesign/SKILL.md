---
name: vue-saas-redesign
description: Guidelines for redesigning this Vue 3 app's UI into a modern SaaS-style interface with a left vertical sidebar nav (replacing the top nav bar), consistent spacing, and a polished professional look. Use when asked to redesign, restyle, modernize, or overhaul the app shell/navigation/layout.
---

# Vue SaaS Redesign

Converts this app's top-nav layout into a modern SaaS app shell: fixed left sidebar
navigation + top utility bar + content area, with a consistent spacing/typography
system applied across views. This skill defines the target design and the process
for getting there — it does not replace project rules below, it operates within them.

**MANDATORY:** Any `.vue` file created or significantly modified as part of this
redesign must be delegated to the **vue-expert** subagent, per the repo's
`CLAUDE.md`. This skill supplies the design spec that agent should implement
against — it doesn't substitute for the delegation rule.

## Current State (baseline to change)

- `client/src/App.vue` renders a sticky `<header class="top-nav">` with a horizontal
  `nav-tabs` row, and global styles live in a single unscoped `<style>` block in
  that file.
- Nav items are `router-link`s for: Overview (`/`), Inventory, Orders, Finance
  (`/spending`), Demand Forecast (`/demand`), Reports — labels sourced from
  `useI18n()` (`t('nav.*')`), so keep using `t()` for any nav label rather than
  hardcoding English strings.
- `<FilterBar />` renders directly below the header as a full-width bar (Time
  Period / Warehouse / Category / Order Status selects) — it is filter UI, not
  navigation, and must stay visually distinct from the sidebar.
- `ProfileMenu` and `LanguageSwitcher` currently live in the top-nav's right side.
- Views (`client/src/views/*.vue`) build their own `.page-header`, `.stats-grid`,
  `.card`, `.badge` etc. using the global classes defined in `App.vue`'s
  `<style>` block — **the redesign must preserve these class names' contract**
  (or update every view that uses them) since views aren't scoped to their own
  styles for these shared primitives.

## Target Layout

```
┌─────────────┬──────────────────────────────────────────┐
│             │  Top bar: page title / breadcrumb · FilterBar · ProfileMenu │
│  Sidebar    ├──────────────────────────────────────────┤
│  (fixed)    │                                            │
│  - logo     │  Main content (scrollable)                 │
│  - nav      │                                            │
│  - (footer: │                                            │
│    profile/ │                                            │
│    collapse)│                                            │
└─────────────┴──────────────────────────────────────────┘
```

- App root becomes a CSS Grid / flex row: `.app-shell { display: flex }` with a
  fixed-width `<aside class="sidebar">` (e.g. `260px` expanded) and a `flex: 1`
  main region — not the current column stack of header + main.
  - `.sidebar` scrolls independently if nav items overflow (`overflow-y: auto`);
  it does not scroll with page content.
- Sidebar is sticky/fixed full-height (`height: 100vh; position: sticky; top: 0`),
  visually separated from content with a `1px` border (not a shadow, to match
  the existing flat card style already in this codebase).
- Move `router-link` nav items into the sidebar, stacked vertically, each with an
  icon + label. Active state = left accent bar (`3px` solid, accent color) +
  tinted background, replacing the current `::after` bottom-border active style
  from `App.vue`.
- Keep `FilterBar` and `ProfileMenu`/`LanguageSwitcher` in a slim top bar inside
  the main region (not the sidebar) — filters and account controls are page-level
  utilities, not navigation.
- Collapse-to-icons toggle is optional polish, not required for "done."

## Design Tokens (extend, don't replace, the existing palette)

Reuse the palette already declared in `CLAUDE.md`'s Design System section —
don't introduce a new color language:

- Neutrals: `#0f172a` (text/headings), `#64748b` (secondary text), `#e2e8f0`
  (borders), `#f8fafc` (page background), `#ffffff` (surfaces)
- Accent (active nav / links / primary actions): `#2563eb` on `#eff6ff` tint —
  already used for `.nav-tabs a.active` in `App.vue`
- Status colors: green `#059669`/`#d1fae5`, blue `#2563eb`/`#dbeafe`, yellow
  `#ea580c`/`#fed7aa`, red `#dc2626`/`#fecaca` — already defined as `.badge.*`
  and `.stat-card.*` variants; reuse verbatim.

Spacing: standardize on a 4px base scale (4/8/12/16/24/32px) — this codebase
already leans on `0.25rem/0.5rem/0.75rem/1rem/1.5rem/2rem` (which map to that
scale at 16px root). Audit views for one-off spacing values that don't land on
this scale and normalize them as you touch each file — don't do a blanket
find/replace across files you aren't otherwise modifying.

Typography: keep the existing `Inter` font stack and the weight/size pairing
already used for `.page-header h2` (1.875rem/700), `.card-title` (1.125rem/700),
`.stat-value` (2.25rem/700) — the redesign changes *layout*, not the type scale.

## Process

1. **Read before writing.** Read `App.vue`, `FilterBar.vue`, `ProfileMenu.vue`,
   `LanguageSwitcher.vue`, and at least one view (`Dashboard.vue` is the largest
   surface) before changing anything, to confirm the class-name contract above
   still holds.
2. **Design the sidebar as its own component** (e.g.
   `client/src/components/AppSidebar.vue`) rather than inlining it in `App.vue`
   — this repo already factors nav-adjacent UI into components
   (`ProfileMenu`, `LanguageSwitcher`); match that pattern. Scope its styles
   with `<style scoped>` rather than adding more to `App.vue`'s global block.
3. **Restructure `App.vue`'s template** to the sidebar + main-region shell
   described above. Move the global `<style>` block's layout rules
   (`.app`, `.top-nav`, `.nav-container`, `.nav-tabs`, `.main-content`) to match
   the new shell; leave the shared primitives (`.card`, `.badge`, `.stat-*`,
   `table`/`th`/`td`, `.loading`, `.error`) untouched unless a specific view
   needs adjustment.
4. **Delegate the actual `.vue` authoring/editing to the vue-expert subagent**
   (per `CLAUDE.md`), passing it this design spec plus the specific files in
   scope for that step. Don't write `.vue` file content directly.
5. **Verify with Playwright MCP tools** against `http://localhost:3000` (start
   both `server` and `client` per the root `CLAUDE.md` Quick Start) — check:
   - Every route (`/`, `/inventory`, `/orders`, `/spending`, `/demand`,
     `/reports`) renders inside the new shell without layout breakage.
   - Active-state highlighting matches the current route.
   - `FilterBar` still filters data correctly (this is layout-only work — don't
     touch `useFilters.js` or `api.js` filter logic).
   - Language switching and the profile menu (tasks, profile details modals)
     still open/close correctly from their new position.
   - Responsive behavior at narrower widths doesn't clip the sidebar or main
     content.
6. **Run code-reviewer** on the changed files before considering the work done,
   per the root `CLAUDE.md` subagent rules.

## Non-goals

- Don't change routing, API calls, filter logic, or i18n keys/strings — this is
  a layout and visual-design change only.
- Don't rename existing global CSS classes (`.card`, `.badge`, `.stat-*`, etc.)
  used across views unless you're updating every view that references them in
  the same pass — a partial rename silently breaks unstyled views.
- Don't add a UI component library/dependency to achieve this — the app has no
  such dependency today; build the sidebar with plain Vue + scoped CSS to match
  the rest of the codebase.
