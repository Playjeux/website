# Playjeux Studios — Under Maintenance

A barebones, fully-static single-page "under maintenance" site built with
**SvelteKit 3 + `@sveltejs/adapter-static`** (Svelte 5, Vite 8), deployed as
a **Static Site on DigitalOcean App Platform**. JS source with JSDoc typings
(see `jsconfig.json`). Build requires **Node ≥ 22.17** (SvelteKit 3 engines,
declared in `package.json` `engines`).

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

## Deploy to DigitalOcean App Platform (static)

The adapter outputs a **pure static site** to `build/` — no server functions —
served by App Platform's static hosting behind your existing servers/CDN. A
spec is provided at `.do/app.yaml`.

### Option A — Dashboard (recommended)
1. Push this repo to GitHub.
2. App Platform dashboard → **Create → App → GitHub →** select the repo & branch
   (`main`).
3. As the component type choose **Static Site**. Build settings:
   - **Build command:** `npm run build`
   - **Output directory:** `build`   (this is what gets served)
   - Node 22+ is used via the `node-js` engine slug (respects `engines.node`).
4. Attach the domain(s) already configured in your cluster, then **Deploy**.

### Option B — `doctl` with the app spec
```sh
doctl apps create --spec .do/app.yaml
doctl apps update <APP_ID> --spec .do/app.yaml   # later
```
Edit `repo` in `.do/app.yaml` first.

Notes: `catchall_document: index.html` is enabled in the spec so any path
serves the maintenance page (SPA catch-all) instead of a 404 — remove it if
you'd rather 404 unknown paths.

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