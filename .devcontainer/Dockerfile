# =============================================================================
# ai-devcontainer-base
# Base Dev Container image with AI/agent CLI tooling preinstalled.
# =============================================================================
FROM mcr.microsoft.com/devcontainers/base:noble

# Set the SHELL option -o pipefail before RUN with a pipe in it (DL4006)
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Avoid prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# Ensure ~/.local/bin (native tool installs) is on PATH for vscode user
ENV PATH=/home/vscode/.local/bin:${PATH}

# ---------------------------------------------------------------------------
# Core terminal utilities
# tmux: tracked via Renovate (datasource=github-releases). The ARG below is
# a version sentinel — Renovate opens PRs when a new upstream release lands.
# The apt install uses Ubuntu Noble's package (version strings differ from
# GitHub tags, so the ARG is not passed to apt-get).
# ---------------------------------------------------------------------------
# renovate: datasource=github-releases depName=tmux/tmux
ARG TMUX_VERSION=3.6
# hadolint ignore=DL3008
RUN apt-get update \
    && apt-get install -y --no-install-recommends tmux \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ---------------------------------------------------------------------------
# Node.js (LTS) via NodeSource
# ---------------------------------------------------------------------------
ARG NODE_VERSION=22
# hadolint ignore=DL3008
RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ---------------------------------------------------------------------------
# GitHub CLI
# ---------------------------------------------------------------------------
# renovate: datasource=github-releases depName=cli/cli
ARG GH_VERSION=2.87.3
# hadolint ignore=DL3008
RUN mkdir -p /etc/apt/keyrings \
    && chmod 755 /etc/apt/keyrings \
    && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg \
    | tee /etc/apt/keyrings/githubcli-archive-keyring.gpg >/dev/null \
    && chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
    | tee /etc/apt/sources.list.d/github-cli.list >/dev/null \
    && apt-get update \
    && apt-get install -y --no-install-recommends "gh=${GH_VERSION}" \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ---------------------------------------------------------------------------
# Claude Code (native installer)
# Native install is the recommended method as of 2026-02; npm is deprecated.
# The installer places the binary at ~/.local/bin/claude for the target user.
# Run as vscode so $HOME resolves to /home/vscode, matching Dev Container
# conventions. Switch back to root afterwards for subsequent system-level steps.
# renovate: datasource=github-releases depName=anthropics/claude-code
ARG CLAUDE_CODE_VERSION=2.1.69
USER vscode
RUN curl -fsSL https://claude.ai/install.sh | bash -s "${CLAUDE_CODE_VERSION#v}"
USER root

# ---------------------------------------------------------------------------
# AI / Agent CLI tools (npm global installs)
# ---------------------------------------------------------------------------
# renovate: datasource=npm depName=@github/copilot
ARG GITHUB_COPILOT_VERSION=0.0.422
# renovate: datasource=npm depName=@google/gemini-cli
ARG GEMINI_CLI_VERSION=0.32.1
# renovate: datasource=npm depName=@openai/codex
ARG OPENAI_CODEX_VERSION=0.111.0
# renovate: datasource=npm depName=opencode-ai
ARG OPENCODE_VERSION=1.2.18
# renovate: datasource=npm depName=@sourcegraph/amp
ARG AMP_VERSION=0.0.1772481904-gb6b1d9
# renovate: datasource=npm depName=vibe-kanban
ARG VIBE_KANBAN_VERSION=0.1.24

RUN npm install -g \
    @github/copilot@${GITHUB_COPILOT_VERSION} \
    @google/gemini-cli@${GEMINI_CLI_VERSION} \
    @openai/codex@${OPENAI_CODEX_VERSION} \
    opencode-ai@${OPENCODE_VERSION} \
    @sourcegraph/amp@${AMP_VERSION} \
    vibe-kanban@${VIBE_KANBAN_VERSION} \
    && npm cache clean --force

# ---------------------------------------------------------------------------
# AgentOS
# ---------------------------------------------------------------------------
ARG AGENT_OS_REPO=https://github.com/buildermethods/agent-os.git
# renovate: datasource=github-releases depName=buildermethods/agent-os
ARG AGENT_OS_REF=v3.0.0

RUN git clone --depth 1 --branch ${AGENT_OS_REF} ${AGENT_OS_REPO} /home/vscode/agent-os \
    && rm -rf /home/vscode/agent-os/.git \
    && chown -R vscode:vscode /home/vscode/agent-os

# ---------------------------------------------------------------------------
# Verification labels
# ---------------------------------------------------------------------------
LABEL maintainer="danstis" \
    description="Dev Container base image with AI/agent CLI tooling" \
    org.opencontainers.image.source="https://github.com/danstis/ai-devcontainer-base"

# Drop back to the non-root user expected by Dev Containers (satisfies DL3002)
USER vscode
