# Requisites — Hướng dẫn nhập thông tin pháp lý

> Trang này hướng dẫn cách nhập **Requisites** (thông tin pháp lý) cho Company/Contact trong Bitrix24.
> Requisites là **nguồn dữ liệu chính** cho Document Template (HĐ, Đề nghị TT) — nếu thiếu, tài liệu tự động sẽ bị lỗi.

---

## Requisites là gì?

Requisites là **sub-entity đặc biệt** trong Bitrix24, dùng để lưu trữ thông tin pháp lý của doanh nghiệp. Requisites **KHÔNG phải là field thông thường** trên Company — mà là một entity riêng gắn vào Company.

### Cấu trúc dữ liệu

```
Company (hoặc Contact)
  │
  └── Requisite                    ← Thông tin pháp lý (tên, MST, người đại diện)
        │
        ├── Address                ← Địa chỉ pháp lý, địa chỉ thực tế
        │
        └── Bank Detail            ← Thông tin tài khoản ngân hàng
```

**Quan trọng:**
- 1 Company có thể có nhiều Requisites (VD: trụ sở chính + chi nhánh)
- 1 Requisite có thể có nhiều Addresses (nhiều loại: pháp lý, thực tế)
- 1 Requisite có thể có nhiều Bank Details (nhiều tài khoản NH)

---

## Requisite Fields — Trường bắt buộc cho HĐ

> Các trường dưới đây **bắt buộc phải có** để Document Template (HĐ bản quyền, Đề nghị TT) hoạt động đúng.

### Company Requisite (KH)

| Requisite Field | Template Variable | Mô tả | Bắt buộc |
|----------------|-------------------|-------|----------|
| `RQ_COMPANY_NAME` | `{CompanyRequisiteRqCompanyName}` | Tên pháp lý công ty | **YES** |
| `RQ_VAT_ID` | `{CompanyRequisiteRqVatId}` | Mã số thuế | **YES** |

### Address (gắn vào Requisite)

| Address Field | Template Variable | Mô tả | Bắt buộc |
|--------------|-------------------|-------|----------|
| `ADDRESS_1` | `{CompanyAddressLegal}` | Địa chỉ pháp lý | **YES** |
| `CITY` | — | Thành phố | Nên có |
| `COUNTRY` | — | Quốc gia | Nên có |

### Trường bổ sung (nên có cho HĐ hoàn chỉnh)

| Requisite Field | Mô tả | Ghi chú |
|----------------|-------|---------|
| `RQ_COMPANY_FULL_NAME` | Tên đầy đủ (tiếng Anh) | Cho HĐ song ngữ |
| `RQ_DIRECTOR` | Giám đốc | Người ký HĐ phía KH |
| `RQ_CEO_NAME` | Tên người đứng đầu | |
| `RQ_CEO_WORK_POS` | Chức vụ người đứng đầu | |
| `RQ_COMPANY_REG_DATE` | Ngày đăng ký kinh doanh | |

> **Lưu ý:** Contact cũng cần có `NAME` + `POST` (chức vụ) — dùng cho `{ContactName}` và `{ContactsContactPost}` trong Document Template.

---

## SYNITY Requisite (My Company)

> Thông tin SYNITY đã được cấu hình sẵn, dùng làm **Bên A** trong tất cả HĐ.

| Thông tin | Giá trị | Requisite ID |
|-----------|---------|-------------|
| Tên pháp lý | CÔNG TY TNHH CHUYỂN ĐỔI SỐ SYNITY | 658 |
| MST | 0318972367 | 658 |
| Ngân hàng | MB Bank — CN Đông Sài Gòn | Bank Detail ID: 2 |
| Số tài khoản | 1788168168 | Bank Detail ID: 2 |

> **Khi tạo Smart Invoice:** Phải set `MC_REQUISITE_ID = 658` và `MC_BANK_DETAIL_ID = 2` trong Requisite Link để Document Template lấy đúng thông tin SYNITY.

---

## Requisite Link — Liên kết với Deal/Smart Invoice

Khi tạo Document Template cho Deal hoặc Smart Invoice, Bitrix cần biết **dùng Requisite nào** cho mỗi bên:

| Link Field | Mô tả | Bắt buộc |
|-----------|-------|----------|
| `REQUISITE_ID` | Requisite của KH (Bên B) | **YES** |
| `BANK_DETAIL_ID` | Bank Detail của KH | Nếu có |
| `MC_REQUISITE_ID` | Requisite của SYNITY (Bên A) | **YES** |
| `MC_BANK_DETAIL_ID` | Bank Detail của SYNITY | **YES** |

### Cách set Requisite Link

**Trên giao diện:** Khi tạo Smart Invoice hoặc Document, chọn Requisite cho cả 2 bên trong phần "Details" / "Requisites".

**Qua API:**
```
crm.requisite.link.register({
  fields: {
    ENTITY_TYPE_ID: 31,           // Smart Invoice
    ENTITY_ID: [ID],
    REQUISITE_ID: [KH Req ID],
    BANK_DETAIL_ID: 0,            // KH không cần bank detail
    MC_REQUISITE_ID: 658,         // SYNITY
    MC_BANK_DETAIL_ID: 2          // SYNITY MB Bank
  }
})
```

---

## Cách nhập Requisites (hướng dẫn nhân sự)

### Bước 1: Vào Company card

1. Mở Company trong CRM
2. Click tab **"Details"** (hoặc "Requisites" / "Chi tiết")
3. Nếu đã có Requisite → chỉnh sửa
4. Nếu chưa có → click **"Add"** → chọn template "Company"

### Bước 2: Điền thông tin bắt buộc

| Trường | Nguồn dữ liệu | Cách lấy |
|--------|---------------|---------|
| Company Name (tên pháp lý) | masothue.com | Tra theo MST hoặc tên công ty |
| Tax ID / VAT ID (MST) | masothue.com | Tra theo tên công ty |
| Address (địa chỉ pháp lý) | masothue.com | Lấy từ thông tin đăng ký |

### Bước 3: Kiểm tra

- [ ] `RQ_COMPANY_NAME` — đã điền đúng tên pháp lý?
- [ ] `RQ_VAT_ID` — đã điền MST?
- [ ] Address — đã thêm địa chỉ pháp lý?
- [ ] Contact liên kết — đã có tên + chức vụ?

> **Nguồn tra cứu:** [masothue.com](https://masothue.com) — nhập MST hoặc tên công ty để lấy đầy đủ thông tin pháp lý.

---

## Tình trạng dữ liệu hiện tại

> Cập nhật: 02/2026

| Chỉ số | Số lượng | Ghi chú |
|--------|---------|---------|
| Tổng Requisites | 53 | Company + Contact |
| Có `RQ_COMPANY_NAME` + `RQ_VAT_ID` | ~15 | Đủ cho Document Template |
| Chỉ có Address (rỗng) | ~38 | `ADDRESS_ONLY=Y`, cần bổ sung |
| Có Bank Detail (KH) | 1 | Không ảnh hưởng HĐ |

**Ưu tiên bổ sung:** Khi Deal chuyển đến giai đoạn Contract (Bước 08), nhân sự **phải kiểm tra và bổ sung** Requisite cho Company trước khi tạo Document Template.

---

## API Reference

| Method | Mô tả |
|--------|-------|
| `crm.requisite.list` | Danh sách Requisites |
| `crm.requisite.add` | Thêm Requisite cho Company/Contact |
| `crm.requisite.update` | Cập nhật Requisite |
| `crm.address.add` | Thêm Address cho Requisite |
| `crm.requisite.bankdetail.list` | Danh sách Bank Details |
| `crm.requisite.link.register` | Liên kết Requisite với Deal/Invoice |
| `crm.requisite.link.get` | Xem Requisite Link của Deal/Invoice |
| `crm.requisite.preset.list` | Danh sách templates |

> **Chi tiết API:** Xem [Bitrix24 REST API — Requisites](https://apidocs.bitrix24.com/api-reference/crm/requisites/)

---

## Liên kết

| Đi đến | Link |
|--------|------|
| Company Fields | [Link](company-fields.md) |
| Deal Fields — Document Templates | [Link](deal-fields.md#document-templates--biến-crm) |
| Bước 05 — Kiểm tra Requisite | [Link](../buoc/05-new-opportunity.md) |
| Bước 08 — Gate trước khi tạo HĐ | [Link](../buoc/08-contract-closing.md) |
| Trang chủ | [Link](../README.md) |
