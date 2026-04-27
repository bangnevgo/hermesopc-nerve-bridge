# HermesOPC-Nerve-Bridge

> ⚠️ **DEPRECATED** — Arsitektur ini sudah tidak digunakan.
> Lihat [OpenClaw Agent Skills.md](https://github.com/bangnevgo/OpenClaw-Agent-Skills) untuk arsitektur terbaru.

---

Integrasi **Hermes Agent (Python CLI)** ke dalam **Nerve UI (OpenClaw)** menggunakan arsitektur file-based bridge.

## Arsitektur Lama (Deprecated)

```
Nerve UI → OpenClaw Agent (hermes-aja) → incoming/request.json 
   → bridge_service.py (polling 2 detik) → Hermes CLI 
   → outgoing/response_nerve_req.json → OpenClaw Agent → Nerve UI
```

**Masalah dengan arsitektur lama:**
- Blocking: agent harus tunggu 5 detik
- Polling: bridge_service.py harus polling terus
- Fragile: timing-dependent, bisa miss kalau polling slow
- hermes-aja tidak punya identitas sendiri — hanya relay

---

## Arsitektur Baru (2026-04-27)

```
HERMES (Brain — Chief of Staff)
       │
       │ writes command
       ▼
/home/bangnevgo/AI-Team/shared-memory/hermes/out/
       │
       │ agent reads at startup
       ▼
OPENCLAW AGENT (Stephani, Nancy, Siska, Lena)
       │
       │ executes, writes report
       ▼
/home/bangnevgo/AI-Team/shared-memory/hermes/in/
       │
       │ Hermes reads when ready
       ▼
HERMES processes response
```

**Keuntungan arsitektur baru:**
- Non-blocking — agent baca kapan ready
- No polling — async, event-based via filesystem
- Hermes bisa queue commands
- Agent punya identity sendiri
- Lebih robust dan natural untuk async workflow

## Struktur Folder Baru

```
AI-Team/shared-memory/
├── hermes/
│   ├── out/                    # Commands dari Hermes ke agent
│   ├── in/                     # Reports dari agent ke Hermes
│   ├── memory/                  # Hermes memory
│   └── .openclaw/              # OpenClaw workspace state
└── openclaw/
    └── {agent-id}/in/          # Per-agent inbox
        ├── stephani-the-social-media-manager/
        ├── nancy-lead-manager/
        ├── siska-finance-advertising-manager/
        └── lena-website-manager/
```

## Communication Pattern

### Hermes → OpenClaw Agent (Command)
1. Hermes write command file ke `hermes/out/`
2. Agent baca dari inbox masing-masing saat startup
3. Format: `YYYYMMDD_HHMMSS_command.md`

### OpenClaw Agent → Hermes (Report)
1. Agent write report ke `hermes/in/`
2. Hermes baca dari situ saat ready
3. Format filename: `laporan_YYYYMMDD_dari_{agent-id}.md`

```markdown
FROM:{agent-id}
TO:hermes
TYPE:laporan
---
{isi laporan}
```

## Hermes = Brain, OpenClaw = Hands

| Komponen | Role | Description |
|----------|------|-------------|
| **HERMES** | Otak (Chief of Staff) | Deep thinking, self-learning, coordinates, reports |
| **hermes-aja** | Avatar | Hermes "nongkrong" di OpenClaw ecosystem |
| **OPENCLAW AGENTS** | Tangan (Execution) | Executes what Hermes plans |

## Agents yang Terintegrasi

| Agent | Fungsi | Hermes Bridge |
|-------|--------|---------------|
| Stephani | Social Media Manager | ✅ Active |
| Nancy | Lead Manager | ✅ Active |
| Siska | Finance & Ads Manager | ✅ Active |
| Lena | Website Manager | ✅ Active |

## Setup Hermes Bridge Baru

### 1. Hermes TOOLS.md (Hermes side)
```markdown
## Hermes Bridge — Kirim Perintah ke OpenClaw Agent
Path: /home/bangnevgo/AI-Team/shared-memory/hermes/out/
Format: command_YYYYMMDD_HHMMSS.md
```

### 2. Agent TOOLS.md (OpenClaw side)
```markdown
## HERMES BRIDGE — Kirim Laporan Ke Hermes
Path: /home/bangnevgo/AI-Team/shared-memory/hermes/in/
Format: laporan_YYYYMMDD_dari_{agent-id}.md
```

## Requirement

- Hermes Agent CLI (`hermes`)
- OpenClaw dengan workspace agents
- Shared folder: `/home/bangnevgo/AI-Team/shared-memory/`

## Referensi

- [OpenClaw Agent Skills](https://github.com/bangnevgo/OpenClaw-Agent-Skills) — Dokumentasi lengkap agents & skills
- [NEVGO Ecosystem Framework](https://github.com/bangnevgo/NEVGO-Ecosystem-Framework) — Arsitektur lengkap

## Author

Bang Nding — Nevgo Institute
