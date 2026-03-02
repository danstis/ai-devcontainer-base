# ai-devcontainer-base

A base image for [VS Code Dev Containers](https://containers.dev/) with modern AI-assisted development tooling preinstalled.

This repository defines the baseline image used by project-specific development containers. The goal is to provide a consistent, ready-to-use environment for local development and agentic coding workflows while remaining compatible with standard Dev Container features.

## Goals

- Provide a stable, reusable base image for multiple projects.
- Include a curated set of AI/agent CLIs out-of-the-box.
- Remain compatible with regular Dev Container features and customizations.
- Keep startup and maintenance friction low for contributors.

## Preinstalled tooling

The image includes the following AI/agent CLIs, all installed via npm with pinned versions:

- [Claude Code](https://www.npmjs.com/package/@anthropic-ai/claude-code) (`@anthropic-ai/claude-code`)
- [GitHub Copilot CLI](https://www.npmjs.com/package/@github/copilot) (`@github/copilot`)
- [Gemini CLI](https://www.npmjs.com/package/@google/gemini-cli) (`@google/gemini-cli`)
- [Codex CLI](https://www.npmjs.com/package/@openai/codex) (`@openai/codex`)
- [OpenCode](https://www.npmjs.com/package/opencode-ai) (`opencode-ai`)
- [Amp CLI](https://www.npmjs.com/package/@sourcegraph/amp) (`@sourcegraph/amp`)
- [Vibe Kanban](https://www.npmjs.com/package/vibe-kanban) (`vibe-kanban`)

Version updates are tracked automatically by [Mend Renovate](https://docs.renovatebot.com/). When a new version of any tool is published to npm, Renovate raises a PR to bump the pinned version in the Dockerfile.

## Intended usage

This repository will publish (or locally build) a base image that downstream projects can reference from their `.devcontainer/devcontainer.json`.

See [`devcontainer-example.json`](./devcontainer-example.json) for a complete, opinionated example. Copy it to your project's `.devcontainer/devcontainer.json` and customise as needed.

## Compatibility principles

To preserve compatibility with the broader Dev Containers ecosystem, the base image should:

- Follow Dev Container best practices for non-root users and shell setup.
- Avoid breaking standard feature installation behavior.
- Keep essential developer tooling available in common `PATH` locations.
- Prefer explicit version pinning for reproducible builds.

## Automated dependency updates

All AI agent CLI versions are pinned in the Dockerfile via `ARG` declarations with `# renovate:` comments. Mend Renovate monitors npm for new releases and automatically opens PRs to update these versions. When a PR merges to `main`, the CI/CD pipeline rebuilds and publishes the image.

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
