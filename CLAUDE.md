# Lancelot — Personal Website & AI Learning Blog

## Project Overview

Lancelot is a personal website and blog where Lancelot documents their AI learning journey. The site features regular blog posts (daily/weekly updates) about what they've learned, what they've built, and insights along the way.

## Tech Stack

- **Framework:** Astro (static site generator)
- **Content:** Markdown files for blog posts (option to add a visual CMS like Decap/Tina later)
- **Styling:** CSS (light/cream theme)
- **Animations:** CSS keyframes + lightweight JS for intro animation
- **Hosting:** GitHub Pages (auto-deploys on push)
- **Package manager:** npm

## Site Structure

### Pages

- **Landing page** — Minimal intro with pixelated knight animation. Short personal greeting, hub to other sections. Inspired by arvid.xyz (simple structure) and lovefrom.com (refined intro animation).
- **Blog** — List of posts (daily/weekly AI learning updates), written in Markdown.
- **About** — About Lancelot.
- **Questions** — Contact information + FAQ-style general information.

### Navigation

Simple horizontal top nav linking to all pages.

### Social Links

Displayed on the site (specific platforms TBD).

## Design Principles

- **Aesthetic:** Minimalist, clean, no clutter.
- **Color:** Light/cream background (`#fafafa`), dark text.
- **Typography:** Clean, readable font (specific choice TBD).
- **Tone:** Personal, approachable, informal.
- **Intro animation:** A pixelated knight walks in from the right of the screen, stops at the center, turns to face the user, and jumps. As he falls, the page scrolls down to reveal the main content.

## SEO

- Semantic HTML (`<header>`, `<main>`, `<article>`, `<nav>`, etc.)
- Meta tags on every page (title, description, Open Graph, Twitter cards)
- Auto-generated sitemap.xml
- robots.txt
- Clean URL structure (e.g., `/blog/my-post-title`)
- Image alt texts on all images
- Fast load times (static output)
- Mobile responsive design

## Commands

- `npm run dev` — Start local development server
- `npm run build` — Build the site for production
- `npm run preview` — Preview the production build locally

## Working with Lancelot

- **Experience level:** New to web development — no prior experience building websites.
- Explain technical concepts clearly when they come up.
- Suggest options rather than assuming preferences.
- Keep things simple — avoid unnecessary complexity.
- When making changes, explain what was done and why.
- Lancelot will provide resources and inspiration over time — integrate them as they come.
