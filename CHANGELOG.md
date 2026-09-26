# Changelog

All notable changes to `@rathnasgala2/theme-default` are documented here.

## Unreleased

2026-09-25 code-discipline review remediation (THD-H5): this file
previously carried three dated sub-headings under `## Unreleased` above a
`## 2.0.0 - Unreleased` heading, even though `2.0.0` has been on the
registry since 2026-09-22T12:58:03Z — a state that made no sense read
either way (see the review's own explanation). The three dated entries
below are folded into this one `Unreleased` section (Keep a Changelog:
exactly one `Unreleased` section, dated headings below it), and `2.0.0`'s
heading now carries its actual release date. Everything in this section
ships as the next version; bumping `package.json`'s `version` for that
release is an owner decision (recommended: `2.1.0`, since nothing below is
a breaking change to the token/CSS-hook contract).

### Changed (THD-M11, 2026-09-25 third pass)

- `@rathnasgala2/theme-tooling` sibling checkout re-pinned to
  `ae2ee4979a4e5f6a4335c99b869f80eacd359788` across `ci.yml`, `nightly.yml`
  and `release.yaml` (still `0.1.0`, unpublished); that commit serves the
  `visual:check` fixture over loopback HTTP instead of opening the
  rendered file directly, which resolves the harness defect the previous
  pass's README/CHANGELOG entries described — the pinned commit's
  `visual:check` now reports zero `serious`/`critical` axe violations and
  no horizontal overflow across all six palette/viewport combinations, so
  those entries' "harness cannot load this theme's CSS" claim no longer
  holds and is corrected below (README "Visual/accessibility check").
- Digest chain regenerated (`fixtureDigest`/`evidenceDigest`/`integrity`)
  for the re-pin: the pinned `theme-tooling` commit also adds a new
  `color-accent`/`color-code-canvas` contrast pair to its floor catalog,
  which changes `evidenceDigest`'s conformance-result input even though no
  file in this repository's own published set changed bytes.

### Added (THD-M12, 2026-09-25 third pass)

- `components.css`'s `a` rule sets `text-decoration-skip-ink: auto` so the
  underline breaks around descenders instead of cutting through them,
  admitted by the pinned `theme-tooling` commit's `check-css-grammar.mjs`
  (closed to `auto`/`none`/`all`). Digest chain regenerated.

### Added (THD-M10, 2026-09-25 second pass)

- `.github/workflows/ci.yml` gains its own `visual` job: installs Chromium
  and runs `@rathnasgala2/theme-tooling`'s `visual:check` (Playwright +
  axe-core) against this theme at 320/768/1440px in both palettes,
  uploading the screenshots as a build artifact. `tooling/package.json`
  gains the matching `visual:check` dispatch. See README "Visual/
  accessibility check" for the one open finding this surfaced (a harness
  defect in the pinned `theme-tooling` commit, not a token gap in this
  theme).

### Changed (THD-M1, THD-H8, 2026-09-25 second pass)

- `@rathnasgala2/theme-tooling`/`@rathnasgala2/template` sibling checkouts
  re-pinned to `8fd9b36f85f4ae0a34dfb6ebb7c55319672071d2` and
  `d2b2f0ffc38407851e293e5a8a92d8863a0182d1` across `ci.yml`,
  `nightly.yml` and `release.yaml` (`stylingContractDigest` is unchanged:
  the re-pinned contract's `catalogDigest` matches what was already
  committed).
- `theme.json.slotHooks` no longer declares `landmark-main-content`: no
  rule in this theme's CSS matches `#main-content` (the forced-colors
  override that used to was removed without replacement in the prior
  pass), and `css:check` now asserts `slotHooks` set-equality against the
  CSS in both directions (THM-M3), which a stale declared-but-unused hook
  now fails.
- `color-link-visited` is finally referenced: `a:visited` reads it.
  `a:hover` gets a `text-decoration-thickness` change (no color shift),
  and `select:focus-visible`/`a:focus-visible` each scope a
  `--gala-color-focus` custom-property override — `gala-base` still paints
  the actual ring; this theme never declares `outline-*` itself. Contract
  2.1.0's `pseudoClasses` catalog is admitted by the pinned
  `theme-tooling` commit's `css:check` as of this pass.
- `color-accent` (and `color-focus`, which tracked it in the light
  palette) and `color-surface-raised` move to new values in both palettes
  to clear three contrast floors `theme-tooling` added
  (`color-surface-raised`/`color-surface` >=1.3:1, `color-accent`/
  `color-text` and `color-accent`/`color-surface` >=3:1 each) — see
  README "The 35-token catalog and both palettes" for the exact values
  and margins.
- `components.css`'s `hr` (`prose-divider`) icon no longer uses the
  wide-tile trick documented in the prior pass: `background-position`,
  `background-repeat`, `background-size` and `width` are now admitted by
  `grammar:check`'s property catalog, so the mark renders once, at its
  native 16×8 size, centered in a `var(--gala-space-8)`-wide box.
- Digest chain regenerated (`fixtureDigest`/`evidenceDigest`/`integrity`/
  asset digests) for all of the above.

### Added (Contract 2.1.0 adoption, 2026-09-25)

- `theme.json`'s `contractVersion` moves to `2.1.0` and `stylingContractDigest`
  is refreshed to `@rathnasgala2/template`'s current `contracts/theme-styling-contract.jcs`
  `catalogDigest`. `cssLayers`/`templateRange` are unchanged (the three-file
  shape already excludes `gala-base`, and `^2.0.0` already admits the
  template's current version).
- `components.css` sheds every rule the template's new `gala-base` layer
  now supplies identically on every page regardless of theme: the h1/h2
  heading `line-height`, the header/footer/main padding, the `img`
  `max-width`, and the no-op `prefers-reduced-motion` guard. The focus ring
  this theme's `--gala-color-focus`/`--gala-focus-width` tokens feed is now
  real: `gala-base` paints it with `outline-style: solid` under
  `:focus-visible`, so this theme carries no `outline-*` longhand of its own
  at all (superseding THD-H1's interim "declare neither" fix below). The
  fifth inert-outline site, `#main-content`'s `forced-colors` override, is
  removed without replacement: forced-colors mode already overrides
  `outline-color` to a system color regardless of the author value, so
  `gala-base`'s ring needs no theme-side forced-colors handling.

### Added (THD-H7, 2026-09-25)

- A slight negative `letter-spacing` on `h1`-`h3` for the serif heading
  face, and a `1.5` `line-height` on `pre`/`code`, as this theme's own
  refinement on top of `gala-base`'s type scale, overflow/wrap handling and
  responsive spacing (which now do the rest of what THD-H7 asked for).

### Added (THD-H8, 2026-09-25)

- `assets/divider-mark.svg`: this theme's one reference passive asset (178
  bytes, sanitiser-clean), declared in `theme.json.assets` and
  `package.json.files`, painted as `hr`'s `background-image`. README's new
  "Iconography: the one reference asset" section documents the pattern
  (a wide tile with the mark near one edge, since the closed CSS property
  grammar has no `width`/`background-position`/`-repeat`/`-size`) for the
  other four themes to follow.

### Changed (THD-M1, THD-M9, THD-L1, THD-L2, 2026-09-25)

- Five of the eight previously-unreferenced tokens now have a real use:
  `color-accent` colors `h1` and the header-actions link's background,
  `color-on-accent`/`space-3` style that link as a pill, `color-surface-raised`
  lifts `select`/`option` above the flat header/footer surface, and
  `space-8` widens the article-end break. `color-link-visited` still has no
  use (blocked on `:visited` support in the pinned `css:check`);
  `color-success`/`color-warning` still have no matching state anywhere in
  this theme's markup and are left declared rather than misapplied.
- `::selection` carries a short comment explaining why it is scoped to the
  root compound only (THD-M9).
- `print.css`'s hardcoded `#ffffff`/`#000000` literals become `Canvas`/
  `CanvasText` system colors (THD-L2). The redundant reduced-motion guard
  (THD-L1) was removed as part of the contract 2.1.0 delta above.

### Changed (2026-09-25)

- `.github/workflows/{ci.yml,nightly.yml,release.yaml}` move the template
  sibling checkout to `e66d8771189966db1f8f876b005ab59d4f676bcb # 2.1.0
(unreleased)` and the theme-tooling sibling checkout to
  `68dceb301c071f3a60c2bf4c4f3215a6c3478502 # 0.1.0 (unpublished)`. The
  template commit is not yet pushed to GitHub, so these workflows cannot
  run in CI against it until it is.

### Changed (THD-H1, 2026-09-25)

- Removed the four inert `outline-color`/`outline-width` declaration pairs
  from `components.css` (`#main-content`, `a`, `select`) — `outline-style`
  was never set alongside them, so they painted nothing (the initial value
  of `outline-style` is `none`), and the previous README/CHANGELOG claim
  that they produced a themed visible focus ring was false. A themed ring
  needs `:focus-visible`, unavailable in this template contract version
  (TPL-H2); a single comment in `components.css` documents where it will
  be restored.

### Changed (THD-C1, 2026-09-25)

- Added a bare-root `[data-gala-publication-root]` block (light palette)
  plus a `@media (prefers-color-scheme: dark)` override to `tokens.css`,
  before the two resolved-mode blocks, so every `--gala-*` token still has
  a real value when `data-gala-resolved-color-mode` is not set (no
  JavaScript, a text-mode crawler, or a pre-hydration paint) — previously
  the theme applied no styling at all in that case.

### Changed (THD-M6, 2026-09-25)

- Replaced this repository's own copy of `tooling/scripts`/`tooling/test`
  (25 files, identical across all five theme repositories except one
  package-name literal) with a dependency on the new
  `@rathnasgala2/theme-tooling` repository, which now implements every
  gate once. `tooling/` here carries only `run.mjs` and a trimmed
  `package.json`. See `../theme-tooling/CHANGELOG.md` for what moved and
  what changed in the process (THD-H2/H3/H4/M2/M3/M4/M5).
- Deleted the root `package-lock.json` (THD-L5): the published
  `package.json` has never had a dependency for it to lock.

### Changed (SCHEMA-REPIN-2.11.0, 2026-09-22)

- `tooling/package.json` re-pins `@rathnasgala2/schemas` from the LOCAL-1
  local tarball (`file:../../../local-packages/rathnasgala2-schemas-2.8.0.tgz`)
  to the exact published registry version `2.11.0`
  (`https://registry.npmjs.org/@rathnasgala2/schemas/-/schemas-2.11.0.tgz`,
  integrity `sha512-5hXxpLaXqEKKhoRLBBEq98rzJQ9K4u3b8nDMt/ZZmV2gL4XRmTOCwjF1U618UJ1CgmFnlifhQWRuDvQQVyFfTA==`).
  This fixes CI, which was failing on every push because the `file:` path
  does not exist on GitHub Actions runners. `urn:gala:schema:theme-contract:2.0.0`
  and the `build-input` root this tooling validates against are
  byte-identical between 2.8.0 and 2.11.0 (contract re-pin packet,
  2026-09-19); `tooling/package-lock.json` and `sbom.cdx.json` regenerated
  accordingly; full `npm run verify` re-run and green.
- Added `tooling/scripts/check-no-local-schema-pin.mjs` (wired into
  `verify` as `schema-pin:check`) so a `file:`/`local-packages` specifier
  for `@rathnasgala2/schemas` can never silently return.

### Changed (THEMES-2.8.0, 2026-09-18)

- `tooling/package.json` pins `@rathnasgala2/schemas` to the packed
  `rathnasgala2-schemas-2.8.0.tgz` tarball (sha256
  `6352293855cdcff9054d43ced876740644f6b45bc813eda3990b646ec9bef563`,
  LOCAL-1), up from 2.6.1; `tooling/package-lock.json` integrity and
  `sbom.cdx.json` regenerated. `urn:gala:schema:theme-contract:2.0.0`, the
  `build-input` root and the published `examples/valid/build-input/canonical.json`
  this tooling validates against are byte-identical between 2.6.1 and 2.8.0;
  the 2.7.0-2.8.0 delta is confined to the deployment roots, the OpenAPI
  bundle and the App catalogs, none of which this repository consumes.

### Added (FOLLOW-UP SUPPLY-CHAIN-JS, 2026-09-17)

- `tooling/scripts/resolve-template-dir.mjs`'s `resolveTemplateDir()` now
  also honours `WORKSPACE_ROOT` (DEC-015 name), checked after the existing
  `GALA_TEMPLATE_DIR` override and before the fixed relative default:
  `<WORKSPACE_ROOT>/template` when set. Fixes running `tooling`'s tests/
  scripts from a location where the fixed relative default cannot reach
  the sibling `template` checkout (a git worktree one level deeper than
  the real checkout, LOCAL-38) without a one-off `GALA_TEMPLATE_DIR`.
  Added `tooling/test/resolve-template-dir.test.mjs`.
- Added `tooling/test/tooling-drift.test.mjs` (task packet S2-T14's drift
  gate), previously present only in the other four theme repositories: it
  compares this repository's own `tooling/scripts/` against the canonical
  source resolved the same `WORKSPACE_ROOT`/relative-default way, which
  from here is this repository's own checkout (a "self case" that runs
  the real comparison rather than skipping it). Reads the package-identity
  literal from `package.json` at run time instead of a hardcoded per-repo
  name, so the file is byte-identical across every theme repository,
  `theme-default` included, and skips (with a printed reason) rather than
  failing when the canonical `theme-default` cannot be found (a
  single-repo CI checkout of one of the other four themes).

## 2.0.0 - 2026-09-22 (task packet S2-T13)

### Added

- Initial closed package file set: `package.json` (dependency-free,
  script-free, 4-key closed shape), `theme.json` (all 35 tokens for light
  and dark palettes, `stylesheets`/`cssLayers` three-file shape, the
  51-hook `slotHooks` subset this theme's CSS uses, budgets, and the
  digest chain), `tokens.css`/`components.css`/`print.css`, and a
  compact-JCS `LICENSE` license-evidence file (SPDX `Apache-2.0`).
- WCAG 2.2 AA contrast for every named token pair in both palettes,
  `outline-color`/`outline-width` on every interactive hook (no
  `outline-style` override, no `:focus` pseudo-class available in this
  template contract version — corrected 2026-09-25, THD-H1: these two
  longhands alone never painted a visible ring, and were removed),
  `forced-colors: active` system-color mappings, and a defensive
  `prefers-reduced-motion: reduce` rule.
- `tooling/` local dev/test/SBOM project (private, unpublished, its own
  lockfile) with the closed-hook CSS conformance test, the WCAG contrast
  test, the theme-contract schema test, the closed-package-file-set test,
  the forbidden-constructs absence test, the digest-cycle generator/test,
  and the template-conformance byte-equality test (two builds against
  `@rathnasgala2/template`'s `main` branch, consumed by path).
- SBOM (`sbom.cdx.json`, CycloneDX 1.6, generated for the published package
  surface — which has zero runtime dependencies).

### Notes

- `fixtureDigest`/`evidenceDigest` are this repository's own genuine local
  conformance evidence (five of the eight DEC-097 runner IDs:
  `schema`/`semantic`/`package`/`css`/`absence`), not the DEC-097-mandated
  _shared_ fixture release/result the not-yet-existing `S2-T11` reusable
  CI workflow will eventually produce and re-issue across all five theme
  packages. See README "What `fixtureDigest`/`evidenceDigest` are, and are
  not."
