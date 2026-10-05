---
name: static-site-deploy
description: Use when preparing or executing a production deploy of [PROJECT_NAME] to [HOST], or when touching build export settings, redirects/`.htaccess`, or DNS/SSL for [DOMAIN].
---

# Deploying [PROJECT_NAME]

*(This assumes a static-export-to-shared-hosting deploy via FTP, the shape it was written for. Adapt or trim the checklist for a platform that handles more of this for you, e.g. Vercel/Netlify/Cloudflare Pages, or a container deploy.)*

## Prerequisites (verify before deploying)

- Domain registered and DNS pointed at [HOST]. Deploy can fail meaningfully (SSL, routing) if this isn't done first.
- The static export / build adapter is configured correctly for the target host.
- Redirect/rewrite config (`.htaccess`, `_redirects`, etc.) is present and configures: clean URLs, HTTPS redirect (301 http→https), a real 404 handler.
- 404 page exists and has real content, not a stub.

## Deploy steps

1. Run the project's build command to generate the output folder.
2. Upload the build output to the host (e.g. via FTP to `public_html`).
3. Verify the redirect/rewrite config made it into the upload and clean URLs / HTTPS redirect work.
4. Confirm the SSL certificate is active on [DOMAIN] (often issued automatically once DNS is correct — check the host's control panel if not).
5. Smoke test all pages on both mobile and desktop.
6. Verify any forms / third-party embeds (contact form, booking widget, etc.) actually deliver/work end to end, not just render.

## After go-live

- Submit the sitemap in [SEARCH CONSOLE / equivalent] — ownership is often verified via a DNS TXT record through the host's control panel.
- Re-run the project's SEO/accessibility/performance audit against the live URLs, not just local builds — real hosting can change caching/TTFB behavior enough to affect scores.

---
*Template — fill in `[PROJECT_NAME]`, `[HOST]`, and `[DOMAIN]` for the target project.*
