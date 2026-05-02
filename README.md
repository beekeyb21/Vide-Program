# Vide Program

> A plan factory for AI-coding agents — turn a software idea into a complete, ready-to-execute Markdown blueprint that an agentic coder (e.g. Claude Code) can run end-to-end without re-asking strategy questions.

*"Vide" as in **vide-coding** — not video.*

---

## Why this exists

Modern coding agents (Claude Code, Cursor, Aider, etc.) are powerful, but they fail in a predictable way: when given an open-ended idea, they make **strategic decisions silently** — picking architectures, abstractions, and trade-offs that the human never approved. The result is fast code that solves the wrong problem.

**Vide Program separates concerns:**

| Layer | Role | Decides | Does NOT decide |
|-------|------|---------|------------------|
| Human | Creative direction | What to build, what success looks like | Implementation details |
| Meta-Planner (Claude.ai) | Architecture & planning | Module boundaries, phases, constraints | Code-level details |
| Implementer (Claude Code CLI) | Execution | How to write the code that satisfies the plan | Strategy, scope, architecture |

The plan is materialised as a fixed set of 9 Markdown files on disk. The Implementer agent reads those files and executes — it cannot "improvise" because the constraints are explicit.

---

## Architecture

```
  ┌──────────────┐    idea    ┌──────────────────┐    plan     ┌────────────────┐
  │    Human     │ ─────────▶ │  Meta-Planner    │ ──────────▶ │  Implementer   │
  │  (creative)  │            │   (Claude.ai)    │   (9 .md)   │ (Claude Code)  │
  └──────────────┘            └──────────────────┘             └────────────────┘
         ▲                            │                                │
         │                            ▼                                ▼
         │                    ┌──────────────────┐            ┌────────────────┐
         └────── review ──────│  Notion Handoff  │◀── status ─│   Supervisor   │
                              │   (state log)    │            │ (session mgmt) │
                              └──────────────────┘            └────────────────┘
```

**Why three layers (not two):** A single agent that both plans and executes will collapse strategy into code-level decisions. Splitting the planner from the executor forces architecture decisions to be written down — which means a human can veto them before any code is generated.

---

## The 9-file blueprint

Every project produced by Vide Program contains exactly these files. The schema is fixed; agents always know where to look.

| File | Purpose |
|------|---------|
| `README.md` | Entry point + overview |
| `BRIEF.md` | Creative DNA: tone, audience, style, production stack |
| `CLAUDE.md` | Context injection for the executor agent |
| `ARCHITECTURE.md` | Data flow, modules, tech stack |
| `RULES.md` | 7 hard constraints encoded into every plan |
| `PHASES.md` | Phase plan with explicit exit criteria |
| `OPERATIONS.md` | Setup, run, debug guide |
| `modules/*.md` | Per-module spec |
| `progress.md` | Updated by the executor after each session |

The 7 hard constraints (`RULES.md`) cover: solo-dev assumptions, language conventions, context-budget thresholds, task lifecycle, blocked-task escalation (`fail≥3 → STOP`), no-architecture-decisions-by-agent, and zero-tolerance for `.env` access.

---

## Quickstart

**Prerequisites**
- Python 3.12+
- [Claude Code CLI](https://docs.anthropic.com/en/docs/claude-code)
- A Notion workspace + integration token

**1. Clone and configure**
```bash
git clone https://github.com/beekeyb21/Vide-Program.git ~/Project/vide-program
cd ~/Project/vide-program
cp .env.example .env
# edit .env — paste your Notion integration token
```

**2. Install dependencies**
```bash
pip install notion-client
```

**3. Verify connection**
```bash
python3 -c "import os; from notion_client import Client; \
  print(Client(auth=os.environ['NOTION_TOKEN']).users.me())"
```

**4. Use the workflow**
- Open Claude.ai with the Meta-Planner SOP, give it your project idea (10-question intake), receive a Factory Prompt.
- Open Claude Code (Factory window): paste Factory Prompt → it generates the 9-file blueprint folder.
- Open Claude Code (Builder window): point it at the new folder → it executes phase by phase.

See [`OPERATIONS.md`](./OPERATIONS.md) for the full session protocol.

---

## Repository structure

```
vide-program/
├── README.md              ← you are here
├── BRIEF.md               ← creative DNA
├── CLAUDE.md              ← agent context
├── ARCHITECTURE.md        ← system design
├── RULES.md               ← 7 hard constraints
├── PHASES.md              ← phase plan
├── OPERATIONS.md          ← setup & run
├── modules/               ← per-module specs
│   ├── 01_supervisor.md
│   └── 02_quick_ref.md
├── .env.example
└── .gitignore
```

The other `*.md` files at the root (`progress.md`, `PROJECT_CHRONICLE.md`, etc.) are session artefacts produced by the workflow itself — they're kept in-repo as a worked example of what Vide Program produces.

---

## Status & roadmap

- ✅ **v0.1 — spec frozen**: 9-file blueprint schema + 7 constraints validated on internal projects.
- 🔄 **v0.2 (in progress)**: open-source the Supervisor Python package as a reusable session-management library.
- 📋 **v0.3 (planned)**: agent-agnostic executor adapter (support Cursor / Aider / Codex CLI alongside Claude Code).

---

## License

Released under the MIT License — see `LICENSE`. *(License file pending — will be added before public announcement.)*

---

## Contact

- GitHub: [@beekeyb21](https://github.com/beekeyb21)
- Issues & feedback: please open a GitHub issue.

---

<details>
<summary><b>Tiếng Việt — bản gốc</b></summary>

## Vide Program là gì?

Vide Program là **nhà máy sinh plan** — nhận vào ý tưởng phần mềm bất kỳ, output ra bộ kế hoạch hoàn chỉnh để Claude Code thực thi mà không cần hỏi lại về strategy.

### Kiến trúc 3 tầng

- **Bạn (Human)** — Đưa ý tưởng, quyết định sáng tạo, review kết quả
- **Claude.ai (Meta-Planner)** — Thiết kế kiến trúc, sinh plan
- **Claude Code (Implementer)** — Execute code, không quyết strategy
- **Notion** — Kênh giao tiếp giữa các agent

### Nguyên tắc cứng

- Meta-Planner **KHÔNG** viết code
- Claude Code **KHÔNG** quyết strategy
- RULES sống trong `CLAUDE.md` local, không phải Notion

### Tài liệu cốt lõi

| File | Mục đích |
|------|----------|
| `BRIEF.md` | DNA sáng tạo, tone, audience, style |
| `CLAUDE.md` | Context injection, team, constraints |
| `ARCHITECTURE.md` | Data flow, modules, tech stack |
| `RULES.md` | 7 constraints Claude Code |
| `PHASES.md` | Phase plan + exit criteria |
| `OPERATIONS.md` | Setup, chạy, debug guide |
| `modules/*.md` | Spec từng module |
| `progress.md` | Trạng thái + metrics mỗi session |

</details>
