# Deal — Trường thông tin CRM

> Trang này là **nguồn tham chiếu duy nhất** cho tất cả trường thông tin Deal trong Bitrix24.
> Nhân sự và AI/Automation tra cứu tại đây khi tạo hoặc cập nhật Deal.

---

## Thông tin chung

| Thuộc tính | Giá trị |
|-----------|---------|
| CRM Entity | Deal |
| API method | `crm.deal.add`, `crm.deal.update`, `crm.deal.list` |
| Pipeline | Deal Pipeline (DEAL_STAGE entity, category 0) |
| Stages | NEW → PREPARATION → PREPAYMENT_INVOICE → EXECUTING → FINAL_INVOICE → WON / LOSE |

---

## Deal Stages

| ID | STAGE_ID | Tên hiển thị | SOP Step | Ghi chú |
|----|----------|-------------|----------|---------|
| 101 | `NEW` | New Opportunity | Bước 05 | Tạo Deal từ Good Lead |
| 103 | `PREPARATION` | Proposal & Solution | Bước 06 | Trình bày giải pháp |
| 105 | `PREPAYMENT_INVOICE` | Quotation | Bước 07 | Gửi báo giá |
| 107 | `EXECUTING` | Contract & Closing | Bước 08 | Ký hợp đồng |
| 109 | `FINAL_INVOICE` | Payment | Bước 09 | Thu tiền |
| 544 | `PARTIAL_PAYMENT` | PARTIAL PAYMENT | Bước 09 | Thanh toán từng đợt |
| 111 | `WON` | Deal won | Bước 09 | Deal thành công |
| 113 | `LOSE` | Deal lost | Bất kỳ | Deal thất bại |
| 115 | `APOLOGY` | Analyze failure | Sau LOSE | Phân tích nguyên nhân |

---

## Trường thông tin bắt buộc theo từng bước

### Bước 05 — New Opportunity (`NEW`)

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
| `COMMENTS` | Ghi chú | **YES** | Tóm tắt BANT + brief nhu cầu | Xem [mẫu ghi chú](#mẫu-ghi-chú-comments-bước-05) |

> **Lưu ý:** `LEAD_ID` tự động liên kết khi convert Lead. `UF_CRM_REFERRER` phải copy thủ công từ Lead sang Deal.

### Bước 06 — Proposal & Solution (`PREPARATION`)

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ghi chú |
|-------------|-------------|----------|-----------|---------|
| `STAGE_ID` | Stage | **YES** | `PREPARATION` | Proposal & Solution |
| `OPPORTUNITY` | Giá trị dự kiến | **Cập nhật** | Điều chỉnh theo scope chốt | Chính xác hơn Bước 05 |
| `UF_CRM_SERVICE_TYPE` | Loại dịch vụ | **YES** | Chọn từ dropdown | Xác định rõ loại triển khai |
| `COMMENTS` | Ghi chú | **Cập nhật** | Bổ sung feedback + scope chốt | Xem [mẫu ghi chú](#mẫu-ghi-chú-comments-bước-06) |

> **Lưu ý:** Sau khi KH xác nhận phạm vi, `OPPORTUNITY` phải cập nhật chính xác — đây là cơ sở tạo báo giá.

### Bước 07 — Quotation (`PREPAYMENT_INVOICE`)

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ghi chú |
|-------------|-------------|----------|-----------|---------|
| `STAGE_ID` | Stage | **YES** | `PREPAYMENT_INVOICE` | Quotation |
| `OPPORTUNITY` | Giá trị Deal | **YES** | Số tiền chính xác theo báo giá | Phải khớp Estimate |
| `TAX_VALUE` | Thuế VAT | Nên có | 8-10% trên OPPORTUNITY | Auto nếu cấu hình |
| `UF_CRM_ESTIMATE` | Báo giá liên kết | **YES** | Link Estimate đã tạo | ID Estimate |
| `UF_CRM_BUDGET` | Ngân sách KH | Nên có | Ngân sách KH đề cập | Đối chiếu OPPORTUNITY |
| `COMMENTS` | Ghi chú | **Cập nhật** | Ngày gửi, hạn, ưu đãi | Xem [mẫu ghi chú](#mẫu-ghi-chú-comments-bước-07) |

> **Lưu ý:** `UF_CRM_ESTIMATE` liên kết Deal với Estimate để tra cứu báo giá.

### Bước 08 — Contract & Closing (`EXECUTING`)

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ví dụ |
|-------------|-------------|----------|-----------|-------|
| `STAGE_ID` | Stage | **YES** | `EXECUTING` | Contract & Closing |
| `UF_CRM_CONTRACT_NO` | Số hợp đồng | **YES** | Số HĐ chính thức | "HĐ-2026-001" |
| `UF_CRM_CONTRACT_DATE` | Ngày ký HĐ | **YES** | Ngày ký chính thức | "2026-02-15" |
| `UF_CRM_B24_PORTAL` | Portal Bitrix24 | **YES** | URL portal KH | "abc.bitrix24.vn" |
| `UF_CRM_LICENSE_KEY` | License Key | Nên có | Key bản quyền B24 | Sau khi mua license |
| `UF_CRM_PAYMENT_METHOD` | Phương thức TT | **YES** | Chọn từ dropdown | Chuyển khoản / Tiền mặt |
| `UF_CRM_CONTRACT_NUMBER_OF_PAYMENTS` | Số đợt TT | Nên có | Số đợt theo HĐ | "3" |
| `UF_CRM_PAYMENT_CYCLE` | Chu kỳ TT | Nên có | Chu kỳ thanh toán | Theo nghiệm thu |
| `CLOSEDATE` | Ngày dự kiến đóng | **Cập nhật** | Ngày dự kiến hoàn tất | Theo timeline triển khai |
| `COMMENTS` | Ghi chú | **Cập nhật** | Thông tin HĐ + email/domain | Xem [mẫu ghi chú](#mẫu-ghi-chú-comments-bước-08) |

> **Lưu ý:** `UF_CRM_CONTRACT_NO` và `UF_CRM_CONTRACT_DATE` phải điền trước khi chuyển sang Payment. `UF_CRM_B24_PORTAL` cần xác nhận từ KH qua eSign.

### Bước 09 — Payment (`FINAL_INVOICE` → `WON`)

#### Khi bắt đầu thu tiền:

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ghi chú |
|-------------|-------------|----------|-----------|---------|
| `STAGE_ID` | Stage | **YES** | `FINAL_INVOICE` | Payment |
| `UF_CRM_PAYMENT_METHOD` | Phương thức TT | **YES** | Đã có từ Bước 08 | |
| `COMMENTS` | Ghi chú | **Cập nhật** | Lịch sử thanh toán | Xem [mẫu ghi chú](#mẫu-ghi-chú-comments-bước-09) |

#### Khi Deal Won:

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ghi chú |
|-------------|-------------|----------|-----------|---------|
| `STAGE_ID` | Stage | **YES** | `WON` | Deal won |
| `CLOSED` | Đóng Deal | Auto | `Y` khi WON | |
| `CLOSEDATE` | Ngày đóng | Auto | Ngày chuyển WON | |
| `OPPORTUNITY` | Giá trị Deal | **Xác nhận** | Số tiền cuối cùng theo HĐ | Phải khớp tổng Invoice |

#### Khi Partial Payment:

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ghi chú |
|-------------|-------------|----------|-----------|---------|
| `STAGE_ID` | Stage | Tùy chọn | `PARTIAL_PAYMENT` | Nếu chưa TT hết |
| `UF_CRM_CONTRACT_AMOUNT_INSTALLMENT` | Số tiền từng đợt | Nên có | Danh sách số tiền | Theo lịch HĐ |
| `UF_CRM_CONTRACT_INSTALLMENT_DATE` | Ngày TT từng đợt | Nên có | Ngày hạn | Theo lịch HĐ |

> **Lưu ý:** Deal chỉ chuyển WON khi đợt 1 đã thanh toán — điều kiện bắt buộc trước Kick-off.

### Deal Lost (áp dụng ở bất kỳ bước nào)

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền |
|-------------|-------------|----------|-----------|
| `STAGE_ID` | Stage | **YES** | `LOSE` (Deal lost) |
| `UF_CRM_LOST_REASON` | Lý do Lost | **YES** | Chọn từ dropdown |
| `COMMENTS` | Ghi chú | **YES** | Chi tiết lý do, bài học |
| `UF_CRM_REFERRER` | Referrer | Kiểm tra | Nếu có → gửi email thông báo đối tác |

---

## UF Fields (Custom)

| Field | Type | Tên hiển thị | Mô tả |
|-------|------|-------------|-------|
| `UF_CRM_REFERRER` | crm (Contact) | Referrer (Partner) | Đối tác giới thiệu |
| `UF_CRM_LOST_REASON` | enumeration | Lost Reason | Lý do Deal lost |
| `UF_CRM_B24_PORTAL` | string | Portal Bitrix24 | URL portal KH |
| `UF_CRM_LICENSE_KEY` | string | License Key | Key bản quyền B24 |
| `UF_CRM_BUDGET` | string | Ngân sách | Ngân sách KH |
| `UF_CRM_ESTIMATE` | crm | Báo giá | Link Estimate |
| `UF_CRM_CONTRACT_NO` | string | Số hợp đồng | Số HĐ chính thức |
| `UF_CRM_CONTRACT_DATE` | date | Ngày ký HĐ | Ngày ký chính thức |
| `UF_CRM_SERVICE_TYPE` | enumeration | Loại dịch vụ | Loại triển khai |
| `UF_CRM_PAYMENT_METHOD` | enumeration | Phương thức TT | Chuyển khoản / Tiền mặt |
| `UF_CRM_CONTRACT_NUMBER_OF_PAYMENTS` | string | Số đợt TT | Số đợt thanh toán |
| `UF_CRM_PAYMENT_CYCLE` | string | Chu kỳ TT | Chu kỳ thanh toán |
| `UF_CRM_CONTRACT_AMOUNT_INSTALLMENT` | array | Số tiền từng đợt | Danh sách số tiền |
| `UF_CRM_CONTRACT_INSTALLMENT_DATE` | string | Ngày TT từng đợt | Ngày hạn TT |
| `UF_CRM_CARE_STATUS` | enumeration | Care Status | Trạng thái chăm sóc |

---

## Mẫu ghi chú COMMENTS

### Mẫu ghi chú COMMENTS Bước 05

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

### Mẫu ghi chú COMMENTS Bước 06

```
--- Proposal & Solution [DD/MM/YYYY] ---
Modules chốt: [Danh sách modules KH đồng ý]
Scope GĐ1: [Phạm vi giai đoạn 1]
Scope GĐ2: [Phạm vi giai đoạn 2, nếu có]
Số user: [Số lượng]
Timeline: [Thời gian triển khai dự kiến]

Feedback KH:
- [Feedback 1]
- [Feedback 2]

Điều chỉnh so với đề xuất ban đầu:
- [Thay đổi 1]

KH xác nhận phạm vi: [Có/Chưa] - [Ngày]
```

### Mẫu ghi chú COMMENTS Bước 07

```
--- Báo giá [DD/MM/YYYY] ---
Estimate #: [Số báo giá]
Ngày gửi: [DD/MM/YYYY]
Ngày hết hạn: [DD/MM/YYYY] (7 ngày)
Ưu đãi: [Mô tả ưu đãi + deadline]
Giá trị BG: [Số tiền] VND (chưa VAT)

Follow-up:
- Ngày 3: [Kết quả]
- Ngày 5: [Kết quả]
- Ngày 7: [Kết quả / Hết hạn]
```

### Mẫu ghi chú COMMENTS Bước 08

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

### Mẫu ghi chú COMMENTS Bước 09

```
--- Thanh toán ---
Đợt 1 (Bản quyền): [Số tiền] VND
  - Ngày gửi Invoice: [DD/MM/YYYY]
  - Ngày nhận tiền: [DD/MM/YYYY]
  - Hóa đơn VAT: #[Số] - [DD/MM/YYYY]

Đợt 2 (Triển khai GĐ1): [Số tiền] VND
  - Biên bản nghiệm thu: [DD/MM/YYYY]
  - Ngày gửi Invoice: [DD/MM/YYYY]
  - Ngày nhận tiền: [DD/MM/YYYY]
  - Hóa đơn VAT: #[Số] - [DD/MM/YYYY]

Tổng đã thu: [Số tiền] / [Tổng HĐ] VND
Còn lại: [Số tiền] VND

Bàn giao P. Triển khai: [DD/MM/YYYY]
```

---

## Lưu ý cho AI/Automation

1. **Tạo Deal từ Lead:** `LEAD_ID` auto liên kết, `UF_CRM_REFERRER` cần copy thủ công
2. **TITLE format chuẩn:** `[Công ty] - [Nhu cầu] - [MM/YYYY]` để dễ tìm kiếm
3. **OPPORTUNITY:** Cập nhật qua mỗi bước (ước tính → scope → báo giá → HĐ)
4. **Deal Lost:** Luôn kiểm tra `UF_CRM_REFERRER` để thông báo đối tác
5. **Deal Won:** Chỉ khi đợt 1 đã thanh toán, điều kiện trước Kick-off
6. **Khi có `UF_CRM_REFERRER`:** Gửi email thông báo đối tác tại Bước 07 (gửi BG + Deal Lost) và Bước 09 (thanh toán đợt 1)
