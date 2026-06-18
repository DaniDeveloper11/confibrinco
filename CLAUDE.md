# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm install       # install dependencies (first time only)
npm run dev       # dev server at http://localhost:5173
npm run build     # production build → /dist
npm run preview   # serve the production build locally
```

No test runner is configured.

## Architecture

Vue 3 + Vite + JavaScript (no TypeScript) + Tailwind CSS v4 landing page, designed to grow into a product catalog without rewrites.

**Entry point:** `src/main.js` mounts `App.vue` with vue-router.

**Routing (`src/router/index.js`):**
- `/` → `HomeView.vue` (eagerly loaded — the landing page)
- `/catalogo` → `CatalogoView.vue` (lazy-loaded to keep initial bundle light)
- Hash-based smooth scrolling is handled in `scrollBehavior`

**`@` alias** resolves to `src/` (configured in `vite.config.js`).

**Brand theming (`src/style.css`):** All brand colors are CSS custom properties declared in a single `@theme` block (`--color-brand`, `--color-brand-soft`, `--color-brand-dark`, `--color-ink`, `--color-muted`). Tailwind utilities like `bg-brand` and `text-ink` derive from these tokens — change the tokens here to retheme the entire site with no component edits.

**Product data (`src/data/productos.js`):** Static JS array of sample products. This is the intended replacement point for a real API/CMS call; `ProductCard.vue` already consumes the shape `{ id, nombre, descripcion, precio, imagen }`.

**`LogoPreview.vue`:** Accepts a logo file (drag-and-drop or click), overlays it on a product mockup, and lets the user resize it. The mockup is currently a gray rectangle — replace it with a real product image/SVG and adjust the logo drop zone position when productizing this feature.
