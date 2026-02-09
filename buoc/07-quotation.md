# Bước 07: Quotation – Gửi báo giá

> **Pha:** 2 - Deal Closing | **Phụ trách chính:** P. Chuyển đổi (Huy)
> **Thời gian chuẩn:** < 7 ngày (hiệu lực báo giá)
> **CJM Phase:** QUOTATION (Báo giá)

---

## 🎯 Mục tiêu

Tạo và gửi báo giá theo module với nền giá cố định + ưu đãi có deadline. Follow-up khéo léo để giữ độ nóng và tạo urgency.

---

## 🗺️ CJM Actions trong bước này

```
PHASE 6: QUOTATION (Báo giá)
├── Tạo Estimate trong Bitrix24 (từ Deal)
├── Lấy QUOTE_NUMBER + info KH → tạo file Excel báo giá
├── Estimate → Sent → Auto Task Flow: "Thêm Products vào Estimate" (deadline 4h)
├── Gửi báo giá cho KH (Email + Zalo)
├── Nếu báo giá đầu tiên → Tạo Collab "SYNITY x [Tên KH]" + upload PDF
├── Follow-up giữ độ nóng (2-3 kịch bản nhắc nhở)
├── ◇ Khách phản hồi báo giá?
│   ├── Từ chối / cần sửa → Close Estimate (Declined) → Tạo Estimate MỚI
│   └── Đồng ý → Estimate → Approved → Auto copy products vào Deal
└── Lặp lại nếu cần (B2B thường có nhiều vòng đàm phán)
```

---

## ✅ Nhân sự thực hiện

### A. Tạo Estimate trong Bitrix24

- [ ] Tạo **Estimate** từ Deal (CRM → Deal → tạo Estimate)
- [ ] Điền thêm UF fields nếu cần:
  - `UF_CRM_QUOTE_NUMBER_OF_PAYMENTS` — Số đợt thanh toán
  - `UF_CRM_QUOTE_PAYMENT_CYCLE` — Chu kỳ thanh toán
- [ ] Thêm **ưu đãi có deadline** rõ ràng (VD: giảm 10% nếu ký trước [ngày])
- [ ] Ghi rõ thời hạn hiệu lực báo giá: **7 ngày** (trường `CLOSEDATE`)
- [ ] Review Estimate với Chinh trước khi gửi

### B. Tạo file Excel báo giá (ngoài Bitrix)

- [ ] Lấy `QUOTE_NUMBER` từ Estimate vừa tạo (auto sinh, VD: `EST.94.25`)
- [ ] Kết hợp thông tin KH từ Deal + Contact + Company:
  - Tên công ty, địa chỉ, MST (Company)
  - Tên người liên hệ, chức vụ, email (Contact)
  - Products + giá (Estimate)
- [ ] Tạo file Excel báo giá theo **template chuẩn** (Drive)
- [ ] Ghi `QUOTE_NUMBER` lên file báo giá

### C. Chuyển Estimate → Sent & Hoàn thành Task Flow

- [ ] Chuyển Estimate stage → **Sent** (`SENT`)
  - *→ Automation Rule tự động tạo Task Flow (xem section Automation bên dưới)*
- [ ] RP (Chinh) nhận Task Flow, thêm **Products** vào Estimate trong **4 giờ**:
  - Từng module rõ ràng, không gộp chung
  - Áp dụng **nền giá cố định** từ Product Catalog
  - Thiết lập discount nếu có ưu đãi
  - VAT 8% (cấu hình sẵn)
  - Products trong Estimate phải **khớp với file Excel** đã tạo
- [ ] Hoàn thành Task Flow khi đã thêm xong products

### D. Gửi báo giá cho KH

- [ ] Gửi file Excel báo giá cho KH (Email + Zalo group)
- [ ] Cập nhật Deal stage → `PREPAYMENT_INVOICE` (Quotation)
- [ ] Ghi chú CRM: ngày gửi, `QUOTE_NUMBER`, ngày hết hạn ưu đãi
- [ ] **Nếu KH từ đối tác giới thiệu:** Gửi email thông báo cho đối tác (xem Deal → UF Referer)
  - Nội dung: *(Sẽ bổ sung sau)*

### E. Tạo Collab & Upload PDF

- [ ] **Nếu đây là báo giá đầu tiên:**
  1. RP (Chinh) tạo **Collab** mới: `SYNITY x [Tên công ty KH]`
  2. Export file Excel báo giá → **PDF**
  3. Upload PDF vào thư mục **Collab Files > 00. Bao gia**
  4. Đặt tên file PDF: `[QUOTE_NUMBER] - [Tên công ty].pdf`

- [ ] **Nếu đã có Collab (báo giá lần 2+):**
  1. Export file Excel báo giá mới → **PDF**
  2. Upload PDF vào thư mục **Collab Files > 00. Bao gia** (cùng Collab cũ)
  3. Đặt tên file PDF: `[QUOTE_NUMBER] - [Tên công ty].pdf`

### F. Follow-up giữ độ nóng

- [ ] **Ngày 3:** Gửi tin nhắn follow-up lần 1 (dùng template)
- [ ] **Ngày 5:** Gọi điện hỏi thăm (nếu chưa phản hồi)
- [ ] **Ngày 7 (hết hạn):** Gửi thông báo hết hạn + đề xuất voucher bảo lưu

### G. Xử lý phản hồi — Vòng đàm phán

> **B2B:** Quá trình đàm phán giá thường phức tạp, có thể tạo nhiều báo giá. Mỗi Estimate = 1 phiên bản báo giá.

- [ ] **KH đồng ý báo giá:**
  1. Nhân sự chuyển Estimate → **Approved** (`APPROVED`)
  2. *→ Automation Rule tự động copy Products từ Estimate → Deal Products*
  3. Nhân sự cập nhật `OPPORTUNITY` trên Deal khớp với Estimate
  4. Chuyển sang Bước 08 (Contract)

- [ ] **KH từ chối / yêu cầu sửa báo giá:**
  1. Nhân sự chuyển Estimate hiện tại → **Declined** (`DECLAINED`)
  2. Tạo **Estimate MỚI** từ Deal
  3. Điều chỉnh Products / giá / scope theo đàm phán
  4. Lặp lại từ bước A (tạo file Excel mới, gửi lại)

- [ ] **KH không phản hồi sau deadline:**
  1. Nhân sự chuyển Estimate → **Declined** (`DECLAINED`)
  2. Đề xuất voucher bảo lưu cho KH

- [ ] **KH từ chối hoàn toàn / Deal Lost** → xem section H

### H. Deal Lost (áp dụng ở bất kỳ bước nào trong Deal pipeline)

- [ ] Nhân sự close tất cả Estimate đang mở → **Declined**
- [ ] Chuyển Deal stage → **LOST** (`LOSE`)
- [ ] Ghi chú lý do Lost vào Deal (`UF_CRM_LOST_REASON`)
- [ ] **Nếu KH từ đối tác giới thiệu:** Gửi email thông báo cho đối tác (xem Deal → UF Referer)
  - Nội dung: *(Sẽ bổ sung sau)*
- [ ] Tổng kết bài học kinh nghiệm (nội bộ)

> **Lưu ý:** Deal Lost có thể xảy ra ở bất kỳ bước nào (05-09). Khi chuyển Deal → Lost, luôn kiểm tra UF Referer để thông báo đối tác.

---

## ⚡ Automation Rules (Bitrix24 tự động)

> Các Automation Rule dưới đây **chạy tự động** khi điều kiện trigger được kích hoạt. Nhân sự **không cần thao tác** — chỉ cần biết để theo dõi.

### AR-1: Estimate → Sent → Tạo Task Flow

| Thuộc tính | Giá trị |
|-----------|---------|
| **Entity** | Estimate |
| **Trigger** | Estimate chuyển stage `SENT` |
| **Action** | Tạo Task Flow |
| Task name | "Thêm Products vào Estimate `[QUOTE_NUMBER]`" |
| Responsible | RP (hiện tại: Chinh) |
| Deadline | **4 giờ** từ lúc trigger |
| Description | "Thêm products vào Estimate khớp với file Excel báo giá đã tạo" |

**Mục đích:** Đảm bảo Products trong Estimate khớp với file Excel. Estimate PHẢI có products trước khi → Approved (để AR-2 copy chính xác).

### AR-2: Estimate → Approved → Copy Products vào Deal

| Thuộc tính | Giá trị |
|-----------|---------|
| **Entity** | Estimate |
| **Trigger** | Estimate chuyển stage `APPROVED` |
| **Action** | Copy Product Rows từ Estimate → Deal Products |
| Deal | Deal liên kết (`DEAL_ID`) |

**Mục đích:** Khi KH đồng ý báo giá, products tự động đổ vào Deal — không cần nhân sự nhập lại.

### Estimate Flow tổng hợp

```
NHÂN SỰ                              AUTOMATION RULE
────────                              ───────────────
Tạo Estimate ──────────────────►
Tạo file Excel ────────────────►
Chuyển Estimate → SENT ────────► AR-1: Tạo Task Flow (deadline 4h)
                                       │
Nhận Task, thêm Products ◄────────────┘
Hoàn thành Task Flow ──────────►
Gửi Excel cho KH ─────────────►
Tạo Collab + Upload PDF ──────►
Follow-up ─────────────────────►

  ┌─ KH đồng ý:
  │  Chuyển Estimate → APPROVED ──► AR-2: Copy Products → Deal
  │
  ├─ KH từ chối:
  │  Chuyển Estimate → DECLINED ──►
  │  Tạo Estimate MỚI ────────────► (lặp lại)
  │
  └─ Deal Lost:
     Close Estimates → DECLINED ──►
     Chuyển Deal → LOSE ──────────►
```

---

## 💾 Thao tác CRM (Bitrix24)

### Deal fields — Cập nhật tại Bước 07

> **Chi tiết trường thông tin:**
> - [Deal Fields — Bước 07](../crm/deal-fields.md#bước-07--quotation-prepayment_invoice)
> - [Estimate Fields](../crm/estimate-fields.md) — Trường thông tin Estimate
>
> **Mẫu ghi chú COMMENTS:** Xem [Deal Fields — Mẫu ghi chú Bước 07](../crm/deal-fields.md#mẫu-ghi-chú-comments-bước-07)
>
> **Deal Lost:** Xem [Deal Fields — Deal Lost](../crm/deal-fields.md#deal-lost-áp-dụng-ở-bất-kỳ-bước-nào)

**Cập nhật Deal:** `STAGE_ID` → `PREPAYMENT_INVOICE`, `OPPORTUNITY` (khớp Estimate đã Approved), `COMMENTS` (QUOTE_NUMBER, ngày gửi, hạn, ưu đãi).

**Estimate Approved:** Khi KH đồng ý → Estimate `STATUS_ID` = `APPROVED` → automation copy Products vào Deal. `OPPORTUNITY` trên Deal phải khớp tổng Estimate.

**Estimate Declined:** Khi KH từ chối → Estimate `STATUS_ID` = `DECLAINED` → tạo Estimate mới nếu tiếp tục đàm phán.

**Deal Lost:** `STAGE_ID` → `LOSE`, `UF_CRM_LOST_REASON` (bắt buộc), close tất cả Estimate đang mở → `DECLAINED`, kiểm tra `UF_CRM_REFERRER` để thông báo đối tác.

> **Contact Lifecycle — Deal Lost:** Khi Deal → Lost, cập nhật Contact `UF_CRM_CONTACT_LIFECYCLE_STAGE` = `1026` (**Closed Lost**). Ngoại trừ Contact đã có Deal WON khác (giữ nguyên **Customer**). Xem [Contact Lifecycle Flow](../crm/contact-fields.md#lifecycle-flow).

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Giải pháp đã chốt | Bước 06 | ✅ |
| Template báo giá (bản mới nhất) | Drive | ✅ |
| Bảng giá chuẩn | Internal | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Estimate (Products + giá) | CRM | Team |
| File Excel báo giá | Email + Zalo | KH |
| File PDF báo giá | Collab Files > 00. Bao gia | Team + KH |
| Collab `SYNITY x [KH]` (lần đầu) | Bitrix24 Collab | Team + KH |
| Deal stage = QUOTATION | CRM | Team |
| Estimate → Approved (khi KH đồng ý) | CRM | Team |
| Deal Products (auto copy từ Estimate) | CRM | Team |

---

## 📋 Template tin nhắn follow-up

### Follow-up sau 3 ngày

```
Chào anh/chị [Tên],

Em Huy bên SYNITY. Em gửi báo giá được 3 ngày rồi,
không biết anh/chị đã có dịp review chưa ạ?

Nếu anh/chị có bất kỳ thắc mắc nào về báo giá,
em sẵn sàng giải đáp hoặc arrange 1 buổi nói chuyện nhanh 15 phút.

Lưu ý: Ưu đãi trong báo giá có hiệu lực đến [ngày].
Anh/chị cân nhắc sớm để giữ được mức giá tốt nhất nhé!
```

### Khi báo giá hết hạn (7 ngày)

```
Chào anh/chị [Tên],

Báo giá bên em gửi đã hết hiệu lực từ [ngày] rồi ạ.

Tuy nhiên, để tiện cho anh/chị, bên em có thể chuyển ưu đãi này
thành voucher bảo lưu, anh/chị có thể sử dụng trong tương lai
khi sẵn sàng triển khai.

Anh/chị có muốn em tạo voucher bảo lưu không ạ?
```

---

## 📊 KPIs & SLA

| Chỉ số | Mục tiêu | Đo lường |
|--------|----------|----------|
| Thời gian tạo báo giá | < 5-7 ngày sau meeting | Ngày meeting → Ngày gửi |
| Tỷ lệ KH chấp nhận báo giá | Theo dõi | Accept / Tổng gửi |
| Tỷ lệ follow-up đúng hạn | 100% | Follow-up / Tổng BG gửi |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>KH đòi giảm giá</strong></summary>

**Nguyên tắc:** Giữ nền giá cố định, không giảm giá bừa bãi.

**Cách xử lý:**
1. "Dạ anh/chị, bảng giá bên em là nền giá cố định cho tất cả khách hàng"
2. Nếu KH thực sự cần → đề xuất giảm phạm vi module thay vì giảm giá
3. Ưu đãi chỉ áp dụng trong thời hạn báo giá
</details>

<details>
<summary><strong>KH so sánh giá với đối thủ</strong></summary>

**Cách xử lý:**
> "Dạ em hiểu. Tuy nhiên anh/chị cần xem tổng giá trị nhận được: bên em không chỉ bán phần mềm mà còn triển khai, training, và đồng hành đến khi team dùng được. Nếu chỉ so giá license thì sẽ thiếu phần quan trọng nhất là triển khai."

Giữ vị thế chuyên gia (Bác sĩ – Bệnh nhân).
</details>

<details>
<summary><strong>Nhiều vòng đàm phán, tạo nhiều Estimate</strong></summary>

**Quy tắc:**
1. Mỗi Estimate = 1 phiên bản báo giá. KHÔNG sửa Estimate đã gửi.
2. Khi cần báo giá mới → Close Estimate cũ (`DECLAINED`) → Tạo Estimate MỚI
3. Chỉ 1 Estimate được `APPROVED` cho 1 Deal (Estimate cuối cùng KH đồng ý)
4. Products tự động copy vào Deal khi Estimate → Approved
5. Ghi chú Deal: liệt kê lịch sử tất cả QUOTE_NUMBER (VD: "EST.90 → Declined, EST.94 → Approved")
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| ← Bước trước: 06. Proposal & Solution | [Link](06-proposal-solution.md) |
| → Bước tiếp: 08. Contract & Closing | [Link](08-contract-closing.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| 🏠 Trang chủ | [Link](../README.md) |
