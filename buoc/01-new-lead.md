# Bước 01: New Lead – Tiếp nhận khách hàng

> **Pha:** 1 - Lead Qualification | **Phụ trách chính:** P. Chuyển đổi
> **Lead Stage:** NEW LEAD
> **CJM Phase:** AWARENESS + INTEREST

---

## 🎯 Mục tiêu

Tiếp nhận khách hàng mới từ các kênh (Facebook cá nhân Chinh, đối tác giới thiệu), tạo Lead trong Bitrix24, và gửi Google Form khảo sát.

---

## 🗺️ CJM Actions trong bước này

```
PHASE 1-2: AWARENESS + INTEREST
├── KH biết đến SYNITY (Facebook cá nhân Chinh / Đối tác giới thiệu)
├── KH liên hệ (Comment, Inbox, Gọi điện)
├── Nhân sự SYNITY tạo Lead (Tên + SĐT)
├── Tạo nhóm Zalo với KH
└── Gửi link Google Form khảo sát nhu cầu
```

---

## ✅ Checklist hành động

### A. Tiếp nhận KH mới

- [ ] Xác nhận kênh đến (Facebook / Đối tác / Website / Khác)
- [ ] Xin SĐT khách hàng
- [ ] Tạo Lead trong Bitrix24 (Tên + SĐT + Nguồn đến)
- [ ] Lead stage: **New Lead**

### A1. Nếu KH từ đối tác giới thiệu (Referer)

- [ ] Tạo **Contact đối tác giới thiệu** trong Bitrix24 (nếu chưa có):

| Trường | Giá trị |
|--------|---------|
| Họ tên | [Tên đối tác] |
| SĐT | [Số điện thoại] |
| Email | [Email đối tác] |
| Nguồn | Đối tác giới thiệu |
| Ghi chú | [Mô tả về đối tác: mối quan hệ, ngành, lý do giới thiệu] |

- [ ] Liên kết Contact đối tác vào Lead → UF **Referer**
- [ ] Ghi chú Lead: "KH được giới thiệu bởi [Tên đối tác] - [Mô tả]"

### B. Kết nối & Tạo nhóm Zalo

- [ ] Kết bạn Zalo với KH
- [ ] Tạo nhóm Zalo: "SYNITY x [Tên KH/Công ty]"
- [ ] Thêm nhân sự SYNITY liên quan vào nhóm
- [ ] Gửi tin nhắn chào hỏi (dùng template)

### C. Gửi Google Form khảo sát

- [ ] Gửi link Google Form qua nhóm Zalo
- [ ] Nhấn mạnh: "Anh/chị vui lòng để **người ra quyết định** điền form"
- [ ] Nếu KH chưa điền sau 1 ngày → gọi điện nhắc
- [ ] Lead vẫn giữ stage New Lead cho đến khi KH submit form

---

## 💾 Thao tác CRM (Bitrix24)

### Tạo Lead thủ công

| Bitrix Field | Tên hiển thị | Bắt buộc | Cách điền | Ví dụ |
|-------------|-------------|----------|-----------|-------|
| `TITLE` | Tiêu đề Lead | **YES** | `[Tên KH/Cty] - [Nguồn]` | "Nguyễn Văn A - Facebook" |
| `HONORIFIC` | Danh xưng | **YES** | Anh / Chị / Mr / Mrs | "Anh" |
| `NAME` | Tên | **YES** | Tên người liên hệ | "Văn A" |
| `LAST_NAME` | Họ | Không | Họ người liên hệ | "Nguyễn" |
| `POST` | Chức vụ | **YES** | Vai trò trong công ty | "Giám đốc", "Trưởng phòng IT" |
| `PHONE` | SĐT | **YES** | SĐT chính (dùng match Google Form) | "0901234567" |
| `COMPANY_TITLE` | Tên công ty | **YES** | Tên đầy đủ công ty | "Công ty ABC" |
| `SOURCE_ID` | Nguồn đến | **YES** | Chọn từ danh sách | `FACEBOOK` / `RECOMMENDATION` / `WEB` / `OTHER` |
| `SOURCE_DESCRIPTION` | Chi tiết nguồn | Nên có | Mô tả kênh cụ thể | "FB group Bitrix24 VN", "Partner: Trần B" |
| `ASSIGNED_BY_ID` | Người phụ trách | **YES** | Chọn nhân sự P. Chuyển đổi | ID nhân viên |
| `UF_CRM_REFERRER` | Referrer (Partner) | Nếu từ đối tác | Chọn Contact đối tác giới thiệu | Contact: "Trần Văn B" |
| `COMMENTS` | Ghi chú | Nên có | Kênh cụ thể, context | "KH inbox qua FB cá nhân Chinh" |
| `STATUS_ID` | Stage | **Auto** | Mặc định khi tạo mới | `NEW` (New Lead) |

> **Lưu ý cho AI/Automation:** Khi tạo Lead qua API, các field bắt buộc (YES) phải có giá trị. `PHONE` là field quan trọng nhất vì dùng để match với Google Form ở Bước 02.

### Tạo Contact đối tác giới thiệu (nếu KH từ đối tác)

| Trường | Giá trị | Bắt buộc |
|--------|---------|----------|
| Họ tên | [Tên đối tác] | ✅ |
| SĐT | [Số điện thoại] | ✅ |
| Email | [Email đối tác] | ✅ |
| Nguồn | Đối tác giới thiệu | ✅ |
| Ghi chú | [Mô tả về đối tác] | Nên có |

---

## 📥 Input

| Input | Nguồn | Bắt buộc |
|-------|-------|----------|
| Thông tin KH liên hệ (tên, SĐT) | Kênh marketing | ✅ |
| Nguồn đến (kênh nào) | Xác nhận từ KH | ✅ |

---

## 📤 Output

| Output | Lưu ở đâu | Người nhận |
|--------|-----------|------------|
| Lead đã tạo trong Bitrix24 | CRM | P. Chuyển đổi |
| Nhóm Zalo đã tạo | Zalo | KH + Team |
| Link Google Form đã gửi | Zalo group | KH |

---

## 📋 Template tin nhắn chào hỏi

```
Chào anh/chị [Tên],

Em là [Tên] bên SYNITY – đối tác chính thức của Bitrix24 tại Việt Nam.

Cảm ơn anh/chị đã quan tâm đến giải pháp của bên em.
Để bên em tư vấn đúng nhu cầu, anh/chị vui lòng dành 3-5 phút
điền form khảo sát nhanh nhé:

👉 [Link Google Form]

Lưu ý: Anh/chị nào là người quyết định về giải pháp này thì điền form
sẽ giúp buổi trao đổi sau hiệu quả hơn ạ.

Em cảm ơn anh/chị!
```

---

## 📊 KPIs & SLA

| Chỉ số | Mục tiêu | Đo lường |
|--------|----------|----------|
| Thời gian tạo Lead | Trong ngày KH liên hệ | Ngày liên hệ → Ngày tạo Lead |
| Tỷ lệ gửi Google Form | 100% | Form gửi / Lead tạo |
| Thời gian gửi form | < 2 giờ sau tạo Lead | Timestamp |

---

## ❓ FAQ & Troubleshooting

<details>
<summary><strong>KH không muốn cho SĐT</strong></summary>

**Cách xử lý:**
> "Dạ em hiểu anh/chị. Số điện thoại giúp bên em tiện liên lạc qua Zalo,
> là kênh trao đổi chính để hỗ trợ nhanh nhất. Anh/chị yên tâm thông tin
> được bảo mật hoàn toàn ạ."

Nếu KH vẫn từ chối → trao đổi qua Messenger/Email, ghi chú vào Lead.
</details>

<details>
<summary><strong>KH từ đối tác giới thiệu nhưng không rõ nhu cầu</strong></summary>

**Cách xử lý:**
1. Liên hệ đối tác để hỏi thêm context
2. Tạo Lead với ghi chú từ đối tác
3. Gọi điện KH trước khi gửi form để tìm hiểu sơ bộ
</details>

---

## 🔗 Liên kết

| Điều hướng | Link |
|------------|------|
| → Bước tiếp: 02. Survey | [Link](02-survey.md) |
| ↑ Về Landing P. Chuyển đổi | [Link](../landing/p-chuyen-doi.md) |
| 🏠 Trang chủ | [Link](../README.md) |
