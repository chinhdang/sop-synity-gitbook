# Company — Trường thông tin CRM

> Trang này là **nguồn tham chiếu duy nhất** cho trường thông tin Company trong Bitrix24.
> Company được tạo tại Bước 02 (auto từ Google Form) và bổ sung thông tin qua các bước tiếp theo.

---

## Thông tin chung

| Thuộc tính | Giá trị |
|-----------|---------|
| CRM Entity | Company |
| API method | `crm.company.add`, `crm.company.update`, `crm.company.list` |
| Tạo khi nào | Bước 02 (auto từ Google Form) |

---

## Company fields

| Bitrix Field | Tên hiển thị | Bắt buộc | Nguồn dữ liệu | Bổ sung nếu thiếu |
|-------------|-------------|----------|---------------|-------------------|
| `TITLE` | Tên công ty | **YES** | Google Form | Hỏi KH |
| `COMPANY_TYPE` | Loại công ty | Nên có | Tự xác định | `CLIENT` |
| `INDUSTRY` | Ngành nghề | Nên có | Google Form / tra cứu | Chọn từ danh sách |
| `REVENUE` | Doanh thu | Không bắt buộc | Tra cứu | Nếu có |
| `PHONE` | SĐT công ty | Nên có | Google Form / masothue.com | |
| `EMAIL` | Email công ty | Nên có | Google Form | |
| `WEB` | Website | Nên có | Google Form / tra cứu | Tra Google |
| `ADDRESS` | Địa chỉ pháp lý | **YES** | masothue.com | Tra masothue.com |
| `BANKING_DETAILS` | Thông tin ngân hàng | Bước 08-09 | KH cung cấp | Khi ký HĐ |
| `COMMENTS` | Ghi chú | Nên có | Nhân sự bổ sung | Insight ngành, quy mô |

### Trường bổ sung từ masothue.com (Bước 02)

| Thông tin | Cách lấy | Lưu ở đâu |
|-----------|---------|-----------|
| Mã số thuế (MST) | masothue.com | `REG_NUMBER` hoặc `COMMENTS` |
| Địa chỉ đăng ký | masothue.com | `ADDRESS` |
| Người đại diện | masothue.com | `COMMENTS` |
| Ngày thành lập | masothue.com | `COMMENTS` |

> **Lưu ý:** MST và Địa chỉ pháp lý cần thiết cho bước Hợp đồng (Bước 08). Nếu thiếu → tra masothue.com và bổ sung ngay tại Bước 02.

---

---

## Requisites (Thông tin pháp lý)

> **Requisites KHÔNG phải field trên Company** — mà là sub-entity riêng gắn vào Company.
> Đây là nguồn dữ liệu chính cho Document Template (HĐ, Đề nghị TT).

```
Company
  └── Requisite (tên pháp lý, MST, người đại diện)
        ├── Address (địa chỉ pháp lý)
        └── Bank Detail (tài khoản ngân hàng)
```

### Trường bắt buộc trên Requisite (cho Document Template)

| Requisite Field | Mô tả | Template Variable |
|----------------|-------|-------------------|
| `RQ_COMPANY_NAME` | Tên pháp lý | `{CompanyRequisiteRqCompanyName}` |
| `RQ_VAT_ID` | Mã số thuế | `{CompanyRequisiteRqVatId}` |
| Address `ADDRESS_1` | Địa chỉ pháp lý | `{CompanyAddressLegal}` |

> **Hướng dẫn đầy đủ:** Xem [Requisites — Hướng dẫn nhập thông tin pháp lý](requisite-guide.md)

### Lưu ý về field Company vs Requisite

| Thông tin | Lưu ở Company field | Lưu ở Requisite |
|-----------|:---:|:---:|
| Tên thường gọi | `TITLE` ✅ | — |
| Tên pháp lý (cho HĐ) | — | `RQ_COMPANY_NAME` ✅ |
| MST | — | `RQ_VAT_ID` ✅ |
| Địa chỉ pháp lý (cho HĐ) | — | Address → `ADDRESS_1` ✅ |
| SĐT | `PHONE` ✅ | `RQ_PHONE` (tuỳ chọn) |
| Tài khoản NH | — | Bank Detail ✅ |

> `ADDRESS` và `BANKING_DETAILS` trên Company card thực tế được lưu qua Requisites, không phải trực tiếp trên Company entity.

---

## Lưu ý cho AI/Automation

1. **Auto-create từ Google Form:** Company tạo tự động khi form submit, liên kết với Contact + Lead
2. **MST bắt buộc cho HĐ:** Nếu Google Form không có MST, nhân sự phải tra masothue.com và bổ sung vào **Requisite** (không phải Company field)
3. **Liên kết:** Company luôn liên kết với Contact(s) và Lead/Deal
4. **Một Company có thể có nhiều Contact:** Người liên hệ, người QĐ, kế toán...
5. **Requisite ≠ Company field:** Tên pháp lý, MST, địa chỉ pháp lý lưu trong Requisite sub-entity. Xem [Requisite Guide](requisite-guide.md)
6. **Trước Bước 08:** Requisite phải có đủ `RQ_COMPANY_NAME` + `RQ_VAT_ID` + Address để Document Template hoạt động
