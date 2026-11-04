# Playjeux Studios — Under Maintenance

A barebones, fully-static single-page "under maintenance" site built with
**SvelteKit 3 + `@sveltejs/adapter-static`** (Svelte 5, Vite 8), deployed as
**static pages on Cloudflare Pages**. JS source with JSDoc typings (see
`jsconfig.json`). Build requires **Node ≥ 22.17** (SvelteKit 3 engines).

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
   - **Node.js version:** 22 **or newer** (SvelteKit 3 needs ≥ 22.17)
4. Save & deploy. Cloudflare runs the build and serves `build/` statically.

### Option B — wrangler CLI
```sh
npm run build
npx wrangler pages deploy build --project-name=playjeux-website
```
(follow the prompts; append `--branch=main` / `--commit-dirty=true` as needed)

Note: since this ships a real prerendered `index.html`, no SPA fallback is
required — unknown paths correctly return 404 (fine for a maintenance page).

## Dependencies & audit

- Stack: SvelteKit `3.x`, Svelte `5.x`, Vite `8.x`, `adapter-static` `4.x`,
  TypeScript `6.x` (engine for the JSDoc/svelte-check checks).
- `npm audit` is limited to one **low** advisory: `deepmerge` prototype
  pollution, used at **build time only** by `vite-plugin-svelte`. There is no
  published fix (latest is `4.3.1`), and it is **not** shipped in the `build/`
  output, so we leave it as-is.
- The project `.npmrc` keeps `min-release-age=14` (policy — not modified).
  To stay within it, direct deps are pinned to the newest releases that are
  at least 14 days old: `@sveltejs/kit@3.1.0`, `adapter-static@4.0.0`,
  `svelte@5.57.2`, `svelte-check@4.7.7` (newer patches exists but are
  excluded by the policy). Kit 3.1.0 still resolves the SvelteKit <3.0.0
  security advisories.