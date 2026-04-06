FROM node:22-bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    curl \
    ca-certificates \
    ripgrep \
    build-essential \
    python3 \
    python3-pip \
    bash \
    && rm -rf /var/lib/apt/lists/*

# Instala o Codex oficial
RUN npm install -g @openai/codex
RUN npm install -g @gitlawb/openclaude


WORKDIR /workspace

# Persistência opcional da config do Codex
RUN mkdir -p /home/appuser/.codex

ENTRYPOINT ["/bin/bash"]