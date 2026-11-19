<!-- owner: Automata | zone: technical | purpose: Canonical technical doc index for shikanime-labs/minishells -->
# minishells — technical index

A collection of Nix/devenv shells used for the different open source organisations worked with: system config is plugged in once, then one shell per project is selected with `nix develop .#<shell>` instead of reinventing a dev environment each time.

## Canonical sources

- [Repository README](https://github.com/shikanime-labs/minishells#readme)
- [Technical wiki](https://github.com/shikanime-labs/minishells/wiki)

## Requirements and usage

Nix with flakes enabled is required; direnv + nix-direnv are optional. Enter the default shell with `nix develop`, a specific shell with `nix develop .#cloud-pi-native`, and list all shells with `nix flake show`. The shipped `.envrc` loads the default shell on `direnv allow`, or can be pointed at an attribute with `use flake .#<shell> --accept-flake-config --no-pure-eval`.

## Subsystems

### Shell definitions (`flake.nix`)

All shells live under `devenv.shells`. Most import `base` (a common floor of `containers = mkForce { }` plus `gh` and `sapling`) and layer languages, packages, env vars, and hooks on top. The flake imports `devlib`, so `default` reuses the `git`, `nix`, `shell`, and `shikanime` profiles from `devlib.devenvModules`.

| Shell | Languages | Notable packages |
| --- | --- | --- |
| `default` | — | devlib devenvModules profiles; direnv target |
| `base` | — | common floor (containers off), gh, sapling |
| `cloud-pi-native` | — | kubectl, helm, teleport, vault, velero, cnpg plugin, JRE |
| `cloud-pi-native/console` | JS + pnpm | nodejs, docker, chromium/firefox, Prisma 6 engines, prometheus service |
| `cloud-pi-native/helm-charts` | helm | inherits cloud-pi-native |
| `cloud-pi-native/socle` | python + uv, ruby | inherits cloud-pi-native |
| `codegouvfr` | JS + yarn | containers forced off |
| `jj-vcs` | rust | libiconv |
| `facebook` / `facebook/sapling` | JS + yarn, rust, python | gnumake, mercurial, libiconv |
| `torvalds` / `torvalds/linux` | C | gcc, bison, flex, bc, ncurses, openssl, zlib, elfutils/pahole |
| `longhorn` | go | docker, kubectl, kustomize, golangci-lint + gotest git hooks |
| `nixos` | nix | nixpkgs-review |
| `zed` | — | cmake, clang/llvm, lld, libgit2, X11 libs, sqlite |
| `shikanime-studio` | nix | ghstack |

Hierarchy: `facebook/sapling` imports `facebook`; `cloud-pi-native/*` and `facebook` inherit `base`; `codegouvfr` stands alone.

### Template (`templates/default`)

A flake template for bootstrapping the direnv setup in another checkout: `nix flake init -t github:shikanime-studio/minishells#default`. The shipped `.envrc` runs `use flake github:shikanime-studio/minishells --accept-flake-config --no-pure-eval` when nix is present.

### Adding a shell

Define `devenv.shells.my-shell` in `flake.nix`, import `base` for common packages and settings, then layer languages, packages, env vars, and hooks. `devlib.devenvModules.<name>` profiles can be reused for consistent formatters, hooks, and generators.

## Documentation rule

In-repo `docs/` is canonical for technical detail. The [wiki](https://github.com/shikanime-labs/minishells/wiki) is a rendered human index that links here.
