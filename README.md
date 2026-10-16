# Responsive Design Tokens and Mobile-First CSS

Styling layer for the accessible enterprise dashboard: CSS custom properties (design tokens), CSS Grid and Flexbox, glassmorphism, and light/dark themes. Zero page-level horizontal scroll from 320px to 1440px (48/48 automated checks pass; see `docs/RESPONSIVE-TEST-REPORT.md`). HTML still validates with 0 errors and 0 warnings in the W3C Nu checker.

**Main file: [`css/style.css`](css/style.css)**

## Design tokens (`:root`)
| Group | Tokens |
|---|---|
| Brand colors | `--brand-100/300/400/600/700/900`, `--accent-500`, `--danger-600` |
| Typography | `--font-sans`, fluid scale `--text-xs` to `--text-3xl` (using `clamp()`), `--leading-*`, `--weight-*` |
| Spacing | `--space-1` (0.25rem) to `--space-8` (4rem) |
| Radii | `--radius-sm/md/lg/pill` |
| Effects | `--shadow-sm/md/lg`, `--blur`, `--ease`, `--dur-fast`, `--dur` |
| Layout | `--gutter` (fluid), `--container-max`, `--sidebar-w` |
| Semantic (themed) | `--bg`, `--text`, `--text-muted`, `--surface`, `--glass-border`, `--primary`, `--focus`, `--danger`, `--header-bg`, ... |

## Themes
Light is the default. Dark applies automatically via `prefers-color-scheme`, and the header button (`aria-pressed`) lets users override it; the choice is saved in `localStorage` and applied by `js/theme.js` before first paint to avoid a flash.

## Breakpoints (mobile-first, `min-width`)
| Width | Layout |
|---|---|
| 320px+ | One column, off-canvas sidebar (menu button), tables become stacked cards |
| 768px (48rem) | Sidebar rail + 2-column dashboard grid, sticky header, real tables |
| 1024px (64rem) | 3-column grid, wider sidebar |
| 1440px (90rem) | 4-column grid, larger base type |

The dashboard uses named `grid-template-areas`, redefined at each breakpoint. KPI cards, report cards and settings fieldsets use `repeat(auto-fit, minmax(min(100%, 13rem), 1fr))` so they reflow fluidly between breakpoints.

## Visual effects
- **Glassmorphism:** translucent surfaces with `backdrop-filter: blur() saturate()` inside `@supports`, with a solid fallback elsewhere.
- **Soft shadows** from the shadow tokens; **hover transitions** on cards, buttons, rows, links (hover-capable devices only).
- `prefers-reduced-motion` disables transitions.

## Run and verify
```bash
npm install
npx playwright install chromium   # first time only
npm run build         # assemble pages from src/
npm run validate      # W3C Nu HTML + CSS check (needs Java)
npm run screenshots   # regenerates docs/screenshots and docs/RESPONSIVE-TEST-REPORT.md
npm run serve         # http://localhost:8080
```

## Proof
- `docs/screenshots/`: 16 images (dashboard at 320/768/1024/1440 in light and dark, plus users, reports, settings and modal views).
- `docs/RESPONSIVE-TEST-REPORT.md`: overflow results for 4 pages x 6 viewports x 2 themes.

## Structure
```
css/style.css    js/app.js  js/theme.js
src/components/  src/pages/  scripts/(build, validate, screenshots)
index.html users.html reports.html settings.html   (generated)
docs/
```

## Publish
```bash
git init && git add . && git commit -m "Responsive design tokens and mobile-first CSS"
git remote add origin <repo-url> && git push -u origin main
```
