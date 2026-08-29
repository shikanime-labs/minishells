<!-- owner: shikanime | zone: internal | purpose: known failure modes and the first-responder fix for each -->

# Troubleshooting

## Shell does not load under direnv

**Symptom:** `direnv allow` does nothing, or `nix develop` works but the shell
never activates. **Cause:** missing `use flake` line in `.envrc`, or direnv not
hooked into the shell. **Fix:** add
`use flake .#<shell> --accept-flake-config --no-pure-eval` to `.envrc` and
confirm the direnv hook is sourced in your shell rc.

## Flake eval fails on a new input

**Symptom:** `nix flake show` errors after adding a package or devlib module.
**Cause:** impure eval or an input not present in `flake.lock`. **Fix:** run
with `--accept-flake-config --no-pure-eval`, then `nix flake update` to refresh
the lock before committing.

## A formatter is missing in one shell

**Symptom:** `nix fmt` behaves differently inside shell A vs shell B. **Cause:**
the shell re-declared packages instead of `imports = [ base ]`, or skipped a
devlib profile. **Fix:** route the shell through `base` and reuse
`devlib.devenvModules.<name>`; do not duplicate formatter config.

## `nix fmt` fails on a docs page

**Cause:** treefmt's rumdl-check rejects Markdown lines over 80 columns.
**Fix:** wrap the offending lines to ≤80 and re-run `nix fmt` until clean.
