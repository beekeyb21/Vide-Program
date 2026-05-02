# OPERATIONS.md — Vide Program

## Setup

### Yêu cầu hệ thống

| Tool | Version | Ghi chú |
|------|---------|---------|
| Python | 3.12+ | Supervisor script |
| Claude Code CLI | Latest | Executor |
| Notion Account | — | Workspace + Integration token |
| Git | Latest | Version control |

### Cài đặt

```bash
# 1. Install dependencies
pip install notion-client --break-system-packages

# 2. Verify Notion token (copy from .env — see .env.example)
export NOTION_TOKEN="ntn_REPLACE_ME"

# 3. Verify connection
python3 -c "from notion_client import Client; c=Client(auth='$NOTION_TOKEN'); print(c.users.me())"

# 4. Clone/setup project
git clone <repo-url> ~/Project/vide-program
```

---

## Chạy Vide Program

### Tạo Project Mới

```bash
# Bước 1: User điền Project Intake (trên Claude.ai)
# Bước 2: Meta-Planner sinh Factory Prompt
# Bước 3: Mở Claude Code cửa sổ 1 — Factory mode
Fetch [FACTORY_LINK] và thực hiện đúng theo instructions.

# Bước 4: Mở Claude Code cửa sổ 2 — Builder mode
Fetch [BUILDER_LINK] và thực hiện đúng theo instructions.
```

### Làm việc với Project Có Sẵn

```bash
# Mở Claude Code mới
Fetch [PROJECT_CONTROL_LINK]
Đọc toàn bộ, hiểu state hiện tại, sẵn sàng điều phối.
```

---

## Supervisor Usage

### Snapshot mode (mặc định, cuối mỗi session)
```bash
python ~/Project/supervisor/supervisor.py \
  --project ~/Project/vide-program
```

### Audit mode (review sâu)
```bash
python ~/Project/supervisor/supervisor.py \
  --project ~/Project/vide-program \
  --mode audit
```

### Notion update
```bash
python ~/Project/supervisor/supervisor.py \
  --project ~/Project/vide-program \
  --notion https://www.notion.so/3256a9f8d3b48192b673c7de4ddbf484
```

### Verbose + Dry-run
```bash
# Verbose: xem chi tiết từng bước
python ~/Project/supervisor/supervisor.py \
  --project ~/Project/vide-program \
  --verbose

# Dry-run: verify setup không write gì
python ~/Project/supervisor/supervisor.py \
  --project ~/Project/vide-program \
  --dry-run
```

### Full command cuối session
```bash
python ~/Project/supervisor/supervisor.py \
  --project ~/Project/vide-program \
  --notion https://www.notion.so/3256a9f8d3b48192b673c7de4ddbf484 \
  --verbose
```

---

## Debug Guide

### Lỗi Notion API

```bash
# Verify token
python3 -c "from notion_client import Client; c=Client(auth='ntn_REPLACE_ME'); print(c.blocks.children.list('xxx'))"

# Check page access
# → Vào Notion → Mở page → Settings → Connections → Verify integration connected
```

### Lỗi Supervisor

```bash
# Test imports
python3 -c "from supervisor import Supervisor; print('OK')"

# Verbose mode (nếu có)
python3 ~/Project/supervisor/supervisor.py --project ~/Project/vide-program --verbose
```

### Lỗi Claude Code

```bash
# Verify Claude Code installed
which claude && claude --version

# Reset session context
# → /compact hoặc /clear trong Claude Code
```

---

## Notion Pages Reference

| Trang | URL | Mục đích |
|-------|-----|---------|
| Hướng Dẫn Vận Hành | https://www.notion.so/3366a9f8d3b481699a9fd0a57720ed61 | Master doc |
| Meta-Planner SOP | https://www.notion.so/3256a9f8d3b481768852d8954989dec1 | 6 bước sinh plan |
| Session Handoff | https://www.notion.so/3256a9f8d3b48192b673c7de4ddbf484 | State giữa sessions |
| Lessons Learned | https://www.notion.so/32b6a9f8d3b481798761c6266b64477d | Registry bài học |

---

## Troubleshooting

| Vấn đề | Giải pháp |
|--------|-----------|
| Notion page trống | Integration chưa được connect → Settings → Connections |
| Supervisor lỗi import | `pip install notion-client --break-system-packages` |
| Context tràn | `/compact` trong Claude Code |
| Claude Code quyết sai strategy | Đọc RULES.md → nhắc nhở agent |
| Notion token hết hạn | Tạo integration mới tại https://www.notion.so/my-integrations |
