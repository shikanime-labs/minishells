# Shell Catalog

Every shell lives in `flake.nix` under `devenv.shells` as a `rec` set.
Most shells import `base` and layer languages, packages, env vars, and
services on top. The flake imports `devlib`, so `default` reuses the
`git`, `nix`, `shell`, and `shikanime` profiles from
`devlib.devenvModules`.

## Foundation

- `default` — imports the four devlib devenvModules profiles
  (`git`, `nix`, `shell`, `shikanime`). It is the direnv target.
- `base` — common floor: `containers = mkForce { }` (containers forced
  off), plus the `gh` and `sapling` packages.

## Shells

| Shell | Imports | Languages | Notable packages / services |
| --- | --- | --- | --- |
| `cloud-pi-native` | `base` | — | `jre_minimal`, `kubectl`, `kubectl-cnpg`, `kubernetes-helm`, `sonarqube-cli`, `teleport`, `vault`, `velero`; `TELEPORT_ADD_KEYS_TO_AGENT=no` |
| `cloud-pi-native/console` | `cloud-pi-native` | JavaScript + pnpm | `docker`, `firefox`, `nodejs_26`, `chromium` when available on the host platform; Prisma 6 engine env vars (`PRISMA_FMT_BINARY`, `PRISMA_QUERY_ENGINE_BINARY`, `PRISMA_QUERY_ENGINE_LIBRARY`, `PRISMA_SCHEMA_ENGINE_BINARY`); `prometheus` service with exemplars, OTLP, and tracing enabled |
| `cloud-pi-native/helm-charts` | `cloud-pi-native` | helm | `chart-testing`, `helm-docs` |
| `cloud-pi-native/socle` | `cloud-pi-native` | Python + uv, Ruby | `ansible`, `gnutar`, `yq-go` |
| `codegouvfr` | — (containers forced off) | JavaScript + yarn | — |
| `jj-vcs` | `base` | Rust | `libiconv` |
| `facebook` | `base` | — | — |
| `facebook/sapling` | `facebook` | JavaScript + yarn, Rust, Python | `gnumake`, `libiconv`, `mercurial` |
| `torvalds` | `base` | — | — |
| `torvalds/linux` | `torvalds` | C | kernel build stack: `bc`, `bison`, `flex`, `gcc`, `gnumake`, `ncurses`, `openssl`, `pkg-config`, `python3`, `zlib`, plus `elfutils` and `pahole` when available on the host platform |
| `longhorn` | `base` | Go | `docker`, `gnumake`, `kubectl`, `kustomize`; git hooks `golangci-lint` and `gotest` |
| `nixos` | `base` | Nix | `nixpkgs-review` |
| `zed` | `base` | Rust disabled | native build stack: `cmake`, `clang`, `llvm`, `lld`, `libgit2`, `libx11`, `libxcb`, `libxkbcommon`, `fontconfig`, `gettext`, `git`, `jq`, `curl`, `openssl`, `pkg-config`, `sqlite`, `zlib`, `zstd` |
| `shikanime-studio` | `base` | Nix | `ghstack` |

Hierarchy: `facebook/sapling` imports `facebook`; `torvalds/linux`
imports `torvalds`; every `cloud-pi-native/*` variant imports
`cloud-pi-native`, which imports `base`. `codegouvfr` stands alone with
containers forced off directly.

## Template

`flake.templates.default` points at `./templates/default` — "A devenv
template with default settings". Start from it with
`nix flake init -t github:shikanime-labs/minishells#default`. Note the
shipped `templates/default/.envrc` still runs
`use flake github:shikanime-studio/minishells --accept-flake-config
--no-pure-eval` when nix is present: the repo moved to the
`shikanime-labs` org but the template's flake reference was not updated,
so fresh templates resolve through the old `shikanime-studio` path.
