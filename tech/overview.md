# Hệ thống & Tích hợp — Tổng quan

> Trang này mô tả **kiến trúc tổng quan** của hệ thống SYNITY: các công cụ, luồng dữ liệu, và cách chúng kết nối với nhau.
> **Đối tượng:** P. Kỹ thuật (setup & maintain), Quản lý (nắm bức tranh tổng).

---

## Định hướng kiến trúc

SYNITY đang chuyển dịch kiến trúc tích hợp theo hướng **middleware app**:

| | Trước (v1) | Hiện tại & Tương lai (v2) |
|---|------------|---------------------------|
| **Đồng bộ dữ liệu** | n8n (Integration Hub) | Middleware app (GAS, Cloudflare Worker, Vercel...) |
| **AI Agent & MCP** | — | n8n |
| **Version control** | n8n-atom + Git (khó quản lý) | Source code trong Git repo (dễ review, dễ rollback) |
| **Ví dụ** | Google Form → n8n → Bitrix | Google Form → GAS → Bitrix |

**Lý do chuyển dịch:**
- Code middleware app quản lý trong Git dễ hơn n8n workflow JSON
- PR review, rollback, CI/CD theo quy trình dev chuẩn
- n8n tập trung vào thế mạnh: AI Agent, MCP server, orchestration phức tạp

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
│            MIDDLEWARE APPS (Đồng bộ dữ liệu)           │
│                                                         │
│  ┌────────────┐  ┌────────────┐  ┌──────────────────┐  │
│  │ Google Apps │  │ Cloudflare │  │ Vercel / Custom  │  │
│  │ Script     │  │ Worker     │  │ App              │  │
│  │ (GG Form)  │  │ (UChat,    │  │ (nguồn mới...)   │  │
│  │            │  │  Wix...)   │  │                  │  │
│  └──────┬─────┘  └──────┬─────┘  └────────┬─────────┘  │
│         │               │                 │             │
│  Repo: synity-gas       │    Source code trong Git      │
└─────────┼───────────────┼─────────────────┼─────────────┘
          │               │                 │
          ▼               ▼                 ▼
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

┌─────────────────────────────────────────────────────────┐
│              n8n (AI Agent & MCP)                        │
│                                                         │
│  ┌──────────────────┐  ┌────────────────────────────┐   │
│  │ AI Agent         │  │ MCP Server                 │   │
│  │ Workflows        │  │ Connections                │   │
│  └──────────────────┘  └────────────────────────────┘   │
│                                                         │
│  Vai trò: Orchestration phức tạp, AI Agent, MCP         │
│  KHÔNG dùng cho: Đồng bộ dữ liệu đơn giản              │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│              Git Repo (Version Control)                  │
│                                                         │
│  synity-gas (GAS source code)                           │
│  sop-synity-gitbook (SOP documentation)                 │
│  (middleware repos khi thêm app mới)                    │
│       │                                                 │
│       ├── commit history (ai sửa, sửa gì, khi nào)     │
│       ├── PR review trước khi deploy                    │
│       └── rollback khi lỗi                              │
└─────────────────────────────────────────────────────────┘
```

---

## Công cụ & Vai trò

| Công cụ | Vai trò | Ai quản lý |
|---------|---------|------------|
| **Bitrix24** | CRM chính — Lead, Deal, Contact, Company, Estimate, Invoice | P. Chuyển đổi / Triển khai (dùng), P. Kỹ thuật (config) |
| **Google Apps Script** | Middleware app — Google Form → Bitrix Lead (repo: `synity-gas`) | P. Kỹ thuật |
| **Google Form** | Thu thập data KH (khảo sát nhu cầu) | P. Chuyển đổi (gửi), P. Kỹ thuật (setup flow) |
| **Google Sheet** | Lưu trữ form responses, buffer trước khi đẩy vào Bitrix | P. Kỹ thuật |
| **n8n** | AI Agent & MCP — orchestration phức tạp (KHÔNG dùng cho data sync) | P. Kỹ thuật |
| **Cloudflare Worker / Vercel** | Middleware app cho các nguồn mới (UChat, Wix...) — dự kiến | P. Kỹ thuật |
| **UChat** | Chatbot thu thập lead tự động | P. Kỹ thuật |
| **Wix** | Website — form liên hệ, landing page | P. Kỹ thuật |
| **MISA** | Kế toán — xuất hóa đơn VAT | P. Kế toán |
| **Zalo (ZNS)** | Gửi thông báo cho KH qua Zalo | P. Kỹ thuật (setup), Bitrix (trigger) |
| **Git (GitHub)** | Version control cho middleware app source code — history, PR review, rollback | P. Kỹ thuật |

---

## Danh sách Integration Flows

| # | Flow | Middleware | Mô tả | Trang chi tiết |
|---|------|-----------|-------|----------------|
| 1 | **Google Form → Bitrix** | Google Apps Script | Khảo sát nhu cầu → Lead + Contact + Company | [GAS Đồng bộ khảo sát](gas-dong-bo-khao-sat.md) |
| 2 | **Lead Capture (multi-source)** | n8n (legacy) / middleware | UChat, Wix → Bitrix Lead | [n8n Lead Capture](n8n-lead-capture.md) |
| 3 | *(Thêm khi có flow mới)* | | | |

---

## Phân biệt: Middleware App vs n8n vs Bitrix Automation

| | Middleware App | n8n | Bitrix Automation Rules |
|---|---------------|-----|------------------------|
| **Dùng cho** | Đồng bộ dữ liệu (data sync) | AI Agent, MCP, orchestration phức tạp | Quy trình nội bộ CRM |
| **Chạy ở đâu** | GAS / Cloudflare / Vercel | Server n8n | Trong Bitrix24 |
| **Trigger** | Form submit, webhook, schedule | Webhook, schedule, AI event | Stage change, field change |
| **Ví dụ** | GG Form → GAS → tạo Lead | AI Agent phân tích lead | Estimate → Sent → tạo Task |
| **Version control** | Git repo (source code) | n8n-atom + Git (JSON) | Không (config trong UI) |
| **Ai maintain** | P. Kỹ thuật | P. Kỹ thuật | P. Kỹ thuật (config) |
| **Tài liệu ở đâu** | `tech/gas-*`, `tech/worker-*` | `tech/n8n-*` | `⚡ Automation Rules` trong mỗi bước SOP |

---

## Quy tắc chung cho P. Kỹ thuật

1. **Đồng bộ dữ liệu → dùng middleware app** — GAS, Cloudflare Worker, Vercel, hoặc custom app. Source code lưu trong Git repo.
2. **n8n chỉ dùng cho AI Agent & MCP** — orchestration phức tạp, không dùng cho data sync đơn giản.
3. **1 source = 1 middleware app / 1 repo** — dễ debug, dễ deploy, dễ rollback.
4. **Logging:** Mọi middleware app phải có error handling + log lỗi (vd: Google Sheet "Error Log" tab).
5. **Field mapping:** Tuân theo chuẩn field names trong [CRM Fields Reference](../crm/lead-fields.md).
6. **Version control:** Mọi source code phải trong Git repo — commit history, PR review, rollback.
7. **Secrets:** Webhook token, API key lưu trong environment (Script Properties, Worker secrets) — KHÔNG hardcode trong source code.

---

## Liên kết

| Đi đến | Link |
|--------|------|
| Landing P. Kỹ thuật | [Link](../landing/p-ky-thuat.md) |
| GAS Đồng bộ khảo sát | [Link](gas-dong-bo-khao-sat.md) |
| n8n Lead Capture | [Link](n8n-lead-capture.md) |
| n8n Workflow Management | [Link](n8n-workflow-management.md) |
| CRM Fields Reference | [Lead](../crm/lead-fields.md) · [Contact](../crm/contact-fields.md) · [Company](../crm/company-fields.md) |
| Trang chủ | [Link](../README.md) |
