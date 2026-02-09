# Hệ thống & Tích hợp — Tổng quan

> Trang này mô tả **kiến trúc tổng quan** của hệ thống SYNITY: các công cụ, luồng dữ liệu, và cách chúng kết nối với nhau.
> **Đối tượng:** P. Kỹ thuật (setup & maintain), Quản lý (nắm bức tranh tổng).

---

## Bản đồ hệ thống

```
┌─────────────────────────────────────────────────────────┐
│                    DATA SOURCES                         │
│  Google Form  │  UChat  │  Wix  │  (nguồn mới...)      │
└──────┬────────┴────┬────┴───┬───┴───────────────────────┘
       │             │        │
       ▼             ▼        ▼
┌─────────────────────────────────────────────────────────┐
│                    n8n (Integration Hub)                 │
│                                                         │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐  │
│  │ Lead Capture  │  │ Notification │  │  (workflow    │  │
│  │ Workflows     │  │ Workflows    │  │   mới...)    │  │
│  └──────┬───────┘  └──────┬───────┘  └──────────────┘  │
└─────────┼─────────────────┼─────────────────────────────┘
          │                 │
          ▼                 ▼
┌─────────────────────────────────────────────────────────┐
│              Bitrix24 (CRM + Automation)                 │
│                                                         │
│  Lead ──► Contact + Company ──► Deal ──► Estimate       │
│                                                         │
│  Automation Rules (nội bộ Bitrix):                      │
│  - Email/ZNS xác nhận (Bước 02)                        │
│  - SLA 14 ngày auto Junk (Bước 04)                     │
│  - Task Flow khi Estimate → Sent (Bước 07)             │
│  - Copy Products khi Estimate → Approved (Bước 07)     │
│  - Email nhắc thanh toán + auto Task (Bước 09)         │
└─────────────────────────────────────────────────────────┘
```

---

## Công cụ & Vai trò

| Công cụ | Vai trò | Ai quản lý |
|---------|---------|------------|
| **Bitrix24** | CRM chính — Lead, Deal, Contact, Company, Estimate, Invoice | P. Chuyển đổi / Triển khai (dùng), P. Kỹ thuật (config) |
| **n8n** | Integration hub — kết nối data sources với Bitrix | P. Kỹ thuật |
| **Google Form** | Thu thập data KH (khảo sát nhu cầu) | P. Chuyển đổi (gửi), P. Kỹ thuật (setup flow) |
| **Google Sheet** | Lưu trữ form responses, buffer trước khi đẩy vào Bitrix | P. Kỹ thuật |
| **UChat** | Chatbot thu thập lead tự động | P. Kỹ thuật |
| **Wix** | Website — form liên hệ, landing page | P. Kỹ thuật |
| **MISA** | Kế toán — xuất hóa đơn VAT | P. Kế toán |
| **Zalo (ZNS)** | Gửi thông báo cho KH qua Zalo | P. Kỹ thuật (setup), Bitrix (trigger) |

---

## Danh sách Integration Flows

| # | Flow | Mô tả | Trang chi tiết |
|---|------|-------|----------------|
| 1 | **Lead Capture** | Nhiều nguồn → n8n → Bitrix Lead | [n8n Lead Capture](n8n-lead-capture.md) |
| 2 | *(Thêm khi có flow mới)* | | |

---

## Phân biệt: n8n Integration vs Bitrix Automation

| | n8n Integration | Bitrix Automation Rules |
|---|----------------|------------------------|
| **Chạy ở đâu** | Server n8n (ngoài Bitrix) | Trong Bitrix24 |
| **Trigger** | Webhook, schedule, event ngoài | Stage change, field change trong Bitrix |
| **Ví dụ** | GG Form → n8n → tạo Lead | Estimate → Sent → tạo Task Flow |
| **Ai maintain** | P. Kỹ thuật | P. Kỹ thuật (config) |
| **Tài liệu ở đâu** | `tech/` (trang này) | `⚡ Automation Rules` trong mỗi bước SOP |
| **Khi lỗi** | Kiểm tra n8n dashboard | Kiểm tra Bitrix Automation Rules |

---

## Quy tắc chung cho P. Kỹ thuật

1. **Mọi integration đi qua n8n** — không kết nối trực tiếp source → Bitrix.
2. **1 source = 1 n8n workflow** — dễ debug, dễ tắt/bật từng nguồn.
3. **Logging:** Mọi workflow n8n phải có error handling + notification khi fail.
4. **Field mapping:** Tuân theo chuẩn field names trong [CRM Fields Reference](../crm/lead-fields.md).
5. **Thêm source mới:** Theo template trong [n8n Lead Capture](n8n-lead-capture.md#template-thêm-source-mới).

---

## Liên kết

| Đi đến | Link |
|--------|------|
| Landing P. Kỹ thuật | [Link](../landing/p-ky-thuat.md) |
| n8n Lead Capture | [Link](n8n-lead-capture.md) |
| CRM Fields Reference | [Lead](../crm/lead-fields.md) · [Contact](../crm/contact-fields.md) · [Company](../crm/company-fields.md) |
| Trang chủ | [Link](../README.md) |
