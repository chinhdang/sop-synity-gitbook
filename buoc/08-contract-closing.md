# Bước 08: Contract & Closing – Soạn & ký hợp đồng

> **Pha:** 2 - Deal Closing | **Phụ trách chính:** P. Chuyển đổi (Huy)
> **Thời gian chuẩn:** < 7 ngày
> **CJM Phase:** CONTRACT (Hợp đồng)

---

## 🎯 Mục tiêu

Review hợp đồng với người quyết định, làm rõ điều khoản, ký kết, thu tiền bản quyền trước, và xác nhận email + tên miền.

---

## 🗺️ CJM Actions trong bước này

```
PHASE 7: CONTRACT (Hợp đồng)
├── Buổi REVIEW HỢP ĐỒNG (với người ra quyết định)
├── Làm rõ điều khoản: Thanh toán, Bảo mật, Cách làm việc
├── Gửi hợp đồng (deadline ký: 7 ngày)
├── ◇ Khách ký hợp đồng?
├── Thu tiền BẢN QUYỀN trước (chiến thuật giữ khách)
└── Văn bản xác nhận Email + Tên miền trước khi mua license
```

---

## ✅ Checklist hành động

### A. Soạn hợp đồng

- [ ] Soạn **Hợp đồng Bản quyền** Bitrix24 (từ template)
- [ ] Soạn **Hợp đồng Triển khai** (từ template)
- [ ] Điền thông tin KH từ CRM (MST, địa chỉ pháp lý, người đại diện)
- [ ] Review nội bộ với Chinh

### B. Buổi Review Hợp đồng

- [ ] Đặt lịch meeting review HĐ **với người ra quyết định**
- [ ] Làm rõ các điều khoản quan trọng:
  - **Thanh toán:** Lịch thanh toán theo đợt, phương thức
  - **Bảo mật:** Cam kết bảo mật thông tin
  - **Cách làm việc:** Phối hợp qua Collab, check-in định kỳ
  - **Nghiệm thu:** Quy trình nghiệm thu, auto-approve
- [ ] Ghi nhận feedback / yêu cầu chỉnh sửa (nếu có)
- [ ] Chỉnh sửa HĐ theo feedback

### C. Ký kết

- [ ] Gửi HĐ chính thức (deadline ký: **7 ngày**)
- [ ] Gửi qua eSign (Bitrix24) hoặc bản cứng
- [ ] Theo dõi tiến độ ký
- [ ] KH ký → upload HĐ đã ký vào CRM

### D. Thu tiền bản quyền trước (chiến thuật giữ khách)

- [ ] **Thu tiền BẢN QUYỀN trước** (tách riêng khỏi phí triển khai)
- [ ] Mục đích: KH đã "đầu tư" → khó bỏ giữa chừng
- [ ] Tạo Invoice bản quyền trong CRM

### E. Xác nhận Email & Tên miền

- [ ] Gửi **eSign xác nhận email & tên miền** cho KH
- [ ] KH xác nhận email domain sẽ dùng cho Bitrix24
- [ ] KH xác nhận tên miền (nếu dùng subdomain riêng)
- [ ] Lưu xác nhận vào CRM trước khi mua license

### F. Hoàn tất

- [ ] Cập nhật Deal stage = CONTRACT
- [ ] Bàn giao thông tin cho bước Payment (Bước 09)

---

## 💾 Thao tác CRM (Bitrix24)

| Thao tác | Chi tiết |
|----------|----------|
| Cập nhật Deal stage | → CONTRACT |
| Upload HĐ đã ký | Attach vào Deal |
| Tạo Invoice | Invoice bản quyền Bitrix24 |
| eSign | Xác nhận email & tên miền |
| Ghi chú Deal | Ngày ký, Người ký, Điều khoản đặc biệt |

### Deal fields — Cập nhật tại Bước 08

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ví dụ |
|-------------|-------------|----------|-----------|-------|
| `STAGE_ID` | Stage | **YES** | `EXECUTING` | Contract & Closing |
| `UF_CRM_CONTRACT_NO` | Số hợp đồng | **YES** | Số HĐ chính thức | "HĐ-2026-001" |
| `UF_CRM_CONTRACT_DATE` | Ngày ký HĐ | **YES** | Ngày ký chính thức | "2026-02-15" |
| `UF_CRM_B24_PORTAL` | Portal Bitrix24 | **YES** | URL portal KH | "abc.bitrix24.vn" |
| `UF_CRM_LICENSE_KEY` | License Key | Nên có | Key bản quyền B24 | Sau khi mua license |
| `UF_CRM_PAYMENT_METHOD` | Phương thức TT | **YES** | Chọn từ dropdown | Chuyển khoản / Tiền mặt |
| `UF_CRM_CONTRACT_NUMBER_OF_PAYMENTS` | Số đợt TT | Nên có | Số đợt thanh toán theo HĐ | "3" |
| `UF_CRM_PAYMENT_CYCLE` | Chu kỳ TT | Nên có | Chu kỳ thanh toán | Theo nghiệm thu |
| `CLOSEDATE` | Ngày dự kiến đóng | **Cập nhật** | Ngày dự kiến hoàn tất Deal | Theo timeline triển khai |
| `COMMENTS` | Ghi chú | **Cập nhật** | Thông tin HĐ + email/domain | Xem mẫu bên dưới |

#### Mẫu ghi chú COMMENTS bổ sung tại Bước 08

```
--- Hợp đồng [DD/MM/YYYY] ---
Số HĐ Bản quyền: [Số]
Số HĐ Triển khai: [Số]
Ngày ký: [DD/MM/YYYY]
Người ký: [Tên] - [Chức vụ]

Lịch thanh toán:
- Đợt 1 (Bản quyền): [Số tiền] - [Hạn]
- Đợt 2 (Triển khai GĐ1): [Số tiền] - [Hạn]
- Đợt 3 (...): [Số tiền] - [Hạn]

Email xác nhận: [email@domain.com]
Tên miền xác nhận: [abc.bitrix24.vn]
Điều khoản đặc biệt: [Nếu có]
```

> **Lưu ý cho AI/Automation:** `UF_CRM_CONTRACT_NO` và `UF_CRM_CONTRACT_DATE` phải được điền trước khi chuyển sang Payment. `UF_CRM_B24_PORTAL` cần xác nhận từ KH qua eSign trước khi mua license.

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Báo giá đã chấp nhận | Bước 07 | ✅ |
| Template HĐ Bản quyền | Drive | ✅ |
| Template HĐ Triển khai | Drive | ✅ |
| Thông tin công ty KH (MST, địa chỉ) | CRM | ✅ |
| Template eSign xác nhận email & tên miền | Drive | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Hợp đồng đã ký (2 bản) | CRM + Drive | Team + KH |
| eSign email & tên miền | Bitrix eSign | KH |
| Invoice bản quyền | CRM | KH + Kế toán |
| Deal stage = CONTRACT | CRM | Team |

---

## 📊 KPIs & SLA

| Chỉ số | Mục tiêu | Đo lường |
|--------|----------|----------|
| Thời gian ký HĐ | < 7 ngày sau chấp nhận BG | Ngày accept → Ngày ký |
| Tỷ lệ review HĐ với người QĐ | 100% | Có review / Tổng HĐ |
| Thu bản quyền trước triển khai | 100% | Xác nhận thanh toán |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>KH muốn chỉnh sửa nhiều điều khoản HĐ</strong></summary>

**Cách xử lý:**
1. Lắng nghe và ghi nhận yêu cầu
2. Các điều khoản có thể điều chỉnh: thời hạn thanh toán, phạm vi bảo mật
3. Các điều khoản KHÔNG thay đổi: giá, phạm vi triển khai đã chốt, quy trình nghiệm thu
4. Nếu yêu cầu quá phức tạp → escalate lên Chinh
</details>

<details>
<summary><strong>KH trì hoãn ký HĐ</strong></summary>

**Cách xử lý:**
1. Hỏi lý do cụ thể: "Anh/chị đang cần thêm thông tin gì ạ?"
2. Nhắc deadline: "Báo giá/ưu đãi hiệu lực đến [ngày]"
3. Sau 7 ngày không ký → áp dụng chính sách hết hạn ưu đãi
4. Ghi chú CRM: "KH delay ký HĐ. Lý do: [...]"
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| ← Bước trước: 07. Quotation | [Link](07-quotation.md) |
| → Bước tiếp: 09. Payment | [Link](09-payment.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| 🏠 Trang chủ | [Link](../README.md) |
