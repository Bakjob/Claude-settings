# Running the site

Astro 5, TypeScript (strict), pnpm. The project has no code yet; these
commands apply once it's scaffolded with `pnpm create astro@latest`.

## Setup

```
pnpm install
```

Node 22 (pinned in `package.json` `engines` and in Netlify's settings).

## Dev

```
pnpm dev
```

Serves on http://localhost:4321.

## Build

```
pnpm build      # output in dist/
pnpm preview    # serve the built site locally
```

## Test

No unit or E2E tests. The checks are the build plus:

```
pnpm check      # astro check: TypeScript and template errors
pnpm lint       # eslint
```

## Lint and format

```
pnpm lint
pnpm format     # prettier --write . (also runs after every edit Claude makes)
```

## Smoke test

No smoke test yet. `visual-check` against the main pages covers what a
smoke test would for now.

## Deploy

- **Platform:** Netlify, site `kvarngatans-bageri`, connected to the GitHub
  repo; every push to `main` deploys to production.
- **Domain:** kvarngatansbageri.se (DNS at the registrar, pointed at Netlify).
- **Production branch:** `main`.
- **Environment variables:** none yet. The pickup order form will need
  `ORDER_EMAIL_TO`; add it to `.env.example` and Netlify when it's built.
- **Rollback:** Netlify → Deploys → an earlier deploy → "Publish deploy".
- **After launch:** submit `sitemap-index.xml` in Google Search Console.
