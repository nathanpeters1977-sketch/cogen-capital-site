# Cogen Capital marketing site

Static marketing site for [Cogen Capital](https://www.cogen-capital.com) — radical transparency, public investment rationale, and aligned investing. Built with [Astro](https://astro.build) (TypeScript), designed for Cloudflare Pages.

## Local development

Requires **Node 22+**.

```bash
# If you use nvm:
unset NPM_CONFIG_PREFIX && export NVM_DIR="$HOME/.nvm" && . "$NVM_DIR/nvm.sh" && nvm use 22

npm install
npm run dev      # http://localhost:4321
npm run build    # output → dist/
npm run preview  # preview production build
```

## Deploy on Cloudflare Pages

| Setting | Value |
| --- | --- |
| Framework preset | Astro |
| Build command | `npm run build` |
| Build output directory | `dist` |
| Node version | `22` (set `NODE_VERSION=22` in Pages environment variables if needed) |

Connect the Git repo (or upload `dist`), then trigger a production deploy.

## Domain cutover from Wix

1. In **Cloudflare Pages** → your project → **Custom domains**, add `www.cogen-capital.com` (and apex if desired).
2. At the registrar or in **Wix DNS**, set:
   - **www** → `CNAME` to the Pages hostname Cloudflare shows (e.g. `cogen-capital-site.pages.dev`).
3. **Apex** (`cogen-capital.com`):
   - Prefer moving DNS to Cloudflare and using the A/AAAA (or CNAME flattening) records Pages provides, **or**
   - Keep DNS at the registrar and add the A/AAAA records Cloudflare Pages lists for the apex.
4. **Email**: leave MX / SPF / DKIM / DMARC untouched if mail still runs on Wix, Google Workspace, or another provider. Only change web host records (`A`/`AAAA`/`CNAME` for the site).
5. After DNS propagates, confirm HTTPS on www, then optionally redirect apex → www in Cloudflare.

## Contact form (Formspree)

The contact form posts to:

```text
https://formspree.io/f/mqpkvkyl
```

1. Create a form at [formspree.io](https://formspree.io).
2. Replace `mqpkvkyl` in `src/pages/index.astro` with your real form ID.
3. Redeploy.

## Cost note

Cloudflare Pages free tier + a domain typically **~$10–15/year**, versus ongoing Wix hosting. Formspree has a free tier suitable for low enquiry volume.

## Project structure

```text
/
├── public/
│   ├── favicon.ico / favicon.svg
│   └── robots.txt
├── src/
│   ├── layouts/BaseLayout.astro
│   ├── pages/
│   │   ├── index.astro      # single-page home
│   │   └── notes/index.astro
│   └── styles/global.css
├── astro.config.mjs         # site URL + sitemap
└── package.json
```

Sitemap is generated at build time (`@astrojs/sitemap`) and referenced from `robots.txt`.
