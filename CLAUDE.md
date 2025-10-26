# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based blog site with Preact integration for interactive components. The site is deployed to Netlify at https://tutrial-astro.netlify.app and is written primarily in Japanese.

## Development Commands

All commands use `pnpm` as the package manager (version 10.18.3+):

- `pnpm install` - Install dependencies
- `pnpm dev` - Start dev server at `localhost:4321`
- `pnpm build` - Build production site to `./dist/`
- `pnpm preview` - Preview production build locally
- `pnpm astro ...` - Run Astro CLI commands (e.g., `pnpm astro add`, `pnpm astro check`)

## Architecture

### Routing & Pages

Astro uses file-based routing in `src/pages/`:
- `.astro` files become pages based on their filename
- `.md` files in `src/pages/posts/` are blog posts with frontmatter metadata
- Dynamic routes use bracket notation (e.g., `[tag].astro`)

### Layouts

Two main layouts in `src/layouts/`:

1. **BaseLayout.astro** - Base template for all pages
   - Imports global CSS and Header/Footer components
   - Includes client-side script for hamburger menu (`src/scripts/menu.js`)
   - Accepts `pageTitle` prop

2. **MarkdownPostLayout.astro** - Wraps blog posts
   - Extends BaseLayout
   - Receives markdown frontmatter via `frontmatter` prop
   - Displays post metadata (title, description, author, pubDate, tags, image)
   - Renders tag links with custom styling

### Content Management

**Blog Posts** (`src/pages/posts/*.md`):
- Use frontmatter with fields: `layout`, `title`, `author`, `description`, `image`, `pubDate`, `tags`
- Automatically use MarkdownPostLayout via frontmatter `layout` field

**Tags System**:
- `src/pages/tags/[tag].astro` - Dynamic tag pages using `getStaticPaths()`
- Aggregates all posts via `import.meta.glob('../posts/*.md', {eager: true})`
- Extracts unique tags and filters posts by tag

**RSS Feed**:
- `src/pages/rss.xml.js` uses `@astrojs/rss` integration
- Auto-generates feed from markdown files using `pagesGlobToRssItems()`
- Configured with Japanese language (`ja-jp`)

### Interactive Components

**Preact Integration**:
- Configured in `astro.config.mjs` and `tsconfig.json`
- `src/components/Greeting.jsx` - Interactive greeting component using Preact hooks
- Uses `.jsx` extension with `jsxImportSource: "preact"`

### Theming

**Dark Mode** (`src/components/ThemeIcon.astro`):
- Client-side theme toggle using localStorage
- Inline script applies theme on page load to prevent flash
- CSS classes: `.dark` modifier on `html` element
- Global styles in `src/styles/global.css` include dark mode variants

### Responsive Navigation

- Hamburger menu for mobile (`src/components/Hamburger.astro`)
- Toggled via `src/scripts/menu.js` (adds `.expanded` class)
- Responsive breakpoint at `636px` (full nav displayed above this width)

## Key Patterns

1. **Content Collections**: Blog posts use glob imports with eager loading for static generation
2. **Scoped Styles**: Component-level `<style>` tags provide scoped CSS
3. **Client Scripts**: Use `<script>` tags in components or layouts; `is:inline` for immediate execution
4. **Type Safety**: TypeScript configured with strict mode (`astro/tsconfigs/strict`)

## Site Configuration

- Site URL: `https://tutrial-astro.netlify.app` (defined in `astro.config.mjs`)
- Language: Japanese (`ja`)
- Integrations: Preact, RSS
