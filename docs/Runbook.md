<!-- owner: shikanime | zone: internal | purpose: how to consume, template, and protect the repo without surprises -->

# Runbook

This repo is a shell library, not a deployable service. "Operations" means
keeping the shells loadable and consistent across the fleet.

## Consuming a shell

- From a clone: `nix develop` (default) or `nix develop .#cloud-pi-native`.
- With direnv: `direnv allow`; or target a shell in `.envrc`:
  `use flake .#cloud-pi-native --accept-flake-config --no-pure-eval`.
- Without a clone:
  `nix flake init -t github:shikanime-studio/minishells#default`, then edit
  `.envrc` to the shell you want.

## Releasing

There is no tag or publish step — consumers reference the flake by rev through
their own `flake.nix` / `.envrc`. A change is "released" the moment it merges to
`main`; downstream flake updates pull it in.

## Branch protection

`main` requires one approving review, linear history, signed commits, and
squash+rebase only. PRs are the merge path; direct pushes are rejected.

## CI

`.github/workflows/` runs the format/eval pass on every PR, plus Renovate for
flake input bumps. Land bumps on `main` via squash+rebase (see `AGENTS.md`).
