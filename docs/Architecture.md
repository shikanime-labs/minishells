<!-- owner: shikanime | zone: internal | purpose: explain the shell tree and base-layering so a new shell lands in the right place -->

# Architecture

## Goal

Stop rebuilding a dev environment per repository. `minishells` is a single Nix
flake that defines one reusable `base` shell and layers per-organisation shells
on top of it. The design constraint is **consistency**: every shell inherits the
same formatters, hooks, and generators from `base` (and from `devlib`), so a
contributor gets the same toolchain regardless of which org they are in.

## Shell tree

```text
flake.nix          # defines devenv.shells.* and the flake template
.envrc            # direnv: loads the default shell on `direnv allow`
templates/
  default/        # `nix flake init -t` target: a tiny direnv setup
```

Shells are Nix attribute sets under `devenv.shells`. Each shell imports `base`,
then enables languages/packages/env/hooks:

```nix
devenv.shells.my-shell = {
  imports = [ base ];
  languages.go.enable = true;
  packages = with pkgs; [ gnumake ];
};
```

## Layering rule

`base` carries the common packages and settings. A per-org shell must
`imports = [ base ]` rather than re-declaring those packages; drift between
shells (a formatter present in one but missing in another) is a bug, not a
feature.

## devlib bridge

The flake imports [devlib](https://github.com/shikanime-studio/devlib), so
shells can reuse `devlib.devenvModules.<name>` profiles for formatters, hooks,
and generators. Prefer a devlib profile over hand-rolled config when one exists.
