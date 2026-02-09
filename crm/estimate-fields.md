# Estimate (Báo giá) — Trường thông tin CRM

> Trang này là **nguồn tham chiếu duy nhất** cho trường thông tin Estimate trong Bitrix24.
> Estimate được tạo tại Bước 07 (Quotation) từ Deal. Mỗi Estimate = 1 phiên bản báo giá.

---

## Thông tin chung

| Thuộc tính | Giá trị |
|-----------|---------|
| CRM Entity | Quote (Estimate) |
| API method | `crm.quote.add`, `crm.quote.update`, `crm.quote.list` |
| Product rows | `crm.quote.productrows.set`, `crm.quote.productrows.get` |
| Tạo khi nào | Bước 07 — Từ Deal, khi gửi báo giá cho KH |
| Liên kết | 1 Deal có thể có nhiều Estimates (multi-round negotiation) |

---

## Estimate Stages

| Sort | STATUS_ID | Tên hiển thị | System | Semantics | Khi nào |
|------|-----------|-------------|--------|-----------|---------|
| 10 | `DRAFT` | New | Y | process | Vừa tạo, chưa hoàn thiện |
| 20 | `SENT` | Sent to client | N | process | Đã gửi file báo giá cho KH |
| 30 | `NEGOTIATE` | Negotiation | N | process | Đang đàm phán giá / scope |
| 40 | `PENDING_APPROVAL` | Chờ duyệt/ký | N | process | Chờ nội bộ duyệt (Chinh approve) |
| 60 | `APPROVED` | Đã chấp nhận | Y | **success** | KH đồng ý → auto copy Products vào Deal |
| 70 | `DECLAINED` | Declined | Y | **failure** | KH từ chối → tạo Estimate mới hoặc Deal Lost |
| 80 | `APOLOGY` | Analyze decline | N | failure | Phân tích lý do KH từ chối |

### Estimate Flow

```
DRAFT ──► SENT ──► Negotiation ──► Chờ duyệt/ký ──► APPROVED (success)
                       │                                  │
                       │                                  ▼
                       │                     Auto copy Products → Deal
                       │
                       └──► DECLINED (failure)
                               │
                               ├──► Tạo Estimate MỚI (tiếp tục đàm phán)
                               └──► APOLOGY (phân tích lý do, nếu cần)
```

### Quy tắc quan trọng

1. **1 Estimate = 1 phiên bản báo giá.** KHÔNG sửa Estimate đã gửi — tạo mới.
2. **Chỉ 1 Estimate APPROVED per Deal** (phiên bản cuối cùng KH đồng ý).
3. **Khi Estimate → APPROVED:** Automation copy Products vào Deal Products.
4. **Khi Deal Lost:** Close TẤT CẢ Estimate đang mở → `DECLAINED`.

---

## Tất cả fields của Estimate

### Thông tin chính

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Ví dụ |
|-------------|-------------|----------|-------|-------|
| `TITLE` | Tiêu đề | **YES** | Auto format: `EST.[ID].[STT] - [Tên công ty]` | "EST.94.25 - HDV Academy" |
| `QUOTE_NUMBER` | Mã báo giá | **Auto (RO)** | Hệ thống tự sinh, dùng cho file Excel | `EST.94.25` |
| `STATUS_ID` | Stage | **YES** | Trạng thái Estimate | `DRAFT`, `SENT`, `APPROVED`... |
| `DEAL_ID` | Deal liên kết | **YES** | Deal đang gửi báo giá | ID Deal |
| `OPPORTUNITY` | Tổng giá trị | Auto | Tổng từ Product rows | `217,208,758` |
| `CURRENCY_ID` | Tiền tệ | Auto | Mặc định | `VND` |
| `TAX_VALUE` | Thuế | Auto | Tính từ Products | VAT 8% |

### Thông tin KH (auto từ Deal/Contact/Company)

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Nguồn |
|-------------|-------------|----------|-------|-------|
| `COMPANY_ID` | Công ty | **YES** | Công ty KH | Từ Deal |
| `CONTACT_ID` | Liên hệ | **YES** | Người liên hệ chính | Từ Deal |
| `MYCOMPANY_ID` | Công ty bán | **YES** | SYNITY (hoặc entity pháp lý) | Chọn My Company |
| `CLIENT_TITLE` | Tên KH | Auto | Tên công ty hiển thị | Từ Company |
| `CLIENT_ADDR` | Địa chỉ | Auto | Địa chỉ pháp lý | Từ Company |
| `CLIENT_CONTACT` | Người LH | Auto | Tên người liên hệ | Từ Contact |
| `CLIENT_EMAIL` | Email | Auto | Email KH | Từ Contact |
| `CLIENT_PHONE` | SĐT | Auto | SĐT KH | Từ Contact |
| `CLIENT_TP_ID` | MST | Auto | Mã số thuế | Từ Company Requisites |
| `PERSON_TYPE_ID` | Loại KH | Auto | `2` = Tổ chức | Mặc định |

### Thời hạn

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Ví dụ |
|-------------|-------------|----------|-------|-------|
| `BEGINDATE` | Ngày tạo | Auto | Ngày tạo Estimate | Auto |
| `CLOSEDATE` | Ngày hết hạn | **YES** | Hiệu lực báo giá (7 ngày) | `BEGINDATE` + 7 |
| `ACTUAL_DATE` | Ngày thực tế | Nên có | Ngày close thực tế | Ngày KH phản hồi |

### Nội dung & Ghi chú

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả |
|-------------|-------------|----------|-------|
| `CONTENT` | Nội dung | Không | Mô tả nội dung báo giá |
| `TERMS` | Điều khoản | Nên có | Điều khoản thanh toán, bảo hành |
| `COMMENTS` | Ghi chú | Nên có | Ghi chú nội bộ |

### Phân công

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả |
|-------------|-------------|----------|-------|
| `ASSIGNED_BY_ID` | Người phụ trách | **YES** | Nhân sự quản lý Estimate |
| `OPENED` | Mở cho mọi người | Auto | `Y` mặc định |
| `CLOSED` | Đã đóng | Auto | `Y` khi APPROVED hoặc DECLAINED |

---

## UF Fields (Custom)

| ID | Field | Type | Tên hiển thị | Mô tả |
|----|-------|------|-------------|-------|
| 926 | `UF_CRM_QUOTE_NUMBER_OF_PAYMENTS` | integer | Số đợt thanh toán | Bao nhiêu đợt TT |
| 928 | `UF_CRM_QUOTE_PAYMENT_CYCLE` | enumeration | Chu kỳ thanh toán | Chu kỳ giữa các đợt |
| 992 | `UF_CRM_QUOTE_PAID_PER_INSTALLMENT` | money (multiple) | Số tiền mỗi đợt | Danh sách tiền từng đợt |

### Dropdown: Chu kỳ thanh toán (`UF_CRM_QUOTE_PAYMENT_CYCLE`)

| ID | Giá trị | Khi nào dùng |
|----|---------|-------------|
| 656 | Hàng tháng | KH thanh toán monthly |
| 658 | 2 Tháng | Mỗi 2 tháng |
| 660 | 3 tháng | Mỗi quý (3 tháng) |
| 662 | Hàng Quý | Thanh toán theo quý |
| 744 | Không | Thanh toán 1 lần hoặc theo nghiệm thu |

---

## Product Rows

> Products trong Estimate là danh sách sản phẩm/dịch vụ báo giá. Khi Estimate → APPROVED, automation copy products vào Deal.

### Các field quan trọng của mỗi Product Row

| Field | Mô tả | Ví dụ |
|-------|-------|-------|
| `PRODUCT_ID` | ID sản phẩm từ Catalog | `168` |
| `PRODUCT_NAME` | Tên sản phẩm | "Bitrix24 Professional (12-Month)" |
| `PRICE` | Giá sau discount + thuế | `59,861,592` |
| `PRICE_NETTO` | Giá gốc (chưa discount, chưa thuế) | `79,182,000` |
| `QUANTITY` | Số lượng | `1` |
| `DISCOUNT_TYPE_ID` | Loại discount | `1` = số tiền, `2` = phần trăm |
| `DISCOUNT_RATE` | Tỷ lệ discount | `30` (30%) |
| `TAX_RATE` | Thuế suất | `8` (VAT 8%) |
| `TAX_INCLUDED` | Thuế đã bao gồm | `Y` / `N` |
| `MEASURE_NAME` | Đơn vị | "Licence", "month", "packpage" |

### Cách tạo qua API

```json
// crm.quote.productrows.set
{
  "id": 94,
  "rows": [
    {
      "PRODUCT_ID": 168,
      "PRICE": 79182000,
      "QUANTITY": 1,
      "DISCOUNT_TYPE_ID": 2,
      "DISCOUNT_RATE": 30,
      "TAX_RATE": 8,
      "TAX_INCLUDED": "N"
    },
    {
      "PRODUCT_ID": 396,
      "PRICE": 18900000,
      "QUANTITY": 1,
      "TAX_RATE": 8,
      "TAX_INCLUDED": "N"
    }
  ]
}
```

---

## Quy trình tạo file Excel báo giá

> File Excel báo giá được tạo ngoài Bitrix, kết hợp dữ liệu từ Estimate + Deal + Contact + Company.

### Dữ liệu cần lấy

| Thông tin | Nguồn CRM | API |
|-----------|-----------|-----|
| Mã báo giá (`QUOTE_NUMBER`) | Estimate | `crm.quote.get` |
| Tên công ty, địa chỉ, MST | Company + Requisites | `crm.company.get`, `crm.requisite.list` |
| Người liên hệ, chức vụ, email | Contact | `crm.contact.get` |
| Products (tên, giá, SL, discount, thuế) | Estimate Product Rows | `crm.quote.productrows.get` |
| Tổng giá trị, thuế | Estimate | `crm.quote.get` → `OPPORTUNITY`, `TAX_VALUE` |
| Điều khoản thanh toán | Estimate UF fields | `UF_CRM_QUOTE_NUMBER_OF_PAYMENTS`, `UF_CRM_QUOTE_PAYMENT_CYCLE` |

### Quy trình

```
1. Tạo Estimate trong Bitrix → có QUOTE_NUMBER
2. Lấy data từ Estimate + Contact + Company
3. Điền vào template Excel chuẩn (Drive)
4. Ghi QUOTE_NUMBER lên file Excel
5. Gửi file Excel cho KH
6. Cập nhật Estimate stage → SENT
```

---

## Mẫu ghi chú COMMENTS

### Ghi chú trong Estimate

```
Phiên bản: [1/2/3...]
Thay đổi so với bản trước: [Mô tả thay đổi]
Ưu đãi: [Mô tả ưu đãi + deadline]
Ghi chú đàm phán: [Feedback KH]
```

### Ghi chú trong Deal (lịch sử Estimate)

```
--- Lịch sử báo giá ---
EST.90.23: Declined (03/02/2026) - KH yêu cầu giảm scope
EST.94.25: Approved (07/02/2026) - KH đồng ý sau khi điều chỉnh module
```

---

## Lưu ý cho AI/Automation

1. **Tạo Estimate:** Luôn tạo từ Deal (`DEAL_ID` bắt buộc). `QUOTE_NUMBER` auto sinh.
2. **Products:** Thêm từ Product Catalog, áp dụng giá nền cố định, discount theo chính sách.
3. **Multi-Estimate:** 1 Deal có thể có nhiều Estimates. Close Estimate cũ (`DECLAINED`) trước khi tạo mới.
4. **APPROVED trigger:** Khi Estimate → `APPROVED`, automation rule copy Products vào Deal. Chỉ 1 Estimate được APPROVED.
5. **File Excel:** `QUOTE_NUMBER` là mã định danh chính — dùng để đặt tên file và đối chiếu.
6. **Deal Lost:** Close tất cả Estimate đang mở → `DECLAINED` trước khi chuyển Deal → `LOSE`.
7. **MYCOMPANY_ID:** Chọn đúng entity pháp lý của SYNITY (ID=12 cho SYNITY chính, ID=50 nếu có entity khác).
8. **CLIENT_* fields:** Auto fill từ Company/Contact khi tạo Estimate. Kiểm tra MST (`CLIENT_TP_ID`) có đúng không.
