# Playjeux Studios — Under Maintenance

A barebones, fully-static single-page "under maintenance" site built with
**SvelteKit + `@sveltejs/adapter-static`**, deployed as **static pages on
Cloudflare Pages**. JS source with JSDoc typings (see `jsconfig.json`).

## Scripts

| Command          | Action                            |
| ---------------- | --------------------------------- |
| `npm run dev`    | Local dev server                  |
| `npm run build`  | Static build → `build/`           |
| `npm run preview`| Preview the production build      |
| `npm run check`  | Type-check with `svelte-check`    |

## Assets

Drop the logo at `static/playjeux-logo.png` (already present). It is served
from `/playjeux-logo.png` and also used as the favicon. If the image is ever
missing, the page falls back to a styled text wordmark.

## Deploy to Cloudflare Pages (static)

The adapter outputs a **pure static site** to `build/` — no server functions,
so it's served from Cloudflare's static CDN.

### Option A — Git integration (recommended)
1. Push this repo to GitHub/GitLab.
2. Cloudflare dashboard → **Workers & Pages → Create → Pages → Connect to Git**.
3. Select the repo, choose a production branch (`main`), and set:
   - **Framework preset:** `Svelte`
   - **Build command:** `npm run build`
   - **Build output directory:** `build`
   - **Node.js version:** 22
4. Save & deploy. Cloudflare runs the build and serves `build/` statically.

### Option B — wrangler CLI
```sh
npm run build
npx wrangler pages deploy build --project-name=playjeux-website
```
(follow the prompts; append `--branch=main` / `--commit-dirty=true` as needed)

Note: since this ships a real prerendered `index.html`, no SPA fallback is
required — unknown paths correctly return 404 (fine for a maintenance page).