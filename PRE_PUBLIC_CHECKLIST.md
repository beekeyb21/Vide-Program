# PRE_PUBLIC_CHECKLIST.md — Vide Program

> Audit completed 2026-05-02. **Nothing has been auto-fixed.** Each item below requires your review/approval before action.
> Target: public this repo on GitHub for Xiaomi MIMO grant submission.

---

## ⛔ STOP-SHIP BLOCKERS (must fix before first push)

### B1. Hardcoded Notion API token in `OPERATIONS.md:21`

```
export NOTION_TOKEN="ntn_REDACTED_SEE_GIT_HISTORY_AUDIT"
```

This is a real-looking Notion integration token committed in plaintext.

**Required actions (in this order):**
1. **Revoke immediately** at https://www.notion.so/my-integrations — assume it's compromised the moment the repo goes public.
2. Regenerate a new token, store it in `.env` (which will be gitignored — see B2).
3. Replace line 21 with placeholder: `export NOTION_TOKEN="ntn_REPLACE_ME"` (and add a comment pointing to `.env.example`).
4. Same fix in `OPERATIONS.md:107` (debug command also references the token format).

**Note on git history:** No `git filter-repo` is needed — the parent `.git` has **0 commits** (see B3). The token has never been committed yet. Sanitize it on disk before the first commit.

---

### B2. No `.gitignore` exists anywhere

Checked: `vide-program/.gitignore`, parent `Project/.gitignore`, `~/.gitignore_global` — **none exist**.

**Required actions:**
- Create `vide-program/.gitignore` before the first commit. Recommended contents:

```
# Secrets
.env
.env.*
!.env.example
*.key
*.pem
credentials*
secrets*

# Python
__pycache__/
*.py[cod]
*.egg-info/
.pytest_cache/
.mypy_cache/
.ruff_cache/
.venv/
venv/
env/

# Node
node_modules/
dist/
build/

# OS / editors
.DS_Store
.idea/
.vscode/
*.swp

# Session/work-log artifacts (see item Q1)
context_snapshot.md
next_session_prompt.txt
```

- Also create `vide-program/.env.example` with the variable names but no values:
```
NOTION_TOKEN=ntn_your_token_here
```

---

### B3. Repository is **not a standalone git repo**

Critical structural finding:
- `vide-program/.git` does **not exist**.
- The git repo lives at `/home/jared/Project/.git` and has **0 commits**.
- `git status` is currently tracking the entire `/home/jared/Project/` directory tree (Docs/, Flyer/, Logistic/, matrix_engine_v3/, Red Pill/, supervisor/, etc. — many of which probably should NOT be public).

**Required actions:**
1. Decide the intended scope. Options:
   - **(a)** Init a fresh repo *inside* `vide-program/` (`cd vide-program && git init`). Cleanest. Recommended.
   - **(b)** Use the parent `.git` and add a strict `.gitignore` to exclude every sibling folder. Risky — one mistake leaks unrelated projects.
2. If (a): you'll lose any cross-folder context (e.g., `~/Project/supervisor/` referenced in code). Either copy the supervisor scripts into vide-program, or remove the references.
3. After the user approves, the first commit should be made *after* B1 + B2 are fixed.

---

## ⚠️ HIGH PRIORITY — fix before tagging the repo public

### H1. Private Notion workspace URLs leaked across docs

9 hardcoded Notion URLs (your private workspace) in:

| File | Lines |
|------|-------|
| `README.md` | 83, 84, 85, 86, 87 |
| `OPERATIONS.md` | 75, 95, 139, 140, 141, 142 |
| `CLAUDE.md` | 25, 61 |
| `modules/02_quick_ref.md` | 105, 106, 107, 108 |

These reveal your private Notion workspace structure. Anyone reading the README will see them. Even though Notion access is gated, leaking the URLs:
- Exposes the workspace IDs to phishing/social engineering.
- Looks unprofessional to a grant reviewer (private internal links in a public README).

**Required actions:**
- Replace each Notion URL with a placeholder like `https://www.notion.so/<your-workspace>/<page-id>` OR remove the section entirely from public-facing docs (move to a separate `INTERNAL.md` that's gitignored).
- Decide per file:
  - `README.md` — strongly recommend remove the "Liên kết Notion" section entirely; reviewers don't need it.
  - `OPERATIONS.md` — keep structure, replace URLs with placeholders.

---

### H2. Hardcoded absolute path `/home/jared/...`

`PROJECT_CHRONICLE.md:11` contains: `Tạo /home/jared/Project/vide-program/`

**Required action:** replace with `~/Project/vide-program/` or `<your-workspace>/vide-program/`.

The 25+ other `~/Project/...` references are tilde-prefixed and acceptable, but they assume a fixed layout (`~/Project/supervisor/` as sibling). Consider: in a public release, either bundle the supervisor inside the repo, or change instructions to relative paths.

---

### H3. README is Vietnamese-only — fails grant audience requirement

Current `README.md` is 100% in Vietnamese. Xiaomi MIMO reviewers will not be able to evaluate it.

Issues vs. an international-grant-reviewer audience:
- ❌ No English version
- ❌ No project tagline / one-line value proposition
- ❌ No architecture diagram (only ASCII)
- ❌ No quickstart (defers to OPERATIONS.md)
- ❌ No license declaration
- ❌ No contribution guide / contact
- ❌ Trailing private Notion links (see H1)
- ❌ No demo, screenshots, or example output

**Recommended README v2 structure** (do not implement until you approve):

```
# Vide Program

> Plan factory for AI-coding agents — turn a software idea into a complete,
> ready-to-execute Markdown blueprint that Claude Code can run without
> re-asking strategy questions.

[badges: license, language, status]

## Why
- 1-2 paragraphs framing the agentic-coding pain point this solves.

## Architecture
[mermaid diagram — replaces ASCII]
Three-tier separation: Human (creative) → Meta-Planner (Claude.ai) →
Executor (Claude Code). Notion is the handoff protocol.

## Quickstart
- Prereqs: Python 3.12+, Claude Code CLI, Notion workspace
- 5-step "create your first plan" walkthrough

## How it works
- Project Intake (10-question schema)
- 9-file Markdown blueprint specification
- Session lifecycle + Supervisor

## Repository structure
- Tree of /vide-program/ + brief per-file purpose

## Roadmap

## License
- Pick one (MIT/Apache-2.0 are the most grant-friendly).

## Acknowledgements
- Reference frameworks/inspirations.

---
(Vietnamese version below or in README.vi.md)
```

⚠️ **Mismatch with your task description**:
You mentioned the README should highlight "9Router proxy integration, ElevenLabs Vietnamese TTS, agentic script generation pipeline." **None of those features appear anywhere in this repo** — `vide-program` is a workflow/planning meta-framework, not a TTS or script-generation pipeline. Three possibilities — please clarify before I draft README v2:
1. You meant a different project (e.g., one of the sibling folders like `ma-tran-thuc-pham/` or `matrix_engine_v3/`).
2. Those features exist outside this folder and you want them integrated/referenced here.
3. The README should pitch a different project than what's actually in the repo (I'd push back — misleading a grant reviewer is high-risk).

---

## 🟡 MEDIUM PRIORITY — quality issues, recommend fix

### Q1. Session-state files clutter the repo for outsiders

These files are personal session work-logs that don't belong in a polished public repo:

| File | What it is | Recommendation |
|------|------------|----------------|
| `progress.md` | Per-session progress log | Move to `docs/internal/` or gitignore |
| `context_snapshot.md` | Transient ~500-token snapshot | Gitignore (already in B2 list) |
| `next_session_prompt.txt` | Next-session resume prompt | Gitignore (already in B2 list) |
| `BLOCKERS.md` | Internal blockers | Move to issues, or delete |
| `CURRENT_STATE.md` | Internal state | Delete or merge into progress |
| `CONTEXT_SUMMARY.md` | Internal | Delete or merge |
| `TASK_LOG.md` | Internal | Delete or merge |
| `DECISIONS.md` | Internal ADRs | Keep IF cleaned up; move to `docs/decisions/` |
| `PROJECT_CHRONICLE.md` | Internal timeline | Move to `docs/` or gitignore |

A grant reviewer landing on the repo should see: README → ARCHITECTURE → quickstart. They should not see 8 internal session files at the top level.

---

### Q2. `CLAUDE.md` exposes solo-dev posture

`CLAUDE.md:8` says "User: newbie, no IT background". Honest, but reads weakly to a reviewer evaluating technical capability for a grant. Consider keeping `CLAUDE.md` (it's an interesting artifact of agentic dev) but rewording the team profile, or moving it to `docs/`.

---

### Q3. Commit history check — N/A

You asked for "30 commit gần nhất". There are **0 commits**. Nothing to flag. (Possibly you were thinking of a different repo?)

When you do start committing, suggested message style for grant-quality optics:
- `feat: …`, `docs: …`, `chore: …` (Conventional Commits)
- No emojis, no Vietnamese in commit subjects
- Sign-off with your real name + email if you want professional attribution

---

## 🟢 LOW PRIORITY — nice-to-have

- **L1.** Add `LICENSE` file (MIT or Apache-2.0 most common for grant submissions).
- **L2.** Add `CITATION.cff` if you want reviewers to cite the project.
- **L3.** Add badges (License, Status, Language) to README.
- **L4.** Add `CONTRIBUTING.md` or a single "Contact" line.
- **L5.** Replace the 4 ASCII diagrams with Mermaid (renders natively on GitHub).
- **L6.** No `requirements.txt` / `pyproject.toml` / `package.json` exists — if Vide Program is "just docs," that's fine; if you want reviewers to *run* the supervisor, you need a manifest.

---

## ✅ VERIFIED CLEAN

- No `.env`, `.key`, `.pem`, or `credentials*` files on disk.
- No real names or email addresses in any file.
- No GitHub tokens, AWS keys, OpenAI keys, etc. (only the 1 Notion token in B1).
- No `git log` history to scrub (0 commits).
- No `node_modules/` or `__pycache__/` accidentally present.

---

## Recommended order of operations

1. **B1** — revoke + rotate the Notion token. *Today, before anything else.*
2. **B3** — decide repo scope (init inside vide-program/ vs. parent).
3. **B2** — write `.gitignore` and `.env.example`.
4. **H1, H2** — sanitize Notion URLs and absolute path.
5. **H3** — confirm the project description mismatch (9Router/ElevenLabs/TTS) and draft README v2.
6. **Q1** — decide what to do with session-state files.
7. First commit. Then push.
8. Final sanity check: `git ls-files | xargs grep -l "ntn_\|/home/jared\|notion.so/[0-9a-f]"` should return nothing meaningful.

---

*Audit produced by Claude Code on 2026-05-02. No files modified. Awaiting your go-ahead per item.*
