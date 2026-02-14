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
3. Verification command(s) (e.g., `tool --version` or `test -d /home/vscode/<asset>` for home-directory assets).
4. Any required environment variables or auth notes.

## Documentation checklist

Before finalizing a change, check whether updates are needed in:

- `README.md` (what users get and how to consume it)
- `CONTRIBUTING.md` (how maintainers validate and update tooling)
- `SECURITY.md` (if security posture/reporting instructions change)
