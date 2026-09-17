# Contributing to minishells

Portable development environments for organisations that haven't joined the Nix religion yet.

## Workflow

Fork, branch off `main`, open a PR against `main`. One logical change per PR.

## Environment

```sh
direnv allow  # or: nix develop
```

## Validation

`nix flake check` green before submitting.

Security issues: see [SECURITY.md](SECURITY.md).
