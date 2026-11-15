# Vorchestrator (`vorchestrator`)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Sponsor Vikyek](https://img.shields.io/badge/Sponsor-GitHub-ea4aaa?logo=github-sponsors&logoColor=white)](https://github.com/sponsors/Vikyek)

A token-efficient, self-scaling multi-agent orchestration plugin for the Google Antigravity (AGY) CLI.

## Overview

`vorchestrator` acts as a lightweight orchestrator that:
1. **Assesses Task Complexity** before any execution.
2. **Routes Tasks** to optimally-tiered worker subagents (model & effort matched to subtask).
3. **Optimizes Quota Pools** across Gemini and Claude pools dynamically.
4. **Limits Context Window Bloat** using dynamic Tool Schema Pruning and compact payload state handoffs.
5. **Escalates Automatically** through a 5-tier ladder:
   - **T0 (Direct)**: Trivial tasks (1-2 tools).
   - **T1 (Scout)**: `Gemini 3.8 Flash` (`flash_lite`, Low effort) read-only recon agent.
   - **T2 (Worker)**: `Gemini 4 Hydrogen` (`flash`, Medium effort) surgical editing agent.
   - **T3 (Architect)**: `Gemini 4 Argon` (`pro`, High effort) or Claude (`Claude Sonnet 5.5` / `Claude Opus 5.5`) complex reasoning agent.
   - **T4 (Successor)**: `Orchestrator (Argon)` (`gemini-pro-orchestrator-g1-pro`), `Gemini 4 Argon` (`pro`), or `Claude Opus 5.5` orchestration coordinator taking over on failure/bloat.
   - **T5 (`/teamwork-preview`)**: Massive greenfield projects (full parallel autonomous team).

## Plugin Structure

```text
vorchestrator/
├── plugin.json                 # Manifest declaring plugin name
├── rules/
│   └── vorchestrator_rule.md   # Pre-task and Quota activation rules
└── skills/
    └── vorchestrator/
        └── SKILL.md            # Execution and Tool Schema Pruning logic
```

## Setup & Installation

### Automated Installation

Run the bundled installation script to register the plugin and skill into `~/.gemini/config/`:

```bash
git clone https://github.com/Vikyek/vorchestrator.git ~/Projects/vorchestrator
cd ~/Projects/vorchestrator
./install.sh
```

### Manual Plugin & Skill Setup

1. Link or clone this repository into your customizations directory:
   ```bash
   mkdir -p ~/.gemini/config/plugins ~/.gemini/config/skills/vorchestrator
   ln -sfn ~/Projects/vorchestrator ~/.gemini/config/plugins/vorchestrator
   ln -sf ~/Projects/vorchestrator/skills/vorchestrator/SKILL.md ~/.gemini/config/skills/vorchestrator/SKILL.md
   ```

2. Register the plugin in `~/.gemini/config/plugins.json`:
   ```json
   {
     "entries": [
       { "path": "~/.gemini/config/plugins/vorchestrator" }
     ]
   }
   ```

### Agensi / SKILL.md Agent Loading

`vorchestrator` is fully compliant with the open `SKILL.md` standard and compatible with Agensi:
- Root [`SKILL.md`](SKILL.md) provides instant single-skill ingestion.
- Sub-skill path [`skills/vorchestrator/SKILL.md`](skills/vorchestrator/SKILL.md) supports multi-skill pack structures.
- Compatible with Antigravity (AGY), Gemini CLI, Claude Code, Cursor, and Codex CLI.

## Tool Schema Pruning (MCP Optimization)

To reduce token overhead, `vorchestrator` dynamically creates custom `.agents/mcp_config.json` configurations in branched workspaces before spawning worker subagents:
- **Scout Config**: Exposes only `codegraph` (`codegraph_explore`).
- **Worker Config**: Exposes only `toon` for structured payloads.
- **Architect Config**: Full suite (codegraph, context-mode, toon).

---

## 💖 Support & Sponsorship

`vorchestrator` is developed and maintained independently by **[Vikyek](https://github.com/Vikyek)**. Everything is 100% open-source, telemetry-free, and public.

<div align="center">

[![Sponsor Vikyek](https://img.shields.io/badge/Sponsor-%E2%9D%A4-ea4aaa?logo=github&style=for-the-badge)](https://github.com/sponsors/Vikyek)
[![Donate on Liberapay](https://img.shields.io/badge/Donate-Liberapay-f6c915?style=for-the-badge&logo=liberapay&logoColor=black)](https://liberapay.com/Vikyek)
[![Sponsor on Polar](https://img.shields.io/badge/Sponsor-Polar-0062FF?style=for-the-badge&logo=polar&logoColor=white)](https://polar.sh/vikyek)
[![Donate with Stripe](https://img.shields.io/badge/Donate-Stripe-635BFF?style=for-the-badge&logo=stripe&logoColor=white)](https://profile.stripe.com/@vikyek)

</div>

> ### 🎯 Active Sponsorship Goal: Monthly Living Essentials
> **Target:** **2,710 PLN / ~$680 USD per month** (`630 PLN / week`) — Rent & Essential Utilities  
> Covering basic living essentials directly unlocks dedicated, sustainable full-time independent open-source engineering across AI agent tooling and developer infrastructure free from corporate telemetry, ads, or paywalls.

If `vorchestrator` saves quotas and optimizes agent runs in your setup, consider supporting ongoing work:

- **GitHub Sponsors**: [github.com/sponsors/Vikyek](https://github.com/sponsors/Vikyek) *(Monthly tiers and one-time tips)*
- **Liberapay**: [liberapay.com/Vikyek](https://liberapay.com/Vikyek) *(0% platform fee recurrent patronage in EUR/USD/PLN)*
- **Polar**: [polar.sh/vikyek](https://polar.sh/vikyek) *(Developer subscriptions & commercial passes)*
- **Direct Stripe Profile**: [profile.stripe.com/@vikyek](https://profile.stripe.com/@vikyek) *(Direct one-time tips & sponsorship)*

Every contribution—monthly or one-time—directly supports active maintenance, hardware testbeds, and new feature development!

---

## 🌐 Ecosystem

Part of the **[Vikyek](https://github.com/Vikyek)** developer tooling and AI agent middleware ecosystem:

| Project | Description |
|---|---|
| **[toon-mcp](https://github.com/Vikyek/toon-mcp)** | FastMCP server & CLI for token-efficient TOON format serialization |
| **[vorchestrator](https://github.com/Vikyek/vorchestrator)** | Multi-agent orchestration, dynamic tiering & dual-provider quota optimizer |
| **[agy-input-wrap](https://github.com/Vikyek/agy-input-wrap)** | Clean PTY terminal input interceptor & prompt optimizer for AI CLIs |
| **[destructive-action-guard](https://github.com/Vikyek/destructive-action-guard)** | Blast-radius guard & safety interception engine for autonomous agents |
| **[paru-wrapper](https://github.com/Vikyek/paru-wrapper)** | Enhanced Arch Linux pacman/paru wrapper with plugins & hook lifecycle |
| **[vlfstatus](https://github.com/Vikyek/vlfstatus)** | Ultra-lightweight, modular status bar generator for Linux desktops |

---

## License

MIT License
