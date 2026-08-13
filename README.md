# agentplug-treesitter-bin

Release target for the `treesitter` wasm plugin, the syntax-parsing plugin `agentplug-runner` shares across every project it serves. Do not commit source here.

## What publishes into this repo

[AnEntrypoint/agentplug-treesitter](https://github.com/AnEntrypoint/agentplug-treesitter), whose `.github/workflows/release.yml` calls the shared reusable workflow `wasm-plugin-release.yml` hosted in [AnEntrypoint/rs-plugkit](https://github.com/AnEntrypoint/rs-plugkit). A push to `main` touching `src/**` or `Cargo.toml` installs the WASI SDK, builds for `wasm32-wasip1`, and publishes a release tagged `v<version>` titled `agentplug-treesitter v<version>`, where the version is read from the crate's own `Cargo.toml`.

## What lands here

- `treesitter.wasm` -- the built `agentplug_treesitter.wasm`, renamed to its published basename
- `treesitter.wasm.sha256` -- checksum of that exact asset

## How consumers fetch it

`agentplug-runner` resolves the `treesitter` plugin to this repo, downloads `treesitter.wasm`, verifies it against the `.sha256` published alongside the same release, and installs it under `~/.agentplug/plugins/`. It re-polls for a newer release every 600s by default. A plugin whose installed version marker is not real release semver, such as a hand-built local sideload, is never silently overwritten.
