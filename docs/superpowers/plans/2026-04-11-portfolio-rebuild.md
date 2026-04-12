# Portfolio Rebuild Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rebuild Giocrisrai's portfolio from outdated Next.js 10 / Stackbit site into a modern Astro + Tailwind CSS static site optimized for international recruiters, focused on AI/ML value proposition.

**Architecture:** Single-page portfolio built with Astro (static output) and Tailwind CSS. All content hardcoded in Astro components (no CMS needed). Dark theme with amber accent. Deployed to Netlify via existing repo. English as primary language.

**Tech Stack:** Astro 5.x, Tailwind CSS 4.x, Netlify, Node 22

---

## File Structure

```
giocrisrai-portfolio/
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro           # HTML shell, meta tags, fonts
│   ├── components/
│   │   ├── Header.astro               # Fixed navigation bar
│   │   ├── Hero.astro                 # Hero with headline + CTAs
│   │   ├── ValueProp.astro            # 4 expertise blocks
│   │   ├── Projects.astro             # 5 featured project cards
│   │   ├── Experience.astro           # Professional timeline
│   │   ├── Education.astro            # Degrees + key certifications
│   │   ├── Contact.astro              # Contact links
│   │   └── Footer.astro               # Footer with social links
│   ├── pages/
│   │   └── index.astro                # Single page composing all sections
│   └── styles/
│       └── global.css                 # Tailwind imports + custom styles
├── public/
│   └── favicon.svg                    # Simple favicon
├── astro.config.mjs                   # Astro config with Tailwind
├── package.json                       # Dependencies
├── netlify.toml                       # Updated build config
└── tsconfig.json                      # TypeScript config
```

---

### Task 1: Initialize Astro Project (clean slate)

**Files:**
- Delete: `src/`, `content/`, `sourcebit.js`, `stackbit.yaml`, `next.config.js`, `package.json`, `package-lock.json`, `.eslintrc.json`, `.prettierrc`
- Keep: `public/`, `netlify.toml`, `LICENSE.md`, `README.md`, `.gitignore`, `docs/`
- Create: `package.json`, `astro.config.mjs`, `tsconfig.json`, `src/styles/global.css`

- [ ] **Step 1: Remove old project files**

```bash
cd /Users/giocrisraigodoy/Documents/mi_pagina/giocrisrai-portfolio
rm -rf src/ content/ sourcebit.js stackbit.yaml next.config.js package.json package-lock.json .eslintrc.json .prettierrc
```

- [ ] **Step 2: Initialize new package.json**

```bash
npm init -y
```

- [ ] **Step 3: Install Astro and Tailwind**

```bash
npm install astro @astrojs/tailwind tailwindcss @tailwindcss/vite
```

- [ ] **Step 4: Create astro.config.mjs**

Create `astro.config.mjs`:
```javascript
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  vite: {
    plugins: [tailwindcss()]
  },
  output: 'static',
  outDir: './dist'
});
```

- [ ] **Step 5: Create tsconfig.json**

Create `tsconfig.json`:
```json
{
  "extends": "astro/tsconfigs/strict"
}
```

- [ ] **Step 6: Create global.css with Tailwind**

Create `src/styles/global.css`:
```css
@import "tailwindcss";

@theme {
  --color-bg-primary: #0f172a;
  --color-bg-secondary: #1e293b;
  --color-bg-card: #1e293b;
  --color-accent: #f59e0b;
  --color-accent-hover: #d97706;
  --color-text-primary: #f1f5f9;
  --color-text-secondary: #94a3b8;
  --color-text-muted: #64748b;
  --color-border: #334155;
  --font-sans: 'Inter', system-ui, sans-serif;
}

html {
  scroll-behavior: smooth;
}

body {
  background-color: var(--color-bg-primary);
  color: var(--color-text-primary);
  font-family: var(--font-sans);
}

::selection {
  background-color: var(--color-accent);
  color: var(--color-bg-primary);
}
```

- [ ] **Step 7: Create favicon**

Create `public/favicon.svg`:
```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
  <text y=".9em" font-size="90">G</text>
</svg>
```

- [ ] **Step 8: Update netlify.toml**

Update `netlify.toml`:
```toml
[build]
publish = "dist"
command = "npm run build"

[build.environment]
NODE_VERSION = "22"
```

- [ ] **Step 9: Add scripts to package.json**

Update the scripts section of `package.json`:
```json
{
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  }
}
```

- [ ] **Step 10: Verify build works**

```bash
npx astro build
```
Expected: Build succeeds (even if no pages yet, should not error on config)

- [ ] **Step 11: Commit**

```bash
git add -A
git commit -m "feat: initialize Astro + Tailwind project, remove old Next.js code"
```

---

### Task 2: Base Layout + Header

**Files:**
- Create: `src/layouts/BaseLayout.astro`
- Create: `src/components/Header.astro`
- Create: `src/pages/index.astro` (minimal, for testing)

- [ ] **Step 1: Create BaseLayout.astro**

Create `src/layouts/BaseLayout.astro`:
```astro
---
interface Props {
  title?: string;
  description?: string;
}

const {
  title = "Giocrisrai Godoy | ML / AI Engineer",
  description = "ML/AI Engineer building production-grade AI systems — RAG pipelines, autonomous agents, computer vision, and scalable data engineering. Based in Chile, open to global opportunities."
} = Astro.props;
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <meta name="author" content="Giocrisrai Godoy Bonillo" />

    <!-- Open Graph -->
    <meta property="og:title" content={title} />
    <meta property="og:description" content={description} />
    <meta property="og:type" content="website" />
    <meta property="og:locale" content="en_US" />

    <!-- Twitter Card -->
    <meta name="twitter:card" content="summary_large_image" />
    <meta name="twitter:creator" content="@Giocrisrai" />
    <meta name="twitter:title" content={title} />
    <meta name="twitter:description" content={description} />

    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />

    <title>{title}</title>
  </head>
  <body class="antialiased">
    <slot />
  </body>
</html>

<style is:global>
  @import "../styles/global.css";
</style>
```

- [ ] **Step 2: Create Header.astro**

Create `src/components/Header.astro`:
```astro
---
const navLinks = [
  { label: "About", href: "#about" },
  { label: "Projects", href: "#projects" },
  { label: "Experience", href: "#experience" },
  { label: "Education", href: "#education" },
  { label: "Contact", href: "#contact" },
];
---

<header id="header" class="fixed top-0 left-0 right-0 z-50 transition-all duration-300 bg-bg-primary/80 backdrop-blur-md border-b border-border/50">
  <nav class="max-w-6xl mx-auto px-6 h-16 flex items-center justify-between">
    <a href="#" class="text-xl font-bold text-accent hover:text-accent-hover transition-colors">
      GG
    </a>

    <!-- Desktop nav -->
    <ul class="hidden md:flex items-center gap-8">
      {navLinks.map(link => (
        <li>
          <a
            href={link.href}
            class="text-sm text-text-secondary hover:text-accent transition-colors"
          >
            {link.label}
          </a>
        </li>
      ))}
    </ul>

    <!-- Mobile toggle -->
    <button
      id="menu-toggle"
      class="md:hidden text-text-secondary hover:text-accent transition-colors"
      aria-label="Toggle menu"
    >
      <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path id="menu-icon" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
      </svg>
    </button>
  </nav>

  <!-- Mobile menu -->
  <div id="mobile-menu" class="hidden md:hidden border-t border-border/50 bg-bg-primary/95 backdrop-blur-md">
    <ul class="px-6 py-4 space-y-4">
      {navLinks.map(link => (
        <li>
          <a
            href={link.href}
            class="block text-text-secondary hover:text-accent transition-colors mobile-link"
          >
            {link.label}
          </a>
        </li>
      ))}
    </ul>
  </div>
</header>

<script>
  const toggle = document.getElementById('menu-toggle');
  const menu = document.getElementById('mobile-menu');
  const mobileLinks = document.querySelectorAll('.mobile-link');

  toggle?.addEventListener('click', () => {
    menu?.classList.toggle('hidden');
  });

  mobileLinks.forEach(link => {
    link.addEventListener('click', () => {
      menu?.classList.add('hidden');
    });
  });
</script>
```

- [ ] **Step 3: Create minimal index.astro**

Create `src/pages/index.astro`:
```astro
---
import BaseLayout from "../layouts/BaseLayout.astro";
import Header from "../components/Header.astro";
---

<BaseLayout>
  <Header />
  <main class="pt-16">
    <section class="min-h-screen flex items-center justify-center">
      <h1 class="text-4xl font-bold text-accent">Portfolio Coming Soon</h1>
    </section>
  </main>
</BaseLayout>
```

- [ ] **Step 4: Run dev server and verify**

```bash
cd /Users/giocrisraigodoy/Documents/mi_pagina/giocrisrai-portfolio
npx astro dev --port 3000
```
Expected: Site loads at localhost:3000 with dark background, amber "Portfolio Coming Soon" text, and working navigation header.

- [ ] **Step 5: Build and verify**

```bash
npx astro build
```
Expected: Build succeeds, output in `dist/`

- [ ] **Step 6: Commit**

```bash
git add -A
git commit -m "feat: add base layout with SEO meta tags and responsive header"
```

---

### Task 3: Hero Section

**Files:**
- Create: `src/components/Hero.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Hero.astro**

Create `src/components/Hero.astro`:
```astro
---
const socialLinks = [
  { label: "GitHub", href: "https://github.com/Giocrisrai", icon: "github" },
  { label: "LinkedIn", href: "https://www.linkedin.com/in/giocrisrai/", icon: "linkedin" },
  { label: "Hugging Face", href: "https://huggingface.co/giocrisrai", icon: "huggingface" },
];
---

<section id="hero" class="min-h-screen flex items-center relative overflow-hidden">
  <!-- Subtle gradient background -->
  <div class="absolute inset-0 bg-gradient-to-br from-bg-primary via-bg-primary to-bg-secondary"></div>
  <div class="absolute top-1/4 right-1/4 w-96 h-96 bg-accent/5 rounded-full blur-3xl"></div>

  <div class="relative max-w-6xl mx-auto px-6 py-20">
    <div class="max-w-3xl">
      <p class="text-accent font-medium mb-4 text-sm tracking-wider uppercase">
        ML / AI Engineer
      </p>

      <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold leading-tight mb-6">
        Hi, I'm <span class="text-accent">Giocrisrai</span> Godoy
      </h1>

      <p class="text-lg sm:text-xl text-text-secondary leading-relaxed mb-8 max-w-2xl">
        I build production-grade AI systems — from RAG pipelines and autonomous agents
        to computer vision models for industrial robotics. Bridging research, education,
        and real-world engineering.
      </p>

      <div class="flex flex-wrap gap-3 mb-10">
        {["RAG & LLMs", "Computer Vision", "AI Agents & MCP", "Data Engineering", "Deep Learning"].map(tag => (
          <span class="px-3 py-1 text-sm rounded-full border border-accent/30 text-accent bg-accent/5">
            {tag}
          </span>
        ))}
      </div>

      <div class="flex flex-wrap items-center gap-4">
        <a
          href="#projects"
          class="px-6 py-3 bg-accent text-bg-primary font-semibold rounded-lg hover:bg-accent-hover transition-colors"
        >
          View My Work
        </a>
        <a
          href="#contact"
          class="px-6 py-3 border border-border text-text-primary rounded-lg hover:border-accent hover:text-accent transition-colors"
        >
          Get in Touch
        </a>
      </div>

      <!-- Social links -->
      <div class="flex items-center gap-5 mt-10">
        {socialLinks.map(link => (
          <a
            href={link.href}
            target="_blank"
            rel="noopener noreferrer"
            class="text-text-muted hover:text-accent transition-colors"
            aria-label={link.label}
          >
            {link.icon === "github" && (
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
            )}
            {link.icon === "linkedin" && (
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
            )}
            {link.icon === "huggingface" && (
              <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0C5.373 0 0 5.373 0 12s5.373 12 12 12 12-5.373 12-12S18.627 0 12 0zm-1.5 6.5a1.5 1.5 0 110 3 1.5 1.5 0 010-3zm3 0a1.5 1.5 0 110 3 1.5 1.5 0 010-3zM7 14s1.5 3 5 3 5-3 5-3H7z"/></svg>
            )}
          </a>
        ))}
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Update index.astro to include Hero**

Update `src/pages/index.astro`:
```astro
---
import BaseLayout from "../layouts/BaseLayout.astro";
import Header from "../components/Header.astro";
import Hero from "../components/Hero.astro";
---

<BaseLayout>
  <Header />
  <main class="pt-16">
    <Hero />
  </main>
</BaseLayout>
```

- [ ] **Step 3: Visual verification**

Run `npx astro dev --port 3000` and verify:
- Dark background with subtle gradient
- Name displayed large with "Giocrisrai" in amber
- Skill tags visible
- Two CTAs working (scroll links)
- Social icons linking correctly
- Responsive on mobile

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "feat: add hero section with headline, skill tags, and CTAs"
```

---

### Task 4: Value Proposition Section

**Files:**
- Create: `src/components/ValueProp.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create ValueProp.astro**

Create `src/components/ValueProp.astro`:
```astro
---
const expertise = [
  {
    title: "AI Systems & RAG",
    description: "Design and deploy retrieval-augmented generation pipelines, vector search systems (Pinecone, FAISS, Weaviate), and LLM-powered applications. From embeddings to production inference.",
    tags: ["LLMs", "Vector DBs", "Semantic Search", "Prompt Engineering"],
    icon: "brain"
  },
  {
    title: "Computer Vision & Robotics",
    description: "6-DoF pose estimation with Transformers and Diffusion Models for industrial robotics. Visual recognition systems using deep learning architectures (CNN, Transformers).",
    tags: ["PyTorch", "Transformers", "Diffusion Models", "OpenCV"],
    icon: "eye"
  },
  {
    title: "Data Engineering & Pipelines",
    description: "Build modular, reproducible ML pipelines with Kedro and Apache Airflow. ETL/ELT workflows, data versioning, and deployment strategies from prototype to production.",
    tags: ["Kedro", "Airflow", "Docker", "FastAPI"],
    icon: "pipeline"
  },
  {
    title: "AI for Education & Research",
    description: "Created krinoInsight — generative AI for automated grading with effective feedback. Research in applied AI, instructional design, and AI-driven educational tools.",
    tags: ["Generative AI", "EdTech", "Applied Research", "LLMs"],
    icon: "academic"
  }
];
---

<section id="about" class="py-24">
  <div class="max-w-6xl mx-auto px-6">
    <div class="mb-16">
      <h2 class="text-3xl font-bold mb-4">What I Bring</h2>
      <p class="text-text-secondary max-w-2xl">
        End-to-end AI engineering across research, industry, and education. Not just models — complete systems that solve real problems.
      </p>
    </div>

    <div class="grid md:grid-cols-2 gap-6">
      {expertise.map(item => (
        <div class="group p-6 rounded-xl border border-border bg-bg-card hover:border-accent/50 transition-all duration-300">
          <div class="flex items-start gap-4">
            <div class="w-10 h-10 rounded-lg bg-accent/10 flex items-center justify-center shrink-0 group-hover:bg-accent/20 transition-colors">
              {item.icon === "brain" && (
                <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M9.75 3.104v5.714a2.25 2.25 0 01-.659 1.591L5 14.5M9.75 3.104c-.251.023-.501.05-.75.082m.75-.082a24.301 24.301 0 014.5 0m0 0v5.714c0 .597.237 1.17.659 1.591L19.8 15.3M14.25 3.104c.251.023.501.05.75.082M19.8 15.3l-1.57.393A9.065 9.065 0 0112 15a9.065 9.065 0 00-6.23.693L5 14.5m14.8.8l1.402 1.402c1.232 1.232.65 3.318-1.067 3.611l-.772.13c-1.052.177-2.131.177-3.183 0l-.772-.13c-1.717-.293-2.3-2.379-1.067-3.61L15.2 15.3M5 14.5l-1.402 1.402c-1.232 1.232-.65 3.318 1.067 3.611l.772.13c1.052.177 2.131.177 3.183 0l.772-.13c1.717-.293 2.3-2.379 1.067-3.61L9.8 15.3" /></svg>
              )}
              {item.icon === "eye" && (
                <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M2.036 12.322a1.012 1.012 0 010-.639C3.423 7.51 7.36 4.5 12 4.5c4.638 0 8.573 3.007 9.963 7.178.07.207.07.431 0 .639C20.577 16.49 16.64 19.5 12 19.5c-4.638 0-8.573-3.007-9.963-7.178z" /><path stroke-linecap="round" stroke-linejoin="round" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" /></svg>
              )}
              {item.icon === "pipeline" && (
                <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M20.25 6.375c0 2.278-3.694 4.125-8.25 4.125S3.75 8.653 3.75 6.375m16.5 0c0-2.278-3.694-4.125-8.25-4.125S3.75 4.097 3.75 6.375m16.5 0v11.25c0 2.278-3.694 4.125-8.25 4.125s-8.25-1.847-8.25-4.125V6.375m16.5 0v3.75m-16.5-3.75v3.75m16.5 0v3.75C20.25 16.153 16.556 18 12 18s-8.25-1.847-8.25-4.125v-3.75m16.5 0c0 2.278-3.694 4.125-8.25 4.125s-8.25-1.847-8.25-4.125" /></svg>
              )}
              {item.icon === "academic" && (
                <svg class="w-5 h-5 text-accent" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M4.26 10.147a60.436 60.436 0 00-.491 6.347A48.627 48.627 0 0112 20.904a48.627 48.627 0 018.232-4.41 60.46 60.46 0 00-.491-6.347m-15.482 0a50.57 50.57 0 00-2.658-.813A59.905 59.905 0 0112 3.493a59.902 59.902 0 0110.399 5.84c-.896.248-1.783.52-2.658.814m-15.482 0A50.697 50.697 0 0112 13.489a50.702 50.702 0 017.74-3.342M6.75 15a.75.75 0 100-1.5.75.75 0 000 1.5zm0 0v-3.675A55.378 55.378 0 0112 8.443m-7.007 11.55A5.981 5.981 0 006.75 15.75v-1.5" /></svg>
              )}
            </div>
            <div>
              <h3 class="text-lg font-semibold mb-2 group-hover:text-accent transition-colors">{item.title}</h3>
              <p class="text-text-secondary text-sm leading-relaxed mb-3">{item.description}</p>
              <div class="flex flex-wrap gap-2">
                {item.tags.map(tag => (
                  <span class="text-xs px-2 py-0.5 rounded bg-bg-primary text-text-muted">{tag}</span>
                ))}
              </div>
            </div>
          </div>
        </div>
      ))}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add ValueProp to index.astro**

Add import and usage after Hero in `src/pages/index.astro`.

- [ ] **Step 3: Visual verification + commit**

```bash
git add -A
git commit -m "feat: add value proposition section with 4 expertise blocks"
```

---

### Task 5: Featured Projects Section

**Files:**
- Create: `src/components/Projects.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Projects.astro**

Create `src/components/Projects.astro`:
```astro
---
const projects = [
  {
    title: "krinoInsight",
    category: "AI for Education",
    description: "Generative AI-powered software for automated test correction that provides effective, personalized feedback to students while significantly reducing instructor workload. Built for and used at Duoc UC.",
    tags: ["Generative AI", "LLMs", "EdTech", "Python"],
    highlight: "Reducing grading workload for educators"
  },
  {
    title: "6-DoF Pose Estimation for Robotic Bin Picking",
    category: "Computer Vision & Robotics",
    description: "Master's thesis research integrating FoundationPose (Transformers with cross-attention on SE(3)), GDR-Net++ baseline, and Diffusion Policy for multi-modal grasp trajectory generation. Full pipeline from perception to motion planning.",
    tags: ["Transformers", "Diffusion Models", "PyTorch", "ROS 2", "Lie Groups"],
    link: "https://github.com/Giocrisrai/pose6dof-transformers-diffusion",
    highlight: "MSc Thesis — UNIR 2026"
  },
  {
    title: "Chilean Sign Language + Canal 13",
    category: "Computer Vision & NLP",
    description: "Prototype for generating realistic human-like avatars to translate Chilean Sign Language for TV broadcasts. Collaboration between Duoc UC and Canal 13 to improve accessibility across programming.",
    tags: ["Computer Vision", "Deep Learning", "Avatar Generation", "Accessibility"],
    highlight: "Industry-academia collaboration"
  },
  {
    title: "ChatPDFGio",
    category: "RAG & LLMs",
    description: "Intelligent chatbot application for uploading and conversing with PDF documents. FastAPI backend for processing with Streamlit frontend, containerized with Docker.",
    tags: ["RAG", "FastAPI", "LangChain", "Docker", "Streamlit"],
    link: "https://github.com/Giocrisrai/chatpdfgio",
    highlight: "Full-stack RAG application"
  },
  {
    title: "Mining QA Pipelines at Alaya",
    category: "Data Science & MLOps",
    description: "Led development of QA model evaluation pipelines for mining operations using Kedro framework. Built robust databases, deployment packaging, and model optimization. Integrated generative AI to enhance solution value.",
    tags: ["Kedro", "Flask", "ML Deployment", "Mining", "Python"],
    highlight: "Production ML in mining industry"
  }
];
---

<section id="projects" class="py-24 bg-bg-secondary/50">
  <div class="max-w-6xl mx-auto px-6">
    <div class="mb-16">
      <h2 class="text-3xl font-bold mb-4">Featured Projects</h2>
      <p class="text-text-secondary max-w-2xl">
        Real-world AI systems across industry, education, and research — each solving a concrete problem.
      </p>
    </div>

    <div class="space-y-6">
      {projects.map((project, i) => (
        <div class="group p-6 md:p-8 rounded-xl border border-border bg-bg-card hover:border-accent/50 transition-all duration-300">
          <div class="flex flex-col md:flex-row md:items-start gap-4 md:gap-8">
            <div class="md:w-48 shrink-0">
              <span class="text-xs text-accent font-medium uppercase tracking-wider">{project.category}</span>
              <p class="text-xs text-text-muted mt-1">{project.highlight}</p>
            </div>
            <div class="flex-1">
              <div class="flex items-start justify-between gap-4">
                <h3 class="text-xl font-semibold group-hover:text-accent transition-colors">{project.title}</h3>
                {project.link && (
                  <a href={project.link} target="_blank" rel="noopener noreferrer" class="text-text-muted hover:text-accent transition-colors shrink-0" aria-label={`View ${project.title} on GitHub`}>
                    <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
                  </a>
                )}
              </div>
              <p class="text-text-secondary text-sm leading-relaxed mt-2 mb-4">{project.description}</p>
              <div class="flex flex-wrap gap-2">
                {project.tags.map(tag => (
                  <span class="text-xs px-2 py-0.5 rounded bg-accent/10 text-accent/80">{tag}</span>
                ))}
              </div>
            </div>
          </div>
        </div>
      ))}
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Projects to index.astro**

- [ ] **Step 3: Visual verification + commit**

```bash
git add -A
git commit -m "feat: add featured projects section with 5 real-world projects"
```

---

### Task 6: Experience Timeline

**Files:**
- Create: `src/components/Experience.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Experience.astro**

Create `src/components/Experience.astro`:
```astro
---
const experiences = [
  {
    period: "Mar 2024 — Present",
    roles: [
      {
        title: "University Professor & Researcher",
        company: "Duoc UC",
        location: "Puerto Montt, Chile",
        description: "Teaching Data Mining, Machine Learning, and Deep Learning. Developing krinoInsight (AI-powered grading). Led Chilean Sign Language avatar project with Canal 13. Mentoring robotics (Duckietown). Instructional design for AI curricula.",
        type: "primary"
      },
      {
        title: "Data Engineer",
        company: "MTsolutions — Your Smart Factory",
        location: "Remote",
        description: "Building data infrastructure and optimizing workflows with Apache Airflow. Integrating ML solutions into IIoT manufacturing products across LATAM.",
        type: "secondary"
      }
    ]
  },
  {
    period: "Nov 2022 — Jan 2024",
    roles: [
      {
        title: "Data Scientist",
        company: "Alaya Digital Solutions",
        location: "Chile",
        description: "Led QA model evaluations for mining operations with Kedro pipelines. Built databases, deployed ML models via Flask, and integrated generative AI into production solutions.",
        type: "primary"
      }
    ]
  },
  {
    period: "Mar 2019 — Nov 2022",
    roles: [
      {
        title: "Development Engineer",
        company: "GEOGAMA",
        location: "Puerto Montt, Chile",
        description: "Led digital transformation and technology strategy. Process automation, agile methodologies, data science, AI R&D, and environmental technology startup development.",
        type: "primary"
      }
    ]
  },
  {
    period: "Nov 2017 — Mar 2019",
    roles: [
      {
        title: "Continuous Improvement Engineer",
        company: "Rheem Chilena",
        location: "Santiago, Chile",
        description: "Kaizen philosophy, Lean Manufacturing, electronic automation, TPM, and 5S for metalworking manufacturing processes.",
        type: "primary"
      }
    ]
  }
];
---

<section id="experience" class="py-24">
  <div class="max-w-6xl mx-auto px-6">
    <div class="mb-16">
      <h2 class="text-3xl font-bold mb-4">Experience</h2>
      <p class="text-text-secondary max-w-2xl">
        From manufacturing floors to AI research labs — a trajectory built on solving real problems with technology.
      </p>
    </div>

    <div class="relative">
      <!-- Timeline line -->
      <div class="absolute left-0 md:left-32 top-0 bottom-0 w-px bg-border"></div>

      <div class="space-y-12">
        {experiences.map(exp => (
          <div class="relative flex flex-col md:flex-row gap-4 md:gap-8">
            <!-- Period -->
            <div class="md:w-32 shrink-0 text-right">
              <span class="text-sm text-text-muted font-medium">{exp.period}</span>
            </div>

            <!-- Dot -->
            <div class="absolute left-0 md:left-32 w-3 h-3 rounded-full bg-accent border-2 border-bg-primary -translate-x-1.5 mt-1.5 hidden md:block"></div>

            <!-- Content -->
            <div class="flex-1 pl-6 md:pl-8 space-y-4">
              {exp.roles.map(role => (
                <div class={`p-5 rounded-lg border ${role.type === 'primary' ? 'border-border bg-bg-card' : 'border-border/50 bg-bg-card/50'}`}>
                  <h3 class="font-semibold">{role.title}</h3>
                  <p class="text-accent text-sm">{role.company} <span class="text-text-muted">· {role.location}</span></p>
                  <p class="text-text-secondary text-sm mt-2 leading-relaxed">{role.description}</p>
                </div>
              ))}
            </div>
          </div>
        ))}
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Add Experience to index.astro**

- [ ] **Step 3: Visual verification + commit**

```bash
git add -A
git commit -m "feat: add experience timeline section"
```

---

### Task 7: Education, Certifications, Contact & Footer

**Files:**
- Create: `src/components/Education.astro`
- Create: `src/components/Contact.astro`
- Create: `src/components/Footer.astro`
- Modify: `src/pages/index.astro` (final assembly)

- [ ] **Step 1: Create Education.astro**

Create `src/components/Education.astro`:
```astro
---
const degrees = [
  {
    title: "MSc Mathematical Engineering & Computation",
    institution: "Universidad Internacional de La Rioja (UNIR)",
    period: "2025 — Present",
    note: "Thesis: 6-DoF Pose Estimation with Transformers & Diffusion Models for Robotic Bin Picking",
    status: "in-progress"
  },
  {
    title: "MSc Artificial Intelligence",
    institution: "Universidad Internacional de La Rioja (UNIR)",
    period: "2020 — 2021",
    note: "Thesis: Experimental AI Prototype for Biogeochemical Models",
    status: "completed"
  },
  {
    title: "Mechatronics Engineering",
    institution: "",
    period: "",
    note: "Electronics, automation, robotics, and control systems",
    status: "completed"
  }
];

const certifications = [
  { name: "Azure AI Fundamentals", issuer: "Microsoft", year: "2025" },
  { name: "Generative AI with Large Language Models", issuer: "DeepLearning.AI + AWS", year: "2025" },
  { name: "Visual Recognition with Deep Learning", issuer: "Universidad de Chile", year: "2025" },
  { name: "Generative AI Applied to Education", issuer: "UNIR", year: "2025" },
  { name: "Generative AI Applications with RAG & LangChain", issuer: "IBM", year: "2025" },
  { name: "Data Manipulation at Scale", issuer: "University of Washington", year: "2025" },
  { name: "Applied Research & Innovation Diploma", issuer: "Duoc UC (110h, grade 7.0/7.0)", year: "2025" },
  { name: "Reinforcement Learning Algorithms", issuer: "Pluralsight", year: "2023" },
];
---

<section id="education" class="py-24 bg-bg-secondary/50">
  <div class="max-w-6xl mx-auto px-6">
    <div class="mb-16">
      <h2 class="text-3xl font-bold mb-4">Education & Credentials</h2>
      <p class="text-text-secondary max-w-2xl">
        Continuous learning across AI, mathematics, and engineering — backed by 135+ certifications.
      </p>
    </div>

    <div class="grid md:grid-cols-2 gap-12">
      <!-- Degrees -->
      <div>
        <h3 class="text-lg font-semibold mb-6 text-accent">Degrees</h3>
        <div class="space-y-6">
          {degrees.map(deg => (
            <div class="relative pl-6 border-l-2 border-border">
              <div class="absolute -left-1.5 top-1.5 w-3 h-3 rounded-full bg-accent"></div>
              <div class="flex items-center gap-2 mb-1">
                <h4 class="font-semibold text-sm">{deg.title}</h4>
                {deg.status === "in-progress" && (
                  <span class="text-xs px-2 py-0.5 rounded-full bg-accent/10 text-accent">In Progress</span>
                )}
              </div>
              {deg.institution && <p class="text-text-secondary text-sm">{deg.institution}</p>}
              {deg.period && <p class="text-text-muted text-xs">{deg.period}</p>}
              <p class="text-text-muted text-xs mt-1 italic">{deg.note}</p>
            </div>
          ))}
        </div>
      </div>

      <!-- Certifications -->
      <div>
        <h3 class="text-lg font-semibold mb-6 text-accent">Key Certifications</h3>
        <div class="space-y-3">
          {certifications.map(cert => (
            <div class="flex items-start justify-between gap-4 py-2 border-b border-border/50">
              <div>
                <p class="text-sm font-medium">{cert.name}</p>
                <p class="text-xs text-text-muted">{cert.issuer}</p>
              </div>
              <span class="text-xs text-text-muted shrink-0">{cert.year}</span>
            </div>
          ))}
        </div>
        <a
          href="https://www.linkedin.com/in/giocrisrai/details/certifications/"
          target="_blank"
          rel="noopener noreferrer"
          class="inline-flex items-center gap-1 mt-4 text-sm text-accent hover:text-accent-hover transition-colors"
        >
          View all 135+ certifications on LinkedIn
          <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M13.5 6H5.25A2.25 2.25 0 003 8.25v10.5A2.25 2.25 0 005.25 21h10.5A2.25 2.25 0 0018 18.75V10.5m-10.5 6L21 3m0 0h-5.25M21 3v5.25" /></svg>
        </a>
      </div>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Create Contact.astro**

Create `src/components/Contact.astro`:
```astro
<section id="contact" class="py-24">
  <div class="max-w-6xl mx-auto px-6 text-center">
    <h2 class="text-3xl font-bold mb-4">Let's Work Together</h2>
    <p class="text-text-secondary max-w-xl mx-auto mb-10">
      Open to global opportunities in AI engineering, research collaborations, and consulting. Let's build something meaningful.
    </p>

    <div class="flex flex-wrap justify-center gap-4 mb-10">
      <a
        href="mailto:contact@giocrisrai.com"
        class="px-6 py-3 bg-accent text-bg-primary font-semibold rounded-lg hover:bg-accent-hover transition-colors"
      >
        contact@giocrisrai.com
      </a>
      <a
        href="https://www.linkedin.com/in/giocrisrai/"
        target="_blank"
        rel="noopener noreferrer"
        class="px-6 py-3 border border-border text-text-primary rounded-lg hover:border-accent hover:text-accent transition-colors"
      >
        LinkedIn Profile
      </a>
    </div>

    <div class="flex justify-center gap-6">
      <a href="https://github.com/Giocrisrai" target="_blank" rel="noopener noreferrer" class="text-text-muted hover:text-accent transition-colors" aria-label="GitHub">
        <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0c-6.626 0-12 5.373-12 12 0 5.302 3.438 9.8 8.207 11.387.599.111.793-.261.793-.577v-2.234c-3.338.726-4.033-1.416-4.033-1.416-.546-1.387-1.333-1.756-1.333-1.756-1.089-.745.083-.729.083-.729 1.205.084 1.839 1.237 1.839 1.237 1.07 1.834 2.807 1.304 3.492.997.107-.775.418-1.305.762-1.604-2.665-.305-5.467-1.334-5.467-5.931 0-1.311.469-2.381 1.236-3.221-.124-.303-.535-1.524.117-3.176 0 0 1.008-.322 3.301 1.23.957-.266 1.983-.399 3.003-.404 1.02.005 2.047.138 3.006.404 2.291-1.552 3.297-1.23 3.297-1.23.653 1.653.242 2.874.118 3.176.77.84 1.235 1.911 1.235 3.221 0 4.609-2.807 5.624-5.479 5.921.43.372.823 1.102.823 2.222v3.293c0 .319.192.694.801.576 4.765-1.589 8.199-6.086 8.199-11.386 0-6.627-5.373-12-12-12z"/></svg>
      </a>
      <a href="https://www.linkedin.com/in/giocrisrai/" target="_blank" rel="noopener noreferrer" class="text-text-muted hover:text-accent transition-colors" aria-label="LinkedIn">
        <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24"><path d="M20.447 20.452h-3.554v-5.569c0-1.328-.027-3.037-1.852-3.037-1.853 0-2.136 1.445-2.136 2.939v5.667H9.351V9h3.414v1.561h.046c.477-.9 1.637-1.85 3.37-1.85 3.601 0 4.267 2.37 4.267 5.455v6.286zM5.337 7.433c-1.144 0-2.063-.926-2.063-2.065 0-1.138.92-2.063 2.063-2.063 1.14 0 2.064.925 2.064 2.063 0 1.139-.925 2.065-2.064 2.065zm1.782 13.019H3.555V9h3.564v11.452zM22.225 0H1.771C.792 0 0 .774 0 1.729v20.542C0 23.227.792 24 1.771 24h20.451C23.2 24 24 23.227 24 22.271V1.729C24 .774 23.2 0 22.222 0h.003z"/></svg>
      </a>
      <a href="https://huggingface.co/giocrisrai" target="_blank" rel="noopener noreferrer" class="text-text-muted hover:text-accent transition-colors" aria-label="Hugging Face">
        <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 24 24"><path d="M12 0C5.373 0 0 5.373 0 12s5.373 12 12 12 12-5.373 12-12S18.627 0 12 0zm-1.5 6.5a1.5 1.5 0 110 3 1.5 1.5 0 010-3zm3 0a1.5 1.5 0 110 3 1.5 1.5 0 010-3zM7 14s1.5 3 5 3 5-3 5-3H7z"/></svg>
      </a>
    </div>
  </div>
</section>
```

- [ ] **Step 3: Create Footer.astro**

Create `src/components/Footer.astro`:
```astro
<footer class="py-8 border-t border-border">
  <div class="max-w-6xl mx-auto px-6 flex flex-col sm:flex-row items-center justify-between gap-4">
    <p class="text-text-muted text-sm">
      &copy; 2026 Giocrisrai Godoy. Built with Astro.
    </p>
    <p class="text-text-muted text-sm">
      Chile · Open to global opportunities
    </p>
  </div>
</footer>
```

- [ ] **Step 4: Assemble final index.astro**

Update `src/pages/index.astro`:
```astro
---
import BaseLayout from "../layouts/BaseLayout.astro";
import Header from "../components/Header.astro";
import Hero from "../components/Hero.astro";
import ValueProp from "../components/ValueProp.astro";
import Projects from "../components/Projects.astro";
import Experience from "../components/Experience.astro";
import Education from "../components/Education.astro";
import Contact from "../components/Contact.astro";
import Footer from "../components/Footer.astro";
---

<BaseLayout>
  <Header />
  <main>
    <Hero />
    <ValueProp />
    <Projects />
    <Experience />
    <Education />
    <Contact />
  </main>
  <Footer />
</BaseLayout>
```

- [ ] **Step 5: Clean up public/ directory**

Remove old images that are not needed:
```bash
rm -f public/images/1.jpg public/images/2.jpg ... public/images/13.jpg
rm -f public/images/*_thumb.jpg
rm -f public/images/aubrey_hoover.jpg public/images/deegan_wallace.jpg public/images/sean_salazar.jpg
rm -f public/images/exto_preview.png public/images/thank-you.jpg
rm -f public/js/plugins.js
```

- [ ] **Step 6: Full build verification**

```bash
npx astro build
```
Expected: Clean build, dist/ directory with index.html

- [ ] **Step 7: Dev server visual check**

```bash
npx astro dev --port 3000
```
Verify all sections render, smooth scroll works, responsive on mobile.

- [ ] **Step 8: Commit**

```bash
git add -A
git commit -m "feat: add education, contact, footer sections — complete portfolio"
```

---

### Task 8: Final Polish & Deploy Readiness

**Files:**
- Modify: `README.md`
- Modify: `.gitignore`
- Verify: `netlify.toml`

- [ ] **Step 1: Update .gitignore for Astro**

Ensure `.gitignore` includes:
```
node_modules/
dist/
.astro/
```

- [ ] **Step 2: Update README.md**

Brief README with project info and how to run locally.

- [ ] **Step 3: Final build + verify**

```bash
npx astro build && ls -la dist/
```

- [ ] **Step 4: Final commit**

```bash
git add -A
git commit -m "chore: update README, gitignore, and deploy config for Astro"
```
