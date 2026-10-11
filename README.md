# @rathnasgala2/theme-default

The default presentation theme for Galascribe portable publications
(`@rathnasgala2/template@^4.0.0`). It is what a build uses when no other theme
is selected. This package is a **closed, passive file set**: JSON and CSS
only, no JavaScript, no build step, no runtime dependency, no lifecycle
script. A missing selected theme fails a build rather than silently changing
appearance.

In generation 4 the template owns page layout and component structure. This
theme selects the Newsletter composition and supplies its established
116-token Default palette plus a small amount of skin CSS.

## Composition

`theme.json` embeds the complete generation-4 Newsletter composition:

- newspaper home
- issue article
- profile author page
- issue listings
- publication header
- expanded footer
- landscape media
- comfortable density

## Files

- `package.json`: exactly `name`, `version`, `license`, `repository`, `files`.
- `theme.json`: validated against `urn:gala:schema:theme-contract:2.0.0`. It
  carries the contract-4 identity and Newsletter composition,
  `templateRange` (`^4.0.0`), the stylesheet/layer/hook projection, both
  palettes for all 116 tokens, asset digests, size budgets, and the
  generation-4 `stylingContractDigest`.
- `tokens.css`: generated from `theme.json` by `tokens:generate`. Never edit
  it by hand; `tokens:check` fails if it drifts from `theme.json`.
- `components.css`: a small skin layer (cards, chips, avatars, strong text)
  that uses tokens only.
- `print.css`: print rules.
- `LICENSE`, `README.md`.

Token values are the owner-approved Default look. `theme.json` is their only
source: edit its `tokens`, then run `tokens:generate` and `digest:generate`;
never edit `tokens.css`.

## Commands

Node 24.18.0 (see `.nvmrc`). `tooling/` is private and never packed; every
gate lives in `@rathnasgala2/theme-tooling` and is reached through
`tooling/run.mjs`.

```sh
npm --prefix ../theme-tooling run vendor:check
npm --prefix ../theme-tooling ci
GALA_THEME_TOOLING_DIR=../../theme-tooling npm --prefix tooling run tokens:generate
GALA_THEME_TOOLING_DIR=../../theme-tooling npm --prefix tooling run digest:generate
GALA_THEME_TOOLING_DIR=../../theme-tooling npm --prefix tooling run verify
```

Re-run `digest:generate` after editing `theme.json`, `tokens.css`,
`components.css` or `print.css`. It recomputes asset digests and the chain
`fixtureDigest`, `evidenceDigest`, `integrity`.

Only theme tooling is consumed from an explicit source checkout. Before
installation, it verifies its committed Schema 3.8.0, Template 4.0.0, and six
Theme 4.0.0 archives against `vendor/manifest.json` and the lockfile; Schema
and Template then resolve from those integrity-checked installed archives.
`GALA_SCHEMA_DIR` and `GALA_TEMPLATE_DIR` are development overrides, not part
of this flow. None is a runtime dependency, and these commands do not publish
the package.

## Rules

CSS is limited to the root scope and the template's published styling hooks,
contains no `@import`, external `url()`, `outline: none` or script
constructs, and every stylesheet is one `@layer` block matching `theme.json`.
Contrast (WCAG 2.2 AA) is checked for both palettes by `contrast:check`.
The shared tooling README describes each gate.
