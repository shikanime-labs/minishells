<!-- owner: shikanime | zone: internal | purpose: docs landing + index for the minishells dev-shell repo -->

# minishells — Documentation

A Nix/devenv flake that collects the portable development shells used across the
different open-source organisations. Plug system config in once, then spin up a
per-project shell instead of reinventing a dev environment every time. It ships
no running service.

## Internal ops

- [Architecture](./Architecture.md) — the flake's shell tree and how `base`
  layers into per-org shells.
- [Development](./Development.md) — local setup, the format loop, and how to add
  a shell.
- [Runbook](./Runbook.md) — how to consume, template, and protect the repo.
- [Troubleshooting](./Troubleshooting.md) — direnv, flake eval, and devlib
  import failures.
- [Reference](./Reference.md) — shell attributes, env, and the flake template.

## User-facing docs

The user guide lives in the repo [README](../README.md) (enter a shell, list
shells, direnv, template). It is the canonical source for consumers; this
`docs/` directory owns internal ops only and links out rather than duplicating
it.
