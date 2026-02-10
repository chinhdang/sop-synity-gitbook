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

## ✅ Nhân sự thực hiện

### A. Tạo Deal từ Good Lead

- [ ] Chuyển Lead stage → Good Lead
- [ ] Tạo Deal mới trong Bitrix24
- [ ] Deal stage: **New Opportunity**
- [ ] Tên Deal: `[Tên công ty] - [Nhu cầu chính] - [Tháng/Năm]`
- [ ] Liên kết Deal ↔ Contact ↔ Company
- [ ] Ghi chú nguồn: từ Lead #[số]

### B. Kiểm tra & bổ sung thông tin Company

> **Mục đích:** Đảm bảo Company có đủ thông tin pháp lý (Requisite) cho Bước 08 — tạo HĐ tự động.

- [ ] Mở Company card → tab **Details/Requisites**
- [ ] Kiểm tra đã có Requisite chưa? Nếu chưa → tạo mới (template: "SYNITY - Doanh nghiệp VN")
- [ ] **`RQ_COMPANY_NAME`** — Tên pháp lý (tra MST qua [VietQR API](https://api.vietqr.io/v2/business/{MST}) hoặc [masothue.com](https://masothue.com))
- [ ] **`RQ_VAT_ID`** — Mã số thuế (từ Company UF `UF_CRM_COMPANY_1742604428244` hoặc KH cung cấp)
- [ ] **Address** — Địa chỉ pháp lý, tách theo [quy chuẩn VN](../crm/requisite-guide.md#address-gắn-vào-requisite--mapping-cho-việt-nam)
- [ ] Kiểm tra Contact liên kết: đã có **tên + chức vụ** chưa?
- [ ] Bắt đầu thu thập **5 vai trò liên hệ** (bổ sung dần, hoàn chỉnh trước Bước 08):
  - [ ] Người đại diện — `UF_CRM_REP_NAME` + `UF_CRM_REP_POSITION`
  - [ ] QLDA — `UF_CRM_PM_NAME` + `UF_CRM_PM_EMAIL`
  - [ ] Kỹ thuật, Liên lạc, Kế toán — khi có thông tin

> **Hướng dẫn chi tiết:** Xem [Requisites — Hướng dẫn nhập thông tin pháp lý](../crm/requisite-guide.md)

### C. Phân tích nhu cầu chi tiết

- [ ] Review lại kết quả Google Form
- [ ] Review meeting notes & recording từ Bước 03
- [ ] Xác định modules KH cần: CRM, HR, Project, Marketing...
- [ ] Xác định quy mô: số user, số phòng ban
- [ ] Xác định pain points chính
- [ ] Ghi chú tất cả vào Deal

### D. Chuẩn bị brief cho Proposal

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

> **Chi tiết trường thông tin:** Xem [Deal Fields — Bước 05](../crm/deal-fields.md#bước-05--new-opportunity-new)
>
> **Mẫu ghi chú COMMENTS:** Xem [Deal Fields — Mẫu ghi chú Bước 05](../crm/deal-fields.md#mẫu-ghi-chú-comments-bước-05)

**Bắt buộc:** `TITLE` (format: `[Công ty] - [Nhu cầu] - [MM/YYYY]`), `STAGE_ID` = `NEW`, `COMPANY_ID`, `CONTACT_ID`, `ASSIGNED_BY_ID`, `COMMENTS` (BANT + brief).

**Quan trọng:** `UF_CRM_REFERRER` cần copy thủ công từ Lead sang Deal nếu có đối tác.

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
