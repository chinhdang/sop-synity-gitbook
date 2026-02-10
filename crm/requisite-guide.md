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

### Address (gắn vào Requisite) — Mapping cho Việt Nam

> Bitrix24 thiết kế cho hệ thống Mỹ/Âu (City → State). Việt Nam có 4 cấp hành chính khác biệt.
> Dưới đây là **quy chuẩn mapping** đã được xác nhận.

| Bitrix Field | Cấp hành chính VN | Bắt buộc | Ví dụ |
|---|---|---|---|
| `ADDRESS_1` | Số nhà + Đường | **YES** | `412 Nguyễn Thị Minh Khai` |
| `ADDRESS_2` | Toà nhà / Tầng / Phòng | Nếu có | `Tầng 14, Toà nhà HM Town` |
| `REGION` | Phường/Xã, Quận/Huyện | **YES** | `Phường Bàn Cờ, Quận 3` |
| `PROVINCE` | Tỉnh / Thành phố trực thuộc TW | **YES** | `Thành phố Hồ Chí Minh` |
| `CITY` | *(bỏ trống — không dùng cho VN)* | — | — |
| `COUNTRY` | Quốc gia (chuẩn hoá) | **YES** | `Việt Nam` |
| `POSTAL_CODE` | Mã bưu chính | Nếu có | `700000` |

**Lý do mapping:**
- `REGION` = Phường + Quận — gần nghĩa nhất, 2 cấp này luôn đi chung
- `PROVINCE` = Tỉnh/TP — khớp chính xác với "State/Province"
- `CITY` bỏ trống — VN không có cấp "City" tách biệt
- `COUNTRY` luôn = `Việt Nam` (có dấu, chuẩn hoá)

### 5 vai trò liên hệ phía KH (UF fields trên Requisite)

> HĐ dịch vụ triển khai yêu cầu thông tin liên hệ **5 vai trò** phía KH. Các trường này là UF custom fields trên Requisite entity, hiển thị trên form khi dùng preset **"SYNITY - Doanh nghiệp VN"** hoặc preset **"Company"**.

#### 1. Người đại diện pháp luật

| UF Field | Label | Bắt buộc | Dùng trong HĐ |
|----------|-------|----------|---------------|
| `UF_CRM_REP_NAME` | Họ tên Người đại diện | **YES** | "Ông/Bà [tên], đại diện cho [công ty]" |
| `UF_CRM_REP_POSITION` | Chức vụ | **YES** | "chức vụ [X]" |
| `UF_CRM_REP_EMAIL` | Email Người đại diện | Nên có | Gửi HĐ, thông báo |
| `UF_CRM_REP_PHONE` | SĐT Người đại diện | Nên có | Liên hệ ký kết |

#### 2. Quản lý dự án (QLDA)

| UF Field | Label | Bắt buộc | Mô tả |
|----------|-------|----------|-------|
| `UF_CRM_PM_NAME` | Họ tên QLDA | **YES** | Đầu mối chính phía KH |
| `UF_CRM_PM_EMAIL` | Email QLDA | **YES** | Gửi tài liệu, lịch họp |
| `UF_CRM_PM_PHONE` | SĐT QLDA | Nên có | Liên hệ trực tiếp |

#### 3. Kỹ thuật

| UF Field | Label | Bắt buộc | Mô tả |
|----------|-------|----------|-------|
| `UF_CRM_TECH_NAME` | Họ tên Kỹ thuật | Nên có | IT support phía KH |
| `UF_CRM_TECH_EMAIL` | Email Kỹ thuật | Nên có | Cấu hình domain, DNS |
| `UF_CRM_TECH_PHONE` | SĐT Kỹ thuật | Nên có | Hỗ trợ kỹ thuật |

#### 4. Người liên lạc

| UF Field | Label | Bắt buộc | Mô tả |
|----------|-------|----------|-------|
| `UF_CRM_LIAISON_NAME` | Họ tên Người liên lạc | Nên có | Đầu mối liên lạc hàng ngày |
| `UF_CRM_LIAISON_EMAIL` | Email Người liên lạc | Nên có | |
| `UF_CRM_LIAISON_PHONE` | SĐT Người liên lạc | Nên có | |

#### 5. Kế toán

| UF Field | Label | Bắt buộc | Mô tả |
|----------|-------|----------|-------|
| `UF_CRM_ACC_NAME` | Họ tên Kế toán | Nên có | Xử lý thanh toán |
| `UF_CRM_ACC_EMAIL` | Email Kế toán | Nên có | Gửi Đề nghị TT, Invoice |
| `UF_CRM_ACC_PHONE` | SĐT Kế toán | Nên có | |

> **Lưu ý:** Người đại diện có 4 fields (thêm Chức vụ) vì HĐ cần câu: *"Ông/Bà [tên], chức vụ [X], đại diện cho [công ty]"*.
> Các vai trò khác có 3 fields (Họ tên, Email, SĐT).

### Contact Role — Vai trò dự án trên Contact

> Ngoài Requisite UF fields (dùng cho Document Template), mỗi Contact còn có field **`UF_CRM_CONTACT_ROLE`** (enum, multiple) để đánh dấu vai trò trong dự án.

| Enum ID | Vai trò | XML_ID |
|---------|---------|--------|
| 1028 | Người đại diện pháp luật | REP |
| 1030 | Quản lý dự án (QLDA) | PM |
| 1032 | Kỹ thuật | TECH |
| 1034 | Người liên lạc | LIAISON |
| 1036 | Kế toán | ACC |

**Tại sao cần cả hai?**

| | Contact Role (`UF_CRM_CONTACT_ROLE`) | Requisite UF fields |
|---|---|---|
| **Mục đích** | CRM operations: liên lạc, giao việc, báo cáo | Document Template: HĐ, Đề nghị TT |
| **Searchable** | Có — filter tất cả QLDA trên portal | Không (flat text) |
| **Thay người** | Đổi Contact → tự nhiên | Phải sửa text field |
| **Multiple roles** | 1 Contact có thể có nhiều vai trò | N/A |
| **Deal-level** | Contact gắn Company, ảnh hưởng mọi Deal | Requisite gắn Company |

> **Quy trình:** Khi có Contact mới cho 1 vai trò → (1) gán `UF_CRM_CONTACT_ROLE` trên Contact, (2) copy thông tin sang Requisite UF fields khi chuẩn bị HĐ (Bước 08).

### Trường bổ sung (nên có cho HĐ hoàn chỉnh)

| Requisite Field | Mô tả | Ghi chú |
|----------------|-------|---------|
| `RQ_COMPANY_FULL_NAME` | Tên đầy đủ (tiếng Anh) | Cho HĐ song ngữ |
| `RQ_DIRECTOR` | Giám đốc | Built-in field (chỉ hiện ở preset RU/CIS) |
| `RQ_CEO_NAME` | Tên người đứng đầu | Built-in field (chỉ hiện ở preset RU/CIS) |
| `RQ_CEO_WORK_POS` | Chức vụ người đứng đầu | Built-in field (chỉ hiện ở preset RU/CIS) |
| `RQ_COMPANY_REG_DATE` | Ngày đăng ký kinh doanh | |

> **Lưu ý:** `RQ_DIRECTOR`, `RQ_CEO_NAME`, `RQ_CEO_WORK_POS` là country-specific fields (RU/CIS). Chúng tồn tại trong API nhưng **không hiển thị trên UI** khi dùng preset US. Thay vào đó, dùng UF fields `UF_CRM_REP_NAME` + `UF_CRM_REP_POSITION` ở trên.
>
> Contact cũng cần có `NAME` + `POST` (chức vụ) — dùng cho `{ContactName}` và `{ContactsContactPost}` trong Document Template.

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

## Requisite Presets — Templates

> Portal hiện có 2 presets hoạt động:

| Preset ID | Tên | Country | Dùng cho |
|-----------|-----|---------|----------|
| 1 | Company | US (122) | Preset mặc định, requisite cũ |
| 4 | **SYNITY - Doanh nghiệp VN** | US (122) | Preset mới, tối ưu cho HĐ triển khai |

> **Tại sao COUNTRY_ID = US?** Bitrix24 chỉ hỗ trợ 11 nước (RU, BY, KZ, UA, BR, DE, CO, PL, US, FR, UZ). Việt Nam **không có** trong danh sách. Dùng US vì preset US có ít fields nhất (5 fields cơ bản), UF custom fields hoạt động với mọi country.

### Preset 4: "SYNITY - Doanh nghiệp VN" — Thứ tự fields

| Sort | Field | Section |
|------|-------|---------|
| 100 | `RQ_COMPANY_NAME` | Thông tin công ty |
| 110 | `RQ_VAT_ID` | |
| 120 | `RQ_ADDR` | |
| 200 | `UF_CRM_REP_NAME` | Người đại diện pháp luật |
| 210 | `UF_CRM_REP_POSITION` | |
| 220 | `UF_CRM_REP_EMAIL` | |
| 230 | `UF_CRM_REP_PHONE` | |
| 300 | `UF_CRM_PM_NAME` | Quản lý dự án |
| 310 | `UF_CRM_PM_EMAIL` | |
| 320 | `UF_CRM_PM_PHONE` | |
| 400 | `UF_CRM_TECH_NAME` | Kỹ thuật |
| 410 | `UF_CRM_TECH_EMAIL` | |
| 420 | `UF_CRM_TECH_PHONE` | |
| 500 | `UF_CRM_LIAISON_NAME` | Người liên lạc |
| 510 | `UF_CRM_LIAISON_EMAIL` | |
| 520 | `UF_CRM_LIAISON_PHONE` | |
| 600 | `UF_CRM_ACC_NAME` | Kế toán |
| 610 | `UF_CRM_ACC_EMAIL` | |
| 620 | `UF_CRM_ACC_PHONE` | |
| 700 | `RQ_SIGNATURE` | Chữ ký & Con dấu |
| 710 | `RQ_STAMP` | |

> **Lưu ý:** 16 UF fields cũng đã được thêm vào Preset 1 (Company) để requisite cũ vẫn nhập được thông tin 5 vai trò. Bitrix24 không cho phép thay đổi PRESET_ID trên requisite đã tạo.

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

### Bước 2: Tra cứu thông tin pháp lý

> Có MST (từ Company UF `UF_CRM_COMPANY_1742604428244` hoặc KH cung cấp) → tra cứu để lấy tên pháp lý + địa chỉ đăng ký.

**Nguồn tra cứu:**

| Nguồn | Cách dùng | Ưu điểm |
|-------|-----------|---------|
| **VietQR API** | `GET https://api.vietqr.io/v2/business/{MST}` | Nhanh, trả JSON, dùng cho automation |
| [masothue.com](https://masothue.com) | Nhập MST hoặc tên công ty | Giao diện web, tra thủ công |

**Ví dụ tra VietQR API:**

```
GET https://api.vietqr.io/v2/business/3703174686

Response:
{
  "code": "00",
  "data": {
    "name": "CÔNG TY CỔ PHẦN GIEO MẦM XANH TOÀN CẦU - DOANH NGHIỆP XÃ HỘI",
    "shortName": "GIEO MẦM XANH TOÀN CẦU",
    "address": "Số 237, đường D8, Khu 1, Phường Bình Dương, TP Hồ Chí Minh"
  }
}
```

**Mapping kết quả → Requisite:**

| VietQR field | Requisite field |
|---|---|
| `data.name` | `RQ_COMPANY_NAME` |
| `{MST}` (input) | `RQ_VAT_ID` |
| `data.address` | Tách theo quy chuẩn VN (xem Bước 3) |

### Bước 3: Nhập Address theo quy chuẩn VN

Từ địa chỉ tra được (VietQR API hoặc masothue.com), tách thành các field:

**Ví dụ:** `Tầng 14, Toà nhà HM Town, 412 Nguyễn Thị Minh Khai, Phường Bàn Cờ, Quận 3, TP Hồ Chí Minh`

| Field | Điền | Cách tách |
|-------|------|----------|
| `ADDRESS_1` | `412 Nguyễn Thị Minh Khai` | Số nhà + Đường |
| `ADDRESS_2` | `Tầng 14, Toà nhà HM Town` | Toà nhà, tầng, phòng (nếu có) |
| `REGION` | `Phường Bàn Cờ, Quận 3` | Phường/Xã + Quận/Huyện |
| `PROVINCE` | `Thành phố Hồ Chí Minh` | Tỉnh/TP (viết đầy đủ) |
| `CITY` | *(bỏ trống)* | Không dùng cho VN |
| `COUNTRY` | `Việt Nam` | Luôn = "Việt Nam" |

### Bước 4: Nhập thông tin 5 vai trò (khi có)

> Không bắt buộc ở giai đoạn đầu. Bổ sung dần khi có thông tin, **bắt buộc hoàn chỉnh trước Bước 08 (HĐ Dịch vụ Triển khai).**

- [ ] **Người đại diện:** Họ tên + Chức vụ (bắt buộc cho HĐ) + Email + SĐT
- [ ] **QLDA:** Họ tên + Email (bắt buộc) + SĐT
- [ ] **Kỹ thuật:** Họ tên + Email + SĐT (khi có)
- [ ] **Người liên lạc:** Họ tên + Email + SĐT (khi có)
- [ ] **Kế toán:** Họ tên + Email + SĐT (khi có)

### Bước 5: Kiểm tra

- [ ] `RQ_COMPANY_NAME` — đã điền đúng tên pháp lý?
- [ ] `RQ_VAT_ID` — đã điền MST?
- [ ] `ADDRESS_1` — có số nhà + đường?
- [ ] `REGION` — có Phường + Quận?
- [ ] `PROVINCE` — có Tỉnh/TP?
- [ ] `COUNTRY` — = "Việt Nam"?
- [ ] `UF_CRM_REP_NAME` + `UF_CRM_REP_POSITION` — Người đại diện?
- [ ] Contact liên kết — đã có tên + chức vụ?

> **Nguồn tra cứu:** [VietQR API](https://api.vietqr.io/v2/business/{MST}) (nhanh, JSON) hoặc [masothue.com](https://masothue.com) (thủ công). Xem chi tiết ở Bước 2.

---

## Tình trạng dữ liệu hiện tại

> Cập nhật: 02/2026

| Chỉ số | Số lượng | Ghi chú |
|--------|---------|---------|
| Tổng Requisites | 53 | Company + Contact |
| Có `RQ_COMPANY_NAME` + `RQ_VAT_ID` | ~15 | Đủ cho Document Template |
| Chỉ có Address (rỗng) | ~38 | `ADDRESS_ONLY=Y`, cần bổ sung |
| Có Bank Detail (KH) | 1 | Không ảnh hưởng HĐ |
| **Address chuẩn VN** | **15/15** | **Tất cả address đã chuẩn hoá theo mapping VN** |

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
| `crm.requisite.preset.field.list` | Fields trong 1 preset |
| `crm.requisite.userfield.list` | Danh sách UF fields trên Requisite |

### API ngoài — Tra cứu MST

| Method | Endpoint | Mô tả |
|--------|----------|-------|
| `GET` | `https://api.vietqr.io/v2/business/{MST}` | Tra cứu tên pháp lý + địa chỉ từ MST |

**Response:** `data.name` (tên pháp lý), `data.shortName` (tên viết tắt), `data.address` (địa chỉ đăng ký).

**Lưu ý:** Địa chỉ trả về là chuỗi text — cần tách thủ công theo quy chuẩn VN (ADDRESS_1, REGION, PROVINCE). Rate limit: 429 nếu gọi quá nhiều.

> **Chi tiết API:** Xem [Bitrix24 REST API — Requisites](https://apidocs.bitrix24.com/api-reference/crm/requisites/) | [VietQR Tax ID Lookup](https://www.vietqr.io/en/danh-sach-api/tax-id-lookup)

---

## Liên kết

| Đi đến | Link |
|--------|------|
| Company Fields | [Link](company-fields.md) |
| Deal Fields — Document Templates | [Link](deal-fields.md#document-templates--biến-crm) |
| Bước 05 — Kiểm tra Requisite | [Link](../buoc/05-new-opportunity.md) |
| Bước 08 — Gate trước khi tạo HĐ | [Link](../buoc/08-contract-closing.md) |
| Trang chủ | [Link](../README.md) |
