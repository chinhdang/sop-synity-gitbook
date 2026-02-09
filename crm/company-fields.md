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

## Lưu ý cho AI/Automation

1. **Auto-create từ Google Form:** Company tạo tự động khi form submit, liên kết với Contact + Lead
2. **MST bắt buộc cho HĐ:** Nếu Google Form không có MST, nhân sự phải tra masothue.com và bổ sung
3. **Liên kết:** Company luôn liên kết với Contact(s) và Lead/Deal
4. **Một Company có thể có nhiều Contact:** Người liên hệ, người QĐ, kế toán...
