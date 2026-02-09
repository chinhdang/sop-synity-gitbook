# n8n Lead Capture — Nguồn dữ liệu → Bitrix Lead

> **Pattern:** Data source bên ngoài → n8n workflow → Bitrix24 REST API → Tạo/Cập nhật Lead + Contact + Company.
> **Đối tượng:** P. Kỹ thuật (setup & maintain).
> **Liên quan SOP:** [Bước 02 — Survey](../buoc/02-survey.md) (AR-1, AR-2, AR-3)

---

## Luồng chung (áp dụng cho mọi source)

```
┌─────────────┐     ┌─────────────┐     ┌──────────────────────────┐
│ Data Source  │────►│  n8n        │────►│  Bitrix24 REST API       │
│ (GG Form,   │     │  Workflow   │     │                          │
│  UChat,     │     │             │     │  crm.lead.add / update   │
│  Wix...)    │     │  Transform  │     │  crm.contact.add         │
│             │     │  + Validate │     │  crm.company.add         │
└─────────────┘     └─────────────┘     └──────────────────────────┘
```

### Quy trình chuẩn mỗi workflow

1. **Receive** — Nhận data từ source (webhook, polling, hoặc trigger)
2. **Validate** — Kiểm tra data bắt buộc (SĐT, Tên KH)
3. **Match** — Tìm Lead/Contact có sẵn trong Bitrix (match SĐT)
4. **Transform** — Map fields từ source → Bitrix field names
5. **Push** — Gọi Bitrix REST API để tạo/cập nhật
6. **Error handling** — Log lỗi, gửi notification nếu fail

---

## Danh sách Data Sources

| # | Source | Loại | n8n Workflow | Trạng thái | Ghi chú |
|---|--------|------|-------------|------------|---------|
| # | Source | Loại | n8n Workflow | Git file | Trạng thái |
|---|--------|------|-------------|----------|------------|
| 1 | [Google Form](#1-google-form) | Form khảo sát | *(tên workflow)* | *(path trong repo)* | Active |
| 2 | [UChat](#2-uchat) | Chatbot | *(tên workflow)* | *(path trong repo)* | Active |
| 3 | [Wix](#3-wix) | Website form | *(tên workflow)* | *(path trong repo)* | Active |

> 🔧 Tất cả workflow files được quản lý qua [n8n-atom + Git](n8n-workflow-management.md). Mọi thay đổi phải commit vào repo.

---

## 1. Google Form

### Flow chi tiết

```
KH điền Google Form
       │
       ▼
Google Sheet (Form Responses)
       │
       ▼ (n8n: Google Sheets trigger / webhook)
┌──────────────────────────────┐
│  n8n Workflow: [tên]         │
│                              │
│  1. Trigger: New row in      │
│     Google Sheet             │
│  2. Validate: SĐT required  │
│  3. Match Lead by SĐT       │
│     (crm.lead.list)          │
│  4. IF match → Update Lead   │
│     (crm.lead.update)        │
│     IF no match → Add Lead   │
│     (crm.lead.add)           │
│  5. Add Contact              │
│     (crm.contact.add)        │
│  6. Add Company              │
│     (crm.company.add)        │
│  7. Link Contact ↔ Company   │
│     ↔ Lead                   │
│  8. Update Lead stage →      │
│     IN_PROCESS               │
└──────────────────────────────┘
```

### Field Mapping

| Google Form field | Bitrix Lead field | Bắt buộc | Ghi chú |
|-------------------|-------------------|----------|---------|
| *(Họ tên)* | `NAME` + `LAST_NAME` | YES | Tách Họ / Tên |
| *(SĐT)* | `PHONE` | YES | Dùng để match Lead |
| *(Email)* | `EMAIL` | YES | |
| *(Tên công ty)* | `COMPANY_TITLE` | YES | |
| *(Chức vụ)* | `POST` | Nên có | |
| *(Nhu cầu)* | `COMMENTS` | Nên có | Append vào ghi chú |
| *(Quy mô)* | `UF_CRM_...` | Nên có | *(Xác nhận UF field)* |
| *(Nguồn)* | `SOURCE_ID` | Auto | Set = `WEB` hoặc source tương ứng |

> **Lưu ý:** Field mapping chi tiết phụ thuộc vào cấu trúc Google Form hiện tại. P. Kỹ thuật cập nhật bảng này khi form thay đổi.

### Configuration

| Cấu hình | Giá trị | Ghi chú |
|-----------|---------|---------|
| Google Sheet ID | *(điền)* | Sheet chứa form responses |
| Sheet name / Tab | *(điền)* | Tab cụ thể |
| n8n Workflow ID | *(điền)* | |
| n8n Workflow name | *(điền)* | |
| Bitrix Webhook URL | `https://tamgiac.bitrix24.com/rest/1/xxxxx/` | *(không lưu key thật trong SOP)* |
| Trigger type | Google Sheets trigger / Polling | |
| Polling interval | *(điền)* | Nếu dùng polling |

### Troubleshooting

| Triệu chứng | Nguyên nhân có thể | Cách xử lý |
|-------------|-------------------|-------------|
| KH điền form nhưng Lead không cập nhật | SĐT trong form khác SĐT trong Lead | Kiểm tra format SĐT (0xxx vs +84xxx) |
| Lead tạo mới thay vì update Lead có sẵn | Match SĐT không tìm thấy | Kiểm tra n8n node match logic |
| Contact/Company không tạo | API error từ Bitrix | Kiểm tra n8n execution log |
| Workflow không trigger | Google Sheet trigger bị disconnect | Reconnect trigger trong n8n |

---

## 2. UChat

### Flow chi tiết

```
KH chat qua Facebook/Zalo
       │
       ▼
UChat chatbot (thu thập: Tên, SĐT, Email, Nhu cầu)
       │
       ▼ (Webhook → n8n)
┌──────────────────────────────┐
│  n8n Workflow: [tên]         │
│                              │
│  1. Trigger: Webhook từ      │
│     UChat                    │
│  2. Validate: SĐT required  │
│  3. Match Lead by SĐT       │
│  4. Create/Update Lead       │
│  5. Create Contact           │
│  6. Update Lead stage        │
└──────────────────────────────┘
```

### Field Mapping

| UChat field | Bitrix Lead field | Bắt buộc |
|-------------|-------------------|----------|
| *(Họ tên)* | `NAME` + `LAST_NAME` | YES |
| *(SĐT)* | `PHONE` | YES |
| *(Email)* | `EMAIL` | Nên có |
| *(Nhu cầu)* | `COMMENTS` | Nên có |
| *(Nguồn)* | `SOURCE_ID` | Auto = `UC_...` |

### Configuration

| Cấu hình | Giá trị |
|-----------|---------|
| UChat Webhook URL | *(điền)* |
| n8n Workflow ID | *(điền)* |
| n8n Workflow name | *(điền)* |

---

## 3. Wix

### Flow chi tiết

```
KH điền form trên Website (Wix)
       │
       ▼ (Wix Webhook → n8n)
┌──────────────────────────────┐
│  n8n Workflow: [tên]         │
│                              │
│  1. Trigger: Webhook từ Wix  │
│  2. Validate: SĐT required  │
│  3. Match Lead by SĐT       │
│  4. Create/Update Lead       │
│  5. Create Contact           │
│  6. Update Lead stage        │
└──────────────────────────────┘
```

### Field Mapping

| Wix form field | Bitrix Lead field | Bắt buộc |
|---------------|-------------------|----------|
| *(Họ tên)* | `NAME` + `LAST_NAME` | YES |
| *(SĐT)* | `PHONE` | YES |
| *(Email)* | `EMAIL` | Nên có |
| *(Nhu cầu)* | `COMMENTS` | Nên có |
| *(Nguồn)* | `SOURCE_ID` | Auto = `WEB` |

### Configuration

| Cấu hình | Giá trị |
|-----------|---------|
| Wix Webhook URL | *(điền)* |
| n8n Workflow ID | *(điền)* |
| n8n Workflow name | *(điền)* |

---

## Template thêm Source mới

> Khi có data source mới cần đẩy Lead vào Bitrix, copy template dưới đây và điền thông tin.

### [Tên Source]

**Flow:**
```
[Mô tả cách data vào]
       │
       ▼ (Trigger type → n8n)
┌──────────────────────────────┐
│  n8n Workflow: [tên]         │
│                              │
│  1. Trigger: [loại trigger]  │
│  2. Validate: SĐT required  │
│  3. Match Lead by SĐT       │
│  4. Create/Update Lead       │
│  5. Create Contact           │
│  6. Update Lead stage        │
└──────────────────────────────┘
```

**Field Mapping:**

| Source field | Bitrix Lead field | Bắt buộc |
|-------------|-------------------|----------|
| *(Họ tên)* | `NAME` + `LAST_NAME` | YES |
| *(SĐT)* | `PHONE` | YES |
| *(Email)* | `EMAIL` | Nên có |
| *(Nhu cầu)* | `COMMENTS` | Nên có |
| *(Nguồn)* | `SOURCE_ID` | Auto = `[value]` |

**Configuration:**

| Cấu hình | Giá trị |
|-----------|---------|
| Trigger URL/ID | *(điền)* |
| n8n Workflow ID | *(điền)* |
| n8n Workflow name | *(điền)* |
| Git file path | *(điền)* |
| Ngày setup | *(điền)* |
| Người setup | *(điền)* |

---

## Quy tắc chung cho mọi Lead Capture workflow

1. **SĐT là key match** — mọi workflow phải match Lead bằng SĐT trước khi tạo mới.
2. **Format SĐT:** Chuẩn hóa về dạng `0xxxxxxxxx` (10 số, bỏ +84, bỏ dấu cách).
3. **Không tạo Lead trùng** — nếu SĐT đã có trong Bitrix → update, không add mới.
4. **SOURCE_ID** — mỗi source có `SOURCE_ID` riêng để tracking nguồn Lead.
5. **Error notification** — mọi workflow phải gửi alert (Telegram/Email) khi fail.
6. **Lead stage** — sau khi push thành công, Lead stage = `IN_PROCESS` (Submitted Form).
7. **Version control** — mọi workflow phải lưu trong Git repo qua n8n-atom. Xem [Workflow Management](n8n-workflow-management.md).

---

## Liên kết

| Đi đến | Link |
|--------|------|
| Tổng quan hệ thống | [Link](overview.md) |
| n8n Workflow Management | [Link](n8n-workflow-management.md) |
| Landing P. Kỹ thuật | [Link](../landing/p-ky-thuat.md) |
| SOP Bước 02 (Survey) | [Link](../buoc/02-survey.md) |
| Lead Fields Reference | [Link](../crm/lead-fields.md) |
| Contact Fields Reference | [Link](../crm/contact-fields.md) |
| Trang chủ | [Link](../README.md) |
