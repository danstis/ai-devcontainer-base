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

## Validation guidance

If build scripts/config are present, run practical checks before finishing, such as:

- lint/format checks for changed files
- container build validation (when feasible)
- smoke checks that key CLIs are available in `PATH`

If checks cannot run in the current environment, state that clearly in your summary.
