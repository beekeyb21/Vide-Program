# BRIEF.md — Vide Program

## 1. Tổng quan

| Trường | Giá trị |
|--------|----------|
| **Tên** | Vide Program |
| **Type** | Workflow System / Meta-Framework |
| **Mục tiêu** | Nhà máy sinh plan cho Claude Code — đưa ý tưởng phần mềm → bộ kế hoạch hoàn chỉnh |
| **Deliverable** | Bộ 9 files MD chuẩn trên disk, sẵn sàng execute |

---

## 2. Creative DNA

### Domain & Audience
- **Domain:** Workflow automation cho quy trình phát triển phần mềm
- **Audience:** Solo developer không có background IT — cần hệ thống rõ ràng, không ambiguous
- **Pain point:** Claude Code hay quyết strategy sai → plan cần phân rõ ràng AI vs human decisions

### Platform & Format
- **Format:** 9 files Markdown trên disk + Notion pages
- **Execution:** Claude Code CLI (cửa sổ riêng cho Factory vs Builder)
- **Communication:** Notion là kênh trung gian giữa Claude.ai (Meta-Planner) và Claude Code

### Tone & Voice
- **Giọng viết:** Rõ ràng, ngắn gọn, action-oriented. Không fuzzy language.
- **Constraints:** Dùng bảng, checklist, bullet points. Không đoạn văn dài.
- **Emoji:** Dùng có mục đích (📖📌⚡🏭) — không spam

### Reference
- Vide Program không phải framework mới — là **quy trình vận hành** dựa trên Claude Code + Claude.ai + Notion
- Tương tự: automated pipeline, factory pattern, agent orchestration

---

## 3. Production Stack

| Layer | Tool |
|-------|------|
| Meta-Planner | Claude.ai (Claude Opus 4.6) |
| Executor | Claude Code CLI |
| Communication | Notion API |
| Automation | Supervisor Python script |
| Storage | Local filesystem |

---

## 4. Design Principles

1. **Phân rõ AI vs Human decisions** — không để AI tự quyết strategy
2. **Separation of concerns** — Meta-Planner thiết kế, Claude Code thực thi
3. **Notion as protocol** — state lives in Notion, code lives on disk
4. **Session isolation** — mỗi session có context snapshot, không để context tràn
5. **Hard constraints** — 7 constraints Claude Code encode vào mọi plan
