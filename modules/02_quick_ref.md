# Quick Reference Cards

## 👤 Bạn (Human)

| Khi nào | Làm gì |
|---------|--------|
| Muốn project mới | Điền Project Intake → Claude.ai (Meta-Planner) |
| Nhận Factory Prompt | Mở Claude Code #1, paste link |
| Review kết quả | Mở Claude Code #2, paste Builder link |
| Bị blocked | Đọc Handoff → quyết định → gửi lại |
| Session kết thúc | Claude Code tự chạy Supervisor |

---

## 🏭 Meta-Planner (Claude.ai)

**Trigger:** User gửi Project Intake (10 câu)

**SOP 6 bước:**
1. Analyze → hiểu domain + constraints
2. Design → kiến trúc + tech stack
3. Structure → folder + 9 files MD
4. Plan → phases + exit criteria
5. Generate → Factory Prompt + Builder Instructions
6. Output → 2 links (Factory + Builder)

**Output:** 2 Notion pages hoặc raw text

---

## 🏗️ Factory Agent (Claude Code #1)

**Trigger:** `Fetch [FACTORY_LINK] và thực hiện đúng theo instructions.`

**Làm:**
- Tạo folder `/ten-project/`
- Tạo 9 files MD chuẩn
- VIẾT CLAUDE.md với RULES, Team, Permissions

**KHÔNG LÀM:** Viết code thực tế, quyết strategy

**Output:** "Folder sẵn sàng" + link đến folder

---

## 🔨 Builder Agent (Claude Code #2)

**Trigger:** `Fetch [BUILDER_LINK] và thực hiện đúng theo instructions.`

**Làm:**
- Đọc folder (9 files MD)
- Execute Phase 1 → Phase 2 → ...
- Viết code, tests, docs
- Ghi progress.md

**Quy tắc:**
- Fail ≥3 → STOP → Notion Handoff
- Architecture decision → HỎI USER
- KHÔNG xem .env/secrets

**Cuối session:**
```bash
python ~/Project/supervisor/supervisor.py \
  --project ~/Project/ten-project \
  --notion [HANDOFF_URL] \
  --verbose
```

---

## ⚡ Supervisor (Python script)

**Chạy:** Cuối mỗi session Claude Code

**3 modes:**
```bash
# Mặc định — snapshot + Notion sync
supervisor.py --project ~/Project/X

# Review sâu
supervisor.py --project ~/Project/X --mode audit

# Verify không write
supervisor.py --project ~/Project/X --dry-run
```

**Output:**
- `context_snapshot.md` (~500 tokens)
- Notion Handoff updated
- Token budget report

**Exit codes:**
| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Error |
| 2 | Token budget exceeded |

---

## 🔄 Notion Pages

| Trang | URL | Dùng khi nào |
|-------|-----|--------------|
| Hướng Dẫn | https://www.notion.so/3366a9f8d3b481699a9fd0a57720ed61 | Setup lần đầu |
| SOP | https://www.notion.so/3256a9f8d3b481768852d8954989dec1 | Meta-Planner chạy SOP |
| Handoff | https://www.notion.so/3256a9f8d3b48192b673c7de4ddbf484 | Mỗi session |
| Lessons | https://www.notion.so/32b6a9f8d3b481798761c6266b64477d | Ghi bài học |

---

## 🚨 Emergency

| Tình huống | Xử lý |
|-----------|--------|
| Context tràn | `/compact` trong Claude Code |
| Bị blocked | Ghi Handoff → DỪNG → chờ user |
| Token budget 100% | THOÁT NGAY |
| Notion lỗi | Check token → check integration connected |
| Supervisor lỗi | `python3 -c "from supervisor import Supervisor"` |
