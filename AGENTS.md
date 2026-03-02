# AGENTS.md

Guidance for AI coding agents working in this repository.

## Mission context

This repository builds a reusable **Dev Container base image** with AI/agentic tooling preinstalled. Changes should prioritize:

1. Compatibility with VS Code Dev Containers and the broader `containers.dev` ecosystem.
2. Reproducibility and maintainability of tool installation.
3. Clear documentation for both humans and AI agents.

## Working rules

- Keep changes focused and minimal to the task.
- Prefer explicit, deterministic setup steps over implicit behavior.
- Document assumptions in markdown when introducing new scripts/config.
- Avoid introducing proprietary or secret material into the repository.

## Tooling baseline to support

As the image configuration is added, account for the planned CLI baseline:

- Gemini CLI
- GitHub Copilot CLI
- Claude Code
- OpenCode
- Codex CLI
- spec-kit CLI
- Amp CLI

## Documentation expectations

When you change behavior, update relevant docs in the same PR/commit:

- `README.md` for user-facing setup/usage changes
- `CONTRIBUTING.md` for contributor workflow updates
- `CLAUDE.md` for Claude-specific collaboration expectations

## Tool update mechanisms

Every tool added to the image **must** have an automated update path. The current standard is:

- Install the tool via `npm install -g` and pin its version in a Dockerfile `ARG`.
- Add a `# renovate: datasource=npm depName=<package>` comment on the line immediately above the `ARG` so Renovate can raise version-bump PRs automatically.
- The `renovate.json` custom regex manager picks up this pattern — no extra Renovate config is needed for npm packages.

If a future tool cannot be installed via npm:

- Pin the version explicitly in an `ARG` or `ENV` and add the appropriate Renovate comment (e.g. `datasource=github-releases depName=<owner>/<repo>`).
- If no Renovate datasource covers the tool, add a scheduled GitHub Actions workflow (e.g. weekly cron) that checks the upstream release API and opens a PR when a new version is detected.
- Document the chosen update strategy in a comment in the Dockerfile next to the install step.

Adding new CI pipelines to perform version checks and automated update PRs is explicitly encouraged when no existing mechanism covers a tool.

Tool version bump commits (whether from Renovate or manual updates) **must** use the `feat(tools):` conventional commit format so that the semantic versioning pipeline produces a minor-version bump on each tool update.

## Validation guidance

If build scripts/config are present, run practical checks before finishing, such as:

- lint/format checks for changed files
- container build validation (when feasible)
- smoke checks that key CLIs are available in `PATH`

If checks cannot run in the current environment, state that clearly in your summary.
