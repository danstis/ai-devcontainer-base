# Contributing

Thanks for contributing to `ai-devcontainer-base`.

## Scope

This project maintains a reusable Dev Container base image with AI tooling preinstalled. Contributions should preserve compatibility with Dev Container features and keep the image maintainable.

## Basic workflow

1. Fork or branch from the default branch.
2. Make focused changes with clear commit messages.
3. Update documentation for any behavior/config changes.
4. Run relevant checks locally.
5. Open a PR with context, testing notes, and trade-offs.

## Contribution guidelines

- Prefer reproducible installs (pinned versions where practical).
- Keep Docker/image layers efficient and understandable.
- Minimize breaking changes to default shell/user/tooling assumptions.
- Document any new environment variables, auth steps, or prerequisites.
- If you add non-CLI assets to the image (for example, a home-directory checkout), document the path and add a verification check.

## Suggested PR checklist

- [ ] Change is scoped and documented.
- [ ] README reflects user-visible behavior.
- [ ] Installation/version decisions are explained.
- [ ] Validation steps were run (or limitations are noted).
- [ ] Licensing and security impacts were considered.

## Development principles

- **Compatibility first:** avoid surprising downstream Dev Container users.
- **Reproducibility:** deterministic installs over convenience shortcuts.
- **Clarity:** future maintainers should understand what changed and why.

## Code of conduct

By participating in this project, you agree to follow the [Code of Conduct](./CODE_OF_CONDUCT.md).
