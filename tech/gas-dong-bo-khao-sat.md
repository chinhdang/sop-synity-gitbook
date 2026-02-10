# Đồng bộ khảo sát Google Form → Bitrix24 (GAS)

> **Tính năng:** Tự động đồng bộ kết quả khảo sát nhu cầu từ Google Form / Google Sheet vào Bitrix24 CRM thông qua Google Apps Script.
> **Đối tượng:** P. Kỹ thuật (setup, maintain, debug).
> **Repo:** [`synity-gas`](https://github.com/chinhdang/synity-gas) | **Deploy:** `clasp push`
> **SOP liên quan:** [Bước 02 — Survey](../buoc/02-survey.md) (AR-1)

---

## Kiến trúc

```
KH điền Google Form (36 cột)
       │
       ▼
Google Sheet (Form Responses)
       │
       ▼ (GAS installable trigger: onFormSubmit)
Google Apps Script
       │
       ├──► Bitrix24 REST API
       │      ├── Lead (match SĐT → create / update)
       │      ├── Contact (match: phone > email)
       │      ├── Company (match: phone > MST > website > title)
       │      ├── Requisite (RQ_COMPANY_NAME, RQ_VAT_ID, Legal Address)
       │      ├── Timeline Comment (BBCode TABLE — kết quả khảo sát)
       │      └── Link: Contact ↔ Company ↔ Lead
       │
       └──► VietQR API
              └── MST → Tên pháp lý + Địa chỉ GPKD
```

---

## Flow xử lý (10 bước)

| # | Bước | Bitrix API | Ghi chú |
|---|------|-----------|---------|
| 1 | Parse form data (36 cột) | — | `parseFormData()` |
| 2 | Validate SĐT | — | Bắt buộc, reject nếu trống |
| 3 | Normalize phone → `+84xxxxxxxxx` | — | `normalizePhone()` |
| 4 | Match/Create Lead by phone | `crm.lead.list` / `crm.lead.add` | Match 9-digit core |
| 5 | Match/Create Contact (phone > email) | `crm.contact.list` / `crm.contact.add` | + Facebook WEB, Lifecycle=SQL |
| 6 | Match/Create Company (phone > MST > website > title) | `crm.company.list` / `crm.company.add` | + Website WEB |
| 7 | VietQR → Requisite + Legal Address | `crm.requisite.add` / `crm.address.add` | Skip nếu Company found by MST |
| 8 | Link Contact ↔ Company ↔ Lead | `crm.lead.update` / `crm.contact.update` | |
| 9 | Post BBCode comment → Lead timeline | `crm.timeline.comment.add` | Flat table, full question text |
| 10 | Error handling | — | Log to Sheet "Error Log" tab |

---

## Cấu hình

| Key | Nơi lưu | Giá trị |
|-----|---------|---------|
| `BITRIX_WEBHOOK` | GAS Script Properties | `https://tamgiac.bitrix24.com/rest/1/TOKEN/` |
| `FORM_SHEET_ID` | GAS Script Properties | `1TlJVGqQD9peFQ94oYMNOucC0jtosHiI0P0m3uUz5L4o` |
| `DEFAULT_ASSIGNED` | GAS Script Properties | `1` (User ID nhân sự phụ trách) |
| `ERROR_LOG_SHEET` | GAS Script Properties | *(Optional)* Sheet ID cho error log |

> **QUAN TRỌNG:** Webhook token **KHÔNG** lưu trong source code. Chỉ lưu trong Script Properties (GAS editor).

---

## Setup & Deploy

```bash
# 1. Clone repo
git clone https://github.com/chinhdang/synity-gas.git
cd synity-gas

# 2. Install clasp
npm install @google/clasp

# 3. Login Google
npx clasp login

# 4. Push code lên GAS
npx clasp push

# 5. Mở GAS editor
# Chạy setupProperties() → set webhook token
# Chạy createTrigger() → bind trigger vào Sheet
# Chạy testFormSubmit() → test với data mẫu
```

---

## Source Code

### `src/config.js` — Field mapping & defaults

```javascript
/**
 * SYNITY — Google Form → Bitrix24 Lead
 * Field mapping & default values
 *
 * Google Sheet: https://docs.google.com/spreadsheets/d/1TlJVGqQD9peFQ94oYMNOucC0jtosHiI0P0m3uUz5L4o/
 * CẬP NHẬT FORM_COLUMNS khi Google Form thay đổi cấu trúc.
 */

// ─── Google Form column indices (0-based) ───────────────────────────
// Nhóm A: Thông tin người liên hệ → Contact
// Nhóm B: Thông tin doanh nghiệp → Company + Requisite
// Nhóm C: Khảo sát → Lead timeline comment (HTML)

var COL = {
  // ── A: Contact info ──
  TIMESTAMP:     0,   // Dấu thời gian
  FULL_NAME:     1,   // Họ tên
  EMAIL:         2,   // Email
  PHONE:         3,   // Phone
  FACEBOOK:      4,   // Facebook cá nhân
  ROLE:          5,   // Vai trò (= POST/Chức vụ)

  // ── B: Company info ──
  SOURCE:        6,   // Anh chị biết đến SYNITY từ đâu?
  COMPANY_NAME:  7,   // Tên doanh nghiệp
  TAX_ID:        8,   // Mã số thuế doanh nghiệp
  COMPANY_WEB:   9,   // Website (hoặc Facebook) công ty

  // ── C: Survey → Lead comment (HTML) ──
  HEADCOUNT:     10,  // Số lượng nhân sự
  INDUSTRY:      11,  // Ngành nghề
  REVENUE:       12,  // Doanh thu trung bình năm (tỷ/năm)
  PAIN_POINTS:   13,  // Khó khăn lớn nhất trong vận hành
  URGENT_ISSUE:  14,  // Vấn đề cấp bách nhất 3 tháng tới
  CURRENT_TOOLS: 15,  // Công cụ/phần mềm đang dùng
  TOOL_LIKES:    16,  // Hài lòng nhất với công cụ hiện tại
  TOOL_DISLIKES: 17,  // Chưa hài lòng / hạn chế
  IMPROVE_WISH:  18,  // Mong muốn cải thiện rõ rệt nhất
  PARTNER_CRITERIA: 19, // Tiêu chí chọn đơn vị đồng hành
  SYNITY_SOLUTIONS: 20, // Giải pháp SYNITY quan tâm
  SOLUTION_GROUP: 21, // Nhóm giải pháp quan tâm
  DIGITIZE_DEPTS: 22, // Bộ phận muốn số hóa
  HAS_SOP:       23,  // Đã có quy trình vận hành chuẩn chưa
  CHANGE_READY:  24,  // Mức độ sẵn sàng thay đổi
  EXPECTED_CHANGE: 25, // Kỳ vọng thay đổi rõ nhất
  DEPLOY_GOAL:   26,  // Mục tiêu lớn nhất khi triển khai
  CRM_CURRENT:   27,  // Quản lý dữ liệu KH bằng cách nào
  CRM_PROBLEMS:  28,  // Vấn đề lớn ở khâu nào
  SALES_TEAM:    29,  // Đội ngũ sales bao nhiêu người
  CRM_EXPECT:    30,  // Kỳ vọng lớn nhất khi triển khai CRM
  TASK_CURRENT:  31,  // Giao việc bằng cách nào
  TASK_PROBLEMS: 32,  // Khó khăn lớn nhất quản lý công việc
  MGMT_LEVELS:   33,  // Bao nhiêu cấp quản lý cần theo dõi
  TASK_EXPECT:   34,  // Kỳ vọng lớn nhất khi áp dụng QLCV
  SALES_SOP:     35   // Quy trình bán hàng đã chuẩn hóa chưa
};

// ─── Survey fields for BBCode comment (flat table, full question text) ──
// Format bắt chước y hệt pinned comment trên Deal 1938.
// Mỗi field: { col, label } — label = nguyên câu hỏi từ Google Form header.
// Chỉ render field có giá trị (skip empty).

var SURVEY_FIELDS = [
  { col: COL.TAX_ID,           label: 'Mã số thuế doanh nghiệp' },
  { col: COL.COMPANY_WEB,      label: 'Website (hoặc Facebook) công ty' },
  { col: COL.HEADCOUNT,        label: 'Số lượng nhân sự' },
  { col: COL.INDUSTRY,         label: 'Ngành nghề' },
  { col: COL.REVENUE,          label: 'Doanh thu trung bình năm (tỷ/năm)' },
  { col: COL.PAIN_POINTS,      label: 'Những khó khăn lớn nhất anh chị đang gặp phải trong quá trình vận hành doanh nghiệp là gì?' },
  { col: COL.URGENT_ISSUE,     label: 'Nếu chỉ chọn một vấn đề cấp bách nhất cần giải quyết trong 3 tháng tới, đó là gì?' },
  { col: COL.CURRENT_TOOLS,    label: 'Doanh nghiệp đang sử dụng những công cụ/phần mềm nào? (CRM, giao việc, kế toán, CSKH, Marketing…)' },
  { col: COL.TOOL_LIKES,       label: 'Điều anh chị hài lòng nhất với các công cụ hiện tại?' },
  { col: COL.TOOL_DISLIKES,    label: 'Điều anh chị chưa hài lòng hoặc cảm thấy còn hạn chế?' },
  { col: COL.IMPROVE_WISH,     label: 'Anh chị mong muốn hệ thống mới giúp cải thiện điều gì rõ rệt nhất?' },
  { col: COL.PARTNER_CRITERIA, label: 'Những tiêu chí quan trọng nhất của anh chị khi lựa chọn đơn vị đồng hành chuyển đổi số doanh nghiệp?' },
  { col: COL.SYNITY_SOLUTIONS, label: 'Anh chị quan tâm đến các giải pháp nào của SYNITY?' },
  { col: COL.SOLUTION_GROUP,   label: 'Hiện tại anh chị quan tâm đến nhóm giải pháp nào?' },
  { col: COL.DIGITIZE_DEPTS,   label: 'Anh chị mong muốn số hóa những bộ phận nào?' },
  { col: COL.HAS_SOP,          label: 'Hiện doanh nghiệp đã có quy trình vận hành chuẩn hay chưa?' },
  { col: COL.CHANGE_READY,     label: 'Mức độ sẵn sàng thay đổi của đội ngũ nhân sự' },
  { col: COL.EXPECTED_CHANGE,  label: 'Nếu hệ thống vận hành trơn tru, anh chị kỳ vọng doanh nghiệp sẽ thay đổi rõ nhất ở điểm nào?' },
  { col: COL.DEPLOY_GOAL,      label: 'Mục tiêu lớn nhất khi triển khai hệ thống tổng thể là gì?' },
  { col: COL.CRM_CURRENT,      label: 'Hiện tại dữ liệu khách hàng đang được quản lý bằng cách nào?' },
  { col: COL.CRM_PROBLEMS,     label: 'Doanh nghiệp đang gặp vấn đề lớn ở những khâu nào?' },
  { col: COL.SALES_TEAM,       label: 'Đội ngũ sales hiện có bao nhiêu người?' },
  { col: COL.CRM_EXPECT,       label: 'Kỳ vọng lớn nhất khi triển khai CRM là gì?' },
  { col: COL.TASK_CURRENT,     label: 'Hiện tại doanh nghiệp đang giao việc bằng cách nào?' },
  { col: COL.TASK_PROBLEMS,    label: 'Khó khăn lớn nhất trong quản lý công việc hiện nay là gì?' },
  { col: COL.MGMT_LEVELS,      label: 'Doanh nghiệp có bao nhiêu cấp quản lý cần theo dõi tiến độ công việc?' },
  { col: COL.TASK_EXPECT,      label: 'Kỳ vọng lớn nhất khi áp dụng hệ thống quản lý công việc là gì?' },
  { col: COL.SALES_SOP,        label: 'Quy trình bán hàng hiện tại của doanh nghiệp đã được chuẩn hóa chưa?' }
];

// ─── SOURCE_ID mapping ───────────────────────────────────────────────
var SOURCE_MAP = {
  'Được giới thiệu':  'RECOMMENDATION',
  'Facebook':          '3',
  'Website':           'WEB',
  'Google':            'WEB',
  'Quảng cáo':        'ADVERTISING',
  'Sự kiện':          'TRADE_SHOW'
};
var SOURCE_DEFAULT = 'OTHER';

// ─── Bitrix24 defaults ──────────────────────────────────────────────
var LEAD_DEFAULTS = {
  STATUS_ID: 'IN_PROCESS',
  OPENED: 'Y',
  CURRENCY_ID: 'VND'
};

var CONTACT_DEFAULTS = {
  SOURCE_ID: 'WEB',
  OPENED: 'Y',
  TYPE_ID: 'CLIENT',
  UF_CRM_CONTACT_LIFECYCLE_STAGE: '48'  // SQL
};

var COMPANY_DEFAULTS = {
  COMPANY_TYPE: 'CUSTOMER',
  OPENED: 'Y'
};

// ─── Requisite settings ─────────────────────────────────────────────
var REQUISITE_DEFAULTS = {
  PRESET_ID: 4,          // "SYNITY - Doanh nghiệp VN"
  ENTITY_TYPE_ID: 4,     // Company
  ACTIVE: 'Y',
  ADDRESS_ONLY: 'N'
};

var ADDRESS_LEGAL_TYPE_ID = 6;  // Legal address (GPKD)
var ADDRESS_ENTITY_TYPE_ID = 8; // Requisite (parent of Address)

// ─── VietQR API ─────────────────────────────────────────────────────
var VIETQR_API_URL = 'https://api.vietqr.io/v2/business/';
```

### `src/utils.js` — Phone normalization, VietQR parser, BBCode builder

```javascript
/**
 * SYNITY — Google Form → Bitrix24 Lead
 * Utility functions
 */

// ─── Phone normalization ─────────────────────────────────────────────

function normalizePhone(raw) {
  if (!raw) return null;
  var cleaned = String(raw).replace(/[^\d+]/g, '');
  if (/^\+84\d{9,10}$/.test(cleaned)) return cleaned;
  if (/^0\d{9}$/.test(cleaned)) return '+84' + cleaned.substring(1);
  if (/^84\d{9}$/.test(cleaned)) return '+' + cleaned;
  if (/^\d{9}$/.test(cleaned)) return '+84' + cleaned;
  return null;
}

function matchPhone(phone) {
  if (!phone) return '';
  return phone.replace(/^\+?84/, '').replace(/^0/, '');
}

// ─── Name splitting ──────────────────────────────────────────────────

function splitName(fullName) {
  if (!fullName) return { LAST_NAME: '', NAME: '' };
  var trimmed = String(fullName).trim().replace(/\s+/g, ' ');
  var parts = trimmed.split(' ');
  if (parts.length === 1) return { LAST_NAME: '', NAME: parts[0] };
  return { LAST_NAME: parts[0], NAME: parts.slice(1).join(' ') };
}

// ─── String helpers ──────────────────────────────────────────────────

function cleanStr(val) {
  if (val === null || val === undefined) return '';
  return String(val).trim();
}

// ─── SOURCE_ID resolver ──────────────────────────────────────────────

function resolveSourceId(raw) {
  if (!raw) return SOURCE_DEFAULT;
  var val = cleanStr(raw);
  if (SOURCE_MAP[val]) return SOURCE_MAP[val];
  var lower = val.toLowerCase();
  for (var key in SOURCE_MAP) {
    if (lower.indexOf(key.toLowerCase()) !== -1) return SOURCE_MAP[key];
  }
  return SOURCE_DEFAULT;
}

// ─── VietQR address parser ───────────────────────────────────────────

function parseVietQRAddress(raw) {
  var result = {
    ADDRESS_1: '', ADDRESS_2: '', REGION: '',
    PROVINCE: '', CITY: '', COUNTRY: 'Việt Nam'
  };
  if (!raw) return result;

  var parts = raw.split(',').map(function(s) { return s.trim(); }).filter(Boolean);
  if (parts.length === 0) return result;

  var wardDistrictPattern = /^(Phường|Xã|Thị trấn|Quận|Huyện|Thị xã|Thành phố)\s/i;
  var province = '';
  var regionParts = [];
  var streetParts = [];

  for (var i = parts.length - 1; i >= 0; i--) {
    var p = parts[i];
    if (!province && i === parts.length - 1) {
      province = normalizeProvince(p);
      continue;
    }
    if (wardDistrictPattern.test(p)) { regionParts.unshift(p); continue; }
    streetParts.unshift(p);
  }

  result.PROVINCE = province;
  result.REGION = regionParts.join(', ');
  result.ADDRESS_1 = streetParts.join(', ');
  return result;
}

function normalizeProvince(raw) {
  if (!raw) return '';
  var s = raw.trim();
  var map = {
    'tp. hcm': 'Thành phố Hồ Chí Minh',
    'tp hcm': 'Thành phố Hồ Chí Minh',
    'tp.hcm': 'Thành phố Hồ Chí Minh',
    'hcm': 'Thành phố Hồ Chí Minh',
    'hồ chí minh': 'Thành phố Hồ Chí Minh',
    'tp. hồ chí minh': 'Thành phố Hồ Chí Minh',
    'tp hồ chí minh': 'Thành phố Hồ Chí Minh',
    'hà nội': 'Thành phố Hà Nội',
    'tp. hà nội': 'Thành phố Hà Nội',
    'tp hà nội': 'Thành phố Hà Nội',
    'đà nẵng': 'Thành phố Đà Nẵng',
    'tp. đà nẵng': 'Thành phố Đà Nẵng',
    'tp đà nẵng': 'Thành phố Đà Nẵng',
    'hải phòng': 'Thành phố Hải Phòng',
    'cần thơ': 'Thành phố Cần Thơ'
  };
  var lower = s.toLowerCase();
  if (map[lower]) return map[lower];
  if (/^tp\.?\s+/i.test(s)) return 'Thành phố ' + s.replace(/^tp\.?\s+/i, '');
  if (/^(Thành phố|Tỉnh)\s/i.test(s)) return s;
  return s;
}

// ─── BBCode comment builder ──────────────────────────────────────────

function buildSurveyBBCode(values, fullName, companyName, timestamp) {
  var bb = '';

  // Title
  bb += '[B]📋 KẾT QUẢ KHẢO SÁT NHU CẦU[/B]\n';
  bb += '[I]' + (fullName || '') + ' — ' + (companyName || '') + '[/I]\n';
  bb += '[I]Ngày: ' + (timestamp || '') + '[/I]\n\n';

  bb += '[TABLE]';
  bb += '[TR]';
  bb += '[TH][COLOR=#0055aa]Nội dung trường thông tin khảo sát[/COLOR][/TH]';
  bb += '[TH][COLOR=#0055aa]Câu trả lời [COLOR=#FFFFFF]';
  bb += '..........................................................................[/COLOR][/COLOR][/TH]';
  bb += '[/TR]';

  for (var i = 0; i < SURVEY_FIELDS.length; i++) {
    var field = SURVEY_FIELDS[i];
    var val = cleanStr(values[field.col] || '');
    if (!val) continue;

    var vStart = '', vEnd = '';
    if (field.col === COL.SYNITY_SOLUTIONS && val) {
      val = val.replace(/, -- /g, '\n\n');
      vStart = '[B][COLOR=#ff0000]';
      vEnd = '[/COLOR][/B]';
    }

    bb += '[TR]';
    bb += '[TD][B][COLOR=#555555]' + field.label + '[/COLOR][/B][/TD]';
    bb += '[TD]' + vStart + val + vEnd + '[/TD]';
    bb += '[/TR]';

    bb += '[TR]';
    bb += '[TD][COLOR=#cccccc]- - - - - - - - - - - - -[/COLOR][/TD]';
    bb += '[TD][COLOR=#cccccc]- - - - - - - - - - - - - - - - - - - - - - - - - - - - - -[/COLOR][/TD]';
    bb += '[/TR]';
  }

  bb += '[/TABLE]';
  return bb;
}
```

### `src/main.js` — Core handler + Bitrix24 API

```javascript
/**
 * SYNITY — Google Form → Bitrix24 Lead
 * Core handler: onFormSubmit + full Bitrix24 integration
 *
 * Flow (SOP Bước 02, AR-1):
 *   1. Parse form data (36 columns)
 *   2. Validate (SĐT required)
 *   3. Normalize phone → +84xxxxxxxxx
 *   4. Match/Create Lead by phone
 *   5. Match/Create Contact (search: phone > email)
 *   6. Match/Create Company (search: phone[via contact] > MST[via requisite] > website > title)
 *   7. VietQR lookup MST → Create Requisite + Legal Address (skip if found by MST)
 *   8. Link Contact ↔ Company ↔ Lead
 *   9. Post survey data as BBCode timeline comment on Lead
 *  10. Error handling → log to sheet
 *
 * SOP gaps (form thiếu so với SOP yêu cầu):
 *   - HONORIFIC (Lead + Contact): Nhân sự bổ sung thủ công
 */

// ── Bitrix24 API helper ──

function bitrixCall(method, params) {
  var webhook = PropertiesService.getScriptProperties().getProperty('BITRIX_WEBHOOK');
  if (!webhook) throw new Error('BITRIX_WEBHOOK not set in Script Properties');
  if (webhook.charAt(webhook.length - 1) !== '/') webhook += '/';

  var url = webhook + method;
  var response = UrlFetchApp.fetch(url, {
    method: 'post',
    contentType: 'application/json',
    payload: JSON.stringify(params || {}),
    muteHttpExceptions: true
  });
  var json = JSON.parse(response.getContentText());
  if (json.error) {
    throw new Error('Bitrix [' + method + ']: ' + json.error + ' — ' + (json.error_description || ''));
  }
  return json;
}

// ── VietQR API ──

function vietqrLookup(taxId) {
  if (!taxId) return null;
  try {
    var url = VIETQR_API_URL + encodeURIComponent(taxId.trim());
    var response = UrlFetchApp.fetch(url, { muteHttpExceptions: true });
    var json = JSON.parse(response.getContentText());
    if (json.code === '00' && json.data) {
      return { name: json.data.name || '', shortName: json.data.shortName || '', address: json.data.address || '' };
    }
    return null;
  } catch (e) { Logger.log('VietQR error: ' + e.message); return null; }
}

// ── Duplicate detection — search priority: phone > email > MST > website ──

function findLeadByPhone(phone) {
  var digits = matchPhone(phone);
  if (!digits) return null;
  var res = bitrixCall('crm.lead.list', {
    filter: { PHONE: digits, '!STATUS_ID': 'JUNK' },
    select: ['ID', 'STATUS_ID', 'CONTACT_ID', 'COMPANY_ID'],
    order: { ID: 'DESC' }
  });
  return (res.result && res.result.length > 0) ? res.result[0] : null;
}

function findContact(data) {
  // Priority 1: Phone
  if (data.phone) {
    var digits = matchPhone(data.phone);
    if (digits) {
      var res = bitrixCall('crm.contact.list', {
        filter: { PHONE: digits },
        select: ['ID', 'NAME', 'LAST_NAME', 'COMPANY_ID']
      });
      if (res.result && res.result.length > 0) return res.result[0];
    }
  }
  // Priority 2: Email
  if (data.email) {
    var res = bitrixCall('crm.contact.list', {
      filter: { EMAIL: data.email },
      select: ['ID', 'NAME', 'LAST_NAME', 'COMPANY_ID']
    });
    if (res.result && res.result.length > 0) return res.result[0];
  }
  return null;
}

function findCompanyByMST(taxId) {
  if (!taxId) return null;
  var res = bitrixCall('crm.requisite.list', {
    filter: { RQ_VAT_ID: taxId, ENTITY_TYPE_ID: 4 },
    select: ['ID', 'ENTITY_ID']
  });
  if (res.result && res.result.length > 0) {
    var companyRes = bitrixCall('crm.company.get', { id: res.result[0].ENTITY_ID });
    if (companyRes.result) return companyRes.result;
  }
  return null;
}

function findCompany(data, linkedCompanyId) {
  // Priority 1-2: Phone/Email → via Contact's linked COMPANY_ID
  if (linkedCompanyId) {
    try {
      var res = bitrixCall('crm.company.get', { id: linkedCompanyId });
      if (res.result) return { company: res.result, method: 'phone' };
    } catch (e) {}
  }
  // Priority 3: MST
  if (data.taxId) {
    var mstResult = findCompanyByMST(data.taxId);
    if (mstResult) return { company: mstResult, method: 'mst' };
  }
  // Priority 4: Website
  if (data.companyWeb) {
    var res = bitrixCall('crm.company.list', {
      filter: { WEB: data.companyWeb }, select: ['ID', 'TITLE']
    });
    if (res.result && res.result.length > 0) return { company: res.result[0], method: 'website' };
  }
  // Priority 5: Title
  if (data.companyName) {
    var res = bitrixCall('crm.company.list', {
      filter: { '=TITLE': data.companyName }, select: ['ID', 'TITLE']
    });
    if (res.result && res.result.length > 0) return { company: res.result[0], method: 'title' };
  }
  return { company: null, method: null };
}

// ── CRM entity creators ──

function getAssignedId() {
  var val = PropertiesService.getScriptProperties().getProperty('DEFAULT_ASSIGNED');
  return val ? parseInt(val, 10) : 1;
}

function createLead(data) {
  var name = splitName(data.fullName);
  var fields = {
    TITLE: data.companyName ? (data.companyName + ' — ' + data.fullName) : ('Google Form — ' + data.fullName),
    NAME: name.NAME, LAST_NAME: name.LAST_NAME,
    PHONE: [{ VALUE: data.phone, VALUE_TYPE: 'WORK' }],
    COMPANY_TITLE: data.companyName || '',
    SOURCE_ID: resolveSourceId(data.source),
    SOURCE_DESCRIPTION: data.source || '',
    STATUS_ID: LEAD_DEFAULTS.STATUS_ID,
    OPENED: LEAD_DEFAULTS.OPENED,
    CURRENCY_ID: LEAD_DEFAULTS.CURRENCY_ID,
    ASSIGNED_BY_ID: getAssignedId()
  };
  if (data.email) fields.EMAIL = [{ VALUE: data.email, VALUE_TYPE: 'WORK' }];
  if (data.post) fields.POST = data.post;
  return bitrixCall('crm.lead.add', { fields: fields, params: { REGISTER_SONET_EVENT: 'Y' } }).result;
}

function updateLead(leadId, data) {
  var fields = { STATUS_ID: LEAD_DEFAULTS.STATUS_ID, SOURCE_DESCRIPTION: data.source || '' };
  var sourceId = resolveSourceId(data.source);
  if (sourceId) fields.SOURCE_ID = sourceId;
  if (data.email) fields.EMAIL = [{ VALUE: data.email, VALUE_TYPE: 'WORK' }];
  if (data.post) fields.POST = data.post;
  if (data.companyName) fields.COMPANY_TITLE = data.companyName;
  bitrixCall('crm.lead.update', { id: leadId, fields: fields });
}

function createContact(data) {
  var name = splitName(data.fullName);
  var fields = {
    NAME: name.NAME, LAST_NAME: name.LAST_NAME,
    PHONE: [{ VALUE: data.phone, VALUE_TYPE: 'WORK' }],
    SOURCE_ID: CONTACT_DEFAULTS.SOURCE_ID,
    OPENED: CONTACT_DEFAULTS.OPENED,
    TYPE_ID: CONTACT_DEFAULTS.TYPE_ID,
    ASSIGNED_BY_ID: getAssignedId(),
    UF_CRM_CONTACT_LIFECYCLE_STAGE: CONTACT_DEFAULTS.UF_CRM_CONTACT_LIFECYCLE_STAGE
  };
  if (data.email) fields.EMAIL = [{ VALUE: data.email, VALUE_TYPE: 'WORK' }];
  if (data.post) fields.POST = data.post;
  if (data.facebook) fields.WEB = [{ VALUE: data.facebook, VALUE_TYPE: 'FACEBOOK' }];
  return bitrixCall('crm.contact.add', { fields: fields, params: { REGISTER_SONET_EVENT: 'Y' } }).result;
}

function createCompany(data) {
  var fields = {
    TITLE: data.companyName,
    COMPANY_TYPE: COMPANY_DEFAULTS.COMPANY_TYPE,
    OPENED: COMPANY_DEFAULTS.OPENED,
    ASSIGNED_BY_ID: getAssignedId()
  };
  if (data.companyWeb) fields.WEB = [{ VALUE: data.companyWeb, VALUE_TYPE: 'WORK' }];
  return bitrixCall('crm.company.add', { fields: fields, params: { REGISTER_SONET_EVENT: 'Y' } }).result;
}

// ── Requisite + Address (VietQR) ──

function createRequisiteWithAddress(companyId, taxId, vietqrData) {
  var reqRes = bitrixCall('crm.requisite.add', { fields: {
    ENTITY_TYPE_ID: REQUISITE_DEFAULTS.ENTITY_TYPE_ID,
    ENTITY_ID: companyId,
    PRESET_ID: REQUISITE_DEFAULTS.PRESET_ID,
    NAME: vietqrData.name || taxId,
    ACTIVE: REQUISITE_DEFAULTS.ACTIVE,
    ADDRESS_ONLY: REQUISITE_DEFAULTS.ADDRESS_ONLY,
    RQ_COMPANY_NAME: vietqrData.name || '',
    RQ_VAT_ID: taxId
  }});
  var requisiteId = reqRes.result;

  var addressOk = false;
  if (vietqrData.address) {
    try {
      var addr = parseVietQRAddress(vietqrData.address);
      bitrixCall('crm.address.add', { fields: {
        TYPE_ID: ADDRESS_LEGAL_TYPE_ID, ENTITY_TYPE_ID: ADDRESS_ENTITY_TYPE_ID,
        ENTITY_ID: String(requisiteId),
        ADDRESS_1: addr.ADDRESS_1, ADDRESS_2: addr.ADDRESS_2,
        CITY: addr.CITY, REGION: addr.REGION, PROVINCE: addr.PROVINCE, COUNTRY: addr.COUNTRY
      }});
      addressOk = true;
    } catch (e) { Logger.log('Address error: ' + e.message); }
  }
  return { requisiteId: requisiteId, addressOk: addressOk };
}

// ── Link entities ──

function linkEntitiesToLead(leadId, contactId, companyId) {
  var fields = {};
  if (contactId) fields.CONTACT_ID = contactId;
  if (companyId) fields.COMPANY_ID = companyId;
  if (Object.keys(fields).length > 0) bitrixCall('crm.lead.update', { id: leadId, fields: fields });
}

function linkContactToCompany(contactId, companyId) {
  if (!contactId || !companyId) return;
  bitrixCall('crm.contact.update', { id: contactId, fields: { COMPANY_ID: companyId } });
}

// ── Timeline comment (BBCode) ──

function addTimelineComment(leadId, bbcodeContent) {
  bitrixCall('crm.timeline.comment.add', {
    fields: { ENTITY_ID: leadId, ENTITY_TYPE: 'lead', COMMENT: bbcodeContent }
  });
}

// ── Error logging ──

function logError(context, error) {
  Logger.log('[' + new Date().toISOString() + '] ' + context + ': ' + error.message);
  try {
    var sheetId = PropertiesService.getScriptProperties().getProperty('ERROR_LOG_SHEET');
    if (!sheetId) return;
    var ss = SpreadsheetApp.openById(sheetId);
    var sheet = ss.getSheetByName('Error Log');
    if (!sheet) { sheet = ss.insertSheet('Error Log'); sheet.appendRow(['Timestamp', 'Context', 'Error', 'Stack']); }
    sheet.appendRow([new Date(), context, error.message, error.stack || '']);
  } catch (e) {}
}

// ── Main handler ──

function onFormSubmit(e) {
  try {
    var values = e.values || [];
    var data = parseFormData(values);

    if (!data.phone) {
      logError('validate', new Error('SĐT trống. Row: ' + JSON.stringify(values.slice(0, 5))));
      return;
    }

    // 3. Match / Create Lead
    var leadId;
    var existingLead = findLeadByPhone(data.phone);
    if (existingLead) { leadId = parseInt(existingLead.ID, 10); updateLead(leadId, data); }
    else { leadId = createLead(data); }

    // 4. Match / Create Contact (phone > email)
    var contactId, linkedCompanyId = null;
    var existingContact = findContact(data);
    if (existingContact) {
      contactId = parseInt(existingContact.ID, 10);
      linkedCompanyId = existingContact.COMPANY_ID ? parseInt(existingContact.COMPANY_ID, 10) : null;
    } else { contactId = createContact(data); }

    // 5. Match / Create Company (phone > MST > website > title)
    var companyId = null, companyFoundByMST = false;
    if (data.companyName || data.taxId || data.companyWeb || linkedCompanyId) {
      var cr = findCompany(data, linkedCompanyId);
      if (cr.company) { companyId = parseInt(cr.company.ID, 10); companyFoundByMST = (cr.method === 'mst'); }
      else if (data.companyName) { companyId = createCompany(data); }
    }

    // 6. VietQR → Requisite (skip if found by MST)
    if (companyId && data.taxId && !companyFoundByMST) {
      try {
        var vietqr = vietqrLookup(data.taxId);
        if (vietqr) createRequisiteWithAddress(companyId, data.taxId, vietqr);
      } catch (reqErr) { logError('requisite', reqErr); }
    }

    // 7. Link entities
    linkEntitiesToLead(leadId, contactId, companyId);
    if (contactId && companyId) linkContactToCompany(contactId, companyId);

    // 8. Post BBCode comment
    try {
      var bb = buildSurveyBBCode(values, data.fullName, data.companyName, data.timestamp);
      addTimelineComment(leadId, bb);
    } catch (commentErr) { logError('timeline_comment', commentErr); }

    Logger.log('Done. Lead=' + leadId + ' Contact=' + contactId + ' Company=' + companyId);
  } catch (err) { logError('onFormSubmit', err); }
}

function parseFormData(values) {
  return {
    timestamp: cleanStr(values[COL.TIMESTAMP]),
    fullName: cleanStr(values[COL.FULL_NAME]),
    email: cleanStr(values[COL.EMAIL]),
    phone: normalizePhone(values[COL.PHONE]),
    facebook: cleanStr(values[COL.FACEBOOK]),
    post: cleanStr(values[COL.ROLE]),
    source: cleanStr(values[COL.SOURCE]),
    companyName: cleanStr(values[COL.COMPANY_NAME]),
    taxId: cleanStr(values[COL.TAX_ID]),
    companyWeb: cleanStr(values[COL.COMPANY_WEB])
  };
}

// ── Setup (chạy 1 lần từ GAS editor) ──

function setupProperties() {
  PropertiesService.getScriptProperties().setProperties({
    'BITRIX_WEBHOOK': 'https://tamgiac.bitrix24.com/rest/1/YOUR_TOKEN_HERE/',
    'DEFAULT_ASSIGNED': '1',
    'FORM_SHEET_ID': '1TlJVGqQD9peFQ94oYMNOucC0jtosHiI0P0m3uUz5L4o',
    'ERROR_LOG_SHEET': ''
  });
}

function createTrigger() {
  var triggers = ScriptApp.getProjectTriggers();
  for (var i = 0; i < triggers.length; i++) {
    if (triggers[i].getHandlerFunction() === 'onFormSubmit') return;
  }
  var sheetId = PropertiesService.getScriptProperties().getProperty('FORM_SHEET_ID');
  var ss = SpreadsheetApp.openById(sheetId);
  ScriptApp.newTrigger('onFormSubmit').forSpreadsheet(ss).onFormSubmit().create();
}
```

---

## Troubleshooting

| Triệu chứng | Nguyên nhân | Cách xử lý |
|-------------|-------------|-------------|
| Lead không cập nhật sau form | SĐT form khác SĐT Lead | Kiểm tra format (0xxx vs +84xxx) |
| Lead tạo mới thay vì update | Match SĐT không tìm thấy | `clasp logs` → xem search result |
| Contact/Company không tạo | API error từ Bitrix | `clasp logs` hoặc tab "Error Log" |
| Trigger không chạy | Installable trigger disconnect | GAS editor → Triggers → tạo lại |
| INVALID_CREDENTIALS | Webhook token sai/hết hạn | Script Properties → sửa `BITRIX_WEBHOOK` |
| Requisite không tạo | Company đã found by MST | Đúng logic — skip duplicate |

---

## Liên kết

| Đi đến | Link |
|--------|------|
| SOP Bước 02 (Survey) | [Link](../buoc/02-survey.md) |
| Lead Capture — tổng hợp sources | [Link](n8n-lead-capture.md) |
| Tổng quan hệ thống | [Link](overview.md) |
| GitHub repo | [`synity-gas`](https://github.com/chinhdang/synity-gas) |
| Trang chủ | [Link](../README.md) |
