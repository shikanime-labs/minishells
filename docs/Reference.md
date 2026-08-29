<!-- owner: shikanime | zone: internal | purpose: the shell attributes, env, and template surface for consumers -->

# Reference

## Shell attributes

Shells are exposed under `devenv.shells.<name>` in `flake.nix`. Common ones are
selected via:

| Command                | Effect                     |
| ---------------------- | -------------------------- |
| `nix develop`          | Enter the default shell.   |
| `nix develop .#<name>` | Enter a named shell.       |
| `nix flake show`       | List all available shells. |

## `.envrc` directives

| Directive                                                 | Effect                  |
| --------------------------------------------------------- | ----------------------- |
| `use flake`                                               | Load the default shell. |
| `use flake .#<name> --accept-flake-config --no-pure-eval` | Load a named shell.     |

## Flake template

| Template                                     | Effect                                     |
| -------------------------------------------- | ------------------------------------------ |
| `github:shikanime-studio/minishells#default` | Writes a tiny `.envrc` to consume a shell. |

## Key inputs

| Input    | Role                                                                             |
| -------- | -------------------------------------------------------------------------------- |
| `base`   | Shared packages/settings every shell imports.                                    |
| `devlib` | Reusable `devlib.devenvModules.<name>` profiles (formatters, hooks, generators). |

## Auth / registry

No registry auth; shells are local dev environments only.
