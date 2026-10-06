---
name: web-deploy
description: Use when setting up, running or fixing a deploy of a web project to a platform (Vercel, Netlify, Cloudflare Pages) or as a container (Fly.io, Railway, a VPS with Docker), including preview deploys per PR, environment variables and secrets, custom domains, rollbacks and post-deploy checks. For a static export uploaded over FTP, use static-site-deploy instead.
---

# Web deploy

## Project facts

The `## Deploy` section of `DOCS/running.md` (`DOCS` is the docs folder the
project's `CLAUDE.md` names, usually `vault/`) holds the platform, the
project/app name on it, the production domain, the production branch, which
environment variables exist (names only, never values) and where they are
set. The build command is in `## Build`. If the section is missing or thin,
ask the user once and write the answers there. Anything learned while
deploying (a flag that was needed, a gotcha) goes back into that section.

## Ground rules

- **Production deploys are the user's call** unless `CLAUDE.md` says Claude
  may deploy. Preview deploys are fine to run whenever they help verify a
  change.
- **Secrets never touch the repo.** Values go into the platform's env
  settings or the CLI's secret command; the repo gets a `.env.example` with
  names and a comment on where each comes from. Never print a secret's value
  in the terminal or a PR.
- **Prefer the platform's git integration** over deploying from a laptop:
  every PR gets a preview URL and `main` deploys to production. Then a
  deploy is a merge, and a rollback is a click.

## Platforms

### Vercel

- Link once: `vercel link`. Preview: `vercel`. Production: `vercel --prod`.
- Env: `vercel env add NAME production` (and `preview`, `development`);
  `vercel env pull .env.local` for local dev.
- Domain: `vercel domains add example.com`, then the DNS records it prints.
- Rollback: `vercel rollback` (to the previous production deploy) or
  `vercel promote <deployment-url>`.

### Netlify

- Link once: `netlify link`. Draft deploy: `netlify deploy`. Production:
  `netlify deploy --prod`. Build output folder from `running.md`.
- Env: `netlify env:set NAME value --context production`.
- Redirects and headers: `netlify.toml` or `_redirects` in the publish folder.
- Rollback: Deploys → pick an earlier deploy → "Publish deploy".

### Cloudflare Pages

- Deploy a build folder: `npx wrangler pages deploy <dir> --project-name <name>`;
  a non-production `--branch` gives a preview URL.
- Secrets: `npx wrangler pages secret put NAME --project-name <name>`.
- Functions/SSR adapters (Next.js, SvelteKit, Astro) need the framework's
  Cloudflare adapter configured first; check it's in the build.
- Rollback: Deployments → an earlier deployment → "Rollback to this deployment".

### Containers (Fly.io, Railway, a VPS)

- A multi-stage `Dockerfile`: install and build in one stage, copy only the
  output and production dependencies into a slim runtime stage, run as a
  non-root user, expose one port, and add a health check endpoint.
- Tag images with the git SHA, never only `latest`, so a rollback is
  redeploying the previous tag.
- Fly.io: `fly launch` once, then `fly deploy`; secrets with
  `fly secrets set NAME=value`; rollback with
  `fly deploy --image <previous-image>`.
- VPS: `docker compose up -d --build` behind a reverse proxy (Caddy gives
  automatic HTTPS); keep the compose file in the repo, secrets in a `.env`
  on the server only.

## Steps for a deploy

1. **Build locally first** with the command from `running.md`. A build that
   fails here fails there, slower.
2. **Check env:** every name in `.env.example` exists for the target
   environment. Missing ones are the most common "works locally" failure.
3. **Deploy a preview**, and check it before production (step 5).
4. **Production**: through a merge to the production branch, or the
   platform's prod command when `CLAUDE.md` allows it.
5. **Check the live deploy:** the pages load over HTTPS on the real domain,
   forms and API routes work end to end, no console errors. Run
   `visual-check` against the preview or live URL if the change touched UI,
   and the `seo-a11y-auditor` against live URLs after a launch.
6. **Report** the URL, what was checked and how to roll back.

## When a deploy fails

Read the platform's build log from the top error, not the last line. The
usual causes, in order: a missing env var, a Node version mismatch (pin it
in `package.json` `engines` or the platform's settings), a build command or
output folder that differs from the platform's default, a case-sensitive
import that only works on macOS/Windows.
