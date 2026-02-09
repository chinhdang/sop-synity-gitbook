# P. Kế toán – Trang làm việc

> **Vai trò:** Quản lý dòng tiền, xuất hóa đơn, theo dõi công nợ
> **Tham gia tại:** Bước 09 (Payment) & Bước 12 (Off-boarding)

---

## ⚡ Việc cần làm hôm nay

> 💡 *Phần này sẽ tự động cập nhật từ CRM (concept)*

### 🔴 Invoice quá hạn chưa thanh toán

| Khách hàng | Invoice | Số tiền | Quá hạn | Action |
|------------|---------|---------|---------|--------|
| *Load từ CRM...* | -- | -- | -- ngày | [Nhắc thanh toán](#) |

### 🟡 Cần xuất hóa đơn VAT

| Khách hàng | Invoice | Số tiền | Ngày thanh toán | Action |
|------------|---------|---------|-----------------|--------|
| *Load từ CRM...* | -- | -- | -- | [Xuất HĐ](#) |

### 🟢 Invoice sắp đến hạn (7 ngày tới)

| Khách hàng | Invoice | Số tiền | Đến hạn | Action |
|------------|---------|---------|---------|--------|
| *Load từ CRM...* | -- | -- | -- | [Xem](#) |

---

## 🗺️ Tôi tham gia ở đâu?

```
Hành trình khách hàng:

[02] → [03] → [04] → [05] → [06] → [07] → [08] → [09] → [10] → [11] → [12]
                                                   │             │
                                                   ▼             ▼
                                              💰 PAYMENT    💰 OFF-BOARDING

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
P. Kế toán tham gia tại 2 điểm:
• Bước 09: Thu tiền bản quyền + đợt 1 triển khai + xuất hóa đơn
• Bước 12: Thu các đợt thanh toán còn lại theo nghiệm thu
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

### Chi tiết vai trò

| Bước | Tên | Vai trò | Việc cần làm |
|------|-----|---------|--------------|
| 09 | Payment | **R** | Xác nhận thanh toán, xuất hóa đơn VAT |
| 12 | Off-boarding | **R** | Thu tiền theo nghiệm thu, xuất hóa đơn |

> **R** = Responsible (Thực hiện)

---

## 🎯 Quick Actions

### Khi P. Chuyển đổi tạo Invoice mới

```
□ Nhận thông báo invoice mới (từ CRM automation)
□ Kiểm tra thông tin trên invoice:
  □ Tên công ty, MST đúng chưa?
  □ Số tiền khớp với HĐ/báo giá chưa?
  □ Hạng mục thanh toán rõ ràng chưa?
□ Nếu OK → Confirm invoice
□ Nếu sai → Báo P. Chuyển đổi sửa
```

---

### Khi KH thanh toán

```
□ Xác nhận tiền về tài khoản (check bank statement)
□ Cập nhật trạng thái invoice = Paid trong CRM
□ Tạo task xuất hóa đơn VAT (trong 24h)
□ Xuất hóa đơn qua MISA:
  □ Tạo hóa đơn nháp
  □ Kiểm tra thông tin
  □ Xuất hóa đơn thật
□ Gửi hóa đơn cho KH (email từ MISA)
□ Lưu bản sao vào Collab của KH
```

---

### Khi invoice quá hạn

```
□ Check: Invoice đã quá hạn bao nhiêu ngày?
□ Nếu < 3 ngày: Gửi email nhắc nhở tự động (đã setup)
□ Nếu >= 3 ngày:
  □ Báo P. Chuyển đổi/Triển khai follow-up
  □ Tạo task trong CRM
□ Nếu > 7 ngày: Escalate lên Chinh
```

---

## 📊 KPIs của tôi

| KPI | Mục tiêu | Hiện tại |
|-----|----------|----------|
| Thời gian xuất hóa đơn | < 24h sau khi KH thanh toán | -- |
| Tỷ lệ thu đúng hạn | > 85% | --% |
| Công nợ quá hạn | < 10% tổng công nợ | --% |

---

## 📋 Quy trình chi tiết

### Quy trình thanh toán & xuất hóa đơn

```
┌─────────────────────────────────────────────────────────────────┐
│                    QUY TRÌNH THANH TOÁN                         │
└─────────────────────────────────────────────────────────────────┘

P. Chuyển đổi/Triển khai          P. Kế toán              Hệ thống
        │                              │                      │
        │  Tạo Invoice trong CRM       │                      │
        ├─────────────────────────────►│                      │
        │                              │                      │
        │                              │  Automation gửi      │
        │                              │  email cho KH        │
        │                              ├─────────────────────►│
        │                              │                      │
        │                         KH thanh toán               │
        │                              │◄─────────────────────┤
        │                              │                      │
        │                              │  Xác nhận tiền về    │
        │                              │  Update Invoice=Paid │
        │                              ├─────────────────────►│
        │                              │                      │
        │                              │  Tạo task xuất HĐ    │
        │                              │◄─────────────────────┤
        │                              │                      │
        │                              │  Xuất HĐ qua MISA    │
        │                              ├──────────┐           │
        │                              │          │           │
        │                              │◄─────────┘           │
        │                              │                      │
        │                              │  Gửi HĐ cho KH       │
        │                              ├─────────────────────►│
        │                              │                      │
        ▼                              ▼                      ▼
```

### Các loại Invoice

| Loại | Khi nào tạo | Ai tạo | Ghi chú |
|------|-------------|--------|---------|
| **Invoice Bản quyền Bitrix** | Sau khi ký HĐ | P. Chuyển đổi | Thu trước, ưu tiên cao |
| **Invoice Triển khai Đợt 1** | Sau khi ký HĐ | P. Chuyển đổi | Điều kiện để Kick-off |
| **Invoice Triển khai Đợt 2+** | Sau nghiệm thu | P. Triển khai | Kèm biên bản nghiệm thu |
| **Invoice Tính năng bổ sung** | Sau khi KH ký báo giá | P. Triển khai | Nếu có phát sinh |

---

## 📋 Templates

### Email nhắc thanh toán (tự động)

```
Subject: [SYNITY] Nhắc nhở thanh toán - Invoice #[số]

Kính gửi Anh/Chị [Tên],

Bên em gửi nhắc nhở về invoice thanh toán:

📋 Invoice: #[số]
💰 Số tiền: [số tiền] VNĐ
📅 Hạn thanh toán: [ngày]

Thông tin chuyển khoản:
- Ngân hàng: [Tên NH]
- Số tài khoản: [STK]
- Chủ tài khoản: CÔNG TY TNHH SYNITY
- Nội dung: [Tên công ty] thanh toán [hạng mục]

Nếu Anh/Chị đã thanh toán, vui lòng bỏ qua email này.

Trân trọng,
SYNITY Team
```

### Email gửi hóa đơn VAT

```
Subject: [SYNITY] Hóa đơn VAT - [Tên công ty]

Kính gửi Anh/Chị [Tên],

Cảm ơn Anh/Chị đã thanh toán. Bên em gửi hóa đơn VAT đính kèm:

📋 Số hóa đơn: [số]
💰 Số tiền: [số tiền] VNĐ
📅 Ngày xuất: [ngày]

Hóa đơn điện tử đã được gửi đến email: [email KH]
Anh/Chị cũng có thể tra cứu tại: [link tra cứu MISA]

Nếu có bất kỳ thắc mắc nào về hóa đơn,
vui lòng liên hệ bộ phận kế toán: [email/SĐT].

Trân trọng,
SYNITY Team
```

---

## ❓ Xử lý tình huống

<details>
<summary><strong>🔴 KH yêu cầu thay đổi thông tin trên hóa đơn</strong></summary>

**Trước khi xuất hóa đơn:**
- Cập nhật thông tin trong CRM
- Xuất hóa đơn với thông tin mới

**Sau khi đã xuất hóa đơn:**
1. Kiểm tra: Thông tin sai ở đâu? (tên, MST, địa chỉ...)
2. Nếu sai MST/tên công ty: **Phải hủy hóa đơn cũ, xuất hóa đơn mới**
3. Nếu sai địa chỉ nhỏ: Có thể xuất biên bản điều chỉnh
4. Liên hệ KH xác nhận trước khi xử lý
</details>

<details>
<summary><strong>🔴 KH thanh toán thiếu/thừa</strong></summary>

**Thanh toán thiếu:**
1. Gửi email thông báo: "Anh/Chị chuyển thiếu [số tiền]"
2. Đề nghị chuyển bổ sung
3. Chỉ xuất hóa đơn khi đã nhận đủ (hoặc theo thỏa thuận)

**Thanh toán thừa:**
1. Thông báo cho KH
2. Đề xuất:
   - Hoàn lại số tiền thừa, hoặc
   - Ghi nhận vào khoản thanh toán tiếp theo
3. Ghi chép rõ ràng trong CRM
</details>

<details>
<summary><strong>🔴 KH yêu cầu xuất hóa đơn trước khi thanh toán</strong></summary>

**Nguyên tắc:** Không xuất hóa đơn trước khi nhận tiền

**Cách trả lời:**
> "Dạ Anh/Chị, theo quy định kế toán, bên em chỉ xuất hóa đơn
> sau khi đã nhận thanh toán. Anh/Chị vui lòng chuyển khoản trước,
> bên em sẽ xuất hóa đơn trong vòng 24h sau khi nhận tiền ạ."

**Ngoại lệ:** Chỉ khi có phê duyệt từ CEO với lý do đặc biệt
</details>

<details>
<summary><strong>🔴 KH quá hạn thanh toán nhiều lần</strong></summary>

**Quy trình escalation:**

| Quá hạn | Hành động |
|---------|-----------|
| 1-3 ngày | Email nhắc nhở tự động |
| 4-7 ngày | P. Chuyển đổi/Triển khai gọi điện |
| > 7 ngày | Báo cáo Chinh để xử lý |
| > 30 ngày | Gửi thư nhắc nợ chính thức |

**Ghi chép:**
- Tất cả lịch sử nhắc nợ lưu trong CRM
- Note rõ: ngày nhắc, hình thức, phản hồi của KH
</details>

---

## 💾 Thao tác trong hệ thống

### Cập nhật Invoice trong Bitrix

```
CRM → Invoices → [Chọn invoice]

Khi KH thanh toán:
├── Status: Paid
├── Payment date: [Ngày thực nhận]
├── Payment method: Bank transfer
└── Note: [Mã giao dịch ngân hàng]
```

### Xuất hóa đơn qua MISA

```
MISA → Hóa đơn → Tạo mới

1. Điền thông tin:
   ├── Khách hàng: [Tên, MST, địa chỉ - lấy từ CRM]
   ├── Hàng hóa/dịch vụ: [Theo invoice]
   ├── Đơn giá, thành tiền
   └── VAT: 8% (hoặc theo quy định)

2. Preview → Kiểm tra kỹ

3. Lưu nháp → Xuất chính thức

4. Gửi email cho KH (trong MISA)
```

---

## 🔗 Liên kết nhanh

| Đi đến | Link |
|--------|------|
| 🏠 Trang chủ SOP | [Link](../README.md) |
| 👤 P. Chuyển đổi | [Link](p-chuyen-doi.md) |
| 🛠️ P. Triển khai | [Link](p-trien-khai.md) |
| 💰 MISA | [misa.vn]() |
| 📊 Báo cáo tài chính | [Bitrix BI]() |

---

## 📞 Liên hệ khi cần hỗ trợ

| Vấn đề | Liên hệ |
|--------|---------|
| Thông tin KH sai | P. Chuyển đổi (Huy) |
| Invoice triển khai | P. Triển khai |
| Vấn đề MISA | IT / Support MISA |
| Escalate công nợ | Chinh (CEO) |
