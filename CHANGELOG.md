# Changelog

All notable changes to `@rathnasgala2/theme-default` are documented here.

## Unreleased

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

## 2.0.0 - Unreleased (task packet S2-T13)

### Added

- Initial closed package file set: `package.json` (dependency-free,
  script-free, 4-key closed shape), `theme.json` (all 35 tokens for light
  and dark palettes, `stylesheets`/`cssLayers` three-file shape, the
  51-hook `slotHooks` subset this theme's CSS uses, budgets, and the
  digest chain), `tokens.css`/`components.css`/`print.css`, and a
  compact-JCS `LICENSE` license-evidence file (SPDX `Apache-2.0`).
- WCAG 2.2 AA contrast for every named token pair in both palettes, focus
  visibility via `outline-color`/`outline-width` on every interactive hook
  (no `outline-style` override, no `:focus` pseudo-class available in this
  template contract version), `forced-colors: active` system-color
  mappings, and a defensive `prefers-reduced-motion: reduce` rule.
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
