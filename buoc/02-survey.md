# Bước 02: Survey – Khảo sát nhu cầu & Cập nhật dữ liệu

> **Pha:** 1 - Lead Qualification | **Phụ trách chính:** P. Chuyển đổi
> **Lead Stage:** SUBMITTED FORM
> **CJM Phase:** SURVEY

---

## 🎯 Mục tiêu

Tiếp nhận dữ liệu từ Google Form mà KH đã điền, tự động cập nhật vào Bitrix24 Lead (match SĐT), tạo Contact + Company, gửi Email & ZNS xác nhận, và đánh giá BANT sơ bộ.

---

## 🗺️ CJM Actions trong bước này

```
PHASE 3: SURVEY (Khảo sát)
├── KH submit Google Form
├── Hệ thống auto cập nhật Bitrix Lead (match SĐT)
├── Auto tạo Contact + Company
├── Auto gửi Email & ZNS xác nhận
├── Lead chuyển stage: Submitted Form
├── Nhân sự review data, bổ sung MST nếu thiếu
└── Đánh giá BANT sơ bộ từ kết quả form
```

---

## ✅ Checklist hành động

### A. Sau khi KH điền form

- [ ] Kiểm tra Lead đã tự động chuyển stage **Submitted Form** chưa
- [ ] Xác nhận Contact + Company đã được tạo tự động trong Bitrix
- [ ] Kiểm tra data đã cập nhật đầy đủ (xem bảng bên dưới)
- [ ] Nếu thiếu MST → tra cứu trên masothue.com và bổ sung
- [ ] Nếu có link Website/Facebook KH → ghi chú insight vào Lead
- [ ] Xác nhận Email & ZNS xác nhận đã gửi cho KH
- [ ] Đánh giá BANT sơ bộ từ thông tin trong form:
  - **Budget:** KH có đề cập ngân sách không?
  - **Authority:** Ai điền form? Có phải người quyết định?
  - **Need:** Nhu cầu chính là gì? Phù hợp với giải pháp SYNITY?
  - **Timeline:** KH có nhu cầu gấp hay dài hạn?

### B. Nếu KH chưa điền form (sau 1 ngày)

- [ ] Gửi tin nhắn nhắc qua Zalo group (dùng template)
- [ ] Nếu sau 2 ngày vẫn chưa điền → gọi điện nhắc
- [ ] Giải tỏa tâm lý "bị bán hàng": nhấn mạnh form giúp tư vấn đúng nhu cầu
- [ ] Nếu KH vẫn từ chối → khảo sát thủ công qua điện thoại, tự điền data vào CRM

---

## 💾 Thao tác CRM (Bitrix24)

### Tự động (Automation khi KH submit Google Form)

```
Google Form submit → Bitrix Automation (match SĐT)
├── Cập nhật Lead fields từ form data
├── Tạo Contact (nếu chưa có)
├── Tạo Company (nếu chưa có)
├── Liên kết Contact ↔ Company ↔ Lead
├── Gửi Email xác nhận cho KH
├── Gửi ZNS xác nhận cho KH
└── Lead stage → Submitted Form
```

### Thủ công (P. Chuyển đổi kiểm tra & bổ sung)

> **Chi tiết trường thông tin:**
> - [Lead Fields — Bước 02](../crm/lead-fields.md#bước-02--survey-in_process)
> - [Contact Fields](../crm/contact-fields.md#contact-kh--tạo-tại-bước-02-auto-từ-google-form)
> - [Company Fields](../crm/company-fields.md)

**Kiểm tra sau form submit:**
- Lead: `EMAIL`, `COMPANY_TITLE`, `COMPANY_ID`, `CONTACT_ID` đã auto cập nhật?
- Contact: Họ tên, Email, SĐT, Chức vụ đã đủ?
- Company: Tên công ty, MST (tra masothue.com nếu thiếu)
- Bổ sung `COMMENTS`: BANT sơ bộ + insight website/FB KH

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Lead đã tạo (stage New Lead) | Bước 01 | ✅ |
| Nhóm Zalo đã tạo | Bước 01 | ✅ |
| Google Form đã gửi cho KH | Bước 01 | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Lead cập nhật data từ form | Bitrix CRM | P. Chuyển đổi |
| Contact đã tạo | Bitrix CRM | Team |
| Company đã tạo | Bitrix CRM | Team |
| Email xác nhận đã gửi | Email (auto) | KH |
| ZNS xác nhận đã gửi | Zalo (auto) | KH |
| Đánh giá BANT sơ bộ | Ghi chú trong Lead | Team |

---

## 📋 Template tin nhắn nhắc điền form

```
Chào anh/chị [Tên],

Em là [Tên] bên SYNITY. Để chuẩn bị tốt nhất cho buổi trao đổi sắp tới,
anh/chị vui lòng dành 3-5 phút điền form khảo sát nhu cầu nhé:

👉 [Link Google Form]

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
| Tỷ lệ data đầy đủ sau review | 100% | Kiểm tra CRM |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>KH nói "gửi tài liệu trước đi, form sau"</strong></summary>

**Cách xử lý:**
> "Dạ em hiểu anh/chị bận. Form này chỉ mất 3-5 phút và giúp bên em
> chuẩn bị tài liệu ĐÚNG với nhu cầu của anh/chị. Nếu không có form,
> em chỉ gửi được tài liệu chung chung, có thể không phù hợp lắm ạ."

Nếu KH vẫn từ chối → ghi chú vào Lead, chuyển sang gọi điện khảo sát thủ công.
</details>

<details>
<summary><strong>Form điền bởi nhân viên, không phải người quyết định</strong></summary>

**Cách xử lý:**
1. Vẫn tiếp nhận thông tin
2. Ghi chú vào Lead: "Form điền bởi [Tên] - [Vai trò]. Cần mời [Người quyết định] vào meeting."
3. Trong bước Book Meeting (Bước 03), đề nghị mời người quyết định tham gia
</details>

<details>
<summary><strong>Automation không cập nhật Lead / không tạo Contact-Company tự động</strong></summary>

**Kiểm tra:**
1. KH có điền đúng SĐT đã tạo trong Lead không? (match SĐT)
2. Google Form có đang kết nối Bitrix automation không?
3. Kiểm tra Bitrix Automation Rules có đang bật không

**Nếu vẫn lỗi:** Cập nhật thủ công và báo IT kiểm tra automation.
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| ← Bước trước: 01. New Lead | [Link](01-new-lead.md) |
| → Bước tiếp: 03. Meeting | [Link](03-meeting.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| 🏠 Trang chủ | [Link](../README.md) |
