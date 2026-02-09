# Lead — Trường thông tin CRM

> Trang này là **nguồn tham chiếu duy nhất** cho tất cả trường thông tin Lead trong Bitrix24.
> Nhân sự và AI/Automation tra cứu tại đây khi tạo hoặc cập nhật Lead.

---

## Thông tin chung

| Thuộc tính | Giá trị |
|-----------|---------|
| CRM Entity | Lead |
| API method | `crm.lead.add`, `crm.lead.update`, `crm.lead.list` |
| Pipeline | Lead Pipeline (STATUS entity) |
| Stages | NEW → IN_PROCESS → PROCESSED → FOLLOW_UP → CONVERTED / JUNK |

---

## Lead Stages

| ID | STATUS_ID | Tên hiển thị | SOP Step | Ghi chú |
|----|-----------|-------------|----------|---------|
| 1 | `NEW` | New Lead | Bước 01 | Tạo Lead mới |
| 3 | `IN_PROCESS` | Submitted Form | Bước 02 | KH đã submit Google Form |
| 5 | `PROCESSED` | Book Meeting | Bước 03 | Đã đặt lịch meeting |
| 558 | `FOLLOW_UP` | Follow-up | Bước 04 | Cần follow-up thêm (~10%) |
| 7 | `CONVERTED` | Good Lead | Bước 04 | Qualified → tạo Deal |
| 9 | `JUNK` | Junk Lead | Bước 04 | Không qualified → đóng |

---

## Trường thông tin bắt buộc theo từng bước

### Bước 01 — New Lead (`NEW`)

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ví dụ |
|-------------|-------------|----------|-----------|-------|
| `TITLE` | Tiêu đề Lead | **YES** | `[Tên KH/Cty] - [Nguồn]` | "Nguyễn Văn A - Facebook" |
| `HONORIFIC` | Danh xưng | **YES** | Anh / Chị / Mr / Mrs | "Anh" |
| `NAME` | Tên | **YES** | Tên người liên hệ | "Văn A" |
| `POST` | Chức vụ | **YES** | Vai trò trong công ty | "Giám đốc", "Trưởng phòng IT" |
| `PHONE` | SĐT | **YES** | SĐT chính (dùng match Google Form) | "0901234567" |
| `COMPANY_TITLE` | Tên công ty | **YES** | Tên đầy đủ công ty | "Công ty ABC" |
| `SOURCE_ID` | Nguồn đến | **YES** | Chọn từ danh sách | `FACEBOOK` / `RECOMMENDATION` / `WEB` / `OTHER` |
| `ASSIGNED_BY_ID` | Người phụ trách | **YES** | Chọn nhân sự P. Chuyển đổi | ID nhân viên |
| `UF_CRM_REFERRER` | Referrer (Partner) | Nếu từ đối tác | Chọn Contact đối tác giới thiệu | Contact: "Trần Văn B" |
| `SOURCE_DESCRIPTION` | Chi tiết nguồn | Nên có | Mô tả kênh cụ thể | "FB group Bitrix24 VN" |
| `LAST_NAME` | Họ | Không bắt buộc | Họ người liên hệ | "Nguyễn" |
| `COMMENTS` | Ghi chú | Nên có | Kênh cụ thể, context | "KH inbox qua FB cá nhân Chinh" |
| `STATUS_ID` | Stage | **Auto** | Mặc định khi tạo mới | `NEW` (New Lead) |

> **Lưu ý:** `PHONE` là field quan trọng nhất vì dùng để match với Google Form ở Bước 02.

### Bước 02 — Survey (`IN_PROCESS`)

| Bitrix Field | Tên hiển thị | Bắt buộc | Nguồn dữ liệu | Ghi chú |
|-------------|-------------|----------|---------------|---------|
| `EMAIL` | Email | **YES** | Google Form (auto) | Kiểm tra đã cập nhật |
| `COMPANY_TITLE` | Tên công ty | **YES** | Google Form (auto) | Kiểm tra đã cập nhật |
| `COMPANY_ID` | Link Company | **YES** | Auto tạo từ form | Kiểm tra liên kết |
| `CONTACT_ID` | Link Contact | **YES** | Auto tạo từ form | Kiểm tra liên kết |
| `POST` | Chức vụ | **YES** | Google Form | Cập nhật nếu form có |
| `COMMENTS` | Ghi chú | **YES** | Nhân sự review | Ghi BANT sơ bộ + insight |
| `OPPORTUNITY` | Giá trị ước tính | Nên có | Đánh giá BANT | Nếu KH đề cập budget |
| `STATUS_ID` | Stage | **Auto** | Automation | `IN_PROCESS` (Submitted Form) |

> **Lưu ý:** Sau khi Google Form submit, automation match theo `PHONE` để cập nhật Lead. Nếu match thất bại, nhân sự cần cập nhật thủ công và báo IT.

### Bước 03 — Meeting (`PROCESSED`)

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ghi chú |
|-------------|-------------|----------|-----------|---------|
| `STATUS_ID` | Stage | **YES** | `PROCESSED` | Book Meeting |
| `OPPORTUNITY` | Giá trị ước tính | Nên có | Số tiền VND từ BANT | vd: 50000000 |
| `COMMENTS` | Ghi chú | **Cập nhật** | Bổ sung meeting notes | Xem [mẫu ghi chú](#mẫu-ghi-chú-comments-bước-03) |

### Bước 04 — Qualification (`CONVERTED` / `JUNK`)

#### Khi Good Lead:

| Bitrix Field | Tên hiển thị | Bắt buộc | Giá trị |
|-------------|-------------|----------|---------|
| `STATUS_ID` | Stage | **YES** | `CONVERTED` (Good Lead) |

#### Khi Junk Lead:

| Bitrix Field | Tên hiển thị | Bắt buộc | Giá trị |
|-------------|-------------|----------|---------|
| `STATUS_ID` | Stage | **YES** | `JUNK` (Junk Lead) |
| `UF_CRM_JUNK_REASON` | Lý do Junk | **YES** | Chọn từ dropdown (xem bảng dưới) |
| `COMMENTS` | Ghi chú | Nếu "Khác" | Bổ sung chi tiết lý do |

---

## UF Fields (Custom)

| Field | Type | Tên hiển thị | Mô tả |
|-------|------|-------------|-------|
| `UF_CRM_REFERRER` (ID=1470) | crm (Contact) | Referrer (Partner) | Link Contact đối tác giới thiệu |
| `UF_CRM_JUNK_REASON` (ID=1472) | enumeration | Junk Reason | Lý do đóng Junk Lead |

### Dropdown: Junk Reason (`UF_CRM_JUNK_REASON`)

| XML_ID | Giá trị hiển thị | Khi nào chọn |
|--------|-----------------|-------------|
| `NO_BUDGET` | Khong du ngan sach | KH không có ngân sách hoặc quá thấp |
| `NOT_DECISION_MAKER` | Khong phai nguoi quyet dinh | Không tiếp cận được người ra quyết định |
| `NEED_NOT_FIT` | Nhu cau khong phu hop | Nhu cầu không phù hợp giải pháp SYNITY |
| `NO_RESPONSE` | Khong phan hoi (auto 14 ngay) | KH không phản hồi, auto đóng sau 14 ngày |
| `OTHER` | Khac | Lý do khác — **bắt buộc ghi chú bổ sung** |

---

## Tất cả fields có trong Lead

| Nhóm | Fields |
|------|--------|
| **ID & Title** | `ID`, `TITLE` |
| **Thông tin cá nhân** | `HONORIFIC`, `NAME`, `LAST_NAME`, `SECOND_NAME`, `POST`, `BIRTHDATE` |
| **Liên hệ** | `PHONE` (multi), `EMAIL` (multi) |
| **Công ty** | `COMPANY_ID`, `COMPANY_TITLE`, `CONTACT_ID` |
| **Stage** | `STATUS_ID`, `STATUS_SEMANTIC_ID`, `STATUS_DESCRIPTION` |
| **Nguồn** | `SOURCE_ID`, `SOURCE_DESCRIPTION`, `ORIGINATOR_ID`, `ORIGIN_ID` |
| **Giá trị** | `OPPORTUNITY`, `CURRENCY_ID`, `IS_MANUAL_OPPORTUNITY` |
| **Địa chỉ** | `ADDRESS`, `ADDRESS_2`, `ADDRESS_CITY`, `ADDRESS_COUNTRY`, `ADDRESS_COUNTRY_CODE`, `ADDRESS_POSTAL_CODE`, `ADDRESS_PROVINCE`, `ADDRESS_REGION` |
| **UTM** | `UTM_SOURCE`, `UTM_MEDIUM`, `UTM_CAMPAIGN`, `UTM_CONTENT`, `UTM_TERM` |
| **Hoạt động** | `LAST_ACTIVITY_BY`, `LAST_ACTIVITY_TIME`, `LAST_COMMUNICATION_TIME` |
| **Hệ thống** | `ASSIGNED_BY_ID`, `CREATED_BY_ID`, `MODIFY_BY_ID`, `MOVED_BY_ID`, `DATE_CREATE`, `DATE_MODIFY`, `DATE_CLOSED`, `MOVED_TIME`, `OPENED`, `IS_RETURN_CUSTOMER`, `HAS_EMAIL`, `HAS_PHONE`, `HAS_IMOL`, `COMMENTS` |
| **Custom (UF)** | `UF_CRM_REFERRER`, `UF_CRM_JUNK_REASON` |

---

## Mẫu ghi chú COMMENTS

### Mẫu ghi chú COMMENTS Bước 03

```
--- Meeting [DD/MM/YYYY] ---
Người QĐ: [Tên] - [Chức vụ]
Người chi tiền: [Tên] - [Chức vụ] (nếu khác người QĐ)

BANT chi tiết:
- Budget: [Có/Chưa có] - [Ghi chú]
- Authority: [Người QĐ có mặt? Quy trình approve?]
- Need: [Nhu cầu chính phù hợp giải pháp SYNITY?]
- Timeline: [Khi nào muốn triển khai?]

Next Actions:
1. [Hành động] - [Ai] - [Deadline]
2. [Hành động] - [Ai] - [Deadline]

Đánh giá sơ bộ: [Qualified / Cần follow-up / Không qualified]
```

---

## Lưu ý cho AI/Automation

1. **Tạo Lead qua API:** Các field bắt buộc (YES) phải có giá trị, đặc biệt `PHONE`
2. **Match Google Form:** Automation match theo `PHONE` → cập nhật Lead → tạo Contact + Company
3. **Auto Junk (SLA 14 ngày):** Set `UF_CRM_JUNK_REASON` = `NO_RESPONSE`
4. **Khi có `UF_CRM_REFERRER`:** Gửi email thông báo đối tác tại các touchpoint (Bước 03, 04)
5. **Convert Lead → Deal:** `UF_CRM_REFERRER` cần copy thủ công sang Deal
