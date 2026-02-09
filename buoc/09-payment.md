# Bước 09: Payment – Thu tiền

> **Pha:** 2 - Deal Closing | **Phụ trách chính:** P. Chuyển đổi + Kế toán
> **Thời gian chuẩn:** < 7 ngày
> **CJM Phase:** CONTRACT (Thu tiền đợt đầu) + PAYMENT (Thu tiền theo đợt)

---

## 🎯 Mục tiêu

Thu tiền bản quyền Bitrix24 và đợt 1 triển khai (điều kiện Kick-off). Trong quá trình triển khai, thu tiền theo từng đợt gắn với biên bản nghiệm thu.

---

## 🗺️ CJM Actions trong bước này

```
Từ PHASE 7 (CONTRACT):
├── Thu thanh toán TRIỂN KHAI ĐỢT 1

Từ PHASE 11 (PAYMENT):
├── Gửi ĐỀ NGHỊ THANH TOÁN kèm Biên bản nghiệm thu
├── Thanh toán theo MODULE/Đợt (gắn với nghiệm thu)
├── Email tự động nhắc thanh toán
├── ◇ Chưa TT trong 4h? → Tạo task nhắc thủ công
└── Xuất HÓA ĐƠN (trong luồng Payment)
```

---

## ✅ Nhân sự thực hiện

### A. Thu tiền đợt đầu (trước Kick-off)

- [ ] Tạo Invoice bản quyền Bitrix24 trong CRM
- [ ] Tạo Invoice đợt 1 triển khai trong CRM
- [ ] Gửi thông tin thanh toán cho KH (Email + Zalo)
- [ ] Theo dõi thanh toán
- [ ] Xác nhận tiền về (phối hợp với Kế toán)
- [ ] Kế toán xuất hóa đơn VAT (trong vòng 24h)
- [ ] Cập nhật Deal stage = WON
- [ ] **Nếu KH từ đối tác giới thiệu:** Gửi email thông báo cho đối tác (xem Deal → Lead gốc → UF Referer)
  - Nội dung: *(Sẽ bổ sung sau)*
- [ ] **Bàn giao cho P. Triển khai** để tiến hành Kick-off

### B. Thu tiền các đợt tiếp theo (trong quá trình triển khai)

- [ ] Gửi **ĐỀ NGHỊ THANH TOÁN** kèm Biên bản nghiệm thu đã ký
- [ ] Thanh toán theo **MODULE/Đợt** (gắn với nghiệm thu cụ thể)
- [ ] Tạo Invoice trong CRM cho từng đợt
- [ ] Gửi thông tin thanh toán

### C. Nhắc thanh toán (tự động + thủ công)

- [ ] Hệ thống tự động gửi email nhắc thanh toán khi đến hạn
- [ ] **Nếu chưa thanh toán trong 4 giờ** → Tạo task nhắc thủ công
- [ ] P. Chuyển đổi gọi điện/nhắn Zalo nhắc KH
- [ ] Ghi chú CRM: ngày nhắc, kết quả

### D. Xuất hóa đơn

- [ ] Kế toán xác nhận tiền về
- [ ] Xuất hóa đơn VAT qua MISA (trong vòng 24h)
- [ ] Gửi hóa đơn điện tử cho KH qua email
- [ ] Lưu hóa đơn vào CRM

---

## 💾 Thao tác CRM (Bitrix24)

| Thao tác | Chi tiết |
|----------|----------|
| Tạo Invoice | Cho từng đợt thanh toán |
| Cập nhật Deal stage | → PAYMENT → WON |
| Ghi chú Deal | Lịch sử thanh toán từng đợt |
| Email đối tác | Thông báo KH đã thanh toán lần đầu cho Referer (nếu có) |

### Deal fields — Cập nhật tại Bước 09

> **Chi tiết trường thông tin:** Xem [Deal Fields — Bước 09](../crm/deal-fields.md#bước-09--payment-final_invoice--won)
>
> **Mẫu ghi chú COMMENTS:** Xem [Deal Fields — Mẫu ghi chú Bước 09](../crm/deal-fields.md#mẫu-ghi-chú-comments-bước-09)

**Payment:** `STAGE_ID` → `FINAL_INVOICE`, cập nhật `COMMENTS` (lịch sử thanh toán từng đợt).

**Deal Won:** `STAGE_ID` → `WON` khi đợt 1 đã thanh toán. Kiểm tra `UF_CRM_REFERRER` để thông báo đối tác.

> **Contact Lifecycle — Deal Won:** Khi Deal → WON, cập nhật Contact `UF_CRM_CONTACT_LIFECYCLE_STAGE` = `52` (**Customer**). Xem [Contact Lifecycle Flow](../crm/contact-fields.md#lifecycle-flow).

**Partial Payment:** Dùng `PARTIAL_PAYMENT` nếu chưa thanh toán hết.

---

## ⚡ Automation Rules (Bitrix24 tự động)

> Các Automation Rule dưới đây **chạy tự động** khi điều kiện trigger được kích hoạt. Nhân sự **không cần thao tác** — chỉ cần biết để theo dõi.

### AR-1: Invoice đến hạn → Auto email nhắc thanh toán

| Thuộc tính | Giá trị |
|-----------|---------|
| **Entity** | Invoice / Deal |
| **Trigger** | Invoice đến hạn thanh toán |
| **Action** | Gửi email nhắc thanh toán tự động cho KH |
| Template | Xem [Template email nhắc thanh toán](#-template-email-nhắc-thanh-toán) |

### AR-2: Chưa thanh toán sau 4h → Tạo Task nhắc thủ công

| Thuộc tính | Giá trị |
|-----------|---------|
| **Entity** | Invoice / Deal |
| **Trigger** | KH chưa thanh toán sau 4 giờ kể từ email nhắc (AR-1) |
| **Action** | Tạo Task cho P. Chuyển đổi |
| Task name | "Nhắc thanh toán [Tên KH] - Invoice #[số]" |
| Responsible | P. Chuyển đổi |
| Nội dung | Gọi điện / nhắn Zalo nhắc KH thanh toán |

### Luồng thanh toán tổng hợp

```
NHÂN SỰ                              AUTOMATION RULE
────────                              ───────────────
Tạo Invoice ──────────────────►
Gửi thông tin TT cho KH ─────►
                                      Đến hạn ──► AR-1: Auto email nhắc
                                      Chưa TT sau 4h ──► AR-2: Tạo Task
Nhận Task, gọi/nhắn KH ◄─────────────┘
Xác nhận tiền về ─────────────►
Kế toán xuất HĐ VAT (MISA) ──►
Gửi HĐ điện tử cho KH ──────►
```

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Hợp đồng đã ký | Bước 08 | ✅ |
| Invoice bản quyền | Bước 08 | ✅ |
| Biên bản nghiệm thu (cho đợt sau) | Bước 11 | ✅ |
| Thông tin thanh toán công ty SYNITY | Kế toán | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Invoice đã thanh toán | CRM | Kế toán |
| Hóa đơn VAT | MISA + Email | KH |
| Deal stage = WON | CRM | Team |
| Thông tin bàn giao cho Triển khai | CRM + Zalo | P. Triển khai |

---

## 📋 Template email nhắc thanh toán

```
Subject: [SYNITY] Nhắc nhở thanh toán - Invoice #[số]

Kính gửi Anh/Chị [Tên],

Bên em gửi nhắc nhở về invoice thanh toán:

📋 Invoice: #[số]
💰 Số tiền: [số tiền] VNĐ
📅 Hạn thanh toán: [ngày]

Thông tin chuyển khoản:
- Ngân hàng: [Tên NH]
- Số tài khoản: [STK]
- Chủ tài khoản: CÔNG TY TNHH SYNITY
- Nội dung: [Tên công ty] thanh toán [hạng mục]

Nếu Anh/Chị đã thanh toán, vui lòng bỏ qua email này.

Trân trọng,
SYNITY Team
```

---

## 📊 KPIs & SLA

| Chỉ số | Mục tiêu | Đo lường |
|--------|----------|----------|
| Thời gian thu tiền đợt 1 | < 7 ngày sau ký HĐ | Ngày ký → Ngày nhận tiền |
| Xuất hóa đơn VAT | < 24h sau khi nhận tiền | Kế toán xác nhận |
| Nhắc thanh toán thủ công | Trong 4h nếu auto fail | Task completion time |
| Thu tiền đợt sau | < 7 ngày sau nghiệm thu | Ngày nghiệm thu → Ngày nhận tiền |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>KH xin thanh toán chậm</strong></summary>

**Cách xử lý:**
1. Lắng nghe lý do
2. Nếu hợp lý → cho phép gia hạn tối đa 7 ngày (cần Chinh approve)
3. Ghi chú CRM: "KH xin gia hạn TT. Lý do: [...]. Hạn mới: [ngày]"
4. Nhắc: "Dạ theo HĐ, Kick-off chỉ tiến hành sau khi nhận thanh toán đợt 1 ạ"
</details>

<details>
<summary><strong>KH thanh toán nhưng sai nội dung chuyển khoản</strong></summary>

**Cách xử lý:**
1. Kế toán xác nhận dựa trên số tiền + thông tin người chuyển
2. Nhắn KH gửi lại ảnh chụp giao dịch để đối chiếu
3. Vẫn tiến hành xuất hóa đơn bình thường
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| ← Bước trước: 08. Contract & Closing | [Link](08-contract-closing.md) |
| → Bước tiếp: 10. Kick-off | [Link](10-kickoff.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| ↑ Về Landing P. Kế toán | [Link](../landing/p-ke-toan.md) |
| 🏠 Trang chủ | [Link](../README.md) |
