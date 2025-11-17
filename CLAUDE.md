# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Astro-based course website for DESN368, a web development fundamentals course focusing on HTML and CSS. The site uses MDX for content and follows a minimalist structure.

## Essential Commands

```bash
npm run dev      # Start development server at localhost:4321
npm run build    # Build production site to ./dist/
npm run preview  # Preview production build locally
```

## Architecture

The project follows Astro's standard structure with key modifications:

- **src/pages/**: Page routes (`.astro` files) - file-based routing where each file becomes a URL route
- **src/layouts/**: Shared layout components (BaseLayout.astro wraps all pages)
- **src/styles/**: Global CSS (styles.css contains all styling)
- **src/pages/weeks/**: Individual week content pages for the course

## Key Implementation Details

1. **Content Management**: Course weeks are currently hardcoded in index.astro with a status system ("available", "locked"). Week content lives in separate pages under src/pages/weeks/

2. **Styling Approach**: All styles are centralized in src/styles/styles.css and imported directly in page components. The site uses CSS custom properties for theming (particularly EWU brand colors).

3. **MDX Integration**: The project has MDX support configured via @astrojs/mdx, allowing for component usage within markdown content.

4. **TypeScript**: Strict TypeScript configuration is inherited from Astro's defaults.

## Development Notes

- The site is designed as a course platform with a progressive unlock system for weekly content
- No testing framework is currently configured
- No linting or formatting tools are set up beyond TypeScript checking
- Static assets go in the public/ directory