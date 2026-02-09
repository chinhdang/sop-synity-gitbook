# Bước 08: Contract & Closing – Soạn & ký hợp đồng

> **Pha:** 2 - Deal Closing | **Phụ trách chính:** P. Chuyển đổi (Huy)
> **Thời gian chuẩn:** < 7 ngày
> **CJM Phase:** CONTRACT (Hợp đồng)

---

## 🎯 Mục tiêu

Review hợp đồng với người quyết định, làm rõ điều khoản, ký kết, và khởi tạo luồng thu tiền bản quyền.

---

## 🗺️ CJM Actions trong bước này

```
PHASE 7: CONTRACT (Hợp đồng)
├── Buổi REVIEW HỢP ĐỒNG (với người ra quyết định)
├── Làm rõ điều khoản: Thanh toán, Bảo mật, Cách làm việc
├── Gửi hợp đồng (deadline ký: 7 ngày)
├── ◇ Khách ký hợp đồng?
├── KH eSign HĐ Bản quyền → Tạo Invoice → Auto gửi Đề nghị TT
└── Thu tiền BẢN QUYỀN trước khi triển khai
```

---

## ⚖️ Nguyên tắc: Luôn tách 2 Hợp đồng

> **Quy định:** Mọi deal đều có **2 hợp đồng riêng biệt**, không gộp chung.

| # | Hợp đồng | Phạm vi | Bắt buộc |
|---|----------|---------|----------|
| 1 | **HĐ Bản quyền Bitrix24** | Cung cấp license phần mềm | **Luôn có** (kể cả KH chỉ mua bản quyền) |
| 2 | **HĐ Dịch vụ Triển khai** | Dịch vụ tư vấn, cấu hình, training | Có khi KH mua dịch vụ triển khai |

### Lý do tách hợp đồng

1. **Không phải KH nào cũng mua cả hai:** Có KH chỉ mua bản quyền → cần HĐ riêng hoàn chỉnh.
2. **Hoàn thiện hồ sơ pháp lý:** Mỗi HĐ bản quyền có bộ hồ sơ đầy đủ cho thanh tra/kiểm tra:
   - Báo giá
   - Hợp đồng
   - Đề nghị thanh toán
   - Biên bản bàn giao bản quyền
3. **Loại rủi ro bồi thường:** HĐ dịch vụ triển khai có điều khoản bồi thường. Nếu gộp bản quyền vào → SYNITY phải bồi thường cả giá trị license (không hợp lý vì triển khai không làm tổn hại phần mềm). Tách riêng = loại rủi ro phần không chịu trách nhiệm.

---

## ✅ Nhân sự thực hiện

### A. Kiểm tra Requisites trước khi tạo HĐ

> **⛔ Gate:** Document Template lấy dữ liệu từ Requisites. Nếu thiếu → HĐ sẽ bị lỗi/trống.

- [ ] Mở Company card → tab **Details/Requisites** → kiểm tra:
  - [ ] `RQ_COMPANY_NAME` — Tên pháp lý *(nếu thiếu: tra [masothue.com](https://masothue.com))*
  - [ ] `RQ_VAT_ID` — Mã số thuế
  - [ ] Address — Địa chỉ pháp lý (theo [quy chuẩn VN](../crm/requisite-guide.md#address-gắn-vào-requisite--mapping-cho-việt-nam))
- [ ] Kiểm tra **5 vai trò liên hệ** (bắt buộc cho HĐ Dịch vụ Triển khai):
  - [ ] **Người đại diện:** `UF_CRM_REP_NAME` + `UF_CRM_REP_POSITION` *(bắt buộc)*
  - [ ] **QLDA:** `UF_CRM_PM_NAME` + `UF_CRM_PM_EMAIL` *(bắt buộc)*
  - [ ] **Kỹ thuật:** `UF_CRM_TECH_NAME` + `UF_CRM_TECH_EMAIL` *(nên có)*
  - [ ] **Người liên lạc:** `UF_CRM_LIAISON_NAME` *(nên có)*
  - [ ] **Kế toán:** `UF_CRM_ACC_NAME` + `UF_CRM_ACC_EMAIL` *(nên có)*
- [ ] Kiểm tra Contact:
  - [ ] Tên đầy đủ + Danh xưng (Ông/Bà) + Chức vụ
- [ ] Kiểm tra Deal:
  - [ ] Product Rows — đã có sản phẩm (tên gói + mô tả)

> **Hướng dẫn:** Xem [Requisites — Hướng dẫn nhập](../crm/requisite-guide.md)

### B. Soạn hợp đồng

- [ ] Soạn **HĐ Bản quyền Bitrix24** (từ Document Template) — luôn có
- [ ] Soạn **HĐ Dịch vụ Triển khai** (từ template) — nếu KH mua dịch vụ
- [ ] Kiểm tra tài liệu tự động: tên KH, MST, địa chỉ hiển thị đúng?
- [ ] Đảm bảo 2 HĐ có **số HĐ riêng biệt**
- [ ] Review nội bộ với Chinh

### C. Buổi Review Hợp đồng

- [ ] Đặt lịch meeting review HĐ **với người ra quyết định**
- [ ] Làm rõ các điều khoản quan trọng:
  - **Thanh toán:** Lịch thanh toán theo đợt, phương thức
  - **Bảo mật:** Cam kết bảo mật thông tin
  - **Cách làm việc:** Phối hợp qua Collab, check-in định kỳ
  - **Nghiệm thu:** Quy trình nghiệm thu, auto-approve
- [ ] Ghi nhận feedback / yêu cầu chỉnh sửa (nếu có)
- [ ] Chỉnh sửa HĐ theo feedback

### D. Ký kết

> **Nguyên tắc:** eSign giúp xúc tiến nhanh để tiến hành thanh toán. **HĐ ký tay đóng dấu** (bản cứng có dấu đỏ) mới là bản pháp lý chính thức.

- [ ] Gửi HĐ chính thức (deadline ký: **7 ngày**)
- [ ] **eSign (Bitrix24):** Gửi eSign cho KH ký nhanh → tiến hành thu tiền bản quyền trước
  - eSign **KHÔNG thay thế** HĐ ký tay đóng dấu
- [ ] **HĐ ký tay đóng dấu (bản cứng):** Song song chuẩn bị bản cứng
  - In HĐ → ký tay → đóng dấu (cả 2 bên)
  - HĐ bản cứng có dấu đỏ = **bản pháp lý chính thức**
- [ ] Theo dõi tiến độ ký cả 2 hình thức
- [ ] Upload vào CRM: eSign + bản scan HĐ ký tay đóng dấu

### E. Luồng bản quyền: eSign → Invoice → Đề nghị TT

> **Chiến lược:** Thu tiền bản quyền TRƯỚC khi bắt đầu triển khai. KH đã "đầu tư" → khó bỏ giữa chừng.
>
> **Tên miền Bitrix24** đã ghi rõ trong HĐ Bản quyền → không cần eSign xác nhận riêng.

```
KH đồng ý báo giá (Bước 07)
  │
  ▼
Soạn HĐ Bản quyền Bitrix (Document Template)     ← Bước B
  │
  ▼
KH review + đồng ý HĐ                             ← Bước C
  │
  ▼
Gửi HĐ eSign (Bitrix24)                            ← Bước D
  │
  ▼
KH eSign xong → Tạo Invoice thanh toán bản quyền
  │
  ▼
Invoice tự động gửi email Đề nghị TT cho KH        ← Bitrix workflow
  │  (email kèm file PDF Đề nghị TT — tạo tự động)
  ▼
KH thanh toán → Bước 09 (Kích hoạt license)
```

- [ ] Sau khi KH eSign HĐ bản quyền → **tạo Invoice** thanh toán bản quyền trong CRM
- [ ] Invoice tự động gửi **email Đề nghị thanh toán** cho KH (Bitrix workflow)
  - Email kèm **file PDF Đề nghị TT** — tạo tự động từ Document Template
  - Nhân sự **không cần gửi thủ công** — chỉ kiểm tra email đã gửi thành công
- [ ] Lưu **tên miền Bitrix24** của KH vào Deal: `UF_CRM_B24_PORTAL`
- [ ] **KHÔNG bắt đầu triển khai** khi chưa thu đủ tiền bản quyền
- [ ] Chi tiết quy trình kích hoạt + nghiệm thu: Xem [Bước 09 — Luồng Bản quyền](09-payment.md#a-luồng-bản-quyền-bitrix24)

### F. Hoàn tất

- [ ] Cập nhật Deal stage = CONTRACT
- [ ] Bàn giao thông tin cho bước Payment (Bước 09)

---

## 💾 Thao tác CRM (Bitrix24)

| Thao tác | Chi tiết |
|----------|----------|
| Cập nhật Deal stage | → CONTRACT |
| Upload HĐ bản quyền | eSign + bản scan ký tay đóng dấu → attach vào Deal |
| Upload HĐ triển khai | eSign + bản scan ký tay đóng dấu → attach vào Deal (nếu có) |
| Tạo Invoice bản quyền | Sau khi KH eSign → Invoice auto gửi email Đề nghị TT |
| Lưu tên miền B24 | `UF_CRM_B24_PORTAL` (đã ghi trong HĐ bản quyền) |
| Ghi chú Deal | Số HĐ (2 số riêng biệt), Ngày ký, Người ký, Điều khoản đặc biệt |

### Deal fields — Cập nhật tại Bước 08

> **Chi tiết trường thông tin:** Xem [Deal Fields — Bước 08](../crm/deal-fields.md#bước-08--contract--closing-executing)
>
> **Mẫu ghi chú COMMENTS:** Xem [Deal Fields — Mẫu ghi chú Bước 08](../crm/deal-fields.md#mẫu-ghi-chú-comments-bước-08)

**Bắt buộc:** `STAGE_ID` → `EXECUTING`, `UF_CRM_CONTRACT_NO`, `UF_CRM_CONTRACT_DATE`, `UF_CRM_B24_PORTAL`, `UF_CRM_PAYMENT_METHOD`.

**Quan trọng:** Số HĐ + Ngày ký phải điền trước khi chuyển sang Payment. Portal B24 (`UF_CRM_B24_PORTAL`) lấy từ HĐ bản quyền.

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Báo giá đã chấp nhận | Bước 07 | ✅ |
| Template HĐ Bản quyền | Document Template (Bitrix) | ✅ |
| Template HĐ Triển khai | Document Template (Bitrix) | Nếu có dịch vụ |
| Thông tin công ty KH (Requisite) | CRM | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Hợp đồng đã ký (2 bản) | CRM + Drive | Team + KH |
| Invoice bản quyền + email Đề nghị TT (auto) | CRM + Email | KH + Kế toán |
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
