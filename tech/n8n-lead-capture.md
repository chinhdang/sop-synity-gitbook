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

> ⚡ **Lưu ý kiến trúc (02/2025):** Source này dùng **Google Apps Script (GAS) gọi trực tiếp Bitrix24 API**, không đi qua n8n. Code và hướng dẫn setup nằm trong repo riêng: [`synity-gas`](https://github.com/chinhdang/synity-gas).

### Flow chi tiết

```
KH điền Google Form
       │
       ▼
Google Sheet (Form Responses)
       │
       ▼ (GAS installable trigger: onFormSubmit)
┌──────────────────────────────┐
│  Google Apps Script           │
│  (repo: synity-gas)          │
│                              │
│  1. Trigger: onFormSubmit    │
│     (installable trigger)    │
│  2. Validate: SĐT required  │
│  3. Match Lead by SĐT       │
│     (crm.lead.list)          │
│  4. IF match → Update Lead   │
│     (crm.lead.update)        │
│     IF no match → Add Lead   │
│     (crm.lead.add)           │
│  5. Match/Add Contact        │
│     (+ Facebook, Chức vụ)    │
│  6. Match/Add Company        │
│     (+ Website)              │
│  7. VietQR API (MST) →      │
│     Requisite + Legal Addr   │
│  8. Link Contact ↔ Company   │
│     ↔ Lead                   │
│  9. Survey → HTML comment    │
│     (crm.timeline.comment)   │
│ 10. Lead stage = IN_PROCESS  │
└──────────────────────────────┘
```

### Field Mapping (36 cột Google Form)

**Nhóm A: Thông tin người liên hệ → Contact**

| Cột | Google Form field | Bitrix entity | Bitrix field | Ghi chú |
|-----|-------------------|---------------|-------------|---------|
| 1 | Họ tên | Contact + Lead | `NAME` + `LAST_NAME` | VN: từ đầu = Họ |
| 2 | Email | Contact + Lead | `EMAIL` | `VALUE_TYPE: WORK` |
| 3 | Phone | Contact + Lead | `PHONE` | **+84xxxxxxxxx** (bắt buộc). Dùng match Lead |
| 4 | Facebook cá nhân | Contact | `WEB` | `VALUE_TYPE: FACEBOOK` |
| 5 | Vai trò | Contact + Lead | `POST` | Chức vụ KH |

**Nhóm B: Thông tin doanh nghiệp → Company + Requisite**

| Cột | Google Form field | Bitrix entity | Bitrix field | Ghi chú |
|-----|-------------------|---------------|-------------|---------|
| 6 | Biết đến SYNITY từ đâu | Lead | `SOURCE_ID` + `SOURCE_DESCRIPTION` | Map → RECOMMENDATION / WEB / OTHER |
| 7 | Tên doanh nghiệp | Company + Lead | `TITLE` / `COMPANY_TITLE` | Match exact → tạo mới nếu chưa có |
| 8 | Mã số thuế | Requisite | `RQ_VAT_ID` | → VietQR API → `RQ_COMPANY_NAME` + Legal Address |
| 9 | Website công ty | Company | `WEB` | `VALUE_TYPE: WORK` |

**Nhóm C: Khảo sát (cột 10-35) → Lead timeline comment (HTML)**

| Cột | Nội dung | Section trong comment |
|-----|----------|---------------------|
| 10-12 | Nhân sự, Ngành nghề, Doanh thu | Thông tin doanh nghiệp |
| 13-17 | Khó khăn, Công cụ hiện tại | Thực trạng & Khó khăn |
| 18-22 | Mong muốn, Giải pháp, Bộ phận | Nhu cầu & Kỳ vọng |
| 23-26 | Quy trình, Sẵn sàng, Mục tiêu | Mức độ sẵn sàng |
| 27-30 | CRM, Sales team | CRM & Sales |
| 31-35 | Giao việc, QLCV | Quản lý công việc |

> **Column mapping chi tiết:** `src/config.js` (repo synity-gas). Cập nhật khi form thay đổi.
>
> **SOP gap:** `HONORIFIC` (Danh xưng) không có trong form → nhân sự bổ sung thủ công trên Contact + Lead.

### Configuration

| Cấu hình | Nơi lưu | Ghi chú |
|-----------|---------|---------|
| Google Sheet ID | GAS Script Properties: `FORM_SHEET_ID` | Sheet chứa form responses |
| Bitrix Webhook URL | GAS Script Properties: `BITRIX_WEBHOOK` | *(không lưu key trong code/SOP)* |
| Default assignee | GAS Script Properties: `DEFAULT_ASSIGNED` | User ID nhân sự phụ trách |
| Error log sheet | GAS Script Properties: `ERROR_LOG_SHEET` | (Optional) Sheet ID cho error log |
| Source code | Repo: `synity-gas` | Deploy qua `clasp push` |

### Troubleshooting

| Triệu chứng | Nguyên nhân có thể | Cách xử lý |
|-------------|-------------------|-------------|
| KH điền form nhưng Lead không cập nhật | SĐT trong form khác SĐT trong Lead | Kiểm tra format SĐT (0xxx vs +84xxx) |
| Lead tạo mới thay vì update Lead có sẵn | Match SĐT không tìm thấy | Kiểm tra GAS execution log (`clasp logs`) |
| Contact/Company không tạo | API error từ Bitrix | Kiểm tra GAS execution log + tab "Error Log" |
| Trigger không chạy | Installable trigger bị disconnect | Mở GAS editor → Triggers → kiểm tra / tạo lại |
| Lỗi 403 từ Bitrix | Webhook token hết hạn hoặc sai | Kiểm tra Script Properties → BITRIX_WEBHOOK |

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

1. **Thứ tự ưu tiên tìm trùng (duplicate detection):**
   - **Contact:** Phone → Email
   - **Company:** Phone (qua Contact linked) → Email → MST (qua Requisite `RQ_VAT_ID`) → Website (`WEB`) → Tên công ty (`TITLE`)
   - Nếu tìm thấy ở bước ưu tiên cao → dừng, không tìm tiếp.
   - Nếu Company tìm thấy qua MST → **không tạo Requisite mới** (đã tồn tại).
2. **Format SĐT:** Chuẩn hóa về dạng `+84xxxxxxxxx` (bắt buộc có +84).
3. **Không tạo Lead trùng** — nếu SĐT đã có trong Bitrix → update, không add mới.
4. **SOURCE_ID** — mỗi source có `SOURCE_ID` riêng để tracking nguồn Lead.
5. **Error notification** — mọi workflow phải gửi alert (Telegram/Email) khi fail.
6. **Lead stage** — sau khi push thành công, Lead stage = `IN_PROCESS` (Submitted Form).
7. **Version control** — mọi workflow phải lưu trong Git repo qua n8n-atom. Xem [Workflow Management](n8n-workflow-management.md).
8. **Timeline comment format:** BBCode (không dùng HTML). Bitrix timeline hỗ trợ `[TABLE]`, `[B]`, `[COLOR]`, `[I]`.

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
