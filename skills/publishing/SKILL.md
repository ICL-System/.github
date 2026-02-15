---
name: publishing
description: Use when publishing ICL packages to registries (crates.io, PyPI, npm), bumping versions, creating GitHub releases, or tagging releases. Triggers on phrases like "publish", "release", "bump version", "tag", "npm publish", "cargo publish", "pypi", "new version", or any reference to making a new release of ICL packages.
---

# Publishing ICL Packages

Workflow for releasing a new version of ICL across all 4 registries. Follow exactly — partial publishes cause user confusion.

## Package Registry Map

| Registry | Package | Source | Build Tool |
|----------|---------|--------|------------|
| crates.io | `icl-core` | `crates/icl-core/` | `cargo publish` |
| crates.io | `icl-cli` | `crates/icl-cli/` | `cargo publish` (depends on icl-core) |
| PyPI | `icl-runtime` | `bindings/python/` | `maturin build --release` + `twine upload` |
| npm | `icl-runtime` | `bindings/javascript/` | `node build.mjs` + `npm publish` |

## Version Files to Bump

ALL of these must match. Grep for the old version to confirm zero stale refs after bumping.

| File | Field |
|------|-------|
| `Cargo.toml` (workspace root) | `[workspace.package] version` |
| `crates/icl-cli/Cargo.toml` | `icl-core = { version = "X.Y.Z" }` dependency |
| `bindings/python/pyproject.toml` | `version` |
| `bindings/javascript/package.json` | `version` |
| `bindings/javascript/Cargo.toml` | `version` |

After bumping, run `source "$HOME/.cargo/env" && cargo update` to refresh `Cargo.lock`.

## Pre-Publish Checklist

```bash
cd <ICL_WORKSPACE>/ICL-Runtime
source "$HOME/.cargo/env"
cargo build --workspace && cargo test -p icl-core --lib  # All tests pass
```

## Publish Order (CRITICAL)

Order matters — icl-cli depends on icl-core on crates.io.

### 1. crates.io: icl-core (must be first)

```bash
cd <ICL_WORKSPACE>/ICL-Runtime
source "$HOME/.cargo/env"
CARGO_REGISTRY_TOKEN=<token> cargo publish -p icl-core
```

Wait for "Published" message before proceeding.

### 2. crates.io: icl-cli

```bash
CARGO_REGISTRY_TOKEN=<token> cargo publish -p icl-cli
```

### 3. PyPI: icl-runtime

Requires a Python venv with `maturin` and `twine`:

```bash
python3 -m venv /tmp/publish-venv
/tmp/publish-venv/bin/pip install maturin twine
cd bindings/python
source "$HOME/.cargo/env"
/tmp/publish-venv/bin/maturin build --release
cd ../..
/tmp/publish-venv/bin/twine upload target/wheels/icl_runtime-<version>-*.whl \
  -u __token__ -p <pypi-token>
rm -rf /tmp/publish-venv  # cleanup
```

### 4. npm: icl-runtime

The npm job uses `build.mjs` which builds 3 wasm-pack targets (nodejs, bundler, web) into `dist/`. It publishes from `bindings/javascript/` using the checked-in `package.json` (which has keywords, conditional exports, etc.).

```bash
cd bindings/javascript
node build.mjs          # builds dist/nodejs/, dist/bundler/, dist/web/
npm publish --access public
```

**CRITICAL**: Never publish from `bindings/javascript/pkg/` — that's a stale single-target artifact. Always publish from `bindings/javascript/` which uses the `dist/` layout.

## After Publishing

### Update docs

- `CHANGELOG.md` — New version entry
- `ICL-Runtime/ROADMAP.md` — Check off items, update status line
- `README.md` — Version references if applicable
- `ICL-Docs/src/cli/reference.md` — CLI version output

### Git commit, push, and tag

```bash
cd <ICL_WORKSPACE>/ICL-Runtime
git add -A && git commit -m "chore: Release vX.Y.Z" && git push
git tag -a vX.Y.Z -m "vX.Y.Z — <description>" && git push origin vX.Y.Z
```

Tag ICL-Docs and ICL-Spec too:

```bash
cd <ICL_WORKSPACE>/ICL-Docs
git tag -a vX.Y.Z -m "vX.Y.Z — Docs for <description>" && git push origin vX.Y.Z

cd <ICL_WORKSPACE>/ICL-Spec
git tag -a vX.Y.Z -m "vX.Y.Z — Spec aligned with <description>" && git push origin vX.Y.Z
```

### Create GitHub Release

```bash
cd <ICL_WORKSPACE>/ICL-Runtime
gh release create vX.Y.Z --title "vX.Y.Z — <title>" --notes "<release notes>" --latest
```

### Verify all registries

```bash
cargo search icl-core               # crates.io
cargo search icl-cli                 # crates.io
pip index versions icl-runtime       # PyPI (needs pip or a venv)
npm view icl-runtime version         # npm
```

## Automated Publishing (CI)

The `publish.yml` workflow triggers on `v*` tags and publishes to all 3 registries. It requires these GitHub Secrets:

| Secret | Purpose |
|--------|---------|
| `CRATES_IO_TOKEN` | crates.io API token |
| `PYPI_TOKEN` | PyPI API token (prefixed with `pypi-`) |
| `NPM_TOKEN` | npm automation token || `SPEC_REPO_TOKEN` | GitHub PAT for cross-repo access to ICL-Spec |
All 3 have `continue-on-error: true` — always verify publishes succeeded after tagging.

## Token Locations

All tokens are stored exclusively in GitHub Secrets (Settings → Secrets → Actions). No tokens are persisted locally.

| Secret | Purpose |
|--------|---------|
| `CRATES_IO_TOKEN` | crates.io API token |
| `PYPI_TOKEN` | PyPI API token (prefixed with `pypi-`) |
| `NPM_TOKEN` | npm automation token |
| `SPEC_REPO_TOKEN` | GitHub PAT for cross-repo access to ICL-Spec |

For local manual publishes, pass the token inline or export it temporarily — never persist to disk.

## Common Pitfalls

- **Publishing icl-cli before icl-core propagates**: Wait for icl-core to show "Published" before publishing icl-cli
- **npm publishes from wrong directory**: The old `pkg/` dir has a wasm-pack-generated `package.json` without keywords or conditional exports. Always publish from `bindings/javascript/` (the `dist/` layout)
- **PyPI trusted publisher fails**: Use `twine upload` with `__token__` auth, not `gh-action-pypi-publish`
- **Unpublished npm version has 24h blackout**: If you unpublish a version from npm, the same version number cannot be re-published for 24 hours — bump the patch version instead
