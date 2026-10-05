# 0006: The issue tracker is chosen in the bootstrap interview; each tracker skill stays concrete

**Status:** Active

**Decision:** Linear is no longer the assumed tracker. Bootstrap asks:
Linear, GitHub Issues, no tracker, or another tool. Linear installs
`linear-dev-workflow`, GitHub Issues installs the new
`github-issues-workflow` (real `gh` commands, `in progress` / `in review`
labels as states), no tracker replaces the `CLAUDE.md` tracker section with
a todo list in the vault. `CLAUDE-template.md`'s tracker section is now
tracker-neutral.

**Why:** The user asked for Linear not to be hardcoded, and to be asked about
issue handling for each project. Small and game projects often don't want
Linear at all.

**Rules out:** One abstract "generic tracker" skill with tool names
templated away. [0003](./0003-linear-stays-concrete.md)'s reasoning still
holds per tracker: a workflow skill is only useful if its commands are real.
A new tracker gets its own concrete skill instead.

**See also:** [0003](./0003-linear-stays-concrete.md),
[0005](./0005-plugin-layout.md)
