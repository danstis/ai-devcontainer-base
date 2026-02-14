# ai-devcontainer-base

A base image for [VS Code Dev Containers](https://containers.dev/) with modern AI-assisted development tooling preinstalled.

This repository defines the baseline image used by project-specific development containers. The goal is to provide a consistent, ready-to-use environment for local development and agentic coding workflows while remaining compatible with standard Dev Container features.

## Goals

- Provide a stable, reusable base image for multiple projects.
- Include a curated set of AI/agent CLIs out-of-the-box.
- Remain compatible with regular Dev Container features and customizations.
- Keep startup and maintenance friction low for contributors.

## Planned preinstalled tooling

The initial image target includes:

- Gemini CLI
- GitHub Copilot CLI
- Claude Code
- OpenCode
- Codex CLI
- spec-kit CLI
- Amp CLI
- AgentOS (checked out to `~/agent-os`)

> Tool installation scripts and exact version pinning will be managed in this repo as the build configuration is added.

## Intended usage

This repository will publish (or locally build) a base image that downstream projects can reference from their `.devcontainer/devcontainer.json`.

See [`devcontainer-example.json`](./devcontainer-example.json) for a complete, opinionated example. Copy it to your project's `.devcontainer/devcontainer.json` and customise as needed.

## Compatibility principles

To preserve compatibility with the broader Dev Containers ecosystem, the base image should:

- Follow Dev Container best practices for non-root users and shell setup.
- Avoid breaking standard feature installation behavior.
- Keep essential developer tooling available in common `PATH` locations.
- Pre-seed home-directory assets (like AgentOS) under `/home/vscode` when needed.
- Prefer explicit version pinning for reproducible builds.

## Repository structure

This repository is currently in bootstrap phase. The initial documentation files establish contribution and agent-collaboration standards. Build definitions and installation scripts will follow.

## Quality Assurance

This repository uses automated testing to ensure the image builds correctly and all tools are functional. The CI pipeline includes:

- **Linting**: Dockerfile (Hadolint), GitHub Workflows (Actionlint), and Markdown.
- **Build Verification**: Ensures the Docker image builds successfully.
- **Smoke Tests**: Verifies that all installed CLIs are executable and available in the default `PATH` for the `vscode` user.

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for workflow and contribution guidelines.

## Security

See [SECURITY.md](./SECURITY.md) for vulnerability reporting guidance.

## License

This project is licensed under the MIT License. See [LICENSE](./LICENSE).
