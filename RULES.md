# RULES.md — Vide Program

## 7 Constraints của Claude Code (encode vào mọi plan)

| # | Rule | Chi tiết |
|---|------|----------|
| 1 | Solo IT background | User không có IT background — mọi thuật ngữ cần giải thích hoặc link docs |
| 2 | Ngôn ngữ | Tiếng Việt cho user-facing, English cho code/comments, tiếng Anh khi user yêu cầu |
| 3 | Context Budget | >50% OK, 30-50% cảnh báo compact, <10% DỪNG NGAY |
| 4 | Task Lifecycle | PLAN → EXECUTE → VERIFY → HOUSEKEEP (mỗi bước phải done trước khi next) |
| 5 | Block Protocol | Fail ≥3 lần → STOP → ghi report → Notion Handoff → chờ instruction |
| 6 | Architecture Block | Quyết định ảnh hưởng kiến trúc → HỎI USER, không tự quyết |
| 7 | Secrets Zero-Tolerance | KHÔNG bao giờ xem, sửa, hoặc output nội dung .env, secrets, API keys |

---

## Session Rules

### Task Lifecycle
1. **PLAN** — Hiểu requirements, break down thành sub-tasks <15 phút
2. **EXECUTE** — Thực hiện sub-task, không nhảy bước
3. **VERIFY** — Chạy tests, kiểm tra output đúng
4. **HOUSEKEEP** — Update progress.md, ghi notes, cleanup

### Context Management
- Khi context >50%: Bình thường, tiếp tục
- Khi context 30-50%: Cảnh báo → compact ngay (summarize done, focus current)
- Khi context <10%: DỪNG → save progress → báo user

### Task Sizing
- Task >20 phút → BẮT BUỘC chia thành sub-tasks <15 phút
- Mỗi sub-task = 1 unit of work rõ ràng, có output cụ thể

---

## Code Quality Rules

| Rule | Áp dụng |
|------|---------|
| DRY (Don't Repeat Yourself) | Kiểm tra existing code TRƯỚC khi viết mới |
| No leakage | Không duplicate logic — dùng shared modules |
| Consistency | Giữ style đồng nhất với codebase hiện tại |
| @dataclass config | Config objects phải là dataclass, không dict lộn xộn |
| JSON output | Tool outputs phải dùng JSON schema rõ ràng |
| Max 300 lines | Mỗi module ≤300 lines — chia nếu lớn hơn |
| Import verification | Luôn verify imports + execution sau khi viết |

---

## File Hygiene Rules

| Rule | Action |
|------|--------|
| `__init__.py` updated | Khi thêm module mới → update `__init__.py` |
| Duplicates | Phát hiện duplicate → refactor ngay |
| CLAUDE.md updated | Khi architecture/task thay đổi → update CLAUDE.md |
| Progress saved | Sau mỗi session → BẮT BUỘC update progress.md |

---

## Communication Rules

- **Report:** Mỗi bước quan trọng TRƯỚC khi thực hiện
- **Approval:** Hỏi TRƯỚC commit, xóa file, thay đổi lớn cấu trúc
- **Options:** Khi user không rõ → đề xuất 2-3 options kèm giải thích
- **Problem reporting:** Mô tả vấn đề + đề xuất solutions — KHÔNG chỉ report có vấn đề
- **No self-decision:** Khi ảnh hưởng architecture → HỎI, không tự quyết

---

## Error Recovery

| Tình huống | Action |
|-----------|--------|
| Fail lần 1-2 | Phân tích nguyên nhân → fix |
| Fail lần 3 | STOP → ghi báo cáo 4 phần → Notion Handoff |
| Interrupt (Ctrl+C) | Save progress ngay → ghi next_session_prompt.txt |
| Context <10% | DỪNG → save → báo user |
| Không tìm thấy file | Kiểm tra path, gợi ý tạo mới nếu cần |

---

## Blocked Report Template

```markdown
## Blocked Report

**Phase đang chạy:** [Phase X]
**Task bị blocked:** [Tên task cụ thể]

### Đã thử
1. [Attempt 1] — [Mô tả] → [Kết quả]
2. [Attempt 2] — [Mô tả] → [Kết quả]
3. [Attempt 3] — [Mô tả] → [Kết quả]

### Cần từ Meta-Planner
[Chỉ rõ quyết định strategy mà agent không thể tự quyết]

### Lesson Learned
[Bài học rút ra — ghi vào Lessons Learned Registry]
```
