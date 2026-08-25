# Contributing

Thanks for your interest in this project (TianoShield Task 1.1: automated
triage tooling for TianoCore EDK II).

## How to contribute

1. Open an issue first for anything beyond a trivial fix (typo, broken
   link), describing the problem or enhancement.
2. Fork the repo and create a branch off `main`.
3. Make your changes. For notebook changes, please:
   - Clear cell outputs before committing where practical
   - Keep API keys out of notebooks entirely; use Colab Secrets / environment
     variables as the existing notebooks do.
4. Open a pull request against `main`. Describe what changed and why.

## Coding / notebook standards

- No hard-coded secrets, tokens, or API keys anywhere in committed files.
- New dependencies should be added to the relevant notebook's install cell
  AND to the `Dependencies` section of the top-level `README.md`.

## Reporting bugs / requesting features

Use GitHub Issues. Please include:
- Which sub-project (TianoForge / Bug-Report-Enhancement / Bug-Title-Opt /
  Advisory-Enhancement)
- Steps to reproduce (including whether you ran `sample/` or `AllBugs/`)
- Expected vs. actual behavior

## Reporting security issues

Do **not** open a public issue for security vulnerabilities — see
[SECURITY.md](SECURITY.md).

## License

By contributing, you agree that your contributions will be licensed under
the project's [MIT License](LICENSE).
