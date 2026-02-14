# =============================================================================
# ai-devcontainer-base
# Base Dev Container image with AI/agent CLI tooling preinstalled.
# =============================================================================
FROM mcr.microsoft.com/devcontainers/base:noble

# Avoid prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

# ---------------------------------------------------------------------------
# Node.js (LTS) via NodeSource
# ---------------------------------------------------------------------------
ARG NODE_VERSION=22
RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION}.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ---------------------------------------------------------------------------
# GitHub CLI
# ---------------------------------------------------------------------------
RUN (type -p wget >/dev/null || (apt-get update && apt-get install -y wget)) \
    && mkdir -p -m 755 /etc/apt/keyrings \
    && wget -qO- https://cli.github.com/packages/githubcli-archive-keyring.gpg \
       | tee /etc/apt/keyrings/githubcli-archive-keyring.gpg >/dev/null \
    && chmod go+r /etc/apt/keyrings/githubcli-archive-keyring.gpg \
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" \
       | tee /etc/apt/sources.list.d/github-cli.list >/dev/null \
    && apt-get update \
    && apt-get install -y --no-install-recommends gh \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ---------------------------------------------------------------------------
# AI / Agent CLI tools (npm global installs)
# ---------------------------------------------------------------------------
RUN npm install -g \
        @anthropic-ai/claude-code \
        @google/gemini-cli \
        @openai/codex \
    && npm cache clean --force

# ---------------------------------------------------------------------------
# GitHub Copilot CLI (gh extension)
# ---------------------------------------------------------------------------
RUN gh extension install github/gh-copilot

# ---------------------------------------------------------------------------
# OpenCode CLI
# ---------------------------------------------------------------------------
RUN curl -fsSL https://opencode.ai/install | bash

# ---------------------------------------------------------------------------
# Amp CLI
# ---------------------------------------------------------------------------
RUN curl -fsSL https://amp.dev/install | bash

# ---------------------------------------------------------------------------
# Verification labels
# ---------------------------------------------------------------------------
LABEL maintainer="danstis" \
      description="Dev Container base image with AI/agent CLI tooling" \
      org.opencontainers.image.source="https://github.com/danstis/ai-devcontainer-base"
