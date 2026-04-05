# Lancelot Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a minimalist personal website and blog for Lancelot to document their AI learning journey, hosted on GitHub Pages.

**Architecture:** Astro static site generator with Markdown-based blog posts. Four pages (Landing, Blog, About, Questions) connected by a simple top nav. A pixelated knight intro animation on the landing page using CSS keyframes and vanilla JS. Deployed to GitHub Pages via GitHub Actions.

**Tech Stack:** Astro, CSS, vanilla JavaScript, GitHub Pages, GitHub Actions

---

## File Structure

```
Website/
├── CLAUDE.md
├── astro.config.mjs              # Astro config with GitHub Pages settings
├── package.json
├── tsconfig.json
├── public/
│   ├── favicon.svg
│   ├── robots.txt
│   └── knight-sprite.png          # Pixelated knight sprite sheet
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro       # Shared HTML shell: head, nav, footer, SEO meta
│   ├── components/
│   │   ├── Nav.astro              # Horizontal top navigation
│   │   ├── KnightAnimation.astro  # Knight intro animation component
│   │   ├── Footer.astro           # Footer with social links
│   │   └── BlogPostCard.astro     # Card component for blog list page
│   ├── pages/
│   │   ├── index.astro            # Landing page with knight animation
│   │   ├── about.astro            # About Lancelot
│   │   ├── questions.astro        # Contact info + FAQ
│   │   └── blog/
│   │       ├── index.astro        # Blog listing page
│   │       └── [...slug].astro    # Dynamic blog post page
│   ├── content/
│   │   └── blog/
│   │       └── hello-world.md     # First blog post
│   └── styles/
│       └── global.css             # Global styles, colors, typography, reset
└── .github/
    └── workflows/
        └── deploy.yml             # GitHub Actions workflow for GitHub Pages
```

---

### Task 1: Scaffold Astro Project

**Files:**
- Create: `package.json`, `astro.config.mjs`, `tsconfig.json`, `src/pages/index.astro`

- [ ] **Step 1: Initialize Astro project**

Run:
```bash
cd /Users/lancelot/Desktop/Website
npm create astro@latest . -- --template minimal --no-install --no-git --typescript relaxed
```

This creates the minimal Astro starter in the current directory.

- [ ] **Step 2: Install dependencies**

Run:
```bash
npm install
```

Expected: `node_modules/` created, no errors.

- [ ] **Step 3: Verify dev server starts**

Run:
```bash
npm run dev
```

Expected: Server starts at `http://localhost:4321`. Stop with Ctrl+C after confirming.

- [ ] **Step 4: Commit**

```bash
git add package.json package-lock.json astro.config.mjs tsconfig.json src/
git commit -m "feat: scaffold Astro project with minimal template"
```

---

### Task 2: Global Styles & Base Layout

**Files:**
- Create: `src/styles/global.css`
- Create: `src/layouts/BaseLayout.astro`

- [ ] **Step 1: Create global styles**

Create `src/styles/global.css`:

```css
/* Reset */
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* Base */
:root {
  --color-bg: #fafafa;
  --color-text: #1a1a1a;
  --color-text-light: #555;
  --color-accent: #333;
  --color-border: #e0e0e0;
  --font-body: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --max-width: 680px;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: var(--font-body);
  background-color: var(--color-bg);
  color: var(--color-text);
  line-height: 1.7;
  font-size: 17px;
}

a {
  color: var(--color-text);
  text-decoration-thickness: 1px;
  text-underline-offset: 3px;
}

a:hover {
  color: var(--color-text-light);
}

img {
  max-width: 100%;
  height: auto;
}

.container {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 1.5rem;
}
```

- [ ] **Step 2: Create base layout**

Create `src/layouts/BaseLayout.astro`:

```astro
---
import '../styles/global.css';

interface Props {
  title: string;
  description?: string;
}

const { title, description = "Lancelot's AI learning journey" } = Astro.props;
const canonicalURL = new URL(Astro.url.pathname, Astro.site);
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <meta name="generator" content={Astro.generator} />

    <!-- Open Graph -->
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:type" content="website" />
    <meta property="og:url" content={canonicalURL} />

    <!-- Twitter -->
    <meta name="twitter:card" content="summary" />
    <meta name="twitter:title" content={title} />
    <meta name="twitter:description" content={description} />

    <link rel="canonical" href={canonicalURL} />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <title>{title}</title>
  </head>
  <body>
    <slot />
  </body>
</html>
```

- [ ] **Step 3: Update index.astro to use layout**

Replace `src/pages/index.astro` with:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout title="Lancelot">
  <main class="container">
    <h1>Lancelot</h1>
    <p>Coming soon.</p>
  </main>
</BaseLayout>
```

- [ ] **Step 4: Verify it renders**

Run:
```bash
npm run dev
```

Expected: Page shows "Lancelot" heading with cream background, clean sans-serif font, centered content.

- [ ] **Step 5: Commit**

```bash
git add src/styles/global.css src/layouts/BaseLayout.astro src/pages/index.astro
git commit -m "feat: add global styles and base layout with SEO meta tags"
```

---

### Task 3: Navigation & Footer Components

**Files:**
- Create: `src/components/Nav.astro`
- Create: `src/components/Footer.astro`
- Modify: `src/layouts/BaseLayout.astro`

- [ ] **Step 1: Create Nav component**

Create `src/components/Nav.astro`:

```astro
---
const currentPath = Astro.url.pathname;

const links = [
  { href: '/', label: 'Home' },
  { href: '/blog', label: 'Blog' },
  { href: '/about', label: 'About' },
  { href: '/questions', label: 'Questions' },
];
---

<nav>
  <div class="nav-inner container">
    <a href="/" class="nav-logo">Lancelot</a>
    <ul class="nav-links">
      {links.map(link => (
        <li>
          <a
            href={link.href}
            class:list={[{ active: currentPath === link.href || (link.href !== '/' && currentPath.startsWith(link.href)) }]}
          >
            {link.label}
          </a>
        </li>
      ))}
    </ul>
  </div>
</nav>

<style>
  nav {
    padding: 1.5rem 0;
    border-bottom: 1px solid var(--color-border);
  }

  .nav-inner {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .nav-logo {
    font-weight: 600;
    font-size: 1.1rem;
    text-decoration: none;
  }

  .nav-links {
    display: flex;
    list-style: none;
    gap: 1.5rem;
  }

  .nav-links a {
    text-decoration: none;
    font-size: 0.95rem;
    color: var(--color-text-light);
  }

  .nav-links a:hover,
  .nav-links a.active {
    color: var(--color-text);
  }

  @media (max-width: 480px) {
    .nav-inner {
      flex-direction: column;
      gap: 0.75rem;
    }

    .nav-links {
      gap: 1rem;
    }
  }
</style>
```

- [ ] **Step 2: Create Footer component**

Create `src/components/Footer.astro`:

```astro
---
const year = new Date().getFullYear();
---

<footer>
  <div class="footer-inner container">
    <div class="social-links">
      <!-- Update these hrefs with actual social links -->
      <a href="#" target="_blank" rel="noopener noreferrer">GitHub</a>
      <a href="#" target="_blank" rel="noopener noreferrer">LinkedIn</a>
      <a href="#" target="_blank" rel="noopener noreferrer">Twitter</a>
    </div>
    <p>&copy; {year} Lancelot</p>
  </div>
</footer>

<style>
  footer {
    border-top: 1px solid var(--color-border);
    padding: 2rem 0;
    margin-top: 4rem;
  }

  .footer-inner {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.85rem;
    color: var(--color-text-light);
  }

  .social-links {
    display: flex;
    gap: 1rem;
  }

  .social-links a {
    color: var(--color-text-light);
    text-decoration: none;
  }

  .social-links a:hover {
    color: var(--color-text);
  }
</style>
```

- [ ] **Step 3: Add Nav and Footer to BaseLayout**

Modify `src/layouts/BaseLayout.astro` — add imports and place components:

```astro
---
import '../styles/global.css';
import Nav from '../components/Nav.astro';
import Footer from '../components/Footer.astro';

interface Props {
  title: string;
  description?: string;
}

const { title, description = "Lancelot's AI learning journey" } = Astro.props;
const canonicalURL = new URL(Astro.url.pathname, Astro.site);
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <meta name="generator" content={Astro.generator} />

    <!-- Open Graph -->
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:type" content="website" />
    <meta property="og:url" content={canonicalURL} />

    <!-- Twitter -->
    <meta name="twitter:card" content="summary" />
    <meta name="twitter:title" content={title} />
    <meta name="twitter:description" content={description} />

    <link rel="canonical" href={canonicalURL} />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <title>{title}</title>
  </head>
  <body>
    <Nav />
    <slot />
    <Footer />
  </body>
</html>
```

- [ ] **Step 4: Verify nav and footer render**

Run:
```bash
npm run dev
```

Expected: Top nav with "Lancelot" logo and links (Home, Blog, About, Questions). Footer at bottom with social links and copyright.

- [ ] **Step 5: Commit**

```bash
git add src/components/Nav.astro src/components/Footer.astro src/layouts/BaseLayout.astro
git commit -m "feat: add navigation and footer components"
```

---

### Task 4: Knight Intro Animation on Landing Page

**Files:**
- Create: `src/components/KnightAnimation.astro`
- Create: `public/knight-sprite.png` (placeholder)
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create a placeholder knight sprite**

For now, create a simple SVG placeholder that we'll replace with a proper pixel art sprite later. Create `public/knight-placeholder.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" width="64" height="64">
  <!-- Simple pixelated knight silhouette -->
  <rect x="12" y="2" width="8" height="8" fill="#1a1a1a"/><!-- head -->
  <rect x="14" y="4" width="2" height="2" fill="#fafafa"/><!-- eye -->
  <rect x="10" y="10" width="12" height="10" fill="#1a1a1a"/><!-- body -->
  <rect x="6" y="12" width="4" height="2" fill="#1a1a1a"/><!-- sword -->
  <rect x="10" y="20" width="4" height="8" fill="#1a1a1a"/><!-- left leg -->
  <rect x="18" y="20" width="4" height="8" fill="#1a1a1a"/><!-- right leg -->
  <rect x="8" y="0" width="4" height="4" fill="#1a1a1a"/><!-- helmet plume -->
</svg>
```

- [ ] **Step 2: Create KnightAnimation component**

Create `src/components/KnightAnimation.astro`:

```astro
<div id="knight-intro" class="knight-intro">
  <div class="knight-stage">
    <img id="knight" src="/knight-placeholder.svg" alt="" class="knight" />
  </div>
</div>

<style>
  .knight-intro {
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background-color: var(--color-bg);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
    transition: transform 0.8s cubic-bezier(0.4, 0, 0.2, 1);
  }

  .knight-intro.done {
    transform: translateY(-100%);
    pointer-events: none;
  }

  .knight {
    width: 64px;
    height: 64px;
    image-rendering: pixelated;
    position: absolute;
    /* Start off-screen right */
    right: -80px;
    top: 50%;
    transform: translateY(-50%);
  }

  .knight.walking {
    animation: walkIn 2s ease-out forwards;
  }

  .knight.facing {
    animation: none;
    right: auto;
    left: 50%;
    transform: translateX(-50%) translateY(-50%) scaleX(-1);
  }

  .knight.jumping {
    animation: jump 0.6s ease-in forwards;
    right: auto;
    left: 50%;
    transform: translateX(-50%) translateY(-50%) scaleX(-1);
  }

  @keyframes walkIn {
    0% {
      right: -80px;
    }
    100% {
      right: calc(50% - 32px);
    }
  }

  @keyframes jump {
    0% {
      transform: translateX(-50%) translateY(-50%) scaleX(-1);
    }
    40% {
      transform: translateX(-50%) translateY(calc(-50% - 60px)) scaleX(-1);
    }
    100% {
      transform: translateX(-50%) translateY(100vh) scaleX(-1);
    }
  }
</style>

<script>
  const knight = document.getElementById('knight');
  const intro = document.getElementById('knight-intro');

  if (knight && intro) {
    // Step 1: Knight walks in from right
    setTimeout(() => {
      knight.classList.add('walking');
    }, 300);

    // Step 2: Knight stops at center, turns to face user
    setTimeout(() => {
      knight.classList.remove('walking');
      knight.classList.add('facing');
    }, 2500);

    // Step 3: Knight jumps
    setTimeout(() => {
      knight.classList.remove('facing');
      knight.classList.add('jumping');
    }, 3200);

    // Step 4: Intro screen slides away, revealing content
    setTimeout(() => {
      intro.classList.add('done');
    }, 3600);

    // Step 5: Remove intro element from DOM
    setTimeout(() => {
      intro.remove();
    }, 4500);
  }
</script>
```

- [ ] **Step 3: Update landing page**

Replace `src/pages/index.astro` with:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import KnightAnimation from '../components/KnightAnimation.astro';
---

<BaseLayout title="Lancelot">
  <KnightAnimation />
  <main class="container landing">
    <section class="hero">
      <h1>hi, i'm lancelot</h1>
      <p>i'm learning about AI and sharing everything along the way.</p>
      <nav class="hero-links">
        <a href="/blog">read the blog</a>
        <span class="separator">/</span>
        <a href="/about">about me</a>
        <span class="separator">/</span>
        <a href="/questions">questions</a>
      </nav>
    </section>
  </main>
</BaseLayout>

<style>
  .landing {
    min-height: 80vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .hero {
    text-align: center;
  }

  .hero h1 {
    font-size: 2rem;
    font-weight: 600;
    margin-bottom: 0.75rem;
  }

  .hero p {
    color: var(--color-text-light);
    margin-bottom: 1.5rem;
    font-size: 1.1rem;
  }

  .hero-links a {
    color: var(--color-text-light);
    font-size: 0.95rem;
  }

  .hero-links a:hover {
    color: var(--color-text);
  }

  .separator {
    margin: 0 0.5rem;
    color: var(--color-border);
  }
</style>
```

- [ ] **Step 4: Verify animation works**

Run:
```bash
npm run dev
```

Expected: Page loads, knight walks in from right, stops at center, turns (flips horizontally), jumps up and falls down, then the intro screen slides up revealing the landing page content underneath.

- [ ] **Step 5: Commit**

```bash
git add public/knight-placeholder.svg src/components/KnightAnimation.astro src/pages/index.astro
git commit -m "feat: add pixelated knight intro animation on landing page"
```

---

### Task 5: Blog Setup with Markdown Content Collections

**Files:**
- Create: `src/content.config.ts`
- Create: `src/content/blog/hello-world.md`
- Create: `src/components/BlogPostCard.astro`
- Create: `src/pages/blog/index.astro`
- Create: `src/pages/blog/[slug].astro`

- [ ] **Step 1: Configure content collection**

Create `src/content.config.ts`:

```ts
import { defineCollection, z } from 'astro:content';
import { glob } from 'astro/loaders';

const blog = defineCollection({
  loader: glob({ pattern: '**/*.md', base: './src/content/blog' }),
  schema: z.object({
    title: z.string(),
    description: z.string(),
    date: z.coerce.date(),
    tags: z.array(z.string()).optional(),
  }),
});

export const collections = { blog };
```

- [ ] **Step 2: Create first blog post**

Create `src/content/blog/hello-world.md`:

```markdown
---
title: "Hello World"
description: "The first post on my AI learning blog. Here we go!"
date: 2026-04-05
tags: ["intro"]
---

# Hello World

Welcome to my blog! I'm Lancelot, and I'm documenting my journey learning about AI.

I'll be posting regular updates about what I'm learning, what tools I'm exploring, and what I'm building along the way.

Stay tuned!
```

- [ ] **Step 3: Create BlogPostCard component**

Create `src/components/BlogPostCard.astro`:

```astro
---
interface Props {
  title: string;
  description: string;
  date: Date;
  slug: string;
}

const { title, description, date, slug } = Astro.props;

const formattedDate = date.toLocaleDateString('en-US', {
  year: 'numeric',
  month: 'long',
  day: 'numeric',
});
---

<article class="post-card">
  <a href={`/blog/${slug}`}>
    <time datetime={date.toISOString()}>{formattedDate}</time>
    <h3>{title}</h3>
    <p>{description}</p>
  </a>
</article>

<style>
  .post-card a {
    display: block;
    padding: 1.25rem 0;
    text-decoration: none;
    border-bottom: 1px solid var(--color-border);
  }

  .post-card a:hover h3 {
    color: var(--color-text-light);
  }

  time {
    font-size: 0.85rem;
    color: var(--color-text-light);
  }

  h3 {
    margin: 0.25rem 0 0.4rem;
    font-size: 1.15rem;
    font-weight: 600;
    transition: color 0.15s;
  }

  p {
    color: var(--color-text-light);
    font-size: 0.95rem;
  }
</style>
```

- [ ] **Step 4: Create blog listing page**

Create `src/pages/blog/index.astro`:

```astro
---
import BaseLayout from '../../layouts/BaseLayout.astro';
import BlogPostCard from '../../components/BlogPostCard.astro';
import { getCollection } from 'astro:content';

const posts = (await getCollection('blog')).sort(
  (a, b) => b.data.date.valueOf() - a.data.date.valueOf()
);
---

<BaseLayout title="Blog — Lancelot" description="Lancelot's AI learning blog posts">
  <main class="container">
    <header class="page-header">
      <h1>Blog</h1>
      <p>My AI learning journey, one post at a time.</p>
    </header>
    <section>
      {posts.map(post => (
        <BlogPostCard
          title={post.data.title}
          description={post.data.description}
          date={post.data.date}
          slug={post.id}
        />
      ))}
    </section>
  </main>
</BaseLayout>

<style>
  .page-header {
    padding: 3rem 0 2rem;
  }

  .page-header h1 {
    font-size: 1.8rem;
    margin-bottom: 0.5rem;
  }

  .page-header p {
    color: var(--color-text-light);
  }
</style>
```

- [ ] **Step 5: Create blog post page**

Create `src/pages/blog/[slug].astro`:

```astro
---
import BaseLayout from '../../layouts/BaseLayout.astro';
import { getCollection, render } from 'astro:content';

export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map(post => ({
    params: { slug: post.id },
    props: { post },
  }));
}

const { post } = Astro.props;
const { Content } = await render(post);

const formattedDate = post.data.date.toLocaleDateString('en-US', {
  year: 'numeric',
  month: 'long',
  day: 'numeric',
});
---

<BaseLayout title={`${post.data.title} — Lancelot`} description={post.data.description}>
  <main class="container">
    <article class="blog-post">
      <header>
        <time datetime={post.data.date.toISOString()}>{formattedDate}</time>
        <h1>{post.data.title}</h1>
      </header>
      <div class="prose">
        <Content />
      </div>
      <footer>
        <a href="/blog">&larr; Back to blog</a>
      </footer>
    </article>
  </main>
</BaseLayout>

<style>
  .blog-post header {
    padding: 3rem 0 2rem;
  }

  .blog-post time {
    font-size: 0.85rem;
    color: var(--color-text-light);
  }

  .blog-post h1 {
    font-size: 1.8rem;
    margin-top: 0.5rem;
  }

  .prose {
    padding-bottom: 2rem;
  }

  .prose :global(h2) {
    font-size: 1.4rem;
    margin: 2rem 0 0.75rem;
  }

  .prose :global(p) {
    margin-bottom: 1.25rem;
  }

  .prose :global(code) {
    background: #f0f0f0;
    padding: 0.15em 0.35em;
    border-radius: 3px;
    font-size: 0.9em;
  }

  .prose :global(pre) {
    background: #1a1a1a;
    color: #fafafa;
    padding: 1rem;
    border-radius: 6px;
    overflow-x: auto;
    margin-bottom: 1.25rem;
  }

  footer {
    padding: 2rem 0;
    border-top: 1px solid var(--color-border);
  }

  footer a {
    font-size: 0.9rem;
    color: var(--color-text-light);
  }
</style>
```

- [ ] **Step 6: Verify blog works**

Run:
```bash
npm run dev
```

Expected: `/blog` shows the "Hello World" post card. Clicking it navigates to `/blog/hello-world` with the full post content, date, and back link.

- [ ] **Step 7: Commit**

```bash
git add src/content.config.ts src/content/blog/hello-world.md src/components/BlogPostCard.astro src/pages/blog/
git commit -m "feat: add blog with Markdown content collections and post pages"
```

---

### Task 6: About Page

**Files:**
- Create: `src/pages/about.astro`

- [ ] **Step 1: Create About page**

Create `src/pages/about.astro`:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout title="About — Lancelot" description="About Lancelot and this AI learning journey">
  <main class="container">
    <article class="page-content">
      <h1>About</h1>
      <p>
        Hi, I'm Lancelot. I'm on a journey to learn about AI — what it can do,
        how it works, and how I can use it in my own projects.
      </p>
      <p>
        This website is where I document everything I learn along the way.
        Think of it as a public notebook: sometimes messy, always honest.
      </p>
      <p>
        If you're curious about AI too, I hope you'll find something useful here.
      </p>
    </article>
  </main>
</BaseLayout>

<style>
  .page-content {
    padding: 3rem 0;
  }

  .page-content h1 {
    font-size: 1.8rem;
    margin-bottom: 1.5rem;
  }

  .page-content p {
    margin-bottom: 1.25rem;
    color: var(--color-text);
  }
</style>
```

- [ ] **Step 2: Verify page renders**

Run:
```bash
npm run dev
```

Expected: `/about` shows the About page with heading and placeholder text. Nav link works.

- [ ] **Step 3: Commit**

```bash
git add src/pages/about.astro
git commit -m "feat: add About page"
```

---

### Task 7: Questions Page

**Files:**
- Create: `src/pages/questions.astro`

- [ ] **Step 1: Create Questions page**

Create `src/pages/questions.astro`:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout title="Questions — Lancelot" description="Contact information and frequently asked questions">
  <main class="container">
    <article class="page-content">
      <h1>Questions</h1>

      <section class="contact">
        <h2>Get in touch</h2>
        <p>
          The best way to reach me is by email:
          <a href="mailto:your@email.com">your@email.com</a>
        </p>
        <p>
          You can also find me on
          <a href="#" target="_blank" rel="noopener noreferrer">GitHub</a>,
          <a href="#" target="_blank" rel="noopener noreferrer">LinkedIn</a>, and
          <a href="#" target="_blank" rel="noopener noreferrer">Twitter</a>.
        </p>
      </section>

      <section class="faq">
        <h2>Frequently asked</h2>

        <div class="faq-item">
          <h3>What is this site?</h3>
          <p>
            A personal blog where I document my journey learning about AI.
            I post regular updates on what I'm learning, building, and exploring.
          </p>
        </div>

        <div class="faq-item">
          <h3>Can I use or share your content?</h3>
          <p>
            Feel free to share links. If you'd like to reference or republish
            something, just reach out first.
          </p>
        </div>

        <div class="faq-item">
          <h3>Do you take questions or suggestions?</h3>
          <p>
            Absolutely — send me an email. I'd love to hear from you.
          </p>
        </div>
      </section>
    </article>
  </main>
</BaseLayout>

<style>
  .page-content {
    padding: 3rem 0;
  }

  .page-content h1 {
    font-size: 1.8rem;
    margin-bottom: 2rem;
  }

  h2 {
    font-size: 1.3rem;
    margin-bottom: 1rem;
  }

  .contact {
    margin-bottom: 3rem;
  }

  .contact p {
    margin-bottom: 0.75rem;
  }

  .faq-item {
    margin-bottom: 1.5rem;
  }

  .faq-item h3 {
    font-size: 1.05rem;
    margin-bottom: 0.4rem;
  }

  .faq-item p {
    color: var(--color-text-light);
  }
</style>
```

- [ ] **Step 2: Verify page renders**

Run:
```bash
npm run dev
```

Expected: `/questions` shows contact info and FAQ sections. Nav link works.

- [ ] **Step 3: Commit**

```bash
git add src/pages/questions.astro
git commit -m "feat: add Questions page with contact info and FAQ"
```

---

### Task 8: SEO Essentials (Sitemap, robots.txt, Favicon)

**Files:**
- Create: `public/robots.txt`
- Create: `public/favicon.svg`
- Modify: `astro.config.mjs`

- [ ] **Step 1: Install Astro sitemap integration**

Run:
```bash
npm install @astrojs/sitemap
```

- [ ] **Step 2: Update Astro config**

Replace `astro.config.mjs` with:

```js
import { defineConfig } from 'astro/config';
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: 'https://<your-github-username>.github.io',
  integrations: [sitemap()],
});
```

Note: Replace `<your-github-username>` with your actual GitHub username. If using a custom repo name, the URL would be `https://<username>.github.io/<repo-name>`.

- [ ] **Step 3: Create robots.txt**

Create `public/robots.txt`:

```
User-agent: *
Allow: /

Sitemap: https://<your-github-username>.github.io/sitemap-index.xml
```

- [ ] **Step 4: Create a simple favicon**

Create `public/favicon.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="4" fill="#1a1a1a"/>
  <text x="50%" y="55%" dominant-baseline="middle" text-anchor="middle" font-family="sans-serif" font-weight="bold" font-size="20" fill="#fafafa">L</text>
</svg>
```

- [ ] **Step 5: Build and verify**

Run:
```bash
npm run build
```

Expected: Build succeeds. Check `dist/sitemap-index.xml` exists.

- [ ] **Step 6: Commit**

```bash
git add astro.config.mjs public/robots.txt public/favicon.svg package.json package-lock.json
git commit -m "feat: add sitemap, robots.txt, and favicon for SEO"
```

---

### Task 9: GitHub Pages Deployment

**Files:**
- Create: `.github/workflows/deploy.yml`
- Modify: `astro.config.mjs` (if needed for base path)

- [ ] **Step 1: Create GitHub Actions workflow**

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

- [ ] **Step 2: Verify the build works locally**

Run:
```bash
npm run build && npm run preview
```

Expected: Build succeeds. Preview server starts. All pages render correctly at their URLs.

- [ ] **Step 3: Commit**

```bash
git add .github/workflows/deploy.yml
git commit -m "feat: add GitHub Actions workflow for GitHub Pages deployment"
```

- [ ] **Step 4: Push to GitHub**

After creating a GitHub repo (e.g., via `gh repo create`):

```bash
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

Then enable GitHub Pages in repo settings: Settings → Pages → Source: "GitHub Actions".

Expected: The workflow runs and the site is live at `https://<username>.github.io/<repo-name>`.

---

## Summary

| Task | What it builds |
|------|---------------|
| 1 | Astro project scaffold |
| 2 | Global styles + base layout with SEO meta |
| 3 | Navigation + footer |
| 4 | Knight intro animation |
| 5 | Blog with Markdown content collections |
| 6 | About page |
| 7 | Questions page |
| 8 | Sitemap, robots.txt, favicon |
| 9 | GitHub Pages deployment |

After completing all 9 tasks, the site will be live and ready for blog posts. To add a new post, create a `.md` file in `src/content/blog/` and push to `main`.
