---
name: static-site-deploy
description: Use when preparing or executing a production deploy of a static-export site to shared/FTP-style hosting, or when touching build export settings, redirects/`.htaccess`, or DNS/SSL for the site's domain.
---

# Deploying a static site

This checklist assumes a static export uploaded to shared hosting, e.g. over FTP. Trim it for a platform that handles more of this for you (Vercel, Netlify, Cloudflare Pages) or a container deploy.

## Project facts

The host, the domain, the build command and the upload target are in the `## Deploy` and `## Build` sections of `DOCS/running.md` (`DOCS` is the docs folder the project's `CLAUDE.md` names, usually `vault/`). If they're missing, ask the user once and add them there.

## Prerequisites (verify before deploying)

- Domain registered and DNS pointed at the host. Deploy can fail meaningfully (SSL, routing) if this isn't done first.
- The static export / build adapter is configured correctly for the target host.
- Redirect/rewrite config (`.htaccess`, `_redirects`, etc.) is present and configures: clean URLs, HTTPS redirect (301 http→https), a real 404 handler.
- 404 page exists and has real content, not a stub.

## Deploy steps

1. Run the project's build command to generate the output folder.
2. Upload the build output to the host (e.g. via FTP to `public_html`).
3. Verify the redirect/rewrite config made it into the upload and clean URLs / HTTPS redirect work.
4. Confirm the SSL certificate is active on the domain (often issued automatically once DNS is correct — check the host's control panel if not).
5. Smoke test all pages on both mobile and desktop.
6. Verify any forms / third-party embeds (contact form, booking widget, etc.) actually deliver/work end to end, not just render.

## After go-live

- Submit the sitemap in Google Search Console (or the equivalent `running.md` names) — ownership is often verified via a DNS TXT record through the host's control panel.
- Re-run the `seo-a11y-auditor` against the live URLs, not just local builds — real hosting can change caching/TTFB behavior enough to affect scores.
- Record anything that differed from this checklist in `running.md`'s `## Deploy` section.
