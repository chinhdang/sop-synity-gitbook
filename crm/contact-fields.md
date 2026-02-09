# Contact — Trường thông tin CRM

> Trang này là **nguồn tham chiếu duy nhất** cho trường thông tin Contact trong Bitrix24.
> Contact được tạo tại Bước 02 (auto từ Google Form) hoặc thủ công tại Bước 01 (Contact đối tác giới thiệu).

---

## Thông tin chung

| Thuộc tính | Giá trị |
|-----------|---------|
| CRM Entity | Contact |
| API method | `crm.contact.add`, `crm.contact.update`, `crm.contact.list` |
| Tạo khi nào | Bước 01 (Referer) hoặc Bước 02 (KH, auto từ Google Form) |

---

## Tất cả fields của Contact

### Thông tin cá nhân

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Ví dụ |
|-------------|-------------|----------|-------|-------|
| `HONORIFIC` | Danh xưng | **YES** | Chọn từ danh sách (xem bảng dưới) | `HNR_EN_1` (Ông) |
| `NAME` | Tên | **YES** | Tên người liên hệ | "Văn A" |
| `LAST_NAME` | Họ | Không bắt buộc | Họ người liên hệ | "Nguyễn" |
| `SECOND_NAME` | Tên đệm | Không | Tên đệm | "Văn" |
| `POST` | Chức vụ | **YES** | Vai trò trong công ty | "Giám đốc", "Trưởng phòng IT" |
| `BIRTHDATE` | Ngày sinh | Không | Ngày sinh | "1990-05-15" |
| `PHOTO` | Ảnh đại diện | Không | Upload file ảnh | File ID |

#### Giá trị HONORIFIC

| STATUS_ID | Tên hiển thị |
|-----------|-------------|
| `HNR_EN_1` | Ông |
| `HNR_EN_2` | Mrs. |
| `HNR_EN_3` | Ms. |
| `HNR_EN_4` | Dr. |

### Liên hệ (Multi-value fields)

> Các field này là **mảng (array)**, mỗi giá trị có `VALUE_TYPE` phân loại.

| Bitrix Field | Tên hiển thị | Bắt buộc | VALUE_TYPE phổ biến | Ví dụ |
|-------------|-------------|----------|-------------------|-------|
| `PHONE` | Số điện thoại | **YES** | `WORK`, `MOBILE`, `HOME` | `+84901234567` |
| `EMAIL` | Email | **YES** | `WORK`, `HOME` | `name@company.com` |
| `WEB` | Website / Link | Nên có | `WORK`, `HOME`, `FACEBOOK` | `https://company.com` |
| `IM` | Kênh chat | Nên có | `TELEGRAM`, `FACEBOOK`, `OPENLINE`, `BITRIX24` | Auto từ Open Channel |

**Cách tạo qua API:**
```json
{
  "PHONE": [
    {"VALUE": "+84901234567", "VALUE_TYPE": "WORK"}
  ],
  "EMAIL": [
    {"VALUE": "name@company.com", "VALUE_TYPE": "WORK"},
    {"VALUE": "personal@gmail.com", "VALUE_TYPE": "HOME"}
  ]
}
```

### Công ty & Liên kết

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Ví dụ |
|-------------|-------------|----------|-------|-------|
| `COMPANY_ID` | Công ty | **YES** | Link đến Company entity | ID Company |
| `TYPE_ID` | Loại Contact | **YES** | Phân loại Contact | Xem bảng dưới |
| `LEAD_ID` | Lead gốc | Auto | Lead đã tạo Contact này | Auto |

#### Giá trị TYPE_ID

| STATUS_ID | Tên hiển thị | Khi nào dùng |
|-----------|-------------|-------------|
| `CLIENT` | Clients | KH mua hàng — **mặc định cho KH** |
| `SUPPLIER` | Suppliers | Nhà cung cấp |
| `PARTNER` | Partners | **Đối tác giới thiệu (Referer)** |
| `EMPLOYEE` | Employee | Nhân viên |
| `OTHER` | Other | Khác |

### Nguồn & Tracking

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Ví dụ |
|-------------|-------------|----------|-------|-------|
| `SOURCE_ID` | Nguồn | **YES** | Kênh tiếp cận | `RECOMMENDATION`, `FACEBOOK`, `WEB`... |
| `SOURCE_DESCRIPTION` | Chi tiết nguồn | Nên có | Mô tả kênh cụ thể | "FB group Bitrix24 VN" |
| `ORIGINATOR_ID` | Hệ thống tạo | Auto | ID hệ thống tạo Contact | `email-tracker` |
| `ORIGIN_ID` | ID nguồn | Auto | ID bản ghi từ hệ thống gốc | |

#### Giá trị SOURCE_ID phổ biến

| STATUS_ID | Tên hiển thị | Khi nào dùng |
|-----------|-------------|-------------|
| `CALL` | Call | KH gọi điện đến |
| `EMAIL` | E-Mail | KH liên hệ qua email |
| `WEB` | Website | KH từ website |
| `RECOMMENDATION` | By Recommendation | **KH từ đối tác giới thiệu** |
| `WEBFORM` | CRM form | KH điền CRM form |
| `OTHER` | Other | Nguồn khác |

### Địa chỉ

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Ví dụ |
|-------------|-------------|----------|-------|-------|
| `ADDRESS` | Địa chỉ | Nên có | Địa chỉ liên hệ | "Block A4, Chung cư Era Town" |
| `ADDRESS_2` | Địa chỉ 2 | Không | Bổ sung | |
| `ADDRESS_CITY` | Thành phố | Nên có | | "TP. Hồ Chí Minh" |
| `ADDRESS_PROVINCE` | Tỉnh/Vùng | Không | | |
| `ADDRESS_REGION` | Khu vực | Không | | |
| `ADDRESS_POSTAL_CODE` | Mã bưu chính | Không | | |
| `ADDRESS_COUNTRY` | Quốc gia | Không | | "Việt Nam" |

### Phân công & Hệ thống

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả | Ví dụ |
|-------------|-------------|----------|-------|-------|
| `ASSIGNED_BY_ID` | Người phụ trách | **YES** | Nhân sự quản lý Contact | ID nhân viên |
| `OPENED` | Mở cho mọi người | Auto | Ai cũng thấy Contact | `Y` (mặc định) |
| `EXPORT` | Cho phép export | Auto | Xuất dữ liệu | `Y` / `N` |
| `COMMENTS` | Ghi chú | Nên có | Thông tin bổ sung | Xem mẫu bên dưới |

### UTM Tracking

| Bitrix Field | Tên hiển thị | Bắt buộc | Mô tả |
|-------------|-------------|----------|-------|
| `UTM_SOURCE` | UTM Source | Auto | Nguồn chiến dịch |
| `UTM_MEDIUM` | UTM Medium | Auto | Kênh chiến dịch |
| `UTM_CAMPAIGN` | UTM Campaign | Auto | Tên chiến dịch |
| `UTM_CONTENT` | UTM Content | Auto | Nội dung chiến dịch |
| `UTM_TERM` | UTM Term | Auto | Từ khóa |

---

## UF Fields (Custom)

| ID | Field | Type | Tên hiển thị | Mô tả |
|----|-------|------|-------------|-------|
| 230 | `UF_CRM_CONTACT_LIFECYCLE_STAGE` | enumeration | Contact Lifecycle Stage | Vòng đời Contact |
| 348 | `UF_CRM_UC_USER_NS` | string | *(chưa đặt tên)* | Cần review |
| 830 | `UF_CRM_SMAX_ZALO_USER_PID` | string | Zalo User PID | SMAX integration — Zalo user ID |
| 834 | `UF_CRM_SMAX_ZALO_GROUP_PID` | string | Zalo Group PID | SMAX integration — Zalo group ID |
| 836 | `UF_CRM_SMAX_ZALO_PAGE_PID` | string | Zalo Page PID | SMAX integration — Zalo page ID |

### Dropdown: Contact Lifecycle Stage (`UF_CRM_CONTACT_LIFECYCLE_STAGE`)

| ID | Giá trị | Mô tả | Khi nào chuyển |
|----|---------|-------|---------------|
| 44 | Subscriber | Mới biết đến, chưa tương tác | Mặc định khi tạo |
| 46 | MQL | Marketing Qualified Lead | Sau khi điền form (Bước 02) |
| 48 | SQL | Sales Qualified Lead | Sau meeting, đánh giá BANT (Bước 03) |
| 50 | Opportunity | Cơ hội bán hàng | Khi convert Lead → Deal (Bước 04) |
| 52 | Customer | Khách hàng | Khi Deal Won (Bước 09) |
| 54 | Evangelist | KH trung thành, giới thiệu | Khi KH giới thiệu KH mới |

---

## Contact KH — Tạo tại Bước 02 (auto từ Google Form)

| Bitrix Field | Bắt buộc | Nguồn dữ liệu | Bổ sung nếu thiếu |
|-------------|----------|---------------|-------------------|
| `HONORIFIC` | **YES** | Google Form | Hỏi KH qua Zalo |
| `NAME` | **YES** | Google Form | Hỏi KH qua Zalo |
| `POST` | **YES** | Google Form | Hỏi KH qua Zalo |
| `PHONE` | **YES** | Google Form / Lead | Đã có từ Lead |
| `EMAIL` | **YES** | Google Form | Hỏi KH qua Zalo |
| `COMPANY_ID` | **YES** | Auto liên kết | Kiểm tra |
| `TYPE_ID` | **YES** | Mặc định | `CLIENT` |
| `SOURCE_ID` | **YES** | Copy từ Lead | |
| `ASSIGNED_BY_ID` | **YES** | Auto | Người phụ trách Lead |
| `WEB` | Nên có | Google Form / tra cứu | Tra Google |
| `IM` | Auto | Open Channel | Auto khi KH chat |
| `COMMENTS` | Nên có | Nhân sự bổ sung | Vai trò, ghi chú |
| `UF_CRM_CONTACT_LIFECYCLE_STAGE` | Nên có | Theo quy trình | `44` (Subscriber) → cập nhật theo bước |

> **Lưu ý cho AI/Automation:** Contact KH auto-created khi Google Form submit (match `PHONE`). Nếu auto-create thất bại, tạo thủ công và liên kết với Lead + Company.

---

## Contact Đối tác giới thiệu (Referer) — Tạo tại Bước 01

| Bitrix Field | Bắt buộc | Cách điền | Ví dụ |
|-------------|----------|-----------|-------|
| `HONORIFIC` | **YES** | Chọn danh xưng | `HNR_EN_1` (Ông) |
| `NAME` | **YES** | Tên đối tác | "Văn B" |
| `POST` | **YES** | Chức vụ đối tác | "Giám đốc" |
| `PHONE` | **YES** | SĐT đối tác | "+84909876543" |
| `EMAIL` | **YES** | Email đối tác | "partner@company.com" |
| `TYPE_ID` | **YES** | Loại Contact | `PARTNER` |
| `SOURCE_ID` | **YES** | Nguồn | `RECOMMENDATION` |
| `ASSIGNED_BY_ID` | **YES** | Người phụ trách | ID nhân viên |
| `COMMENTS` | Nên có | Mô tả về đối tác | "Đối tác lâu năm, ngành IT" |

> **Lưu ý:** Sau khi tạo Contact Referer, liên kết vào Lead → `UF_CRM_REFERRER`. Contact này sẽ nhận email thông báo tại 6 touchpoint trong SOP.

---

## Mẫu ghi chú COMMENTS

### Contact KH

```
Vai trò: [Người ra quyết định / Người liên hệ / Kế toán]
Kênh liên lạc ưu tiên: [Zalo / Email / Điện thoại]
Ghi chú: [Thông tin đặc biệt]
```

### Contact Đối tác giới thiệu

```
Đối tác: [Tên công ty đối tác]
Mối quan hệ: [Đối tác lâu năm / KH cũ / Bạn bè]
Ngành: [Ngành nghề đối tác]
Số KH đã giới thiệu: [Số lượng]
```

---

## Lưu ý cho AI/Automation

1. **Auto-create từ Google Form:** Match theo `PHONE` của Lead → tạo Contact + liên kết
2. **Contact Referer:** Tạo thủ công tại Bước 01, `TYPE_ID` = `PARTNER`, dùng làm giá trị cho `UF_CRM_REFERRER`
3. **Liên kết:** Contact luôn phải liên kết với Company (`COMPANY_ID`) và Lead/Deal
4. **Decision Maker:** Bổ sung "Người ra quyết định" vào `COMMENTS` sau Meeting (Bước 03)
5. **Lifecycle Stage:** Cập nhật `UF_CRM_CONTACT_LIFECYCLE_STAGE` theo tiến trình SOP
6. **Multi-value fields:** `PHONE`, `EMAIL`, `WEB`, `IM` là mảng — có thể có nhiều giá trị, mỗi giá trị cần `VALUE_TYPE`
7. **SMAX Zalo fields:** `UF_CRM_SMAX_ZALO_*` do SMAX integration tự quản lý, không cần điền thủ công
