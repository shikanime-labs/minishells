# Minishells

Portable Nix/devenv shells for different open source organizations. Instead of
reinventing a dev environment every time, plug in system config once and spin up
a shell per project.

**Language:** Nix

## Structure

- `flake.nix` — Main flake exposing all shells
- `.envrc` — Direnv configuration for automatic shell loading
- Individual shell definitions as flake outputs

## Usage

Each shell provides a self-contained development environment for a specific
project or organization. Load via `direnv` or `nix develop`.

## Commit Style

- Plain-text capitalized title, no conventional-commit prefix
- Body with labels: `Design:`, `Related:`, `Closes #`
- Keep Markdown lines wrapped at 80 columns and run `nix fmt` before shipping

## Stack


## Protect `main`

- Require 1 approving review
- Require linear history (no merge commits)
- Require signed commits
- Squash+rebase merge only

_Licensed under Apache-2.0. Shells should be self-contained. Test with
`nix flake check` before submitting. Always use worktrees when making changes._
