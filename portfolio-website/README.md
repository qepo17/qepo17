# Personal Portfolio

This repository contains the source code for my personal portfolio website built with Astro.

## 🌐 Live Site

Visit [taufiqseptryana.com](https://taufiqseptryana.com) to see the live portfolio.

## 🚀 Tech Stack

- **Framework**: Astro
- **Styling**: Tailwind CSS
- **Deployment**: Cloudflare Pages
- **Content Management**: Markdown-based blog system
- **Language**: TypeScript

## 📁 Project Structure

```
qepo17/
└── portfolio-website/        # Astro portfolio website
    ├── src/
    │   ├── components/       # Reusable UI components
    │   ├── content/         # Blog posts and content
    │   ├── layouts/         # Page layouts
    │   └── pages/           # Routes and pages
    ├── public/              # Static assets
    ├── astro.config.mjs     # Astro configuration
    ├── tailwind.config.mjs  # Tailwind CSS config
    └── package.json         # Dependencies
```

## 🛠️ Development

### Prerequisites

- Node.js (v18+)
- npm or pnpm

### Getting Started

```bash
cd portfolio-website
npm install
npm run dev
```

Visit `http://localhost:4321` to view the development server.

### Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build

## 🚀 Deployment

The site is automatically deployed to Cloudflare Pages when changes are pushed to the main branch.

## 📝 Blog

The blog is powered by Astro's content collections with Markdown support. Blog posts are located in `src/content/blog/`.