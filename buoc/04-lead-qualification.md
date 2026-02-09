# Bước 04: Lead Qualification – Đánh giá & Phân loại Lead

> **Pha:** 1 - Lead Qualification | **Phụ trách chính:** P. Chuyển đổi
> **Lead Stage:** FOLLOW-UP → GOOD LEAD / JUNK LEAD
> **CJM Phase:** QUALIFICATION
> ⏰ **SLA:** 14 ngày close Lead (từ ngày tạo Lead)

---

## 🎯 Mục tiêu

Đánh giá Lead sau meeting, phân loại thành **Good Lead** (chuyển tạo Deal) hoặc **Junk Lead** (đóng). Đảm bảo mọi Lead được close trong vòng 14 ngày.

---

## 🗺️ CJM Actions trong bước này

```
PHASE 6: QUALIFICATION (Phân loại)
├── Đánh giá sau meeting (90% xác định được ngay)
├── 10% cần follow-up thêm → Lead stage: Follow-up
├── ◇ Lead qualified?
├── YES → Good Lead → Tạo Deal (stage New Opportunity)
├──       Liên kết Deal ↔ Contact ↔ Company
└── NO  → Junk Lead → Đóng Lead (required: Lý do Junk)
```

---

## ✅ Checklist hành động

### A. Đánh giá sau meeting

- [ ] Review đánh giá qualified từ Bước 03
- [ ] Xác nhận BANT đủ điều kiện:
  - **Budget:** KH có ngân sách hoặc có thể approve ngân sách
  - **Authority:** Đã xác định người ra quyết định
  - **Need:** Nhu cầu phù hợp giải pháp SYNITY
  - **Timeline:** KH có timeline triển khai rõ ràng
- [ ] **~90% trường hợp:** Xác định được ngay → chuyển Good Lead hoặc Junk Lead
- [ ] **~10% trường hợp:** Cần follow-up thêm → chuyển stage Follow-up

### B. Follow-up (cho 10% cần thêm thông tin)

- [ ] Chuyển Lead stage → **Follow-up**
- [ ] Ghi chú rõ: cần follow-up thêm thông tin gì
- [ ] Lên lịch gọi/gặp lại KH để làm rõ
- [ ] Sau follow-up → quay lại đánh giá: Good Lead hay Junk Lead
- [ ] **Lưu ý SLA 14 ngày:** Không để Lead ở Follow-up quá lâu

### C. Good Lead → Tạo Deal

- [ ] Chuyển Lead stage → **Good Lead**
- [ ] Tạo **Deal** mới trong Bitrix24:
  - Stage: **New Opportunity**
  - Title: `[Tên công ty] - [Nhu cầu chính] - [Tháng/Năm]`
  - Ví dụ: `ABC Corp - CRM Sales - 02/2026`
- [ ] Liên kết Deal ↔ Contact ↔ Company
- [ ] Chuyển giao cho phòng ban phụ trách Deal pipeline
- [ ] Ghi chú tóm tắt vào Deal: kết quả meeting, BANT, next action

### D. Junk Lead → Đóng Lead

- [ ] Chuyển Lead stage → **Junk Lead**
- [ ] **Bắt buộc chọn lý do Junk** (dropdown field):
  - Không đủ ngân sách
  - Không phải người quyết định
  - Nhu cầu không phù hợp
  - Không phản hồi (auto 14 ngày)
  - Khác (ghi chú)
- [ ] Ghi chú bổ sung (nếu chọn "Khác")
- [ ] **Nếu KH từ đối tác giới thiệu:** Gửi email thông báo cho đối tác (xem Lead → UF Referer)
  - Nội dung: *(Sẽ bổ sung sau)*
- [ ] Đóng Lead

---

## 💾 Thao tác CRM (Bitrix24)

### Khi Good Lead — Cập nhật Lead & Tạo Deal

> **Chi tiết trường thông tin:**
> - [Lead Fields — Bước 04](../crm/lead-fields.md#bước-04--qualification-converted--junk)
> - [Deal Fields — Bước 05](../crm/deal-fields.md#bước-05--new-opportunity-new) (tạo Deal mới)

**Lead:** `STATUS_ID` → `CONVERTED` (Good Lead)

**Tạo Deal:** `TITLE` (format: `[Công ty] - [Nhu cầu] - [MM/YYYY]`), `STAGE_ID` = `NEW`, liên kết `COMPANY_ID` + `CONTACT_ID`, copy `UF_CRM_REFERRER` nếu có.

### Khi Junk Lead — Đóng Lead

> **Chi tiết trường thông tin:** Xem [Lead Fields — Junk Lead](../crm/lead-fields.md#khi-junk-lead)

**Bắt buộc:** `STATUS_ID` → `JUNK`, `UF_CRM_JUNK_REASON` (chọn dropdown). Nếu có `UF_CRM_REFERRER` → gửi email thông báo đối tác.

**5 lý do Junk:** Không đủ ngân sách / Không phải người QĐ / Nhu cầu không phù hợp / Không phản hồi (auto 14 ngày) / Khác (ghi chú).

### Automation (SLA 14 ngày)

```
Automation Rules:
├── Ngày 10: Gửi cảnh báo cho P. Chuyển đổi
│   └── "Lead [Tên KH] sắp hết hạn 14 ngày. Vui lòng close."
├── Ngày 14: Auto chuyển Junk Lead
│   ├── Lead stage → Junk Lead
│   └── Lý do: "Không phản hồi (auto 14 ngày)"
└── Notification: Gửi cho quản lý khi auto Junk
```

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Lead stage Book Meeting | Bước 03 | ✅ |
| Đánh giá qualified sau meeting | Bước 03 | ✅ |
| Recording + Recap meeting | Bước 03 | ✅ |
| Thông tin BANT | Bước 02 + 03 | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| **Good Lead** → Deal (New Opportunity) | Bitrix CRM | P. Chuyển đổi / P. Triển khai |
| Deal liên kết Contact + Company | Bitrix CRM | Team |
| **HOẶC Junk Lead** (đóng + lý do) | Bitrix CRM | Quản lý |

---

## 📊 KPIs & SLA

| Chỉ số | Mục tiêu | Đo lường |
|--------|----------|----------|
| Thời gian close Lead | < 14 ngày | Ngày tạo Lead → Ngày close |
| Lead Qualification Rate | Theo dõi | Good Lead / Tổng Lead |
| Tỷ lệ Junk Lead | Theo dõi | Junk Lead / Tổng Lead |
| Tỷ lệ Lead auto Junk (14 ngày) | < 10% | Auto Junk / Tổng Junk |
| Tỷ lệ có lý do Junk | 100% | Junk có lý do / Tổng Junk |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>Lead sắp hết 14 ngày nhưng chưa chốt được</strong></summary>

**Cách xử lý:**
1. Ngày 10 sẽ nhận cảnh báo tự động
2. Ưu tiên liên lạc KH ngay: gọi điện + nhắn Zalo
3. Nếu KH vẫn đang trao đổi nhưng cần thêm thời gian:
   - Ghi chú rõ lý do vào Lead
   - Báo quản lý để xin gia hạn (nếu có cơ chế)
4. Nếu không liên lạc được → để auto Junk vào ngày 14
</details>

<details>
<summary><strong>KH qualified nhưng chưa sẵn sàng triển khai ngay</strong></summary>

**Cách xử lý:**
1. Vẫn chuyển **Good Lead** → tạo Deal
2. Deal stage = New Opportunity (chưa cần push)
3. Ghi chú vào Deal: "KH qualified nhưng timeline triển khai [thời gian]. Follow-up lại [ngày]"
4. Tạo Task nhắc follow-up theo timeline KH mong muốn
</details>

<details>
<summary><strong>KH ban đầu Junk nhưng sau đó liên hệ lại</strong></summary>

**Cách xử lý:**
1. Tạo Lead mới (không mở lại Lead cũ)
2. Ghi chú: "KH từng là Junk Lead [ngày]. Lý do lần trước: [lý do]"
3. Tiến hành lại quy trình từ Bước 01
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| ← Bước trước: 03. Meeting | [Link](03-meeting.md) |
| → Bước tiếp: 05. New Opportunity (Deal Pipeline) | [Link](05-new-opportunity.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| 🏠 Trang chủ | [Link](../README.md) |
