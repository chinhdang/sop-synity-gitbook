# Bước 09: Payment – Thu tiền & Kích hoạt bản quyền

> **Pha:** 2 - Deal Closing | **Phụ trách chính:** P. Chuyển đổi + Kế toán
> **Thời gian chuẩn:** < 7 ngày
> **CJM Phase:** CONTRACT (Thu tiền bản quyền) + PAYMENT (Thu tiền triển khai)

---

## 🎯 Mục tiêu

Thu tiền bản quyền → kích hoạt license → nghiệm thu bàn giao bản quyền. Thu tiền triển khai đợt 1 (điều kiện Kick-off). Trong quá trình triển khai, thu tiền theo từng đợt gắn với biên bản nghiệm thu.

---

## 🗺️ CJM Actions trong bước này

```
LUỒNG BẢN QUYỀN:
├── Invoice + Đề nghị TT đã gửi auto (từ Bước 08.E)
├── KH thanh toán đầy đủ
├── Mua & kích hoạt license (đúng tên miền trong HĐ)
├── Screenshot chứng minh (phiên bản + thời hạn + tên miền)
├── Biên bản nghiệm thu bàn giao bản quyền (eSign → in → ký đóng dấu)
└── Đóng gói bộ hồ sơ pháp lý bản quyền

LUỒNG TRIỂN KHAI:
├── Đề nghị thanh toán triển khai đợt 1
├── KH thanh toán → Bàn giao cho P. Triển khai (Kick-off)
├── Đợt sau: Đề nghị TT kèm Biên bản nghiệm thu
├── Email tự động nhắc thanh toán
├── ◇ Chưa TT trong 4h? → Tạo task nhắc thủ công
└── Xuất HÓA ĐƠN (MISA)
```

---

## ⚖️ Nguyên tắc: eSign vs Ký tay đóng dấu

> Trong bước này, nhiều giấy tờ sử dụng **eSign (Bitrix24)** để xúc tiến nhanh.
> **eSign KHÔNG thay thế** bản ký tay đóng dấu — bản có dấu đỏ là bản pháp lý chính thức.

| Giấy tờ | eSign (nhanh) | Ký tay đóng dấu (chính thức) |
|----------|:---:|:---:|
| Đề nghị thanh toán | ✅ | — |
| Biên bản nghiệm thu bàn giao bản quyền | ✅ (trước) | ✅ (sau, kèm bộ HĐ) |

---

## ✅ Nhân sự thực hiện

### A. Luồng Bản quyền Bitrix24

> **Luồng hoàn chỉnh:** Đề nghị TT → Thanh toán → Kích hoạt license → Screenshot → Biên bản nghiệm thu.
>
> **Bắt buộc:** Phải hoàn tất luồng này TRƯỚC khi Kick-off triển khai.

#### A1. Đề nghị thanh toán bản quyền

> **Invoice + Đề nghị TT đã được tạo tự động ở Bước 08.E** (sau khi KH eSign HĐ bản quyền). Nhân sự chỉ cần kiểm tra.

- [ ] Kiểm tra Invoice bản quyền đã tạo trong CRM (từ Bước 08)
- [ ] Kiểm tra email Đề nghị TT đã gửi tự động cho KH (kèm PDF — Bitrix workflow)
- [ ] Nếu email chưa gửi → gửi lại thủ công hoặc kiểm tra workflow

#### A2. KH thanh toán

- [ ] Theo dõi thanh toán
- [ ] Xác nhận tiền về (phối hợp với Kế toán)
- [ ] **KHÔNG mua license khi chưa nhận đủ tiền**

#### A3. Mua & kích hoạt bản quyền

- [ ] Mua license trên Bitrix24 Partner Portal
- [ ] Kích hoạt trên đúng **tên miền ghi trong HĐ bản quyền** (`UF_CRM_B24_PORTAL`)
- [ ] Kiểm tra: đúng phiên bản, đúng thời hạn, đúng tên miền

#### A4. Screenshot chứng minh

- [ ] Chụp screenshot từ Bitrix24 Admin Panel hiển thị **cùng 1 màn hình:**
  - ✅ Phiên bản Bitrix24 (tên gói)
  - ✅ Thời hạn license (ngày bắt đầu → ngày hết hạn)
  - ✅ Tên miền (portal URL)
- [ ] Gửi screenshot cho KH qua email/Zalo
- [ ] Lưu screenshot vào CRM (attach vào Deal)

#### A5. Biên bản nghiệm thu bàn giao bản quyền

- [ ] Soạn **Biên bản nghiệm thu bàn giao bản quyền** (từ template)
- [ ] Gửi cho KH qua **eSign** (Bitrix24) để xác nhận nhanh
- [ ] **In biên bản** → ký tay → đóng dấu (cả 2 bên)
  - eSign chỉ để xúc tiến nhanh
  - Bản ký tay đóng dấu = **bản pháp lý chính thức**
- [ ] **Đóng gói bộ hồ sơ pháp lý bản quyền** hoàn chỉnh:

| # | Tài liệu | Nguồn |
|---|----------|-------|
| 1 | Báo giá bản quyền | Bước 07 |
| 2 | Hợp đồng bản quyền (ký tay đóng dấu) | Bước 08 |
| 3 | Đề nghị thanh toán | A1 |
| 4 | Biên bản nghiệm thu bàn giao bản quyền (ký tay đóng dấu) | A5 |

- [ ] Gửi bộ hồ sơ cho KH (bản cứng hoặc scan)
- [ ] Lưu bộ hồ sơ vào CRM + Drive

### B. Thu tiền triển khai đợt 1 (trước Kick-off)

- [ ] Tạo **Đề nghị thanh toán** triển khai đợt 1
- [ ] Gửi cho KH (email/eSign)
- [ ] Theo dõi thanh toán
- [ ] Xác nhận tiền về (phối hợp với Kế toán)
- [ ] Cập nhật Deal stage = WON
- [ ] **Nếu KH từ đối tác giới thiệu:** Gửi email thông báo cho đối tác (xem Deal → Lead gốc → UF Referer)
  - Nội dung: *(Sẽ bổ sung sau)*
- [ ] **Bàn giao cho P. Triển khai** để tiến hành Kick-off

### C. Thu tiền các đợt tiếp theo (trong quá trình triển khai)

- [ ] Gửi **ĐỀ NGHỊ THANH TOÁN** kèm Biên bản nghiệm thu đã ký
- [ ] Thanh toán theo **MODULE/Đợt** (gắn với nghiệm thu cụ thể)
- [ ] Tạo Invoice trong CRM cho từng đợt
- [ ] Theo dõi thanh toán + xác nhận tiền về

### D. Nhắc thanh toán

- [ ] Hệ thống tự động gửi email nhắc thanh toán khi đến hạn (xem [AR-1](#ar-1-invoice-đến-hạn--auto-email-nhắc-thanh-toán))
- [ ] **Nếu chưa thanh toán trong 4 giờ** → Hệ thống tạo task nhắc (xem [AR-2](#ar-2-chưa-thanh-toán-sau-4h--tạo-task-nhắc-thủ-công))
- [ ] P. Chuyển đổi gọi điện/nhắn Zalo nhắc KH
- [ ] Ghi chú CRM: ngày nhắc, kết quả

### E. Xuất hóa đơn

- [ ] Kế toán xác nhận tiền về
- [ ] Xuất hóa đơn VAT qua MISA (trong vòng 24h)
- [ ] Gửi hóa đơn điện tử cho KH qua email
- [ ] Lưu hóa đơn vào CRM

---

## 💾 Thao tác CRM (Bitrix24)

| Thao tác | Chi tiết |
|----------|----------|
| Tạo Invoice | Bản quyền (A1) + Triển khai đợt 1 (B) + các đợt sau (C) |
| Upload screenshot | Screenshot license activation → attach vào Deal |
| Upload bộ hồ sơ | Biên bản nghiệm thu + bộ HĐ bản quyền → attach vào Deal |
| Cập nhật Deal stage | → PAYMENT → WON |
| Ghi chú Deal | Lịch sử thanh toán từng đợt, ngày kích hoạt license |
| Email đối tác | Thông báo KH đã thanh toán lần đầu cho Referer (nếu có) |

### Deal fields — Cập nhật tại Bước 09

> **Chi tiết trường thông tin:** Xem [Deal Fields — Bước 09](../crm/deal-fields.md#bước-09--payment-final_invoice--won)
>
> **Mẫu ghi chú COMMENTS:** Xem [Deal Fields — Mẫu ghi chú Bước 09](../crm/deal-fields.md#mẫu-ghi-chú-comments-bước-09)

**Payment:** `STAGE_ID` → `FINAL_INVOICE`, cập nhật `COMMENTS` (lịch sử thanh toán từng đợt).

**Deal Won:** `STAGE_ID` → `WON` khi bản quyền đã thanh toán + triển khai đợt 1 đã thanh toán. Kiểm tra `UF_CRM_REFERRER` để thông báo đối tác.

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
Gửi Đề nghị TT cho KH ─────►
                                      Đến hạn ──► AR-1: Auto email nhắc
                                      Chưa TT sau 4h ──► AR-2: Tạo Task
Nhận Task, gọi/nhắn KH ◄─────────────┘
Xác nhận tiền về ─────────────►
Kích hoạt license (bản quyền) ►
Screenshot + gửi KH ──────────►
Biên bản nghiệm thu (eSign) ──►
In + ký tay đóng dấu ─────────►
Đóng gói bộ hồ sơ ────────────►
Kế toán xuất HĐ VAT (MISA) ──►
Gửi HĐ điện tử cho KH ──────►
```

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| HĐ bản quyền đã ký (eSign) | Bước 08 | ✅ |
| HĐ triển khai đã ký (eSign) | Bước 08 | Nếu có |
| Invoice bản quyền + email Đề nghị TT (auto) | Bước 08.E | ✅ |
| Template Biên bản nghiệm thu bàn giao bản quyền | Drive | ✅ |
| Biên bản nghiệm thu triển khai (cho đợt sau) | Bước 11 | ✅ |
| Thông tin thanh toán công ty SYNITY | Kế toán | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Bộ hồ sơ pháp lý bản quyền (4 tài liệu) | CRM + Drive | KH + Kế toán |
| Screenshot license activation | CRM | KH |
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
| Thời gian thu tiền bản quyền | < 7 ngày sau eSign HĐ | Ngày eSign → Ngày nhận tiền |
| Kích hoạt license | < 24h sau nhận tiền | Ngày nhận tiền → Ngày kích hoạt |
| Biên bản nghiệm thu bản quyền | < 3 ngày sau kích hoạt | Ngày kích hoạt → Ngày ký biên bản |
| Xuất hóa đơn VAT | < 24h sau khi nhận tiền | Kế toán xác nhận |
| Thu tiền triển khai đợt 1 | < 7 ngày sau ký HĐ | Ngày ký → Ngày nhận tiền |
| Thu tiền đợt sau | < 7 ngày sau nghiệm thu | Ngày nghiệm thu → Ngày nhận tiền |
| Nhắc thanh toán thủ công | Trong 4h nếu auto fail | Task completion time |

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

<details>
<summary><strong>KH chỉ mua bản quyền, không mua triển khai</strong></summary>

**Cách xử lý:**
1. Chỉ thực hiện **Luồng A** (Bản quyền) — bỏ qua phần B, C
2. Hoàn tất bộ hồ sơ pháp lý bản quyền đầy đủ
3. Deal stage → WON khi bản quyền đã thanh toán + kích hoạt
4. Không cần bàn giao cho P. Triển khai
</details>

<details>
<summary><strong>Screenshot license không hiển thị đủ thông tin</strong></summary>

**Cách xử lý:**
1. Phải chụp **cùng 1 màn hình** hiển thị: phiên bản + thời hạn + tên miền
2. Vào Bitrix24 Admin Panel → License section
3. Nếu không hiển thị hết → zoom out hoặc cuộn để capture đủ
4. Không chấp nhận 2-3 screenshot riêng lẻ — phải cùng 1 ảnh
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
