<!-- owner: shikanime | zone: internal | purpose: the local format loop and how to add a shell -->

# Development

## Prerequisites

- Nix with flakes enabled.
- `direnv` + `nix-direnv` (optional but recommended); `direnv allow` loads the
  default shell.
- This is a `jj` repo. Branch off `main`; never commit to `main` directly.

## Build and check loop

```bash
nix fmt            # treefmt: Nix formatting + markdown lint (80-col)
nix flake show     # list the available shells
nix develop        # enter the default shell
```

CI (`.github/workflows/`) runs the format/eval pass on every PR. `nix fmt` must
be clean before a PR is reviewable.

## Commit style

Plain capitalized title, no conventional-commit prefix. Body uses labels:

```text
Design: <why the shell changed>
Related: <full URL to issue/PR>
Closes: <full URL>
```

Keep Markdown wrapped at 80 columns. `nix fmt` enforces it.

## Adding a shell

1. Add `devenv.shells.<name>` to `flake.nix` and `imports = [ base ]`.
2. Enable languages/packages/env/hooks for that org.
3. Reuse `devlib.devenvModules.<name>` profiles instead of hand-rolling.
4. `nix fmt`, `nix flake show` to confirm it appears, open a PR against `main`.

## Consuming without a clone

`nix flake init -t github:shikanime-studio/minishells#default` writes a tiny
`.envrc`; point it at the shell you want (e.g. `cloud-pi-native`).
