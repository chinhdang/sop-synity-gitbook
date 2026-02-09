# Bước 03: Sale Qualification – Khảo sát & Xác định BANT

> **Pha:** 2 - Consideration | **Phụ trách chính:** P. Chuyển đổi (Huy)
> **Thời gian chuẩn:** 1-2 ngày sau khi kết nối
> **CJM Phase:** SURVEY (Khảo sát)

---

## 🎯 Mục tiêu

Xác định khách hàng có phù hợp không thông qua tiêu chí **BANT** (Budget - Authority - Need - Timeline).

---

## 🗺️ CJM Actions trong bước này

```
PHASE 3: SURVEY (Khảo sát)
├── Gửi form khảo sát nhu cầu sơ bộ
├── ◇ Khách hàng điền form?
├── Hệ thống tự động tạo Contact / Company / Deal
├── Tra cứu bổ sung (MST, thông tin pháp lý)
└── Gửi email/SMS xác nhận đã nhận form
```

---

## ✅ Checklist hành động

### Gửi form khảo sát

- [ ] Chọn form khảo sát phù hợp với nhu cầu KH đã trao đổi sơ bộ
- [ ] Gửi form qua Zalo group (kèm lời nhắn)
- [ ] Nhấn mạnh: "Anh/chị vui lòng để **người ra quyết định** điền form"

### Gọi điện hỗ trợ (nếu KH chưa điền sau 1 ngày)

- [ ] Gọi điện trao đổi về tầm quan trọng của form
- [ ] Giải tỏa tâm lý "bị bán hàng"
- [ ] Kêu gọi tham gia First Meeting

### Sau khi KH điền form

- [ ] Kiểm tra thông tin đã đủ chưa (xem bảng bên dưới)
- [ ] Nếu thiếu MST → tra cứu trên masothue.com và bổ sung
- [ ] Nếu có link website/Facebook → ghi chú insight vào CRM
- [ ] Xác nhận Contact/Company/Deal đã được tạo tự động trong Bitrix

---

## 📥 Input (cần có trước khi bắt đầu)

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| SĐT khách hàng | Bước 02 | ✅ |
| Nhóm Zalo đã tạo | Bước 02 | ✅ |
| Deal đã tạo trong CRM | Bước 02 | ✅ |

---

## 📤 Output (kết quả sau khi hoàn thành)

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Form khảo sát đã điền | JotForm → Bitrix (tự động) | P. Chuyển đổi |
| Contact/Company chuẩn hóa | Bitrix CRM | Team |
| Đánh giá BANT | Ghi chú trong Deal | Chinh, Team |

---

## 💾 Thao tác CRM (Bitrix24)

### Tự động (khi KH điền form)

```
JotForm submit → Bitrix Automation
├── Tạo Contact (nếu chưa có)
├── Tạo Company (nếu chưa có)
├── Liên kết Contact ↔ Company ↔ Deal
└── Gửi email/ZNS xác nhận cho KH
```

### Thủ công (P. Chuyển đổi kiểm tra)

| Thực thể | Trường cần có | Cách bổ sung nếu thiếu |
|----------|---------------|------------------------|
| **Contact** | Họ tên, Email, SĐT, Giới tính, Nguồn đến, Facebook cá nhân | Hỏi KH qua Zalo |
| **Company** | Tên công ty, MST, Địa chỉ pháp lý, Địa chỉ nhận hồ sơ | Tra masothue.com |
| **Deal** | Title rõ ràng, Nguồn, Giá trị dự kiến, Stage = "Qualified" | Cập nhật thủ công |

### Cách đặt tên Deal

```
[Tên công ty] - [Nhu cầu chính] - [Tháng/Năm]
Ví dụ: ABC Corp - CRM Sales - 02/2026
```

---

## 📋 Form & Template

| Tài liệu | Link | Ghi chú |
|----------|------|---------|
| Form khảo sát nhu cầu sơ bộ | [JotForm link] | Dùng cho KH mới |
| Form khảo sát theo ngành (Đào tạo) | [JotForm link] | Nếu KH là trung tâm đào tạo |
| Kịch bản gọi điện hỗ trợ điền form | [Google Doc link] | Tham khảo, không đọc máy móc |
| Template tin nhắn nhắc điền form | Xem bên dưới | Copy & chỉnh sửa |

### Template tin nhắn nhắc điền form

```
Chào anh/chị [Tên],

Em là Huy bên SYNITY. Để chuẩn bị tốt nhất cho buổi trao đổi sắp tới,
anh/chị vui lòng dành 3-5 phút điền form khảo sát nhu cầu nhé:

👉 [Link form]

Form này giúp bên em hiểu rõ tình hình của anh/chị,
từ đó tư vấn đúng trọng tâm, tiết kiệm thời gian cho cả hai bên.

Lưu ý: Anh/chị nào là người quyết định về giải pháp này thì điền form
sẽ giúp buổi meeting hiệu quả hơn ạ.

Em cảm ơn anh/chị!
```

---

## 📊 KPIs & SLA

| Chỉ số | Mục tiêu | Đo lường |
|--------|----------|----------|
| Tỷ lệ KH hoàn thành form | > 80% | Số form nhận / Số form gửi |
| Thời gian KH điền form | < 3 ngày | Từ lúc gửi → lúc nhận |
| Tỷ lệ người quyết định điền form | > 70% | Kiểm tra title/vai trò trong form |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>KH nói "gửi tài liệu trước đi, form sau"</strong></summary>

**Cách xử lý:**
> "Dạ em hiểu anh/chị bận. Form này chỉ mất 3-5 phút và giúp bên em
> chuẩn bị tài liệu ĐÚNG với nhu cầu của anh/chị. Nếu không có form,
> em chỉ gửi được tài liệu chung chung, có thể không phù hợp lắm ạ."

Nếu KH vẫn từ chối → ghi chú vào CRM, chuyển sang gọi điện khảo sát thủ công.
</details>

<details>
<summary><strong>Form được điền bởi nhân viên, không phải người quyết định</strong></summary>

**Cách xử lý:**
1. Vẫn tiếp nhận thông tin
2. Trong cuộc gọi tiền meeting (Bước 04), hỏi: "Anh/chị có thể mời sếp tham gia buổi meeting không ạ?"
3. Ghi chú vào Deal: "Form điền bởi [Tên] - [Vai trò]. Cần mời [Người quyết định] vào meeting."
</details>

<details>
<summary><strong>Automation không tạo Contact/Company tự động</strong></summary>

**Kiểm tra:**
1. KH có điền đúng form đang active không?
2. Email KH đã tồn tại trong hệ thống chưa? (có thể bị trùng)
3. Kiểm tra Bitrix Automation Rules có đang bật không

**Nếu vẫn lỗi:** Tạo thủ công và báo IT kiểm tra automation.
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| ← Bước trước: 02. New Opportunity | [Link](02-new-opportunity.md) |
| → Bước tiếp: 04. Need Analysis | [Link](04-need-analysis.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| 🏠 Trang chủ | [Link](../README.md) |
