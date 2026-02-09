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

## Contact KH — Tạo tại Bước 02 (auto từ Google Form)

| Bitrix Field | Tên hiển thị | Bắt buộc | Nguồn dữ liệu | Bổ sung nếu thiếu |
|-------------|-------------|----------|---------------|-------------------|
| `HONORIFIC` | Danh xưng | **YES** | Google Form | Hỏi KH qua Zalo |
| `NAME` | Tên | **YES** | Google Form | Hỏi KH qua Zalo |
| `LAST_NAME` | Họ | Không bắt buộc | Google Form | Hỏi KH qua Zalo |
| `POST` | Chức vụ | **YES** | Google Form | Hỏi KH qua Zalo |
| `PHONE` | SĐT | **YES** | Google Form / Lead | Đã có từ Lead |
| `EMAIL` | Email | **YES** | Google Form | Hỏi KH qua Zalo |
| `SOURCE_ID` | Nguồn | **YES** | Copy từ Lead | |
| `COMPANY_ID` | Công ty | **YES** | Auto liên kết | Kiểm tra liên kết |
| `WEB` | Website | Nên có | Google Form / tra cứu | Tra Google |
| `IM` | Facebook/Zalo | Nên có | Google Form / tra cứu | Tra Facebook |
| `COMMENTS` | Ghi chú | Nên có | Nhân sự bổ sung | Vai trò, ghi chú đặc biệt |

> **Lưu ý cho AI/Automation:** Contact KH auto-created khi Google Form submit (match `PHONE`). Nếu auto-create thất bại, tạo thủ công và liên kết với Lead + Company.

---

## Contact Đối tác giới thiệu (Referer) — Tạo tại Bước 01

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ví dụ |
|-------------|-------------|----------|-----------|-------|
| `HONORIFIC` | Danh xưng | **YES** | Anh / Chị | "Anh" |
| `NAME` | Tên | **YES** | Tên đối tác | "Văn B" |
| `LAST_NAME` | Họ | Không bắt buộc | Họ đối tác | "Trần" |
| `POST` | Chức vụ | **YES** | Chức vụ đối tác | "Giám đốc" |
| `PHONE` | SĐT | **YES** | SĐT đối tác | "0909876543" |
| `EMAIL` | Email | **YES** | Email đối tác | "partner@company.com" |
| `SOURCE_ID` | Nguồn | **YES** | Đối tác giới thiệu | `RECOMMENDATION` |
| `COMMENTS` | Ghi chú | Nên có | Mô tả về đối tác | "Đối tác lâu năm, ngành IT" |

> **Lưu ý:** Sau khi tạo Contact Referer, liên kết vào Lead → `UF_CRM_REFERRER`. Contact này sẽ nhận email thông báo tại 6 touchpoint trong SOP.

---

## Lưu ý cho AI/Automation

1. **Auto-create từ Google Form:** Match theo `PHONE` của Lead → tạo Contact + liên kết
2. **Contact Referer:** Tạo thủ công tại Bước 01, dùng làm giá trị cho `UF_CRM_REFERRER`
3. **Liên kết:** Contact luôn phải liên kết với Company (`COMPANY_ID`) và Lead/Deal
4. **Decision Maker:** Bổ sung ghi chú "Người ra quyết định" vào `COMMENTS` sau Meeting (Bước 03)
