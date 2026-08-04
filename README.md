# Modules Testing Environment

Signed module testing catalogue for **Synthetiq Player**.

This repository mirrors the official catalogue and adds the latest test modules
(`An1me`, `HiAnime`, `KickAssAnime v4`) so they can be installed and updated
from inside the app without file imports.

## Use in the app

Settings → Media → Import from repository:

```
https://github.com/kas021/modules-testing-environment
```

The app fetches `repository.json`, verifies the P-256 signature, and offers
updates for installed modules.

## Layout

- `catalogue.json` — editable source of truth (module files + changelogs)
- `modules/` — immutable module ZIPs (each version is a separate file)
- `bundles/` — signed bootstrap bundle ZIP
- `repository.json` — generated signed index (do not edit by hand)
- `scripts/` — validate, sign, and publish tooling
- `.github/workflows/publish.yml` — CI that validates + signs + publishes

## Publish flow

The workflow requires the `MODULE_REPOSITORY_SIGNING_JWK` secret (copy it from
`kas021/Synthetiq-Modules`). On every push to `main` it validates manifests,
checksums, identity uniqueness, and version order; computes signatures; then
publishes versioned release assets and commits the signed `repository.json`.