# Decisions

Numbered, permanent records of calls about this library that foreclose an
alternative. Never renumber or delete a file — if a decision is reversed,
add a new numbered file and point the old one's `Status` line at it.

## Template

```markdown
# NNNN: Short title

**Status:** Active (or: Superseded by [NNNN](./NNNN-slug.md))

**Decision:** One or two sentences — what is now true.

**Why:** The actual reasons. Name the concrete case that prompted it if
there was one.

**Rules out:** What this forecloses — the alternative someone might
otherwise reach for later.

**See also:** Links to related decisions, or to a fuller writeup elsewhere
in the vault.
```

## Index

| # | Title | Status |
|---|---|---|
| [0001](./0001-bracket-placeholder-convention.md) | Templates use bracket placeholders, not duplicated examples | Superseded by 0008 |
| [0002](./0002-generalize-not-delete.md) | Narrow single-project skills get generalized to their reusable pattern, not deleted | Active |
| [0003](./0003-linear-stays-concrete.md) | `linear-dev-workflow` stays a real, usable Linear skill instead of a generic tracker template | Active |
| [0004](./0004-vault-scoped-to-this-repo.md) | This repo's `vault/` documents the meta-project, not a stand-in for the vault pattern the templates describe | Active |
| [0005](./0005-plugin-layout.md) | The repo is a Claude Code plugin; templates live in `templates/`, outside the plugin's component folders | Superseded by 0008 |
| [0006](./0006-tracker-chosen-at-bootstrap.md) | The issue tracker is chosen in the bootstrap interview; each tracker skill stays concrete | Active |
| [0007](./0007-github-flow-for-this-repo.md) | This repo itself uses GitHub Issues and pull requests | Active |
| [0008](./0008-per-project-plugins-read-project-files.md) | Skills ship in per-project plugins and read project values from the project's files | Active |
| [0009](./0009-bootstrap-copies-into-the-project.md) | Bootstrap copies skills and agents into the project by default; plugin install is the option | Superseded by 0010 |
| [0010](./0010-plugin-install-is-the-default.md) | Bootstrap enables plugins by default; copying into the project is the option | Active |
