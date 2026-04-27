# Hermes-OpenClaw Orchestration

> **Pioneering Architecture** — Hermes (Brain) orchestrates OpenClaw (Hands) agents via shared folder communication.

---

## Concept

**Hermes = Brain (Chief of Staff)**
- Deep thinking + self-learning
- Plans, coordinates, reports
- Direct access to terminal, file, web, VPS

**OpenClaw = Hands (Execution Layer)**
- Specialized agents execute what Hermes plans
- Stephani (Social), Nancy (Lead), Siska (Finance), Lena (Website)
- Each agent has its own identity and skills

**hermes-aja = Avatar**
- Hermes "present" in OpenClaw ecosystem
- Not a separate agent — it's Hermes via shared folder bridge

```
HERMES (Brain)
       │
       │ writes command
       ▼
/home/bangnevgo/AI-Team/shared-memory/hermes/out/
       │
       │ agent reads at startup
       ▼
OPENCLAW AGENT (Execution)
       │
       │ executes
       │ writes report
       ▼
/home/bangnevgo/AI-Team/shared-memory/hermes/in/
       │
       │ Hermes reads when ready
       ▼
HERMES processes & coordinates
```

## Why This Pattern?

| Old Pattern (Deprecated) | New Pattern |
|------------------------|-------------|
| Blocking: wait 5 seconds | Non-blocking: async |
| Polling required | No polling |
| hermes-aja = relay only | Agent has own identity |
| Timing-dependent | Queue-based |
| Fragile | Robust |

## Communication Flow

### Hermes → OpenClaw Agent (Command)
1. Hermes writes command to `hermes/out/`
2. Agent reads from inbox at startup
3. Format: `command_YYYYMMDD_HHMMSS.md`

### OpenClaw Agent → Hermes (Report)
1. Agent writes report to `hermes/in/`
2. Hermes reads when ready
3. Format: `laporan_YYYYMMDD_dari_{agent-id}.md`

```markdown
FROM:{agent-id}
TO:hermes
TYPE:laporan
---
{report content}
```

## Folder Structure

```
AI-Team/shared-memory/
├── hermes/
│   ├── out/              # Commands from Hermes
│   ├── in/                # Reports to Hermes
│   └── memory/            # Hermes memory
└── openclaw/
    └── {agent-id}/in/    # Per-agent inbox
        ├── stephani-the-social-media-manager/
        ├── nancy-lead-manager/
        ├── siska-finance-advertising-manager/
        └── lena-website-manager/
```

## Integrated Agents

| Agent | Role | Status |
|-------|------|--------|
| **Stephani** | Social Media Manager | ✅ Active |
| **Nancy** | Lead Manager | ✅ Active |
| **Siska** | Finance & Ads Manager | ✅ Active |
| **Lena** | Website Manager | ✅ Active |
| **SiSuz** | Orchestrator | ⚠️ Verify |

## Pioneer Status

This architecture was **discovered and implemented on 2026-04-27**.

Most people migrate from OpenClaw to Hermes. This approach instead:
- Keeps Hermes as the strategic brain
- Uses OpenClaw as distributed execution layer
- Pioneers shared folder orchestration pattern

## Setup

### Hermes Side (TOOLS.md)
```markdown
## Hermes Bridge — Kirim Perintah ke OpenClaw Agent
Path: /home/bangnevgo/AI-Team/shared-memory/hermes/out/
```

### OpenClaw Agent Side (TOOLS.md)
```markdown
## HERMES BRIDGE — Kirim Laporan Ke Hermes
Path: /home/bangnevgo/AI-Team/shared-memory/hermes/in/
Format: laporan_YYYYMMDD_dari_{agent-id}.md
```

## Requirements

- Hermes Agent CLI (`hermes`)
- OpenClaw with workspace agents
- Shared folder: `/home/bangnevgo/AI-Team/shared-memory/`

## References

- [OpenClaw Agent Skills](https://github.com/bangnevgo/OpenClaw-Agent-Skills) — Full agent documentation
- [NEVGO Ecosystem Framework](https://github.com/bangnevgo/NEVGO-Ecosystem-Framework) — Complete architecture

## Author

Bang Nding — Nevgo Institute
