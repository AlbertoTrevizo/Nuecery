# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-page marketing site for Nuecery (a pecan/nut seller), built with Astro 4. The site copy is in Spanish; keep new content in Spanish.

## Commands

- `npm run dev`: dev server (Astro dev toolbar is disabled in `astro.config.mjs`)
- `npm run build`: static build to `dist/` (gitignored)
- `npm run preview`: serve the built `dist/`

There are no tests, linter, or formatter configured.

## Architecture

- `src/pages/index.astro` is the only page. It owns the `<head>` (Google Fonts Karla, Font Awesome 5 from cdnjs, global stylesheet, favicon) and the hero section, then composes `Header`, `Presentations`, `Kitchen`, `Locations`, `Contact`, `Footer`.
- Content lives as hardcoded arrays in component frontmatter, not in a CMS or content collections:
  - Products: `products` array in `Presentations.astro`, rendered via `ProductCard.astro`. Each card links to WhatsApp with a prefilled message naming the product.
  - Points of sale: `locations` array in `Locations.astro`, rendered via `LocationCard.astro`.
  - Recipe photos: `ideas` array in `Kitchen.astro`. Ordering steps: `steps` array in `Contact.astro`.
- WhatsApp and Instagram URLs live in `src/consts.ts`. Import them from there instead of hardcoding.
- Nav links in `Header.astro` point to section ids (`#presentaciones`, `#cocina`, `#puntos-venta`, `#contacto`), so keep them in sync when renaming sections.

## Styling

- Global styles are in `public/styles/styles.css`, served as a static file and linked from `index.astro`, not imported through Astro. It holds the palette and font CSS variables (`--beige`, `--rosa`, `--ciruela`, `--terracota-oscuro`, `--display`, `--body` and others), base typography, and the shared `.container`, `.section`, `.section-head`, `.btn` and `.btn-outline` classes. Use the variables instead of raw hex values.
- `--terracota-oscuro` is for buttons and small text. The lighter `--terracota` and `--caramelo` don't have enough contrast on beige for body text.
- Display font is `Catchy Mager` (`public/fonts/Catchy.ttf`, one weight) for headings. Body is Karla.
- Each component has a scoped `<style>` block for its own layout.

## Images

- Photos live in `src/assets/images/` and render through `astro:assets` `<Image>`, which outputs resized WebP at build time. The originals are 1 to 4 MB, so always pass `width` (and `widths`/`sizes`). Without `width`, Astro also ships the full-size original.
- Only the logo stays in `public/images/nuecery-logo.png`, because it's also the favicon.
