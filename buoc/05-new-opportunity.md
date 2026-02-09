# Bước 05: New Opportunity – Cơ hội mới

> **Pha:** 2 - Deal Closing | **Phụ trách chính:** Chinh + P. Chuyển đổi
> **Deal Stage:** NEW OPPORTUNITY
> **CJM Phase:** INTEREST (Deal level)

---

## 🎯 Mục tiêu

Từ Good Lead đã qualified, tạo Deal và bắt đầu phân tích nhu cầu chi tiết, xác định scope & modules cần triển khai.

---

## 🗺️ CJM Actions trong bước này

```
DEAL STAGE 1: NEW OPPORTUNITY
├── Good Lead → Convert to Deal (auto/thủ công)
├── Phân tích nhu cầu chi tiết từ form + meeting notes
├── Xác định modules cần triển khai
├── Xác định scope sơ bộ
└── Chuẩn bị brief cho Proposal
```

---

## ✅ Checklist hành động

### A. Tạo Deal từ Good Lead

- [ ] Chuyển Lead stage → Good Lead
- [ ] Tạo Deal mới trong Bitrix24
- [ ] Deal stage: **New Opportunity**
- [ ] Tên Deal: `[Tên công ty] - [Nhu cầu chính] - [Tháng/Năm]`
- [ ] Liên kết Deal ↔ Contact ↔ Company
- [ ] Ghi chú nguồn: từ Lead #[số]

### B. Phân tích nhu cầu chi tiết

- [ ] Review lại kết quả Google Form
- [ ] Review meeting notes & recording từ Bước 03
- [ ] Xác định modules KH cần: CRM, HR, Project, Marketing...
- [ ] Xác định quy mô: số user, số phòng ban
- [ ] Xác định pain points chính
- [ ] Ghi chú tất cả vào Deal

### C. Chuẩn bị brief cho Proposal

- [ ] Tóm tắt nhu cầu KH
- [ ] Liệt kê modules đề xuất
- [ ] Xác định scope sơ bộ (GĐ1)
- [ ] Internal brief với Chinh trước khi phát triển giải pháp
- [ ] Deal sẵn sàng → chuyển sang Bước 06

---

## 💾 Thao tác CRM (Bitrix24)

| Thao tác | Chi tiết |
|----------|----------|
| Tạo Deal | Từ Good Lead, stage: New Opportunity |
| Liên kết | Deal ↔ Contact ↔ Company |
| Deal fields | Xem bảng bên dưới |
| Activity | Ghi nhận ngày convert Lead → Deal |

### Deal fields — Tạo mới tại Bước 05

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ví dụ |
|-------------|-------------|----------|-----------|-------|
| `TITLE` | Tiêu đề Deal | **YES** | `[Công ty] - [Nhu cầu] - [MM/YYYY]` | "ABC Corp - CRM Sales - 02/2026" |
| `STAGE_ID` | Stage | **YES** | `NEW` | New Opportunity |
| `COMPANY_ID` | Công ty | **YES** | Link Company từ Lead | Auto khi convert Lead |
| `CONTACT_ID` | Liên hệ chính | **YES** | Link Contact từ Lead | Auto khi convert Lead |
| `ASSIGNED_BY_ID` | Người phụ trách | **YES** | Nhân sự Deal pipeline | ID nhân viên |
| `OPPORTUNITY` | Giá trị dự kiến | Nên có | Số tiền VND ước tính | 50000000 |
| `CURRENCY_ID` | Tiền tệ | Auto | Mặc định | `VND` |
| `SOURCE_ID` | Nguồn | Nên có | Copy từ Lead | `FACEBOOK`, `RECOMMENDATION`... |
| `LEAD_ID` | Lead gốc | Auto | Liên kết Lead đã convert | Lead #ID |
| `UF_CRM_REFERRER` | Referrer (Partner) | Nếu có | Copy từ Lead `UF_CRM_REFERRER` | Contact đối tác |
| `UF_CRM_SERVICE_TYPE` | Loại dịch vụ | Nên có | Chọn từ dropdown | Tùy loại triển khai |
| `COMMENTS` | Ghi chú | **YES** | Tóm tắt BANT + brief nhu cầu | Xem mẫu bên dưới |

#### Mẫu ghi chú COMMENTS khi tạo Deal

```
--- Chuyển từ Lead #[ID] ---
BANT:
- Budget: [Tóm tắt ngân sách]
- Authority: [Người QĐ] - [Chức vụ]
- Need: [Nhu cầu chính]
- Timeline: [Thời gian triển khai dự kiến]

Modules đề xuất: [CRM, HR, Project...]
Scope sơ bộ: [Số user, phòng ban]
Next actions: [Danh sách]
```

> **Lưu ý cho AI/Automation:** `LEAD_ID` tự động liên kết khi convert Lead. `UF_CRM_REFERRER` phải copy thủ công từ Lead sang Deal nếu có đối tác giới thiệu. `TITLE` dùng format chuẩn để dễ tìm kiếm và phân loại.

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Good Lead (đã qualified) | Bước 04 | ✅ |
| Kết quả Google Form | Bước 02 | ✅ |
| Meeting notes & recording | Bước 03 | ✅ |
| Đánh giá BANT | Bước 04 | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Deal đã tạo (stage: New Opportunity) | Bitrix CRM | Chinh + Team |
| Brief nhu cầu chi tiết | Deal notes | Team |
| Scope sơ bộ (modules) | Deal notes | Chinh |

---

## 📊 KPIs & SLA

| Chỉ số | Mục tiêu | Đo lường |
|--------|----------|----------|
| Thời gian từ Good Lead → Deal | Trong ngày | Ngày convert |
| Deal có đủ thông tin | 100% | Checklist fields |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>Good Lead nhưng nhu cầu chưa rõ ràng</strong></summary>

**Cách xử lý:**
1. Vẫn tạo Deal (đã qualified qua meeting)
2. Đặt thêm 1 buổi meeting ngắn (30 phút) để làm rõ
3. Ghi chú: "Cần clarify: [điểm chưa rõ]"
</details>

<details>
<summary><strong>KH muốn nhiều modules nhưng ngân sách hạn chế</strong></summary>

**Cách xử lý:**
1. Ghi nhận tất cả nhu cầu
2. Phân chia thành GĐ1 (ưu tiên) và GĐ2 (sau)
3. Scope GĐ1 phù hợp ngân sách
4. Ghi chú potential upsell cho GĐ2
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| ← Bước trước: 04. Lead Qualification | [Link](04-lead-qualification.md) |
| → Bước tiếp: 06. Proposal & Solution | [Link](06-proposal-solution.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| 🏠 Trang chủ | [Link](../README.md) |
