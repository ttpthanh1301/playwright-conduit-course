# Test Case màn Sign up theo EP, BVA, Decision Table

Dưới đây là bộ **test case cho màn Sign up** dựa trên 3 kỹ thuật: **Equivalence Partitioning**, **Boundary Value Analysis**, và **Decision Table**.

Có các rule sau:

- **Email**: bắt buộc, đúng định dạng email
- **Username**: bắt buộc, tối thiểu **3** ký tự, tối đa **20** ký tự
- **Password**: bắt buộc, tối thiểu **8** ký tự, tối đa **20** ký tự
- Khi dữ liệu không hợp lệ, form hiển thị lỗi tương ứng
- Khi dữ liệu hợp lệ, đăng ký thành công

---

## 1. Scope kiểm thử

**Đối tượng kiểm thử:** Form **Sign up**

**Các thành phần cần test:**

- Trường **Email**
- Trường **Username**
- Trường **Password**
- Nút **Sign up**

**Hành vi kỳ vọng:**

- Validate từng field
- Hiển thị đúng message lỗi
- Không submit khi dữ liệu không hợp lệ
- Submit thành công khi toàn bộ dữ liệu hợp lệ

---

## 2. Assumption / Business Rules

| Field    | Rule                                   |
| -------- | -------------------------------------- |
| Email    | Bắt buộc, đúng định dạng email         |
| Username | Bắt buộc, độ dài từ **3 đến 20** ký tự |
| Password | Bắt buộc, độ dài từ **8 đến 20** ký tự |

**Error message**

- `email is invalid`
- `username is too short (minimum is 3 characters)`
- `username is too long (maximum is 20 characters)`
- `password is too short (minimum is 8 characters)`
- `password is too long (maximum is 20 characters)`

---

## 3. Test case theo Equivalence Partitioning

### 3.1. Email

#### EP-EMAIL-01 — Email hợp lệ

**Mục tiêu:** kiểm tra vùng dữ liệu hợp lệ  
**Test data:** `automation.test@example.com`

**Steps:**

1. Mở màn Sign up
2. Nhập Email = `automation.test@example.com`
3. Nhập Username hợp lệ = `tester123`
4. Nhập Password hợp lệ = `Playwright123`
5. Click **Sign up**

**Expected result:**

- Không hiển thị lỗi email
- Form submit thành công

---

#### EP-EMAIL-02 — Email sai format

**Test data:** `notanemail`

**Steps:**

1. Mở màn Sign up
2. Nhập Email = `notanemail`
3. Nhập Username hợp lệ
4. Nhập Password hợp lệ
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi: `email is invalid`
- Không submit form

---

#### EP-EMAIL-03 — Email bỏ trống

**Test data:** `(empty)`

**Steps:**

1. Mở màn Sign up
2. Để trống Email
3. Nhập Username hợp lệ
4. Nhập Password hợp lệ
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi email
- Không submit form

> **Lưu ý:** hệ thống có thể dùng cùng message `email is invalid` cho empty hoặc có message required riêng. Cần confirm lại với BA/dev/spec.

---

### 3.2. Username

#### EP-USER-01 — Username hợp lệ

**Test data:** `tester123`

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Nhập Username = `tester123`
4. Nhập Password hợp lệ
5. Click **Sign up**

**Expected result:**

- Không hiển thị lỗi username
- Nếu các field khác hợp lệ thì submit thành công

---

#### EP-USER-02 — Username quá ngắn

**Test data:** `ab`

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Nhập Username = `ab`
4. Nhập Password hợp lệ
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi: `username is too short (minimum is 3 characters)`
- Không submit form

---

#### EP-USER-03 — Username quá dài

**Test data:** `abcdefghijklmnopqrstu` (21 ký tự)

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Nhập Username = `abcdefghijklmnopqrstu`
4. Nhập Password hợp lệ
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi: `username is too long (maximum is 20 characters)`
- Không submit form

---

#### EP-USER-04 — Username bỏ trống

**Test data:** `(empty)`

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Để trống Username
4. Nhập Password hợp lệ
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi username too short hoặc required, tùy rule hệ thống
- Không submit form

---

### 3.3. Password

#### EP-PASS-01 — Password hợp lệ

**Test data:** `Playwright123`

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Nhập Username hợp lệ
4. Nhập Password = `Playwright123`
5. Click **Sign up**

**Expected result:**

- Không hiển thị lỗi password
- Nếu các field khác hợp lệ thì submit thành công

---

#### EP-PASS-02 — Password quá ngắn

**Test data:** `Abc1234` (7 ký tự)

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Nhập Username hợp lệ
4. Nhập Password = `Abc1234`
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi: `password is too short (minimum is 8 characters)`
- Không submit form

---

#### EP-PASS-03 — Password quá dài

**Test data:** `Abcdefghijklmnopqrstu` (21 ký tự)

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Nhập Username hợp lệ
4. Nhập Password = `Abcdefghijklmnopqrstu`
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi: `password is too long (maximum is 20 characters)`
- Không submit form

---

#### EP-PASS-04 — Password bỏ trống

**Test data:** `(empty)`

**Steps:**

1. Mở màn Sign up
2. Nhập Email hợp lệ
3. Nhập Username hợp lệ
4. Để trống Password
5. Click **Sign up**

**Expected result:**

- Hiển thị lỗi password too short hoặc required, tùy rule hệ thống
- Không submit form

---

## 4. Test case theo Boundary Value Analysis

### 4.1. Boundary cho Username

**Rule:** `3 <= length <= 20`

| TC ID       | Test data               | Độ dài | Kỳ vọng       |
| ----------- | ----------------------- | -----: | ------------- |
| BVA-USER-01 | `ab`                    |      2 | Lỗi too short |
| BVA-USER-02 | `abc`                   |      3 | Hợp lệ        |
| BVA-USER-03 | `abcd`                  |      4 | Hợp lệ        |
| BVA-USER-04 | `abcdefghijklmnopqrst`  |     20 | Hợp lệ        |
| BVA-USER-05 | `abcdefghijklmnopqrstu` |     21 | Lỗi too long  |

#### BVA-USER-02 — Username ở min boundary hợp lệ

**Test data:** `abc`

**Steps:**

1. Nhập Email hợp lệ
2. Nhập Username = `abc`
3. Nhập Password hợp lệ
4. Click **Sign up**

**Expected result:**

- Không hiển thị lỗi username
- Submit thành công

---

### 4.2. Boundary cho Password

**Rule:** `8 <= length <= 20`

| TC ID       | Test data               | Độ dài | Kỳ vọng       |
| ----------- | ----------------------- | -----: | ------------- |
| BVA-PASS-01 | `Abc1234`               |      7 | Lỗi too short |
| BVA-PASS-02 | `Abc12345`              |      8 | Hợp lệ        |
| BVA-PASS-03 | `Abc123456`             |      9 | Hợp lệ        |
| BVA-PASS-04 | `Abcdefghijklmnop1234`  |     20 | Hợp lệ        |
| BVA-PASS-05 | `Abcdefghijklmnop12345` |     21 | Lỗi too long  |

---

### 4.3. Boundary cho Email

Email thường không có boundary rõ ràng từ UI screenshot, nên với màn này **Equivalence Partitioning phù hợp hơn Boundary Value Analysis**.

Nếu spec có giới hạn độ dài email thì có thể bổ sung:

- Min valid email
- Max valid email length
- Email vượt quá max length

Hiện tại, từ ảnh màn hình **chưa đủ căn cứ** để viết boundary cho email length.

---

## 5. Test case theo Decision Table

Mục tiêu: kiểm tra hành vi hệ thống khi kết hợp nhiều điều kiện.

### 5.1. Decision table tổng quát

| Rule  | Email   | Username  | Password  | Expected result                               |
| ----- | ------- | --------- | --------- | --------------------------------------------- |
| DT-01 | Valid   | Valid     | Valid     | Sign up thành công                            |
| DT-02 | Invalid | Valid     | Valid     | Hiển thị lỗi email, không submit              |
| DT-03 | Empty   | Valid     | Valid     | Hiển thị lỗi email, không submit              |
| DT-04 | Valid   | Too short | Valid     | Hiển thị lỗi username too short, không submit |
| DT-05 | Valid   | Too long  | Valid     | Hiển thị lỗi username too long, không submit  |
| DT-06 | Valid   | Empty     | Valid     | Hiển thị lỗi username, không submit           |
| DT-07 | Valid   | Valid     | Too short | Hiển thị lỗi password too short, không submit |
| DT-08 | Valid   | Valid     | Too long  | Hiển thị lỗi password too long, không submit  |
| DT-09 | Valid   | Valid     | Empty     | Hiển thị lỗi password, không submit           |
| DT-10 | Invalid | Too short | Too short | Hiển thị đồng thời 3 lỗi, không submit        |
| DT-11 | Empty   | Empty     | Empty     | Hiển thị toàn bộ lỗi, không submit            |

---

### 5.2. Chi tiết test case Decision Table

#### DT-01 — Tất cả dữ liệu hợp lệ

**Data:**

- Email: `automation.test@example.com`
- Username: `tester123`
- Password: `Playwright123`

**Expected result:**

- Tạo tài khoản thành công
- Redirect tự động đăng nhập user

---

#### DT-02 — Email invalid, username/password valid

**Data:**

- Email: `notanemail`
- Username: `tester123`
- Password: `Playwright123`

**Expected result:**

- Hiển thị `email is invalid`
- Không submit

---

#### DT-04 — Username too short, field khác valid

**Data:**

- Email: `automation.test@example.com`
- Username: `ab`
- Password: `Playwright123`

**Expected result:**

- Hiển thị `username is too short (minimum is 3 characters)`
- Không submit

---

#### DT-07 — Password too short, field khác valid

**Data:**

- Email: `automation.test@example.com`
- Username: `tester123`
- Password: `Abc1234`

**Expected result:**

- Hiển thị `password is too short (minimum is 8 characters)`
- Không submit

---

#### DT-10 — Nhiều field cùng sai

**Data:**

- Email: `notanemail`
- Username: `ab`
- Password: `123`

**Expected result:**

- Hiển thị đầy đủ các lỗi:
  - `email is invalid`
  - `username is too short (minimum is 3 characters)`
  - `password is too short (minimum is 8 characters)`
- Không submit form

---

#### DT-11 — Tất cả field bỏ trống

**Data:**

- Email: `(empty)`
- Username: `(empty)`
- Password: `(empty)`

**Expected result:**

- Hiển thị lỗi cho tất cả field
- Không submit form

---

## 6. Bộ test case gọn để chạy thực tế

Nếu cần bộ **smoke/regression ngắn gọn nhưng vẫn phủ tốt**, có thể ưu tiên 10 case sau:

| Priority | TC ID | Mô tả                                         |
| -------- | ----- | --------------------------------------------- |
| High     | TC-01 | Sign up thành công với toàn bộ dữ liệu hợp lệ |
| High     | TC-02 | Email sai format                              |
| High     | TC-03 | Email bỏ trống                                |
| High     | TC-04 | Username = 2 ký tự                            |
| High     | TC-05 | Username = 3 ký tự                            |
| High     | TC-06 | Username = 20 ký tự                           |
| High     | TC-07 | Username = 21 ký tự                           |
| High     | TC-08 | Password = 7 ký tự                            |
| High     | TC-09 | Password = 8 ký tự                            |
| High     | TC-10 | Password = 21 ký tự                           |

---

## 7. Test data đề xuất

### Valid data

- **Email:** `automation.test@example.com`
- **Username 3 ký tự:** `abc`
- **Username 20 ký tự:** `abcdefghijklmnopqrst`
- **Password 8 ký tự:** `Abc12345`
- **Password 20 ký tự:** `Abcdefghijklmnop1234`

### Invalid data

- **Email invalid:** `notanemail`
- **Username 2 ký tự:** `ab`
- **Username 21 ký tự:** `abcdefghijklmnopqrstu`
- **Password 7 ký tự:** `Abc1234`
- **Password 21 ký tự:** `Abcdefghijklmnop12345`

---

## 8. Các điểm nên cover thêm

Ngoài 3 kỹ thuật trên, khi test thực tế nên kiểm thêm:

- Có trim khoảng trắng đầu/cuối không  
  Ví dụ:
  - Email: `"  automation.test@example.com  "`
  - Username: `"  tester123  "`

- Email có phân biệt chữ hoa/chữ thường không  
  Ví dụ: `Automation.Test@Example.com`

- Username có cho phép ký tự đặc biệt không  
  Ví dụ:
  - `tester_01`
  - `tester.01`
  - `tester@01`

- Password có được mask đúng không
- Click nhiều lần nút **Sign up** có tạo duplicate request không
- Sau khi hiển thị lỗi, nếu user sửa lại đúng thì message lỗi có biến mất không
- Nếu email đã tồn tại thì có hiển thị đúng lỗi business không

---
