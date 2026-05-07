<p align="center">
  <picture>
    <source srcset="packages/public/vibe-kanban-logo-dark.svg" media="(prefers-color-scheme: dark)">
    <source srcset="packages/public/vibe-kanban-logo.svg" media="(prefers-color-scheme: light)">
    <img src="packages/public/vibe-kanban-logo.svg" alt="Vibe Kanban Logo">
  </picture>
</p>

<p align="center">Get 10X more out of Claude Code, Gemini CLI, Codex, Amp and other coding agents.</p>

<p align="center">
  <a href="https://github.com/ehsanmim/vibe-kanban"><img alt="Status" src="https://img.shields.io/badge/status-community%20fork-blue?style=flat-square" /></a>
  <a href="https://github.com/ehsanmim/vibe-kanban/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/badge/license-Apache%202.0-green?style=flat-square" /></a>
</p>

---

## A community-maintained fork

This is an unofficial, community-maintained fork of [BloopAI/vibe-kanban](https://github.com/BloopAI/vibe-kanban). The original team [announced they were sunsetting](https://www.vibekanban.com/blog/shutdown) the project and removed the Kanban experience from the main branch. The Kanban board is the feature that made this tool valuable, so this fork exists to **keep the local Kanban experience alive** as a self-contained, local-only app.

**Status:** early stage. The fork tracks upstream `main` and the Kanban feature has not yet been restored — that work is in progress, drawing on community pull requests. See [open issues](https://github.com/ehsanmim/vibe-kanban/issues) for the current roadmap.

**Goals:**
- Restore the Kanban board (planning, prioritisation, agent workspaces) by integrating community PRs.
- Stay **local-only** — no cloud account, no remote auth, no telemetry by default.
- Auto-setup that "just works" via `npx` once a forked package is published.

**This fork is maintained on a best-effort basis by a single person.** Pull requests welcome; please open a discussion before large changes. All credit for the original project belongs to [BloopAI](https://github.com/BloopAI) and the upstream contributors.

---

## Overview

In a world where software engineers spend most of their time planning and reviewing coding agents, the most impactful way to ship more is to get faster at planning and review.

Vibe Kanban is built for this. Use kanban issues to plan work locally. When you're ready to begin, create workspaces where coding agents can execute.

- **Plan with kanban issues** — create, prioritise, and assign issues on a kanban board *(restoration in progress)*
- **Run coding agents in workspaces** — each workspace gives an agent a branch, a terminal, and a dev server
- **Review diffs and leave inline comments** — send feedback directly to the agent without leaving the UI
- **Preview your app** — built-in browser with devtools, inspect mode, and device emulation
- **Switch between 10+ coding agents** — Claude Code, Codex, Gemini CLI, GitHub Copilot, Amp, Cursor, OpenCode, Droid, CCR, and Qwen Code
- **Create pull requests and merge** — open PRs with AI-generated descriptions, review on GitHub, and merge

![](packages/public/vibe-kanban-screenshot-overview.png)
![](packages/public/vibe-kanban-screenshot-workspace.png)

## Installation

> **Note:** the upstream `npx vibe-kanban` command runs the original (sunsetted) package without the restored Kanban feature. Until this fork is published to npm under a separate name, the only way to run it is from source — see [Development](#development) below. A published package is on the roadmap.

## Support

For bugs and feature requests on this fork, please open an [issue](https://github.com/ehsanmim/vibe-kanban/issues) or start a thread in [Discussions](https://github.com/ehsanmim/vibe-kanban/discussions).

For questions about the original project, see [BloopAI/vibe-kanban](https://github.com/BloopAI/vibe-kanban).

## Contributing

Contributions are welcome. Because this is a small, single-maintainer fork, please **open a Discussion or issue first** before sending a large PR — it saves both of us time if the change isn't a fit for the local-only direction.

Good first contributions:
- Helping triage and adapt upstream PRs that restore Kanban functionality.
- Removing or stubbing remote/cloud-only code paths so the local experience is friction-free.
- Improving setup ergonomics (`pnpm run dev` first-run experience).

## Development

### Prerequisites

- [Rust](https://rustup.rs/) (latest stable)
- [Node.js](https://nodejs.org/) (>=20)
- [pnpm](https://pnpm.io/) (>=8)

Additional development tools:
```bash
cargo install cargo-watch
cargo install sqlx-cli
```

Install dependencies:
```bash
pnpm i
```

### Running the dev server

```bash
pnpm run dev
```

This will start the backend and web app. A blank DB will be copied from the `dev_assets_seed` folder.

### Building the web app

To build just the web app:

```bash
cd packages/local-web
pnpm run build
```

### Build from source (macOS)

1. Run `./local-build.sh`
2. Test with `cd npx-cli && node bin/cli.js`

### Environment Variables

The following environment variables can be configured at build time or runtime:

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `PORT` | Runtime | Auto-assign | **Production**: Server port. **Dev**: Frontend port (backend uses PORT+1) |
| `BACKEND_PORT` | Runtime | `0` (auto-assign) | Backend server port (dev mode only, overrides PORT+1) |
| `FRONTEND_PORT` | Runtime | `3000` | Frontend dev server port (dev mode only, overrides PORT) |
| `HOST` | Runtime | `127.0.0.1` | Backend server host |
| `MCP_HOST` | Runtime | Value of `HOST` | MCP server connection host (use `127.0.0.1` when `HOST=0.0.0.0` on Windows) |
| `MCP_PORT` | Runtime | Value of `BACKEND_PORT` | MCP server connection port |
| `DISABLE_WORKTREE_CLEANUP` | Runtime | Not set | Disable all git worktree cleanup including orphan and expired workspace cleanup (for debugging) |
| `VK_ALLOWED_ORIGINS` | Runtime | Not set | Comma-separated list of origins allowed to make backend API requests |

**Build-time variables** must be set when running `pnpm run build`. **Runtime variables** are read when the application starts.

#### Running behind a reverse proxy or custom domain

When running behind a reverse proxy (e.g., nginx, Caddy, Traefik) or on a custom domain, you must set the `VK_ALLOWED_ORIGINS` environment variable. Without this, the browser's Origin header won't match the backend's expected host, and API requests will be rejected with a 403 Forbidden error.

```bash
# Single origin
VK_ALLOWED_ORIGINS=https://vk.example.com

# Multiple origins (comma-separated)
VK_ALLOWED_ORIGINS=https://vk.example.com,https://vk-staging.example.com
```

---

## Acknowledgements

This project is a fork of [BloopAI/vibe-kanban](https://github.com/BloopAI/vibe-kanban), licensed under Apache 2.0. All design, original implementation, and the agent-orchestration architecture are the work of the BloopAI team and upstream contributors. This fork only exists to preserve the local Kanban use-case after upstream's decision to sunset the product.

Original project: https://github.com/BloopAI/vibe-kanban
