# n8n Workflow Management — Version Control với n8n-atom + Git

> **Công cụ:** [n8n-atom](https://open-vsx.org/extension/atom8n/n8n-atom-v3) (VS Code / Cursor extension)
> **Đối tượng:** P. Kỹ thuật
> **Mục đích:** Quản lý n8n workflows as code — version control, review, rollback, backup.

---

## Tại sao cần Version Control cho n8n Workflows?

| Vấn đề (không có VC) | Giải pháp (có n8n-atom + Git) |
|----------------------|-------------------------------|
| Workflow sửa xong không biết ai sửa, sửa gì | Git history: commit message + author + timestamp |
| Workflow hỏng, không rollback được | `git revert` hoặc checkout version cũ |
| Không review được trước khi deploy | Pull Request → review → approve → merge |
| Mất workflow khi n8n server lỗi | Git repo = backup tự động |
| Người mới vào không hiểu workflow | Đọc commit history + PR description |
| Không track được version nào đang chạy | Git tag cho production version |

---

## Kiến trúc

```
┌──────────────────┐     ┌──────────────────┐     ┌──────────────┐
│  n8n Instance     │◄───►│  n8n-atom         │◄───►│  Git Repo    │
│  (production)     │sync │  (VS Code/Cursor) │push │  (GitHub)    │
│                   │     │                   │pull │              │
│  Workflows chạy   │     │  Edit workflows   │     │  JSON files  │
│  thực tế          │     │  as code          │     │  + history   │
└──────────────────┘     └──────────────────┘     └──────────────┘
```

---

## Setup ban đầu

### 1. Cài đặt n8n-atom

```bash
# VS Code / Cursor → Extensions → tìm "n8n-atom"
# Hoặc cài từ Open VSX:
# https://open-vsx.org/extension/atom8n/n8n-atom-v3
```

### 2. Kết nối n8n Instance

| Cấu hình | Giá trị |
|-----------|---------|
| n8n URL | *(URL của n8n instance)* |
| API Key | *(n8n API key — không lưu trong SOP)* |

### 3. Tạo Git Repo

| Cấu hình | Giá trị |
|-----------|---------|
| Repo name | *(điền — VD: `synity/n8n-workflows`)* |
| Platform | GitHub |
| Visibility | Private |
| Branch chính | `main` |

### 4. Cấu trúc folder trong repo

```
n8n-workflows/
├── README.md
├── workflows/
│   ├── lead-capture/
│   │   ├── google-form.json
│   │   ├── uchat.json
│   │   └── wix.json
│   ├── notifications/
│   │   └── (workflow files...)
│   └── (thêm folder khi có workflow mới)
└── .gitignore
```

> **Quy tắc đặt tên file:** `[chức-năng].json` — viết thường, dùng dấu gạch ngang, mô tả rõ workflow làm gì.

---

## Quy trình làm việc hàng ngày

### Sửa workflow hiện có

```
1. Pull latest từ Git repo
   └── git pull origin main

2. Pull workflow từ n8n instance (qua n8n-atom)
   └── n8n-atom: Pull Workflows

3. Sửa workflow trong editor
   └── Sửa trực tiếp hoặc dùng n8n-atom UI

4. Test trên n8n instance
   └── n8n-atom: Push to n8n → Test → Verify

5. Commit + Push lên Git
   └── git add . && git commit -m "message" && git push

6. (Nếu team > 1 người) Tạo PR → Review → Merge
```

### Thêm workflow mới

```
1. Tạo workflow trên n8n instance (hoặc qua n8n-atom)

2. Pull workflow về local (qua n8n-atom)
   └── n8n-atom: Pull Workflows

3. Đặt file vào đúng folder trong repo
   └── VD: workflows/lead-capture/new-source.json

4. Commit + Push
   └── git add . && git commit -m "Add [tên] lead capture workflow" && git push

5. Cập nhật tài liệu SOP
   └── Thêm section trong n8n-lead-capture.md
```

### Rollback khi workflow lỗi

```
1. Xác định commit gây lỗi
   └── git log --oneline workflows/lead-capture/google-form.json

2. Revert về version trước
   └── git revert [commit-hash]

3. Push workflow cũ lên n8n instance
   └── n8n-atom: Push to n8n

4. Verify workflow hoạt động
   └── Test trên n8n dashboard
```

---

## Quy tắc Commit Message

Format: `[action] [workflow]: [mô tả ngắn]`

| Action | Khi nào |
|--------|---------|
| `add` | Thêm workflow mới |
| `update` | Sửa workflow hiện có |
| `fix` | Sửa lỗi workflow |
| `remove` | Xóa workflow không dùng |

**Ví dụ:**
```
add google-form: Lead capture from Google Form via Google Sheet
update uchat: Add phone number validation before Bitrix push
fix wix: Fix SĐT format matching (0xxx vs +84xxx)
remove old-jotform: Replaced by Google Form workflow
```

---

## Checklist khi sửa workflow

- [ ] Pull latest từ Git repo trước khi sửa
- [ ] Test workflow trên n8n sau khi sửa
- [ ] Verify: Lead tạo đúng trong Bitrix? Contact + Company tạo đúng?
- [ ] Commit với message rõ ràng (theo format)
- [ ] Push lên Git repo
- [ ] Cập nhật tài liệu SOP nếu logic thay đổi

---

## Monitoring

### Khi workflow fail trên n8n

1. Kiểm tra n8n Dashboard → Executions → Error log
2. Xác định nguyên nhân
3. Fix workflow → Test → Commit
4. Nếu cần rollback: `git revert` → push workflow cũ lên n8n

### Kiểm tra định kỳ

| Tần suất | Kiểm tra |
|----------|----------|
| Hàng ngày | n8n Executions — có workflow fail không? |
| Hàng tuần | Git repo — có workflow nào sửa mà chưa commit không? |
| Hàng tháng | Review: workflow nào không còn dùng? Cleanup nếu cần |

---

## Liên kết

| Đi đến | Link |
|--------|------|
| Tổng quan hệ thống | [Link](overview.md) |
| n8n Lead Capture | [Link](n8n-lead-capture.md) |
| Landing P. Kỹ thuật | [Link](../landing/p-ky-thuat.md) |
| n8n-atom Extension | [Open VSX](https://open-vsx.org/extension/atom8n/n8n-atom-v3) |
| Trang chủ | [Link](../README.md) |
