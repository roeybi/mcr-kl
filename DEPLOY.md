# Publishing this site

The repository *is* the app — `index.html` plus its files. Two ways to publish it,
and both give you **one permanent address** that never changes when the app is
updated.

## 1. GitHub Pages — one paste, then automatic forever

1. In this repository: **Add file → Create new file**, set the path to
   `.github/workflows/pages.yml`, paste the contents of [`pages.yml`](./pages.yml),
   and commit it to `main`.
2. **Settings → Pages → Build and deployment → Source: GitHub Actions → Save.**
   The workflow enables Pages for you and publishes the site.
3. Your link is **https://roeybi.github.io/mcr-kl/** — the same link from now on.

After that, every update is just a push to `main`: the workflow rebuilds and
republishes, and the link never moves.

### Why some files look odd

The write path used to publish this build carries text only:

- the binary assets (the self-hosted fonts and the PWA icons) live in `.b64/`
  as base64 and are decoded by the workflow;
- the 470 kB synced-timing pack lives in `data/parts/` in chunks and is
  stitched back together by the workflow.

The result is byte-for-byte the same build that runs on the platform host and
on Netlify — the offline pack, the service worker and the fonts are identical.

## 2. Netlify — 30 seconds

Drag `mcr-lyrics-netlify.zip` into your Netlify site
(**Deploys → pick the site → drag & drop**). The site keeps the address your
Netlify site already has, forever, and every future build is the same one drag.

## Keeping the builds in step

The app folder is the single source of truth; every new build is pushed here
and published from here, so all three addresses run identical code.
