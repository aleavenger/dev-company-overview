# Dev Company

**AI-powered autonomous company built on [PaperclipAI](https://github.com/paperclipai/paperclip).**

> A real 8-agent autonomous company running production workloads on the PaperclipAI orchestration platform — org charts, budgets, task assignment, cost control, governance, and a full plugin ecosystem.

---

## Overview

This project is an autonomous AI company built on top of [PaperclipAI](https://github.com/paperclipai/paperclip) — an open-source orchestration platform that treats AI teams as actual companies with hierarchy, budgets, work assignment, and human governance.

Dev Company runs a real 8-agent team that builds production software, using multiple AI runtimes (Claude Code, Codex, Cursor, Gemini) coordinated through PaperclipAI's heartbeat execution model. It includes custom plugins, a shared engineering knowledge base, and operational playbooks.

### Key Capabilities

- **Organizational Structure** — Org charts with hierarchical reporting and role-based authority
- **Goal-Driven Work** — Top-down decomposition: Goals > Projects > Issues (atomic tasks)
- **Cost Control** — Per-agent budgets, real-time spend tracking, automatic pause on overspend
- **Heartbeat Execution** — Agents wake on schedules, execute tasks, and sleep — no idle compute
- **Human Governance** — Board approval gates, pause/resume any agent, full override capability
- **Immutable Audit Trail** — Every mutation logged with full context (who, what, when)
- **Plugin Ecosystem** — Extend with custom UI widgets, APIs, monitoring, and integrations

---

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Board / Human Layer                      │
│                  (approve, pause, override, budget)              │
└──────────────────────────────┬──────────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────────┐
│                     Paperclip Platform                          │
│                                                                 │
│  ┌──────────┐  ┌───────────┐  ┌───────────┐  ┌─────────────┐  │
│  │  REST    │  │ Heartbeat │  │  Budget   │  │  Activity   │  │
│  │  API     │  │  Scheduler│  │  Enforcer │  │  Logger     │  │
│  └──────────┘  └───────────┘  └───────────┘  └─────────────┘  │
│                                                                 │
│  ┌──────────┐  ┌───────────┐  ┌───────────┐  ┌─────────────┐  │
│  │  React   │  │  Plugin   │  │  Approval │  │  Routine    │  │
│  │  UI      │  │  Host     │  │  Gates    │  │  Scheduler  │  │
│  └──────────┘  └───────────┘  └───────────┘  └─────────────┘  │
│                                                                 │
└──────────────────────────────┬──────────────────────────────────┘
                               │
              ┌────────────────┼────────────────┐
              │                │                │
     ┌────────▼──────┐ ┌──────▼───────┐ ┌──────▼───────┐
     │ Claude Code   │ │   Codex      │ │   Cursor     │
     │ Adapter       │ │   Adapter    │ │   Adapter    │
     └───────────────┘ └──────────────┘ └──────────────┘
              │                │                │
     ┌────────▼──────┐ ┌──────▼───────┐ ┌──────▼───────┐
     │  Agent:       │ │  Agent:      │ │  Agent:      │
     │  CEO, CTO,    │ │  Sr. Eng,    │ │  QA Eng,     │
     │  Cartographer │ │  PMO         │ │  ...         │
     └───────────────┘ └──────────────┘ └──────────────┘
```

### How Agents Work

```
┌─────────────────────────────────────────────┐
│              Agent Lifecycle                │
│                                             │
│  Sleep ──► Wake (heartbeat) ──► Execute     │
│                                    │        │
│              ┌─────────────────────┘        │
│              ▼                              │
│     Check inbox (assigned issues)           │
│              │                              │
│              ▼                              │
│     Checkout issue (atomic lock)            │
│              │                              │
│              ▼                              │
│     Execute work (adapter runtime)          │
│              │                              │
│              ▼                              │
│     Report results + cost event             │
│              │                              │
│              ▼                              │
│     Sleep (until next heartbeat)            │
│                                             │
└─────────────────────────────────────────────┘
```

---

## Agent Roster

Dev Company runs a real 8-agent team with the following structure:

```
Board (Human)
    │
    CEO ── strategy, coordination, budgets
    │
    CTO ── architecture, code review, planning
    ├── Cartographer ── research, mapping, documentation
    ├── Sr. Engineer (x3) ── implementation
    ├── QA Engineer (x2) ── testing, validation
    └── PMO ── task tracking, workload balance
```

Each agent has:
- **AGENTS.md** — Master instructions and authority rules
- **SOUL.md** — Persona and decision-making principles
- **HEARTBEAT.md** — Execution checklist for each wake cycle
- **TOOLS.md** — Available tools and capability boundaries
- **Memory** — Persistent knowledge across sessions (PARA method)

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| **Server** | Node.js, Express.js, TypeScript |
| **UI** | React, TypeScript, Tailwind CSS, Vite |
| **Database** | PostgreSQL, Drizzle ORM |
| **Adapters** | Claude Code, Codex, Cursor, Gemini, OpenCode, HTTP, Process |
| **Plugins** | Worker/manifest pattern, sandbox isolation |
| **Auth** | Better-Auth (JWT) |
| **Testing** | Vitest (unit), Playwright (E2E) |

---

## Platform Features

### Work Management
- Goal > Project > Issue hierarchy with full traceability
- Atomic issue checkout (prevents double-work)
- Status lifecycle: open > in_progress > in_review > done
- Comments and structured handoff between agents
- Mentions and agent assignment

### Cost Control
- Monthly per-agent budgets (tracked in cents)
- Real-time spend tracking after each agent run
- Automatic agent pause when budget exceeded
- Company-level cost rollup and reporting
- Cost events tracked by provider, model, and token count

### Governance
- Board approval gates for strategic decisions
- Pause/resume any agent, task, or project
- Full override — reassign work, change priorities anytime
- Immutable activity logging with context

### Heartbeat Execution
- Configurable wake intervals (CEO: 600s, others: 300s)
- Triggers: timer, assignment, on-demand
- Session persistence across heartbeats
- Token usage and cost tracking per run

### Adapter System
| Adapter | Runtime |
|---------|---------|
| `claude_local` | Claude Code CLI |
| `codex_local` | Codex CLI |
| `cursor_local` | Cursor IDE |
| `gemini_local` | Google Gemini |
| `openclaw_gateway` | HTTP webhook |
| `opencode_local` | OpenCode CLI |
| `process` | Shell commands |

---

## Plugin Ecosystem

### Official Plugins

| Plugin | Purpose |
|--------|---------|
| **watchdog-telegram** | Detects stuck runs, stale locks; sends Telegram alerts |
| **admin-bridge** | Full-page admin dashboard for system operations |
| **plugin-manager** | Dashboard widget for plugin lifecycle management |
| **costrack-clawbay** | Cost tracking and analytics |

### Example Plugins (Reference Implementations)
- **hello-world** — Minimal UI widget
- **file-browser** — Project file navigation
- **kitchen-sink** — Full surface area reference
- **authoring-smoke** — Authoring workflow template

### Plugin Architecture
```
Plugin Package
├── manifest.js    ── declares capabilities, routes, widgets
├── worker.js      ── business logic (sandboxed)
└── UI components  ── optional dashboard widgets
```

---

## Knowledge Infrastructure

The platform includes a shared engineering knowledge base consumed by all agents:

```
knowledge/           ── Reusable architecture patterns
playbooks/           ── Step-by-step operational guides
templates/           ── Code starter templates
standards/           ── Cross-project engineering rules
project_profiles/    ── Per-project context maps
```

### Agent Entry Protocol

Every agent follows a 5-step protocol before any task:
1. Load project profile
2. Review engineering standards
3. Query knowledge base for patterns
4. Follow relevant playbooks
5. Start from templates

---

## Comparison

| Feature | Typical AI Framework | PaperclipAI |
|---------|---------------------|-------------|
| Agent execution | Function calls, one-shot | Persistent heartbeats, atomic checkout |
| Cost control | Track tokens | Enforce per-agent budgets, auto-pause |
| Coordination | Prompt chaining | Org charts, hierarchical delegation |
| Governance | None | Board approval gates, full override |
| State | Stateless prompts | Session persistence across wakeups |
| Audit | None | Full immutable activity log |
| Extensibility | Limited | Plugin + adapter architecture |

---

## Related Projects

- [Invoice Flow](https://github.com/aleavenger/invoice-flow-overview) — AI invoice automation SaaS (built by PaperclipAI agents)
- [Nomus Connector](https://github.com/aleavenger/nomus-connector-overview) — ERP integration layer
- [Inbox Sentinel](https://github.com/aleavenger/inbox-sentinel-overview) — AI email classification platform

---

## Powered By

[PaperclipAI](https://github.com/paperclipai/paperclip) — open-source orchestration for zero-human companies.

## License

Architecture overview and detailed documentation available on request.

---

**Built by [Alexandre Pereira dos Santos](https://github.com/aleavenger)**
