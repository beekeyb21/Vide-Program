# CLAUDE.md — Vide Program

## Profile

| Category | Value |
|----------|-------|
| Team | Solo (User) + AI Agents (Meta-Planner + Implementer) |
| Experience | User: newbie, no IT background |
| Languages | Mixed (Python, JS/TS, Go, Rust...) |
| Projects | Data/AI/ML, Videos, Game, Automation Scripts, Content Pipeline |
| OS | Linux |
| Docker | Sometimes |
| Codebase | Large (10K-100K lines) |

---

## Team Structure

```
[BẠN] ─────────────────────────────────────────────────────────
  │  Đưa ý tưởng, quyết định sáng tạo, review kết quả
  ▼
[CLAUDE.AI — Meta-Planner] (Claude.ai conversation)
  │  Thiết kế kiến trúc, sinh Factory Prompt (9 files MD)
  │  NOTION: https://www.notion.so/3256a9f8d3b481768852d8954989dec1
  ▼
[NOTION — Communication Channel]
  │  Lưu plan, state, handoff
  ▼
[CLAUDE CODE — Implementer] (2 cửa sổ riêng)
       ┌─ Cửa sổ 1 (Factory): Tạo folder + 9 files MD
       └─ Cửa sổ 2 (Builder): Execute Phase 1→N
```

---

## Session Management

### Start of Session
1. Đọc `progress.md` — hiểu state hiện tại
2. Đọc `next_session_prompt.txt` nếu tồn tại
3. Đọc `README.md` — refresh context

### During Session
- Report progress sau mỗi phase quan trọng
- Khi fail ≥ 3 lần → STOP → ghi blocked report → Notion Handoff
- KHÔNG tự quyết strategy khi ảnh hưởng kiến trúc

### End of Session (BẮT BUỘC — 6 bước)
1. Cập nhật `CLAUDE.md`: Current Status (Phase, Active task, Blocked by)
2. Ghi `progress.md`: ngày, phase, kết quả, metrics
3. Viết `next_session_prompt.txt` — prompt rõ ràng cho session sau
4. Update Notion Handoff page
5. Chạy Supervisor:
   ```bash
   python ~/Project/supervisor/supervisor.py \
     --project ~/Project/vide-program \
     --notion [LINK_HANDOFF]
   ```
6. Ghi Lessons Learned nếu có deviation:
   https://www.notion.so/32b6a9f8d3b481798761c6266b64477d

---

## Communication Protocol

- **Ngôn ngữ:** Tiếng Việt (cho user-facing), English (cho code)
- **Báo cáo:** Mỗi bước quan trọng trước khi thực hiện
- **Approval:** Hỏi trước khi commit, xóa file, thay đổi lớn
- **Khi không chắc:** Đề xuất 2-3 options → user quyết
- **Khi fail >3 lần:** Dừng → báo cáo → chờ instruction

---

## Permissions

✅ Tạo/sửa/xóa files trong project
✅ Viết code, tests, docs
✅ Commit, push, tạo branch (sau khi hỏi)
✅ Auto format code
✅ Debug và fix bugs
❌ Deploy lên server
❌ Xem/sửa .env / secrets / API keys
⚠️ Backup trước khi thay đổi lớn

---

## Current Status (2026-04-02)

| Field | Value |
|-------|-------|
| Phase | 5 — Deliver |
| Status | 🟢 COMPLETE — all phases done, awaiting user approval |
| Active Task | (none — project done) |
| Blocked by | (none) |
| Last Session | 2026-04-02 |

---

*Last Updated: 2026-04-02*
