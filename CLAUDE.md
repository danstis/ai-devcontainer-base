# CLAUDE.md

Claude-specific repository guidance.

## Purpose

Use this file as a concise operating guide when assisting with this repository. It complements `AGENTS.md` and project docs.

## Repository intent

- Build and maintain a base Dev Container image.
- Bake in commonly used AI/agent CLIs.
- Keep the image compatible with standard Dev Container features.

## Collaboration standards

- Prefer small, reviewable changes.
- Keep docs synchronized with implementation.
- Call out trade-offs explicitly (image size, startup time, compatibility, security).
- Favor pinned versions and reproducible installation flows.

## When adding or updating tooling

For each CLI/tool addition, include:

1. Installation source and method.
2. Version pinning strategy (or rationale for floating versions).
3. An automated update mechanism (see below).
4. Verification command (e.g., `tool --version` or `test -d /home/vscode/<asset>` for home-directory assets).
5. Any required environment variables or auth notes.

### Update mechanism requirements

All tools must have an automated update path so versions never silently go stale:

- **npm tools (preferred):** install via `npm install -g`, pin the version in a Dockerfile `ARG`, and add a `# renovate: datasource=npm depName=<package>` comment on the line above the `ARG`. Renovate and the custom regex manager in `renovate.json` handle the rest.
- **Non-npm tools:** add the equivalent Renovate comment for the appropriate datasource (e.g. `datasource=github-releases`). If no Renovate datasource exists, create a scheduled GitHub Actions workflow to poll the upstream release API and open update PRs.
- If a tool genuinely cannot be version-pinned, document why in the Dockerfile and add a pipeline to surface version drift.

Commits that update tool versions (including Renovate-generated PRs) must use the `feat(tools):` conventional commit format. This ensures the semantic versioning pipeline produces a minor-version bump for each tool update. The Renovate `packageRules` in `renovate.json` already enforces this for Dockerfile ARG updates.

## Documentation checklist

Before finalizing a change, check whether updates are needed in:

- `README.md` (what users get and how to consume it)
- `CONTRIBUTING.md` (how maintainers validate and update tooling)
- `SECURITY.md` (if security posture/reporting instructions change)
