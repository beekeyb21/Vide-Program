# ARCHITECTURE.md — Vide Program

## 1. System Overview

Vide Program là **workflow orchestration system** cho Claude Code — không phải 1 ứng dụng mà là quy trình vận hành.

```
┌─────────────────────────────────────────────────────────────┐
│                         BẠN (Human)                          │
│   Đưa ý tưởng → Review kết quả → Quyết định sáng tạo          │
└────────────────────────────┬────────────────────────────────┘
                             │ Project Intake (10 câu)
                             ▼
┌─────────────────────────────────────────────────────────────┐
│              CLAUDE.AI — Meta-Planner                       │
│   • Thiết kế kiến trúc                                       │
│   • Sinh Factory Prompt (9 files MD)                        │
│   • Đọc SOP từ Notion                                        │
│   • Output: Factory Prompt + Builder Instructions            │
└────────────────────────────┬────────────────────────────────┘
                             │ Factory Prompt
                             ▼
┌─────────────────────────────────────────────────────────────┐
│         CLAUDE CODE — Cửa sổ 1 (Factory Mode)               │
│   • Tạo folder /ten-project/                                │
│   • Sinh 9 files MD chuẩn trên disk                         │
│   • KHÔNG viết code thực tế                                 │
└────────────────────────────┬────────────────────────────────┘
                             │ Folder sẵn sàng
                             ▼
┌─────────────────────────────────────────────────────────────┐
│         CLAUDE CODE — Cửa sổ 2 (Builder Mode)               │
│   • Đọc folder (9 files MD)                                 │
│   • Execute Phase 1 → Phase 2 → ... → Final                 │
│   • Ghi progress vào progress.md                            │
│   • KHÔNG quyết strategy                                    │
└────────────────────────────┬────────────────────────────────┘
                             │ End of Session
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                    SUPERVISOR PROGRAM                        │
│   • Tạo context_snapshot.md (~500 tokens)                   │
│   • Update Notion Handoff                                    │
│   • Theo dõi token budget                                    │
│   • Duy trì PROJECT_CHRONICLE.md                           │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. Module Map

### Core Modules

| Module | Responsibility | Input | Output |
|--------|--------------|-------|--------|
| **Project Intake** | Thu thập thông tin từ user | 10 câu hỏi | Structured brief |
| **Meta-Planner** | Thiết kế kiến trúc + plan | Intake | Factory Prompt (9 MD) |
| **Factory Agent** | Tạo folder structure | Factory Prompt | 9 files MD on disk |
| **Builder Agent** | Execute the plan | Folder (9 MD) | Running software |
| **Supervisor** | Session management | Session output | Snapshot + Notion update |

### Notion Pages

| Page | Purpose | Owner |
|------|---------|-------|
| Master Workspace | Tổng quan tất cả projects | Meta-Planner |
| Project Control | Link duy nhất cho 1 project | Meta-Planner |
| Session Handoff | State giữa sessions | Claude Code |
| Lessons Learned | Registry các bài học | Claude Code |
| Meta-Planner SOP | 6 bước sinh plan | Meta-Planner |

---

## 3. Data Flow

### Project Creation Flow
```
1. User → Project Intake (10 câu)
2. User → Claude.ai (Meta-Planner)
3. Claude.ai → Factory Prompt (9 MD)
4. Claude.ai → User (copy Factory Instructions link)
5. User → Claude Code #1: "Fetch [link] và tạo folder"
6. Claude Code #1 → Folder /ten-project/ (9 files MD)
7. User → Claude Code #2: "Fetch [link] và execute"
8. Claude Code #2 → Code + progress.md
9. Claude Code #2 → Supervisor → Notion Handoff
```

### Session Flow
```
1. Claude Code → Đọc progress.md + next_session_prompt.txt
2. Claude Code → Execute current phase
3. Claude Code → Ghi progress.md
4. Claude Code → Supervisor (tạo snapshot)
5. Claude Code → Update Notion Handoff
6. Claude Code → Ghi next_session_prompt.txt
```

---

## 4. Tech Stack

| Layer | Tool | Version |
|-------|------|---------|
| Meta-Planner | Claude.ai (Claude Opus 4.6) | Latest |
| Executor | Claude Code CLI | Latest |
| Communication | Notion API | v2 |
| Supervisor | Python 3 | 3.12+ |
| Auth | Notion Integration Token | ntn_... |

---

## 5. Constraints Architecture

7 constraints được encode vào **mọi plan** từ Meta-Planner:

1. **Solo IT background** → Không dùng thuật ngữ chuyên ngành không giải thích
2. **Ngôn ngữ**: Tiếng Việt (user-facing), English (code)
3. **Context**: >50% OK, 30-50% warn, <10% stop
4. **Task lifecycle**: PLAN→EXECUTE→VERIFY→HOUSEKEEP
5. **Block >3x** → STOP, report, chờ instruction
6. **Architecture decision** → KHÔNG tự quyết
7. **Secrets** → KHÔNG bao giờ xem/sửa .env
