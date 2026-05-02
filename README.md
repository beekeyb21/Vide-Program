# 📖 Vide Program — Hướng Dẫn Vận Hành

> Đọc trang này trước khi bắt đầu bất kỳ project nào.

---

## Vide Program là gì?

Vide Program là **nhà máy sinh plan** — nhận vào ý tưởng phần mềm bất kỳ, output ra bộ kế hoạch hoàn chỉnh để Claude Code thực thi mà không cần hỏi lại về strategy.

> "Vide" trong *vide-coding* — không phải video.

---

## Kiến trúc 3 tầng

```
[BẠN] — Đưa ý tưởng, quyết định sáng tạo, review kết quả
   ↓
[CLAUDE.AI — Meta-Planner] — Thiết kế kiến trúc, sinh plan
   ↓
[NOTION — Kênh giao tiếp] — Lưu plan, state, handoff giữa agents
   ↓
[CLAUDE CODE — Implementer] — Execute code, không quyết strategy
```

**Nguyên tắc cứng:**
- Meta-Planner **KHÔNG** viết code
- Claude Code **KHÔNG** quyết strategy
- Notion là kênh giao tiếp — RULES sống trong `CLAUDE.md` local, không phải Notion

---

## Folder structure chuẩn (9 files)

```
/vide-program/
├── README.md          ← Entry point — đọc cái này trước
├── BRIEF.md           ← DNA sáng tạo (tone, audience, style, production stack)
├── CLAUDE.md          ← Context injection cho agent
├── ARCHITECTURE.md    ← Data flow, modules, tech stack
├── RULES.md           ← Constraints kỹ thuật + sáng tạo
├── PHASES.md          ← Phase plan với exit criteria
├── OPERATIONS.md      ← Setup, chạy, debug
├── modules/
│   ├── 01_setup.md    ← Spec từng module
│   └── ...
└── progress.md        ← Claude Code ghi sau mỗi session
```

---

## Quick Reference — Khi nào dùng gì

| Tình huống | Action |
|------------|--------|
| Bắt đầu project mới | Điền Project Intake → Claude.ai (Meta-Planner) |
| Tạo folder project | Claude Code cửa sổ 1 (Factory mode) |
| Execute code | Claude Code cửa sổ 2 (Builder mode) |
| Bị blocked (fail ≥3) | Ghi report → Notion Handoff → Chờ instruction |
| Kết thúc session | Chạy End of Session Protocol (5 bước) |

---

## Các tài liệu cốt lõi

| File | Mục đích |
|------|----------|
| `README.md` | Entry point — overview + quick reference |
| `BRIEF.md` | DNA sáng tạo, tone, audience, style |
| `CLAUDE.md` | Context injection, team, constraints |
| `ARCHITECTURE.md` | Data flow, modules, tech stack |
| `RULES.md` | 7 constraints Claude Code + creative rules |
| `PHASES.md` | Phase plan + exit criteria |
| `OPERATIONS.md` | Setup, chạy, debug guide |
| `modules/*.md` | Spec từng module |
| `progress.md` | Trạng thái + metrics mỗi session |

---

## Liên kết Notion

- **Master Workspace:** https://www.notion.so/3256a9f8d3b481e7aa16f276ed500820
- **Hướng Dẫn Vận Hành:** https://www.notion.so/3366a9f8d3b481699a9fd0a57720ed61
- **Session Handoff:** https://www.notion.so/3256a9f8d3b48192b673c7de4ddbf484
- **Lessons Learned:** https://www.notion.so/32b6a9f8d3b481798761c6266b64477d
- **Meta-Planner SOP:** https://www.notion.so/3256a9f8d3b481768852d8954989dec1

---

*Last Updated: 2026-04-02*
