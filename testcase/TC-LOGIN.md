# Test Case - Sign in (Generic Error Message)

## 1. Scope

**Màn hình:** Sign in  

**Field:**
- Email
- Password
- Button: Sign in

**Đặc điểm quan trọng:**
- Không validate format riêng từng field
- Chỉ có **1 error message chung**:
  - `email or password is invalid`

---

## 2. Business Rule

| Condition | Expected |
|----------|--------|
| Email + Password đúng | Login thành công |
| Sai Email hoặc Password | Hiển thị lỗi chung |
| Bất kỳ field nào sai | Không login |

---

## 3. Test case theo Equivalence Partitioning

### EP-LOGIN-01 — Login thành công
**Test data:**
- Email: `automation.test@example.com`
- Password: `Playwright123`

**Steps:**
1. Mở màn Sign in
2. Nhập email hợp lệ
3. Nhập password đúng
4. Click **Sign in**

**Expected result:**
- Login thành công
- Redirect về trang Home `/`

---

### EP-LOGIN-02 — Email không tồn tại
**Test data:**
- Email: `notfound@example.com`
- Password: `Playwright123`

**Expected result:**
- Hiển thị: `email or password is invalid`
- Không login

---

### EP-LOGIN-03 — Password sai
**Test data:**
- Email: `automation.test@example.com`
- Password: `WrongPass123`

**Expected result:**
- Hiển thị: `email or password is invalid`
- Không login

---

### EP-LOGIN-04 — Email sai format
**Test data:**
- Email: `abc`
- Password: `Playwright123`

**Expected result:**
- KHÔNG validate format riêng
- Hiển thị: `email or password is invalid`

---

### EP-LOGIN-05 — Email bỏ trống
**Test data:**
- Email: `(empty)`
- Password: `Playwright123`

**Expected result:**
- Hiển thị: `email or password is invalid`
- Không login

---

### EP-LOGIN-06 — Password bỏ trống
**Test data:**
- Email: `automation.test@example.com`
- Password: `(empty)`

**Expected result:**
- Hiển thị: `email or password is invalid`
- Không login

---

### EP-LOGIN-07 — Cả 2 field bỏ trống
**Test data:**
- Email: `(empty)`
- Password: `(empty)`

**Expected result:**
- Hiển thị: `email or password is invalid`
- Không login

---

## 4. Test case theo Decision Table

| TC ID | Email | Password | Expected result |
|------|------|---------|----------------|
| DT-01 | Valid | Valid | Login thành công |
| DT-02 | Invalid | Valid | Error |
| DT-03 | Valid | Invalid | Error |
| DT-04 | Invalid | Invalid | Error |
| DT-05 | Empty | Valid | Error |
| DT-06 | Valid | Empty | Error |
| DT-07 | Empty | Empty | Error |

---

## 5. Test case UI / Behavior

### UI-01 — Hiển thị error message
**Steps:**
1. Nhập sai thông tin
2. Click Sign in

**Expected result:**
- Hiển thị message:
  - `email or password is invalid`
- Message màu đỏ đúng UI

---

### UI-02 — Error biến mất khi user sửa input
**Steps:**
1. Nhập sai → click Sign in
2. Sửa lại đúng email/password

**Expected result:**
- Error message biến mất (hoặc update đúng behavior system)

---

### UI-03 — Password được mask
**Expected result:**
- Password hiển thị dạng `•••••`

---

### UI-04 — Button Sign in clickable
**Expected result:**
- Button luôn clickable (không disable)

---

## 6. Negative / Security (Senior QA nên cover)

### SEC-01 — SQL Injection
**Email:** `' OR 1=1 --`  
**Expected:**
- Không login
- Hiển thị error chung

---

### SEC-02 — Script Injection
**Email:** `<script>alert(1)</script>`  
**Expected:**
- Không execute script
- Hiển thị error

---

### SEC-03 — Brute force
**Steps:**
- Login sai nhiều lần liên tục

**Expected:**
- Có rate limit / captcha (nếu hệ thống có)

---

## 7. Edge cases

### EDGE-01 — Trim space
**Data:**
- Email: `"  automation.test@example.com  "`
- Password: `"  Playwright123  "`

**Expected:**
- System trim → login thành công

---

### EDGE-02 — Case sensitivity email
**Data:**
- Email: `Automation.Test@Example.com`

**Expected:**
- Vẫn login được (email không phân biệt hoa thường)

---

### EDGE-03 — Double click login
**Steps:**
- Click Sign in liên tục

**Expected:**
- Không gửi duplicate request