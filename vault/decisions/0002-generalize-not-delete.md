# 0002: Narrow single-project skills get generalized to their reusable pattern, not deleted

**Status:** Active

**Decision:** When a skill was built for one very specific past need (a
Snowberry-AB-style case-study page family, a Loopia-specific deploy), it gets
rewritten to the general pattern underneath it rather than removed —
`case-study-page` became `content-page-family`, `loopia-deploy` became
`static-site-deploy`.

**Why:** The narrow version still encodes a real, useful technique (how to
handle a family of structurally-identical, content-different pages; the
checklist shape a static-hosting deploy actually needs). Deleting it loses
that technique; keeping the original project's specifics makes it dead weight
that doesn't apply anywhere else. Generalizing keeps the useful mechanism
and drops only the one-off details.

**Rules out:** Silently deleting a skill just because it looked too narrow
at first glance — check whether there's a general pattern worth keeping
first.

**See also:** [0001](./0001-bracket-placeholder-convention.md),
[0003](./0003-linear-stays-concrete.md)
