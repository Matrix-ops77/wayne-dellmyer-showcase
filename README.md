# Hermes Agent — Showcase

> **This is a public career-showcase repo for the private Hermes Agent runtime.**
> The runtime is local-first multi-agent orchestration software. This companion
> repo exists to give hiring managers a verifiable, receipts-based view of the
> architecture, the proof points, and the operating philosophy.

🔒 **Runtime is private.** The runtime code lives in a private repo because it
ties to personal data, paid third-party services (OpenCode Zen, MiniMax,
Honcho, Composio), and a long-running dev loop. This showcase describes what
the runtime does without exposing implementation.

---

## What Hermes Agent is

Hermes Agent is a **multi-agent orchestration runtime** that turns a single
Mac into a team of specialised AI agents. Each agent is a separate Hermes
profile with its own skill bundle, toolset, gateway process, and conversation
memory — so a research task and a code task don't fight each other over the
same context window.

**One sentence:** a self-hosted, multi-agent AI ops stack with a TUI gateway,
native macOS desktop client, web UI, and Telegram routing — all backed by a
self-hosted memory layer (Honcho) with Postgres + pgvector.

---

## Architecture (operator's view, top to bottom)

```
┌─────────────────────────────────────────────────────────────┐
│ Surfaces                                                    │
│  • Native macOS app      /Applications/Hermes.app            │
│  • TUI gateway (port 9120)                                    │
│  • Web UI (port 9119 dashboard + 8787 control plane)         │
│  • Telegram bot (chat ID 1842082873)                          │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Gateway                                                     │
│  • Per-profile gateway process (default + dev/scout/scribe/ │
│    reach)                                                    │
│  • launchd-managed, 7+ plists                                │
│  • session routing + toolset gating                          │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Agent runtime                                               │
│  • Hermes core (Python 3.14)                                 │
│  • Skills ecosystem: 90+ bundled skills, hub-installable     │
│  • MCP integrations: Linear, Notion, Airtable, GitHub,       │
│    NotebookLM, OpenKnowledge, Composio (Gmail, Gumroad,     │
│    LinkedIn, HeyGen, Mixmax)                                │
│  • Sub-agent profiles: scout (research), scribe (writing),   │
│    reach (marketing), dev (build) — each with own SOUL.md   │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Memory + knowledge                                          │
│  • Honcho (self-hosted) on 127.0.0.1:8000                    │
│  • 4 Docker containers via OrbStack: api, deriver,           │
│    Postgres, Redis                                           │
│  • pgvector 1024-dim, snowflake-arctic-embed2 embeddings     │
│  • OpenKnowledge (`.ok/`) knowledge bases in 3 projects      │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│ Model layer                                                 │
│  • OpenCode Zen aggregator (237 providers)                   │
│  • Primary: MiniMax-M3 ($50 Max plan, OAuth)                 │
│  • Local Ollama for embeddings (RAM-conservative)            │
└─────────────────────────────────────────────────────────────┘
```

---

## Proof points (receipts, not assertions)

| Claim | Receipt |
|---|---|
| Self-hosted AI memory stack | 4 Honcho Docker containers (api/deriver/Postgres/Redis) on OrbStack, health endpoint returns 200 at `127.0.0.1:8000/health` |
| Multi-agent system in production | 5 active Hermes profiles (`default`, `dev`, `scout`, `scribe`, `reach`), each with its own gateway process, SOUL.md, and toolset |
| Operator skills in production | 90+ bundled skills, OpenKnowledge MCP integration, Composio MCP for 8+ SaaS services |
| Edge product to revenue | `hermesagent.qzz.io` (SOUL Pack, 2 SKUs, $29 + $79), Cloudflare Workers + DNS |
| Production Next.js product | `bucks-montco-dispatch` (contractor portal), served via launchd on port 4502 |
| Open-source contribution | OpenKnowledge upstream PR (`add Hermes as handoff target`), 6 commits, 2123 tests pass, 2 bugs caught in user testing |
| Infrastructure-first operator | Migrated Docker Desktop → OrbStack, freed 11+ GB; resource caps on every container; nightly cron for memory consolidation |

---

## Operating philosophy (the why behind the choices)

- **Self-host first, vendor later.** I learned the stack by running it on my own
  box. If I can't debug it locally, I don't trust it in production.
- **Receipts over narration.** Every claim in this README has a file path, port
  number, or config value behind it. Vague claims get cut.
- **Operator culture.** I run my own systems. I know what "production" means
  because I keep my own services up.
- **Right tool for the scale.** OrbStack on Apple Silicon, pgvector for the
  data size I actually have, local Ollama for embeddings. No premature
  Kubernetes, no premature vector DB sprawl.
- **MCP-native.** Every external system I depend on has an MCP server if
  one exists. Hermes' skill system loads them as first-class tools.

---

## What this repo IS and IS NOT

| This repo is | This repo is NOT |
|---|---|
| A verifiable resume of the Hermes Agent runtime | The runtime itself |
| A hiring-manager-friendly tour of architecture and proof points | A demo of the live system |
| A snapshot of the operator's view as of mid-2026 | A real-time dashboard |
| MIT-licensed documentation | Code that runs |

---

## License

This showcase repo is MIT-licensed. The runtime is private; this is a
read-only view of the architecture and proof points.

## Contact

Dell7w@gmail.com
