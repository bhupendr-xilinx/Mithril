# Mithril

**A lightweight, IDE-neutral symbol index for any codebase.**

Mithril scans your repository, extracts symbols across many languages, and
writes a **grep-friendly cache** under `.mithril/` that developers, QA,
hardware engineers, and AI assistants can share via git — no database, no
external service, no vendor lock-in.

> *“Where is X?”* → read `INDEX.md`, grep `modules.md`, open source.  
> Stop re-scanning the tree every session.

---

## Why Mithril?

Large repos are expensive to navigate — especially for onboarding, test
traceability, cross-team handoffs, and AI agents that rediscover the same
symbols repeatedly. Mithril gives every tool the same **committed map**:

| Role | Typical use |
|------|-------------|
| **Developer** | Find APIs, assess refactor impact, onboard to a module |
| **QA / test** | Map tests → code; trace Req-IDs → automation |
| **HW / FW** | Locate drivers, register maps, RTL tops, lab scripts |
| **AI agent** | Persistent context; grep before broad search |

---

## Features

- **IDE-neutral** — Cursor, VS Code Copilot, Continue, Windsurf, Claude Code,
  JetBrains, Neovim, plain shell
- **Multi-language** — Python, JS/TS, Go, Rust, C/C++, Java, Kotlin, C#,
  Ruby, Shell, Tcl, Verilog/SV, VHDL, Lua, SQL
- **AI-ready** — `AGENT.md` defines a “read the graph first” workflow
- **Team-shareable** — commit `INDEX.md` + `modules.md`; everyone stays in sync
- **Configurable** — per-repo excludes, entry points, feature areas, traceability globs
- **One-command install** — `install.sh` deploys scripts, docs, and IDE wiring

---

## Quick start

### Install into a project

```bash
git clone https://github.com/bhupendr-xilinx/mithril.git /tmp/mithril
/tmp/mithril/install.sh /path/to/your-project
cd /path/to/your-project
```

Edit `.mithril/config.yaml` (repo name, entry points, excludes), then build:

```bash
python3 .mithril/scripts/build_index.py
git add .mithril .cursor .vscode .github
git commit -m "Add Mithril repository index"
```

### Refresh after merges

```bash
python3 .mithril/scripts/build_index.py
```

---

## What gets installed

```
.mithril/
├── README.md           # Quick reference
├── AGENT.md            # Universal AI instructions
├── DEPLOY.md           # Step-by-step IDE setup
├── IDE.md              # Per-IDE quick reference
├── STANDARDS.md        # Team convention
├── config.yaml         # Your repo profile
├── scripts/
│   └── build_index.py  # Multi-language indexer
├── by-area/            # Subsystem deep dives (you or agents maintain)
├── graphs/             # Optional Mermaid diagrams
├── INDEX.md            # Generated repo map
└── modules.md          # Generated symbol index ← grep this
```

Optional IDE wiring from `install.sh`:

| IDE | What is installed |
|-----|-------------------|
| **Cursor** | `.cursor/agents/mithril.md` |
| **VS Code** | Task **Mithril: rebuild index** |
| **Copilot** | `.github/copilot-instructions.md` (or snippet to merge) |
| **Root rules** | Snippets for `AGENTS.md`, `CLAUDE.md`, `.windsurfrules` |

---

## How agents use it

1. Read `.mithril/INDEX.md` for layout and entry points
2. **Grep** `.mithril/modules.md` for file paths and symbol names
3. Open `.mithril/by-area/` for subsystem deep dives
4. Only then read source or run ripgrep for line-level edits

Full rules: [`agent/AGENT.md`](agent/AGENT.md) (installed as `.mithril/AGENT.md`).

---

## Configuration

Copy and customise `config.example.yaml`:

```yaml
repo:
  name: my-project
  description: One-line summary for INDEX.md

entry_points:
  - path: src/main.py
    role: Application entry

feature_areas:
  - id: api
    title: HTTP API
    paths: [src/api/]

traceability_globs:
  - "**/*.csv"
  - "**/*.org"
```

See [`examples/smartnic-runbench-profile.yaml`](examples/smartnic-runbench-profile.yaml)
for a full Runbench / hardware-validation example.

---

## Supported languages

Python (AST) · JavaScript/TypeScript · Go · Rust · C/C++ · Java · Kotlin · C#
· Ruby · Shell · Tcl · Verilog/SystemVerilog · VHDL · Lua · SQL

Limit languages in `config.yaml` if you want a faster, smaller index.

---

## This repository (tool source)

| Path | Purpose |
|------|---------|
| [`install.sh`](install.sh) | Deploy into a consumer repo |
| [`scripts/build_index.py`](scripts/build_index.py) | Indexer |
| [`agent/AGENT.md`](agent/AGENT.md) | AI agent instructions |
| [`docs/`](docs/) | DEPLOY, IDE, STANDARDS guides |
| [`templates/`](templates/) | Cursor, VS Code, Copilot, root-file snippets |

**Do not commit** generated `INDEX.md` / `modules.md` in *this* tool repo —
those belong in each consumer project.

---

## Documentation

- **[DEPLOY.md](docs/DEPLOY.md)** — full deployment guide for any IDE
- **[IDE.md](docs/IDE.md)** — Cursor, VS Code, Windsurf, Claude, JetBrains, …
- **[STANDARDS.md](docs/STANDARDS.md)** — team convention for `.mithril/`
- **[PUBLISH.md](PUBLISH.md)** — how to fork or release this tool

---

## Author

[Bhupendra Singh](https://github.com/bhupendr-xilinx) · AMD / Xilinx
