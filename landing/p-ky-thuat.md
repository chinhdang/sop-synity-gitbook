# P. Kỹ thuật – Trang làm việc

> **Vai trò:** Setup & maintain hệ thống tích hợp, đảm bảo dữ liệu chảy đúng giữa các công cụ.
> **Công cụ chính:** n8n, n8n-atom (VS Code), Git, Bitrix24 (config), Google Workspace, UChat, Wix.

---

## Phạm vi trách nhiệm

```
P. Kỹ thuật quản lý:

┌─────────────────────────────────────────────────┐
│  EXTERNAL → n8n → BITRIX                        │
│  (Lead Capture, Notifications, Integrations)    │
├─────────────────────────────────────────────────┤
│  BITRIX CONFIG                                  │
│  (Automation Rules, UF Fields, Pipeline Setup)  │
└─────────────────────────────────────────────────┘

KHÔNG quản lý:
- Quy trình bán hàng (P. Chuyển đổi)
- Triển khai cho KH (P. Triển khai)
- Kế toán (P. Kế toán)
```

---

## Tài liệu kỹ thuật

### Hệ thống & Tích hợp

| Tài liệu | Mô tả | Link |
|-----------|-------|------|
| Tổng quan hệ thống | Bản đồ kiến trúc, công cụ, luồng dữ liệu | [→ Xem](../tech/overview.md) |
| n8n Workflow Management | Version control workflows với n8n-atom + Git | [→ Xem](../tech/n8n-workflow-management.md) |
| n8n Lead Capture | Tất cả workflows đẩy Lead vào Bitrix (GG Form, UChat, Wix...) | [→ Xem](../tech/n8n-lead-capture.md) |

### CRM Configuration Reference

| Tài liệu | Mô tả | Link |
|-----------|-------|------|
| Lead Fields | Tất cả fields của Lead (system + UF) | [→ Xem](../crm/lead-fields.md) |
| Deal Fields | Tất cả fields của Deal | [→ Xem](../crm/deal-fields.md) |
| Contact Fields | Fields + Lifecycle Stage | [→ Xem](../crm/contact-fields.md) |
| Company Fields | Fields + Requisites | [→ Xem](../crm/company-fields.md) |
| Estimate Fields | Fields + Stages + Product Rows | [→ Xem](../crm/estimate-fields.md) |

### Automation Rules (trong Bitrix)

> Automation Rules được cấu hình trong Bitrix24, tài liệu nằm trong mỗi bước SOP.

| Bước | Automation | Link |
|------|-----------|------|
| 02. Survey | AR-1~3: Google Form → Lead + Email + ZNS | [→ Xem](../buoc/02-survey.md#-automation-rules-bitrix24-tự-động) |
| 04. Lead Qualification | AR-1~3: SLA 14 ngày (cảnh báo + auto Junk) | [→ Xem](../buoc/04-lead-qualification.md#-automation-rules-bitrix24-tự-động) |
| 07. Quotation | AR-1~2: Task Flow + Copy Products | [→ Xem](../buoc/07-quotation.md#-automation-rules-bitrix24-tự-động) |
| 09. Payment | AR-1~2: Email nhắc TT + auto Task | [→ Xem](../buoc/09-payment.md#-automation-rules-bitrix24-tự-động) |

---

## Checklist khi thêm Data Source mới

Khi có nguồn dữ liệu mới cần đẩy Lead vào Bitrix:

- [ ] Xác định data source gửi data dạng nào (webhook, API, sheet...)
- [ ] Tạo n8n workflow mới (copy từ template có sẵn)
- [ ] Map fields từ source → Bitrix Lead fields
- [ ] Chuẩn hóa SĐT (format `0xxxxxxxxx`)
- [ ] Test: tạo Lead test, kiểm tra Contact + Company tạo đúng
- [ ] Thêm error handling + notification khi fail
- [ ] **Commit workflow vào Git repo** (theo [quy trình n8n-atom](../tech/n8n-workflow-management.md))
- [ ] Cập nhật trang [n8n Lead Capture](../tech/n8n-lead-capture.md) — thêm section cho source mới
- [ ] Thông báo P. Chuyển đổi: có nguồn Lead mới

---

## Monitoring & Troubleshooting

### Kiểm tra hàng ngày

| Hệ thống | Kiểm tra | Cách |
|-----------|---------|------|
| n8n | Có workflow nào fail không? | n8n Dashboard → Executions |
| Git repo | Có workflow sửa trên n8n mà chưa commit không? | Pull từ n8n → so sánh với repo |
| Bitrix | Automation Rules có hoạt động không? | Bitrix → CRM → Automation Rules |
| Google Sheet | Form responses có đẩy vào Sheet không? | Kiểm tra Sheet |

### Khi nhận báo lỗi từ nhân sự

| Triệu chứng | Kiểm tra đầu tiên |
|-------------|-------------------|
| "KH điền form nhưng Lead không cập nhật" | n8n → Workflow executions → xem có lỗi không |
| "Lead tạo mới thay vì update cái cũ" | Kiểm tra SĐT match (format khác nhau?) |
| "Email/ZNS không gửi cho KH" | Bitrix → Automation Rules → xem có trigger không |
| "Task Flow không tạo khi Estimate → Sent" | Bitrix → Estimate → Automation Rules |

---

## Liên kết nhanh

| Đi đến | Link |
|--------|------|
| Trang chủ SOP | [Link](../README.md) |
| P. Chuyển đổi | [Link](p-chuyen-doi.md) |
| P. Triển khai | [Link](p-trien-khai.md) |
| P. Kế toán | [Link](p-ke-toan.md) |
