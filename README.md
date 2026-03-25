# mvdriest.nl

Personal portfolio website for [mvdriest.nl](https://mvdriest.nl), built with Nuxt 3.

## Tech stack

- **[Nuxt 3](https://nuxt.com)** – Vue-based framework for static site generation
- **[@nuxt/content](https://content.nuxt.com)** – file-based CMS; project case studies are written in Markdown
- **[@nuxt/image](https://image.nuxt.com)** – optimised image handling
- **[Tailwind CSS v4](https://tailwindcss.com)** – utility-first CSS
- **[GSAP](https://gsap.com)** + **[Lenis](https://lenis.darkroom.engineering)** – animations & smooth scroll
- **[@nuxthq/studio](https://nuxt.studio)** – local content authoring (dev only, excluded from production builds)
- **pnpm** – package manager

## Project structure

```
.
├── app/
│   ├── assets/          # CSS (Tailwind entry, Lenis, loader)
│   ├── components/      # Vue components
│   ├── layouts/         # Nuxt layouts
│   ├── pages/           # File-based routes
│   └── plugins/         # Nuxt plugins
├── content/
│   └── projecten/       # Markdown files – one file per portfolio case
├── public/
│   ├── CNAME            # Custom domain (mvdriest.nl)
│   └── images/          # Static assets (project images, etc.)
├── .github/
│   └── workflows/
│       ├── studio.yml   # Production build & deploy to GitHub Pages (on push to master)
│       ├── preview.yml  # PR preview deployments
│       └── cleanup.yml  # Clean up PR preview deployments after merge/close
├── content.config.ts    # @nuxt/content collection schema
└── nuxt.config.ts       # Nuxt configuration
```

## Local development

Install dependencies:

```bash
pnpm install
```

Start the development server at <http://localhost:3000>:

```bash
pnpm dev
```

Lint:

```bash
pnpm lint
```

## Deployment

### How it works

Pushes to the **`master`** branch trigger the `studio-nuxt-build` GitHub Actions workflow (`.github/workflows/studio.yml`), which:

1. Installs dependencies with pnpm.
2. Runs `nuxt generate` to produce a fully static site in `.output/public/`.
3. Deploys the output to the **`gh-pages`** branch using [JamesIves/github-pages-deploy-action](https://github.com/JamesIves/github-pages-deploy-action).

GitHub Pages then serves the `gh-pages` branch at the custom domain configured in `public/CNAME` (`mvdriest.nl`).

### PR previews

Opening a pull request triggers the `preview.yml` workflow, which builds and deploys the branch to `https://mvdriest.nl/pr-preview/pr-<number>/` and posts the URL as a PR comment. The preview is cleaned up automatically when the PR is closed.

## DNS & domain

| Concern | Provider |
|---|---|
| Domain registrar | **[Strato](https://www.strato.nl)** |
| DNS | **[Cloudflare](https://www.cloudflare.com)** |

The domain `mvdriest.nl` is registered at Strato. Nameservers are pointed to Cloudflare, where DNS records are managed. Cloudflare proxies traffic to GitHub Pages:

- An `A` record (or `CNAME` for `www`) pointing to the GitHub Pages IP addresses / `mvdriest.github.io`.
- The `CNAME` file in the repository root (`public/CNAME`) tells GitHub Pages which custom domain to use.

