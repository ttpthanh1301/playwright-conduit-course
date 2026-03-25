# 🎭 Khóa Học Playwright Automation với JavaScript

> **Dành cho:** Người đã biết lập trình — bắt đầu thẳng vào Playwright  
> **Web thực hành:** [Conduit](https://conduit.bondaracademy.com) — ứng dụng blog thực tế  
> **Công cụ:** Playwright + JavaScript (Node.js)

---

## 📋 Mục Lục

- [Giới thiệu khóa học](#-giới-thiệu-khóa-học)
- [Cài đặt môi trường](#-cài-đặt-môi-trường)
- [Cấu trúc dự án](#-cấu-trúc-dự-án)
- [Bài 1 — Thiết lập môi trường](#bài-1--thiết-lập-môi-trường)
- [Bài 2 — Phân tích & Thiết kế Test Case ⭐](#bài-2--phân-tích--thiết-kế-test-case-)
- [Bài 3 — Cấu trúc Test & Codegen](#bài-3--cấu-trúc-test--codegen)
- [Bài 4 — Locator Strategy](#bài-4--locator-strategy)
- [Bài 5 — Xử lý UI Components](#bài-5--xử-lý-ui-components)
- [Bài 6 — Assertions](#bài-6--assertions)
- [Bài 7 — Auto-waiting & Debugging](#bài-7--auto-waiting--debugging)
- [Bài 8 — Test Hooks & Grouping](#bài-8--test-hooks--grouping)
- [Bài 9 — POM: Tư duy thiết kế](#bài-9--pom-tư-duy-thiết-kế)
- [Bài 10 — POM: Thực hành](#bài-10--pom-thực-hành)
- [Bài 11 — Fixtures & CLI](#bài-11--fixtures--cli)
- [Bài 12 — API Testing](#bài-12--api-testing)
- [Bài 13 — Reporting & Artifacts](#bài-13--reporting--artifacts)
- [Bài 14 — Git & GitHub](#bài-14--git--github)
- [Bài 15 — GitLab CI/CD](#bài-15--gitlab-cicd)
- [Tài khoản thực hành](#-tài-khoản-thực-hành)

---

## 🎯 Giới thiệu khóa học

Khóa học này hướng dẫn bạn xây dựng bộ **Automation Test hoàn chỉnh** cho ứng dụng web thực tế **Conduit** — một nền tảng blog theo chuẩn RealWorld App — chỉ với một web duy nhất xuyên suốt 15 bài học.

### Tại sao chọn Conduit?

| Tính năng Conduit              | Kỹ năng Automation tương ứng |
| ------------------------------ | ---------------------------- |
| Màn hình Login / Register      | Locators, Form, Validation   |
| Đăng bài viết (Create Article) | CRUD, Hooks, beforeEach      |
| Sửa / Xóa bài viết             | POM, Fixtures                |
| Follow / Unfollow user         | UI Components, Assertions    |
| Feed & Tags                    | Web Table, Filter            |
| Comment                        | Dynamic element, Auto-wait   |
| Settings (Profile)             | Upload, Form phức tạp        |
| API nội bộ (`/api/...`)        | API Testing, Token chaining  |

### Lộ trình học

```
Bài 1      │ Nền tảng: Môi trường & Project Setup
Bài 2      │ QA Foundation: Phân tích & Thiết kế Test Case  ← MỚI
Bài 3–4    │ Core Playwright: Test structure + Locators
Bài 5–7    │ Kỹ năng: UI Components + Assertions + Debug
Bài 8–11   │ Kiến trúc: Hooks + POM + Fixtures
Bài 12     │ Nâng cao: API Testing
Bài 13–15  │ Chuyên nghiệp: Reporting + Git + CI/CD
```

---

## ⚙️ Cài đặt môi trường

### Yêu cầu

- [Node.js LTS](https://nodejs.org/) (kiểm tra: `node -v`)
- [Visual Studio Code](https://code.visualstudio.com/)
- Extension VS Code: **Playwright Test for VSCode** (Microsoft)

### Khởi tạo dự án

```bash
# 1. Tạo thư mục dự án
mkdir playwright-conduit-course
cd playwright-conduit-course

# 2. Khởi tạo Playwright
npm init playwright@latest
# Chọn: JavaScript | tests/ | No GitHub Actions | Yes install browsers

# 3. Tạo cấu trúc thư mục
mkdir pages fixtures test-data utils
```

---

## 📁 Cấu trúc dự án

```
playwright-conduit-course/
│
├── pages/                        # Page Object Model
│   ├── LoginPage.js
│   ├── RegisterPage.js
│   ├── HomePage.js
│   ├── ArticlePage.js
│   ├── EditorPage.js
│   └── SettingsPage.js
│
├── fixtures/                     # Custom Fixtures
│   └── conduitFixtures.js
│
├── tests/                        # Test Cases
│   ├── bai3-first-test.spec.js
│   ├── bai4-locators.spec.js
│   ├── bai5-ui-components.spec.js
│   ├── bai6-assertions.spec.js
│   ├── bai7-debug.spec.js
│   ├── bai8-hooks.spec.js
│   ├── bai10-pom.spec.js
│   ├── bai11-fixtures.spec.js
│   ├── bai12-api.spec.js
│   └── bai13-reporting.spec.js
│
├── test-data/                    # Dữ liệu test
│   ├── users.json
│   ├── articles.json
│   └── test-cases/               # ← Lưu test case & checklist (từ Bài 2)
│       ├── TC-LOGIN.md
│       ├── TC-ARTICLE.md
│       └── traceability-matrix.md
│
├── utils/                        # Hàm tiện ích
│   └── helpers.js
│
├── playwright.config.js          # Cấu hình dự án
└── package.json
```

### File `playwright.config.js`

```javascript
const { defineConfig, devices } = require("@playwright/test");

module.exports = defineConfig({
  testDir: "./tests",
  fullyParallel: false,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: [["html", { outputFolder: "playwright-report", open: "never" }]],

  use: {
    baseURL: "https://conduit.bondaracademy.com",
    screenshot: "only-on-failure",
    video: "retain-on-failure",
    trace: "on-first-retry",
    headless: false,
  },

  projects: [
    {
      name: "chromium",
      use: { ...devices["Desktop Chrome"] },
    },
  ],
});
```

---

## Bài 1 — Thiết lập môi trường

**Mục tiêu:** Cài đặt và chạy được test đầu tiên trên Conduit.

### Lý thuyết

- **Playwright** là framework automation test do Microsoft phát triển, tự động tải Chromium, Firefox, WebKit
- Cấu trúc thư mục sau khi `npm init playwright@latest`:
  - `tests/` — chứa file test
  - `playwright.config.js` — não của dự án, cấu hình browser, baseURL, timeout...
  - `package.json` — quản lý thư viện

### Playwright vs Selenium — Điểm khác biệt chính

| Playwright                       | Selenium                          |
| -------------------------------- | --------------------------------- |
| Auto-waiting tích hợp sẵn        | Phải tự viết wait                 |
| API async/await thuần            | API đồng bộ hoặc Promise thủ công |
| Trace Viewer tích hợp            | Cần tool bên ngoài                |
| Chạy được nhiều browser cùng lúc | Cần driver riêng cho từng browser |

### Thực hành

```bash
# Chạy test mẫu mặc định
npx playwright test

# Xem báo cáo HTML
npx playwright show-report

# Bật chế độ xem trình duyệt
# Vào playwright.config.js → headless: false → chạy lại
npx playwright test
```

---

## Bài 2 — Phân tích & Thiết kế Test Case ⭐

**Mục tiêu:** Biết mình đang test _cái gì_ và _tại sao_ — trước khi viết một dòng code nào.

> 💡 Đây là bài không có code. Mở Conduit trên trình duyệt, quan sát và tư duy như một QA thực thụ. Mọi automation từ Bài 3 trở đi đều bắt nguồn từ bài này.

---

### Phần 1 — Đọc hiểu ứng dụng như một QA

Trước khi viết test, hãy khám phá Conduit bằng tay và đặt **4 câu hỏi có hệ thống** cho mỗi feature:

| Câu hỏi                              | Áp dụng vào Conduit — Login                                           |
| ------------------------------------ | --------------------------------------------------------------------- |
| **Happy path là gì?**                | Email + password đúng → redirect về trang chủ, hiện username          |
| **Negative case nào có thể xảy ra?** | Sai password / email không tồn tại / bỏ trống field                   |
| **Boundary nào cần test?**           | Password tối thiểu mấy ký tự? Form có submit được khi bỏ trống không? |
| **Điều gì xảy ra sau mỗi action?**   | Login xong → URL đổi về `/`, nav bar thay đổi ra sao?                 |

### Phần 2 — Ba kỹ thuật thiết kế test case cốt lõi

#### Equivalence Partitioning — Chia vùng dữ liệu

Thay vì test 100 email khác nhau, chia thành **vùng hợp lệ** và **vùng không hợp lệ** — mỗi vùng chỉ cần 1 đại diện:

```
Email:
  ✅ Hợp lệ     → automation.test@example.com
  ❌ Sai   → notanemail@gmail.com
  ❌ Bỏ trống    → (empty)

Password:
  ✅ Đúng        → Playwright123
  ❌ Sai          → WrongPass999
  ❌ Bỏ trống    → (empty)
```

#### Boundary Value Analysis — Test tại ranh giới

Test ngay tại điểm giới hạn — nơi bug thường ẩn náu nhất:

```
Title bài viết (giả sử max 255 ký tự):
  ✅ 1 ký tự     → "A"               (min hợp lệ)
  ✅ 2 ký tự     → "AA"              (biên min hợp lệ)
  ✅ 254 ký tự   → "AAA...A"         (biên max hợp lệ)
  ✅ 255 ký tự   → "AAA...A"         (max hợp lệ)
  ❌ 0 ký tự     → (bỏ trống)        (dưới min)
  ❌ 256 ký tự   → "AAA...AA"        (vượt max)
```

#### Decision Table — Kết hợp nhiều điều kiện

Khi nhiều điều kiện cùng ảnh hưởng đến kết quả:

| Email         | Password | Kết quả kỳ vọng                     |
| ------------- | -------- | ----------------------------------- |
| Đúng          | Đúng     | Login thành công → redirect `/`     |
| Đúng          | Sai      | Lỗi: `email or password is invalid` |
| Không tồn tại | Đúng     | Lỗi: `email or password is invalid` |
| Bỏ trống      | Bỏ trống | Form không submit được              |

---

### Phần 3 — Viết test case theo format chuẩn

Mỗi test case phải trả lời được 5 câu hỏi: **Ai? Làm gì? Với gì? Điều kiện gì? Kỳ vọng gì?**

```
TC-LOGIN-001
─────────────────────────────────────────────────────────────
Title        : Đăng nhập thành công với tài khoản hợp lệ
Feature      : Authentication / Login
Priority     : High
Type         : Positive
Precondition : Tài khoản automation.test@example.com đã tồn tại

Steps:
  1. Mở trang https://conduit.bondaracademy.com/login
  2. Nhập vào ô Email: automation.test@example.com
  3. Nhập vào ô Password: Playwright123
  4. Click nút "Sign in"

Expected Result:
  ✅ Redirect về trang chủ — URL là /
  ✅ Nav bar hiển thị username "autotest"
  ✅ Link "Sign in" và "Sign up" biến mất
  ✅ Link "New Article" và "Settings" xuất hiện
─────────────────────────────────────────────────────────────
TC-ART-001
─────────────────────────────────────────────────────────────
Title        : Tạo bài viết mới với đầy đủ thông tin
Feature      : Article / Create
Priority     : High
Type         : Positive
Precondition : Đã đăng nhập thành công

Steps:
  1. Click link "New Article" trên nav
  2. Nhập Title: "Automation Test Article"
  3. Nhập About: "Bài viết demo Playwright"
  4. Nhập Body: "Nội dung bài viết"
  5. Nhập Tag: "playwright" → nhấn Enter
  6. Click nút "Publish Article"

Expected Result:
  ✅ Redirect về trang chi tiết bài viết
  ✅ Heading hiển thị đúng title vừa nhập
  ✅ Tag "playwright" hiển thị trên bài
  ✅ Author name đúng với tài khoản đang login
─────────────────────────────────────────────────────────────
```

---

### Phần 4 — Checklist theo feature

Checklist là công cụ **kiểm tra nhanh** — đảm bảo không bỏ sót case nào trước khi bắt tay viết automation.

**Quy tắc viết checklist:** Mỗi dòng phải là một điều kiện có thể **Pass hoặc Fail** độc lập với nhau.

````
╔══════════════════════════════════════════════════════════╗
║  CHECKLIST — SIGN IN                                    ║
╠══════════════════════════════════════════════════════════╣
║  Positive                                                ║
║  □ TC-LOGIN-001  Login thành công → redirect `/`         ║
║                                                          ║
║  Negative (Validation + Authentication)                  ║
║  □ TC-LOGIN-002  Email không tồn tại → error chung       ║
║  □ TC-LOGIN-003  Password sai → error chung              ║
║  □ TC-LOGIN-004  Email sai format → error chung          ║
║  □ TC-LOGIN-005  Bỏ trống email → error chung            ║
║  □ TC-LOGIN-006  Bỏ trống password → error chung         ║
║  □ TC-LOGIN-007  Bỏ trống cả 2 field → error chung       ║
║                                                          ║
║  UI / Behavior                                           ║
║  □ TC-LOGIN-008  Hiển thị đúng message error             ║
║  □ TC-LOGIN-009  Error biến mất khi sửa input            ║
║  □ TC-LOGIN-010  Password được mask                      ║
║  □ TC-LOGIN-011  Button Sign in luôn clickable           ║
║                                                          ║
║  Security                                                ║
║  □ TC-LOGIN-012  SQL Injection không login được          ║
║  □ TC-LOGIN-013  Script Injection không thực thi         ║
║  □ TC-LOGIN-014  Brute force (nhiều lần sai)             ║
║                                                          ║
║  Edge Case                                               ║
║  □ TC-LOGIN-015  Trim space input hoạt động đúng         ║
║  □ TC-LOGIN-016  Email không phân biệt hoa/thường        ║
║  □ TC-LOGIN-017  Double click không gửi duplicate        ║
╚══════════════════════════════════════════════════════════╝
```md
╔══════════════════════════════════════════════════════════╗
║  CHECKLIST — SIGN UP                                    ║
╠══════════════════════════════════════════════════════════╣
║  Positive                                                ║
║  □ TC-SIGNUP-001  Đăng ký thành công với dữ liệu hợp lệ  ║
║  □ TC-SIGNUP-002  Sau đăng ký → auto login / redirect    ║
║                                                          ║
║  Negative — Email                                        ║
║  □ TC-SIGNUP-003  Email sai format → lỗi                 ║
║  □ TC-SIGNUP-004  Email bỏ trống → lỗi                   ║
║  □ TC-SIGNUP-005  Email đã tồn tại → lỗi business        ║
║                                                          ║
║  Negative — Username                                     ║
║  □ TC-SIGNUP-006  Username < 3 ký tự → lỗi               ║
║  □ TC-SIGNUP-007  Username = 3 ký tự → hợp lệ            ║
║  □ TC-SIGNUP-008  Username = 20 ký tự → hợp lệ           ║
║  □ TC-SIGNUP-009  Username > 20 ký tự → lỗi              ║
║  □ TC-SIGNUP-010  Username bỏ trống → lỗi                ║
║                                                          ║
║  Negative — Password                                     ║
║  □ TC-SIGNUP-011  Password < 8 ký tự → lỗi               ║
║  □ TC-SIGNUP-012  Password = 8 ký tự → hợp lệ            ║
║  □ TC-SIGNUP-013  Password = 20 ký tự → hợp lệ           ║
║  □ TC-SIGNUP-014  Password > 20 ký tự → lỗi              ║
║  □ TC-SIGNUP-015  Password bỏ trống → lỗi                ║
║                                                          ║
║  Combination (Decision Table)                            ║
║  □ TC-SIGNUP-016  Email sai + field khác đúng → lỗi email║
║  □ TC-SIGNUP-017  Username sai + field khác đúng → lỗi   ║
║  □ TC-SIGNUP-018  Password sai + field khác đúng → lỗi   ║
║  □ TC-SIGNUP-019  Nhiều field sai → hiển thị nhiều lỗi   ║
║                                                          ║
║  UI / Behavior                                           ║
║  □ TC-SIGNUP-020  Hiển thị đúng message lỗi từng field   ║
║  □ TC-SIGNUP-021  Error biến mất khi sửa đúng            ║
║  □ TC-SIGNUP-022  Password được mask                     ║
║  □ TC-SIGNUP-023  Button Sign up hoạt động đúng          ║
║                                                          ║
║  Edge Case                                               ║
║  □ TC-SIGNUP-024  Trim space input hoạt động đúng        ║
║  □ TC-SIGNUP-025  Email không phân biệt hoa/thường       ║
║  □ TC-SIGNUP-026  Username cho ký tự đặc biệt            ║
║  □ TC-SIGNUP-027  Double click không tạo duplicate       ║
╚══════════════════════════════════════════════════════════╝
````

---

### Phần 5 — Traceability Matrix

Đây là thứ phân biệt **senior** và **junior** — biết mapping từ requirement → test case → automation file → status.

Lưu file này tại `test-data/test-cases/traceability-matrix.md` và cập nhật sau mỗi bài học:

| Feature  | Test Case ID | Mô tả              | Automation File         | Bài học | Status  |
| -------- | ------------ | ------------------ | ----------------------- | ------- | ------- |
| Login    | TC-LOGIN-001 | Login thành công   | bai4-locators.spec.js   | Bài 4   | ✅ Done |
| Login    | TC-LOGIN-002 | Sai password → lỗi | bai6-assertions.spec.js | Bài 6   | ✅ Done |
| Login    | TC-LOGIN-003 | Bỏ trống field     | bai6-assertions.spec.js | Bài 6   | ✅ Done |
| Article  | TC-ART-001   | Tạo bài viết mới   | bai8-hooks.spec.js      | Bài 8   | ✅ Done |
| Article  | TC-ART-003   | Xóa bài viết       | bai8-hooks.spec.js      | Bài 8   | ✅ Done |
| Comment  | TC-CMT-001   | Post comment       | bai8-hooks.spec.js      | Bài 8   | ✅ Done |
| Comment  | TC-CMT-003   | Xóa comment        | bai5-ui.spec.js         | Bài 5   | ✅ Done |
| Article  | TC-ART-001   | Tạo bài qua API    | bai12-api.spec.js       | Bài 12  | 🔲 Todo |
| Settings | TC-SET-001   | Đổi avatar URL     | bai5-ui.spec.js         | Bài 5   | 🔲 Todo |

### Thực hành

Không mở VS Code. Mở Conduit trên trình duyệt và hoàn thành 3 nhiệm vụ sau:

**Nhiệm vụ 1:** Khám phá tính năng **Register** — viết checklist với ít nhất 6 case (positive + negative + edge).

**Nhiệm vụ 2:** Viết đầy đủ test case theo format chuẩn cho `TC-ART-002` (Xóa bài viết) và `TC-CMT-001` (Post comment).

**Nhiệm vụ 3:** Lập Traceability Matrix ban đầu cho toàn bộ khóa học — chỉ cần điền cột Feature, Test Case ID, Mô tả. Các cột còn lại sẽ điền dần qua từng bài.

> ✅ **Output bắt buộc trước khi sang Bài 3:** Folder `test-data/test-cases/` phải có ít nhất 2 file checklist và 1 file traceability matrix.

---

## Bài 3 — Cấu trúc Test & Codegen

**Mục tiêu:** Hiểu cấu trúc file test, dùng Codegen ghi thao tác tự động.

### Lý thuyết

#### Cấu trúc file test

```javascript
// 1. Import thư viện
const { test, expect } = require("@playwright/test");

// 2. Describe — nhóm test case (= Test Suite)
test.describe("Conduit — Chức năng Login", () => {
  // 3. Test case cụ thể — tên phải khớp với Test Case ID ở Bài 2
  test("[TC-LOGIN-001] Đăng nhập thành công", async ({ page }) => {
    // Test Steps
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("user@test.com");
    await page.getByPlaceholder("Password").fill("password123");
    await page.getByRole("button", { name: "Sign in" }).click();

    // 4. Assertion — kiểm tra từng điểm trong Expected Result của test case
    await expect(page).toHaveURL("/");
  });
});
```

> 💡 **Thói quen tốt:** Đặt tên test theo format `[TC-ID] Mô tả ngắn` để trace thẳng về checklist ở Bài 2

> 🥇 **Quy tắc vàng:** Mọi lệnh tương tác browser đều **bắt buộc** có `await`

#### Codegen — Ghi thao tác tự động

```bash
npx playwright codegen https://conduit.bondaracademy.com
```

> Cửa sổ Inspector sẽ tự sinh code khi bạn thao tác trên trình duyệt

#### Refactor code từ Codegen

```javascript
// Code Codegen sinh ra (thừa click):
await page.getByPlaceholder("Email").click();
await page.getByPlaceholder("Email").fill("user@test.com");

// Sau khi refactor (gọn hơn):
await page.getByPlaceholder("Email").fill("user@test.com");

// ⚠️ Luôn THÊM assertion — Codegen không tự sinh cái này!
// Assertion phải khớp với Expected Result trong test case đã viết ở Bài 2
await expect(page).toHaveURL("/");
```

### Luyện tập

1. Chạy Codegen trỏ vào Conduit
2. Thao tác theo đúng **Steps** của `TC-LOGIN-001` đã viết ở Bài 2: **Login → Tạo bài viết mới → Publish**
3. Copy code vào file `tests/bai3-first-test.spec.js`
4. Refactor: xóa click thừa, thêm assertion khớp với **Expected Result** của từng test case

---

## Bài 4 — Locator Strategy

**Mục tiêu:** Chọn locator đúng để test không bị "gãy" khi dev sửa code.

### Thứ tự ưu tiên locator

```
1. getByRole()        ← Ưu tiên số 1 (giống user thật)
2. getByLabel()       ← Tốt cho form input
3. getByPlaceholder() ← Khi không có label
4. getByText()        ← Cho nội dung tĩnh
5. getByTestId()      ← Khi dev gắn data-testid
6. CSS / XPath        ← Chỉ dùng khi bó tay
```

### Áp dụng vào Conduit

```javascript
// ✅ Tìm nút "Sign in" — dùng role
await page.getByRole("button", { name: "Sign in" }).click();

// ✅ Tìm ô Email — dùng placeholder (Conduit không có label)
await page.getByPlaceholder("Email").fill("user@test.com");

// ✅ Tìm link "New Article" trên nav
await page.getByRole("link", { name: "New Article" }).click();

// ✅ Kiểm tra thông báo lỗi xuất hiện
await page.getByText("email or password is invalid").toBeVisible();

// ✅ Tìm tiêu đề bài viết (heading)
await page.getByRole("heading", { name: "Tên bài viết của bạn" });

// ⚠️ Tránh dùng — phụ thuộc vào cấu trúc HTML nội bộ
await page.locator(".auth-page form input:nth-child(1)").fill("...");
```

### Luyện tập

Viết file `tests/bai4-locators.spec.js` — automation hóa `TC-LOGIN-001` và `TC-LOGIN-002` từ checklist Bài 2, **không dùng Codegen**:

```javascript
const { test, expect } = require("@playwright/test");

test.describe("Conduit — Login", () => {
  test("[TC-LOGIN-001] Đăng nhập thành công", async ({ page }) => {
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("user@test.com");
    await page.getByPlaceholder("Password").fill("password123");
    await page.getByRole("button", { name: "Sign in" }).click();

    // Expected Result từ TC-LOGIN-001
    await expect(page).toHaveURL("/");
    await expect(page.getByRole("link", { name: "New Article" })).toBeVisible();
    await expect(page.getByRole("link", { name: "autotest" })).toBeVisible();
  });

  test("[TC-LOGIN-002] Sai password — hiển thị lỗi", async ({ page }) => {
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("user@test.com");
    await page.getByPlaceholder("Password").fill("SaiPassword999");
    await page.getByRole("button", { name: "Sign in" }).click();

    // Expected Result từ TC-LOGIN-002
    await expect(page).toHaveURL(/.*login.*/);
    await expect(page.getByText("email or password is invalid")).toBeVisible();
  });
});
```

> ✅ **Cập nhật Traceability Matrix:** Điền cột Automation File và Status cho TC-LOGIN-001, TC-LOGIN-002

---

## Bài 5 — Xử lý UI Components

**Mục tiêu:** Thành thạo các component phức tạp trên Conduit: custom input, child window, filter list, comment, dynamic element.

### Lý thuyết & Thực hành

#### Checkbox

```javascript
// check() thay vì click() — tránh lật trạng thái sai
await page.getByLabel("Remember me").check();
await expect(page.getByLabel("Remember me")).toBeChecked();

// Verify chưa được check
await expect(page.getByLabel("Subscribe")).not.toBeChecked();
```

#### Xử lý Tags trên Conduit (Custom Input)

Conduit dùng input tag kiểu custom — gõ rồi nhấn Enter:

```javascript
// Trang New Article — thêm tag
await page.getByPlaceholder("Enter tags").fill("playwright");
await page.keyboard.press("Enter");
await page.getByPlaceholder("Enter tags").fill("automation");
await page.keyboard.press("Enter");

// Verify tag đã được thêm — Expected Result của TC-ART-002
await expect(page.getByText("playwright")).toBeVisible();
await expect(page.getByText("automation")).toBeVisible();
```

#### Child Window / New Tab

```javascript
test("Mở link bên ngoài từ Conduit", async ({ browser }) => {
  const context = await browser.newContext();
  const page = await context.newPage();
  await page.goto("https://conduit.bondaracademy.com");

  // Bước 1: Chuẩn bị "lưới" bắt tab mới
  const pagePromise = context.waitForEvent("page");

  // Bước 2: Click link mở tab mới
  await page.getByRole("link", { name: "Conduit" }).click();

  // Bước 3: Lấy tab mới
  const newPage = await pagePromise;
  await newPage.waitForLoadState();

  console.log(await newPage.title());
  await newPage.close();

  // Quay lại tab cũ
  await expect(page.getByRole("link", { name: "New Article" })).toBeVisible();
});
```

#### Lọc bài viết theo Tag

```javascript
const articleList = page.locator("app-article-list .article-preview");

// Kỹ thuật filter — tìm bài chứa tag cụ thể rồi click
await articleList
  .filter({ hasText: "playwright" })
  .first()
  .getByRole("link", { name: "Read more..." })
  .click();
```

#### Xử lý Comment (Dynamic element) — automation hóa TC-CMT-001, TC-CMT-003

```javascript
// TC-CMT-001: Gửi comment → hiện ngay trên trang
await page
  .getByPlaceholder("Write a comment...")
  .fill("Comment tự động từ Playwright!");
await page.getByRole("button", { name: "Post Comment" }).click();
await expect(page.getByText("Comment tự động từ Playwright!")).toBeVisible();

// TC-CMT-003: Xóa comment → biến mất khỏi danh sách
await page
  .locator(".card")
  .filter({ hasText: "Comment tự động" })
  .getByRole("button")
  .click();
await expect(
  page.getByText("Comment tự động từ Playwright!"),
).not.toBeVisible();
```

#### Upload ảnh Avatar (Settings)

```javascript
// TC-SET-001: Đổi avatar bằng URL
await page.goto("/settings");
await page
  .getByPlaceholder("URL of profile picture")
  .fill("https://picsum.photos/200");
await page.getByRole("button", { name: "Update Settings" }).click();
```

> ✅ **Cập nhật Traceability Matrix:** TC-CMT-001, TC-CMT-003, TC-SET-001

---

## Bài 6 — Assertions

**Mục tiêu:** Kiểm tra đúng điều kiện — xương sống của mọi test case.

### Các assertion phổ biến trên Conduit

```javascript
// Kiểm tra URL
await expect(page).toHaveURL("/");
await expect(page).toHaveURL(/.*editor/); // regex

// Kiểm tra hiển thị
await expect(page.getByRole("link", { name: "New Article" })).toBeVisible();
await expect(page.getByText("Loading articles...")).toBeHidden();

// Kiểm tra nội dung
await expect(page.getByRole("heading")).toHaveText("Tiêu đề bài viết");
await expect(page.getByText("Tag")).toContainText("playwright");

// Kiểm tra giá trị input
await expect(page.getByPlaceholder("Username")).toHaveValue("autotest");

// Phủ định
await expect(page.getByRole("link", { name: "Sign in" })).not.toBeVisible();
```

### Hard vs Soft Assertions

```javascript
test("[TC-ART-001] Kiểm tra trang Article sau khi publish", async ({
  page,
}) => {
  // HARD Assertion — sai → dừng ngay
  // Dùng cho điều kiện CỐT LÕI: sai ở đây thì các check sau vô nghĩa
  await expect(page).toHaveURL(/.*article.*/);

  // SOFT Assertion — sai → ghi nhận, chạy tiếp
  // Dùng để kiểm tra NHIỀU ĐIỂM trong Expected Result cùng lúc
  await expect.soft(page.getByRole("heading")).toHaveText("Tiêu đề đúng");
  await expect.soft(page.getByText("autotest")).toBeVisible(); // tác giả
  await expect.soft(page.getByText("playwright")).toBeVisible(); // tag
});
```

> 💡 **Nguyên tắc chọn Hard vs Soft:** Nếu assertion này fail mà các assertion sau vẫn có nghĩa → dùng Soft. Nếu fail thì không cần check gì thêm → dùng Hard.

### Luyện tập thêm

Automation hóa các case còn lại trong checklist Login (TC-LOGIN-003 đến TC-LOGIN-006):

```javascript
// tests/bai6-assertions.spec.js
const { test, expect } = require("@playwright/test");

test.describe("Conduit — Login Assertions", () => {
  test("[TC-LOGIN-001] Verify trang Home đúng elements sau login", async ({
    page,
  }) => {
    await page.goto("/");
    await expect(page).toHaveURL("https://conduit.bondaracademy.com/");
    await expect(page.getByRole("link", { name: "conduit" })).toBeVisible();

    await expect.soft(page.getByText("Global Feed")).toBeVisible();
    await expect.soft(page.getByText("Your Feed")).toBeVisible();
    await expect.soft(page.getByText("Popular Tags")).toBeVisible();
  });

  test("[TC-LOGIN-002] Sai password — lỗi + vẫn ở trang login", async ({
    page,
  }) => {
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("user@test.com");
    await page.getByPlaceholder("Password").fill("SaiPassword123");
    await page.getByRole("button", { name: "Sign in" }).click();

    await expect(page.getByText("email or password is invalid")).toBeVisible();
    await expect(page).toHaveURL(/.*login.*/);
  });

  test("[TC-LOGIN-003] Email không tồn tại — hiện lỗi", async ({ page }) => {
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("notexist@example.com");
    await page.getByPlaceholder("Password").fill("Playwright123");
    await page.getByRole("button", { name: "Sign in" }).click();

    await expect(page.getByText("email or password is invalid")).toBeVisible();
  });
});
```

> ✅ **Cập nhật Traceability Matrix:** TC-LOGIN-003, TC-LOGIN-004

---

## Bài 7 — Auto-waiting & Debugging

**Mục tiêu:** Hiểu cơ chế chờ thông minh, debug khi test fail.

### Cơ chế Auto-waiting của Playwright

Trước khi click/fill, Playwright tự kiểm tra:

1. **Attached** — phần tử có trong HTML chưa?
2. **Visible** — đang hiển thị không?
3. **Stable** — không đang animation?
4. **Enabled** — không bị disabled?

```javascript
// ❌ SAI — Hard Wait làm test chậm và không ổn định
await page.getByRole("button", { name: "Publish Article" }).click();
await page.waitForTimeout(3000); // Đừng bao giờ làm thế này!
await expect(page.getByRole("heading")).toBeVisible();

// ✅ ĐÚNG — Tin tưởng Auto-wait của Playwright
await page.getByRole("button", { name: "Publish Article" }).click();
await expect(page.getByRole("heading")).toBeVisible(); // Tự động chờ

// Trường hợp cần timeout riêng (action cực chậm)
await expect(page.getByText("Article published")).toBeVisible({
  timeout: 15000,
});
```

### Debug với Playwright Inspector

```bash
# Chạy chế độ debug — dừng từng bước
npx playwright test bai7-debug.spec.js --debug
```

### Cấu hình Trace Viewer

```javascript
// playwright.config.js
use: {
  trace: 'on-first-retry', // Ghi trace khi retry lần đầu
}
```

```bash
# Xem trace sau khi test fail
npx playwright show-trace test-results/ten-folder/trace.zip
```

### Luyện tập thêm

```javascript
// tests/bai7-debug.spec.js
test("[TC-ART-001] Verify bài viết load xong sau khi publish", async ({
  page,
}) => {
  await page.goto("/login");
  // ... login ...

  await page.getByRole("link", { name: "New Article" }).click();
  await page.getByPlaceholder("Article Title").fill("Test Auto-wait");
  await page.getByPlaceholder("What's this article about?").fill("Demo");
  await page
    .getByPlaceholder("Write your article (in markdown)")
    .fill("Content");
  await page.getByRole("button", { name: "Publish Article" }).click();

  // Playwright tự chờ redirect xong — khớp với Expected Result TC-ART-001
  await expect(page).toHaveURL(/.*article.*/);
  await expect(
    page.getByRole("heading", { name: "Test Auto-wait" }),
  ).toBeVisible();
});
```

---

## Bài 8 — Test Hooks & Grouping

**Mục tiêu:** Dùng `beforeEach` để Login tự động, `describe` để nhóm test case liên quan.

### Các loại Hooks

| Hook         | Chạy khi nào       | Dùng để                     |
| ------------ | ------------------ | --------------------------- |
| `beforeAll`  | 1 lần trước tất cả | Seed data, tạo account test |
| `beforeEach` | Trước **mỗi** test | Login, mở trang chủ         |
| `afterEach`  | Sau mỗi test       | Xóa bài viết vừa tạo        |
| `afterAll`   | 1 lần sau tất cả   | Dọn dẹp toàn bộ data        |

### Thực hành — Automation hóa Checklist ARTICLE và COMMENT

```javascript
// tests/bai8-hooks.spec.js
const { test, expect } = require("@playwright/test");

test.describe("Conduit — Article & Comment", () => {
  // Chạy trước MỖI test: Login tự động
  test.beforeEach(async ({ page }) => {
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("automation.test@example.com");
    await page.getByPlaceholder("Password").fill("Playwright123");
    await page.getByRole("button", { name: "Sign in" }).click();
    await expect(page.getByRole("link", { name: "New Article" })).toBeVisible();
  });

  test("[TC-ART-001] Tạo bài viết mới", async ({ page }) => {
    const title = `Auto Article ${Date.now()}`;

    await page.getByRole("link", { name: "New Article" }).click();
    await page.getByPlaceholder("Article Title").fill(title);
    await page
      .getByPlaceholder("What's this article about?")
      .fill("Automation demo");
    await page
      .getByPlaceholder("Write your article (in markdown)")
      .fill("Nội dung bài viết.");
    await page.getByRole("button", { name: "Publish Article" }).click();

    // Expected Result TC-ART-001
    await expect(page).toHaveURL(/.*article.*/);
    await expect(page.getByRole("heading", { name: title })).toBeVisible();
  });

  test("[TC-ART-003] Xóa bài viết", async ({ page }) => {
    await page.getByRole("link", { name: "automation.test" }).click();
    await page
      .locator(".article-preview")
      .first()
      .getByRole("link", { name: "Read more..." })
      .click();
    await page.getByRole("button", { name: "Delete Article" }).click();

    // Expected Result TC-ART-003
    await expect(page).toHaveURL("/");
  });

  test("[TC-CMT-001] Post comment — hiện ngay trên trang", async ({ page }) => {
    await page
      .locator(".article-preview")
      .first()
      .getByRole("link", { name: "Read more..." })
      .click();

    await page
      .getByPlaceholder("Write a comment...")
      .fill("Comment test tự động");
    await page.getByRole("button", { name: "Post Comment" }).click();

    // Expected Result TC-CMT-001
    await expect(page.getByText("Comment test tự động")).toBeVisible();
  });
});
```

> ✅ **Cập nhật Traceability Matrix:** TC-ART-001, TC-ART-003, TC-CMT-001

---

## Bài 9 — POM: Tư duy thiết kế

**Mục tiêu:** Hiểu tại sao cần POM, lập kế hoạch phân chia Page Object cho Conduit.

### Vấn đề của code "thẳng"

```javascript
// ❌ Nếu dev đổi placeholder 'Email' → 'Email Address'
// → Phải tìm và sửa ở 50 file test khác nhau!
await page.getByPlaceholder("Email").fill("user@test.com");
```

### POM giải quyết như thế nào

```javascript
// ✅ Chỉ sửa 1 chỗ trong LoginPage.js
// → 50 test case tự động cập nhật theo
await loginPage.login("user@test.com", "password123");
```

### Phân tích Conduit → Chia Page Objects

```
Conduit có những màn nào?          Tương ứng Page Object nào?
─────────────────────────          ─────────────────────────
/login                        →    LoginPage.js
/register                     →    RegisterPage.js
/ (Home / Feed)               →    HomePage.js
/article/[slug]               →    ArticlePage.js
/editor (New/Edit Article)    →    EditorPage.js
/settings                     →    SettingsPage.js
/profile/[username]           →    ProfilePage.js
```

### Quy tắc bắt buộc của POM

| Được làm                  | Không được làm             |
| ------------------------- | -------------------------- |
| Khai báo Locator          | Viết `expect()` trong Page |
| Viết Action (click, fill) | Đặt test data trong Page   |
| Viết Navigation (goto)    | Chứa logic nghiệp vụ       |

### Bài tập thảo luận

Nhìn lại Checklist và Traceability Matrix từ Bài 2 — với `LoginPage.js`, những locator và method nào cần có để cover đủ checklist LOGIN?

**Locators cần có:**

- `emailInput` — ô Email
- `passwordInput` — ô Password
- `signInButton` — nút Sign in
- `errorMessage` — thông báo lỗi (dùng ở TC-LOGIN-002, 003, 004)

**Methods cần có:**

- `goto()` — mở trang /login
- `login(email, password)` — hàm tổng hợp cho TC-LOGIN-001
- `get errorMsg()` — getter trả về locator để test assertion

---

## Bài 10 — POM: Thực hành

**Mục tiêu:** Xây dựng Page Objects thực tế, refactor code từ Bài 8 sang POM.

### Bố cục chuẩn của mỗi file Page

```
1. Imports
2. Class declaration
3. Constructor + Locators
4. Navigation methods (goto)
5. Actions (click, fill, submit...)
6. Getters (trả về locator để test dùng)
```

### `pages/LoginPage.js`

```javascript
exports.LoginPage = class LoginPage {
  /**
   * @param {import('@playwright/test').Page} page
   */
  constructor(page) {
    this.page = page;

    // Locators — cover toàn bộ checklist LOGIN
    this.emailInput = page.getByPlaceholder("Email");
    this.passwordInput = page.getByPlaceholder("Password");
    this.signInButton = page.getByRole("button", { name: "Sign in" });
    this.errorMessage = page.locator(".error-messages");
  }

  async goto() {
    await this.page.goto("/login");
  }

  async fillEmail(email) {
    await this.emailInput.fill(email);
  }

  async fillPassword(password) {
    await this.passwordInput.fill(password);
  }

  async clickSignIn() {
    await this.signInButton.click();
  }

  async login(email, password) {
    await this.fillEmail(email);
    await this.fillPassword(password);
    await this.clickSignIn();
  }

  get errorMsg() {
    return this.errorMessage;
  }
};
```

### `pages/EditorPage.js`

```javascript
exports.EditorPage = class EditorPage {
  constructor(page) {
    this.page = page;

    // Locators — cover checklist CREATE ARTICLE
    this.titleInput = page.getByPlaceholder("Article Title");
    this.aboutInput = page.getByPlaceholder("What's this article about?");
    this.bodyInput = page.getByPlaceholder("Write your article (in markdown)");
    this.tagsInput = page.getByPlaceholder("Enter tags");
    this.publishBtn = page.getByRole("button", { name: "Publish Article" });
  }

  async goto() {
    await this.page.goto("/editor");
  }

  async fillTitle(title) {
    await this.titleInput.fill(title);
  }
  async fillAbout(about) {
    await this.aboutInput.fill(about);
  }
  async fillBody(body) {
    await this.bodyInput.fill(body);
  }

  async addTag(tag) {
    await this.tagsInput.fill(tag);
    await this.page.keyboard.press("Enter");
  }

  async publish() {
    await this.publishBtn.click();
  }

  async createArticle({ title, about, body, tags = [] }) {
    await this.fillTitle(title);
    await this.fillAbout(about);
    await this.fillBody(body);
    for (const tag of tags) {
      await this.addTag(tag);
    }
    await this.publish();
  }
};
```

### `test-data/users.json` — Data Driven từ checklist

```json
[
  {
    "tcId": "TC-LOGIN-001",
    "desc": "Login thành công",
    "email": "automation.test@example.com",
    "password": "Playwright123",
    "expectedResult": "success"
  },
  {
    "tcId": "TC-LOGIN-002",
    "desc": "Sai password",
    "email": "automation.test@example.com",
    "password": "SaiPassword",
    "expectedResult": "error",
    "expectedMessage": "email or password is invalid"
  },
  {
    "tcId": "TC-LOGIN-003",
    "desc": "Email không tồn tại",
    "email": "notexist@example.com",
    "password": "Playwright123",
    "expectedResult": "error",
    "expectedMessage": "email or password is invalid"
  }
]
```

### `tests/bai10-pom.spec.js`

```javascript
const { test, expect } = require("@playwright/test");
const { LoginPage } = require("../pages/LoginPage");
const { EditorPage } = require("../pages/EditorPage");
const userDataset = require("../test-data/users.json");

test("Login thành công — POM", async ({ page }) => {
  const loginPage = new LoginPage(page);
  await loginPage.goto();
  await loginPage.login("automation.test@example.com", "Playwright123");

  await expect(page.getByRole("link", { name: "New Article" })).toBeVisible();
});

// Data Driven Testing — chạy hết checklist LOGIN từ users.json
test.describe("Login DDT với POM", () => {
  for (const data of userDataset) {
    test(`[${data.tcId}] ${data.desc}`, async ({ page }) => {
      const loginPage = new LoginPage(page);
      await loginPage.goto();
      await loginPage.login(data.email, data.password);

      if (data.expectedResult === "success") {
        await expect(page).toHaveURL("/");
      } else {
        await expect(loginPage.errorMsg).toContainText(data.expectedMessage);
      }
    });
  }
});
```

---

## Bài 11 — Fixtures & CLI

**Mục tiêu:** Inject Page Objects tự động vào test, thành thạo dòng lệnh CLI.

### Custom Fixtures

```javascript
// fixtures/conduitFixtures.js
const { test: base, expect } = require("@playwright/test");
const { LoginPage } = require("../pages/LoginPage");
const { EditorPage } = require("../pages/EditorPage");

exports.test = base.extend({
  // Fixture: loginPage — mở sẵn trang /login
  loginPage: async ({ page }, use) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await use(loginPage);
  },

  // Fixture: authenticatedPage — đã login sẵn (dùng cho mọi test cần login)
  authenticatedPage: async ({ page }, use) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login("automation.test@example.com", "Playwright123");
    await page.waitForURL("/");
    await use(page);
  },

  // Fixture: editorPage — đã login + mở trang editor
  editorPage: async ({ page }, use) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login("automation.test@example.com", "Playwright123");
    const editorPage = new EditorPage(page);
    await editorPage.goto();
    await use(editorPage);
  },
});

exports.expect = expect;
```

### File test dùng Fixtures — cực kỳ gọn

```javascript
// tests/bai11-fixtures.spec.js
const { test, expect } = require("../fixtures/conduitFixtures");

test("[TC-LOGIN-001] Login — dùng Fixture", async ({ loginPage, page }) => {
  await loginPage.login("automation.test@example.com", "Playwright123");
  await expect(page.getByRole("link", { name: "New Article" })).toBeVisible();
});

test("[TC-ART-001] Tạo bài viết — dùng Fixture", async ({
  editorPage,
  page,
}) => {
  await editorPage.createArticle({
    title: `Test ${Date.now()}`,
    about: "Automation demo",
    body: "Nội dung test",
    tags: ["playwright", "automation"],
  });
  await expect(page).toHaveURL(/.*article.*/);
});
```

### CLI quan trọng

```bash
# Chạy tất cả
npx playwright test

# Chạy 1 file cụ thể
npx playwright test bai11-fixtures.spec.js

# Lọc theo TC ID — truy ra ngay test case nào đang fail
npx playwright test -g "TC-LOGIN"
npx playwright test -g "TC-ART"

# Chỉ chạy Chrome
npx playwright test --project=chromium

# Hiện trình duyệt
npx playwright test --headed

# Debug mode
npx playwright test --debug

# Xem báo cáo
npx playwright show-report
```

---

## Bài 12 — API Testing

**Mục tiêu:** Gọi API Conduit trực tiếp, luyện kỹ thuật API Chaining lấy Token.

### Conduit API endpoints

```
POST   /api/users/login               → Lấy token
POST   /api/articles                  → Tạo bài viết
GET    /api/articles                  → Lấy danh sách
PUT    /api/articles/:slug            → Sửa bài viết
DELETE /api/articles/:slug            → Xóa bài viết
POST   /api/articles/:slug/comments   → Thêm comment
DELETE /api/articles/:slug/comments/:id → Xóa comment
```

### Checklist API — thêm vào Traceability Matrix

```
╔══════════════════════════════════════════════════════════╗
║  CHECKLIST — API                                         ║
╠══════════════════════════════════════════════════════════╣
║  □ TC-API-001  POST /login → status 200, trả về token    ║
║  □ TC-API-002  POST /articles → status 201, slug hợp lệ  ║
║  □ TC-API-003  GET /articles/:slug → đúng data vừa tạo   ║
║  □ TC-API-004  DELETE /articles/:slug → status 204       ║
║  □ TC-API-005  Gọi API không có token → status 401       ║
╚══════════════════════════════════════════════════════════╝
```

### API Chaining — Login → Create → Delete

```javascript
// tests/bai12-api.spec.js
const { test, expect } = require("@playwright/test");

test("[TC-API-001~004] CRUD Article qua API", async ({ request }) => {
  // TC-API-001: Login lấy Token
  const loginRes = await request.post(
    "https://conduit.bondaracademy.com/api/users/login",
    {
      data: {
        user: {
          email: "automation.test@example.com",
          password: "Playwright123",
        },
      },
    },
  );
  expect(loginRes.status()).toBe(200);
  const { user } = await loginRes.json();
  const token = user.token;

  // TC-API-002: Tạo bài viết
  const createRes = await request.post(
    "https://conduit.bondaracademy.com/api/articles",
    {
      headers: { Authorization: `Token ${token}` },
      data: {
        article: {
          title: `API Test ${Date.now()}`,
          description: "Tạo từ Playwright API",
          body: "Nội dung bài viết API",
          tagList: ["api", "playwright"],
        },
      },
    },
  );
  expect(createRes.status()).toBe(201);
  const { article } = await createRes.json();
  const slug = article.slug;

  // TC-API-003: Verify bài vừa tạo
  const getRes = await request.get(
    `https://conduit.bondaracademy.com/api/articles/${slug}`,
  );
  expect(getRes.status()).toBe(200);
  const fetched = await getRes.json();
  expect(fetched.article.title).toContain("API Test");

  // TC-API-004: Xóa bài viết
  const deleteRes = await request.delete(
    `https://conduit.bondaracademy.com/api/articles/${slug}`,
    { headers: { Authorization: `Token ${token}` } },
  );
  expect(deleteRes.status()).toBe(204);
});
```

### Hybrid Test — API Setup + UI Verify

```javascript
test("Hybrid: Tạo bài qua API → Verify trên UI", async ({ page, request }) => {
  const { ArticleAPI } = require("../services/ArticleAPI");
  const api = new ArticleAPI(request);

  // Dùng API để tạo bài nhanh — bỏ qua bước UI setup lặp đi lặp lại
  const token = await api.getToken(
    "automation.test@example.com",
    "Playwright123",
  );
  const { article } = await api.createArticle(token, {
    title: "Hybrid Test Article",
    description: "Tạo nhanh qua API",
    body: "Nội dung",
    tagList: ["hybrid"],
  });

  // Verify Expected Result trên UI
  await page.goto(`/article/${article.slug}`);
  await expect(
    page.getByRole("heading", { name: "Hybrid Test Article" }),
  ).toBeVisible();
  await expect(page.getByText("hybrid")).toBeVisible();

  // Dọn dẹp qua API — không cần thao tác UI
  await api.deleteArticle(token, article.slug);
});
```

> ✅ **Cập nhật Traceability Matrix:** TC-API-001 đến TC-API-004

---

## Bài 13 — Reporting & Artifacts

**Mục tiêu:** Cấu hình thu thập bằng chứng, đọc báo cáo HTML chuyên nghiệp.

### Chiến lược thu thập bằng chứng

```javascript
// playwright.config.js — cấu hình tối ưu
use: {
  screenshot: 'only-on-failure',   // Chụp ảnh khi fail
  video:      'retain-on-failure', // Quay video, xóa nếu pass
  trace:      'on-first-retry',    // Trace khi retry — chuẩn CI/CD
},

reporter: [
  ['list'],
  ['html', { outputFolder: 'playwright-report', open: 'never' }]
],
```

### Demo — Tạo báo cáo có Pass và Fail

```javascript
// tests/bai13-reporting.spec.js
const { test, expect } = require("@playwright/test");

test.describe("Demo Reporting — Conduit", () => {
  test("[TC-LOGIN-001] PASS — Login thành công", async ({ page }) => {
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("automation.test@example.com");
    await page.getByPlaceholder("Password").fill("Playwright123");
    await page.getByRole("button", { name: "Sign in" }).click();
    await expect(page).toHaveURL("/");
  });

  test("[TC-LOGIN-002] FAIL — Cố tình sai để xem Screenshot + Video", async ({
    page,
  }) => {
    await page.goto("/login");
    await page.getByPlaceholder("Email").fill("automation.test@example.com");
    await page.getByPlaceholder("Password").fill("SaiPassword");
    await page.getByRole("button", { name: "Sign in" }).click();

    await expect(page).toHaveURL("/"); // Sẽ FAIL → sinh Screenshot + Video
  });
});
```

```bash
npx playwright test bai13-reporting.spec.js
npx playwright show-report
```

### Đọc Trace Viewer

Khi mở Trace Viewer, chú ý:

- **Timeline**: từng bước theo mili-giây — đối chiếu với Steps trong test case
- **Screenshots**: ảnh chụp tại thời điểm fail — bằng chứng bug report
- **Network tab**: request/response API — debug khi API trả về sai
- **Console tab**: lỗi JavaScript trong browser

### Kết nối Reporting với Checklist

Báo cáo HTML chính là trạng thái **Pass/Fail thực tế** của checklist — cập nhật Traceability Matrix sau mỗi lần chạy:

```
TC-LOGIN-001  ✅ Pass  → automation đang green
TC-LOGIN-002  ✅ Pass  → automation đang green
TC-ART-001    ❌ Fail  → có bug, cần investigate Trace Viewer
```

---

## Bài 14 — Git & GitHub

**Mục tiêu:** Quản lý code, push dự án lên GitHub, làm việc nhóm.

### Khái niệm cốt lõi

| Thuật ngữ  | Ý nghĩa                         |
| ---------- | ------------------------------- |
| Repository | Kho chứa dự án                  |
| Commit     | "Lưu game" — ghi lại checkpoint |
| Push       | Upload code lên GitHub          |
| Pull       | Download code mới nhất về       |
| Clone      | Copy dự án từ GitHub về lần đầu |

### File `.gitignore` — bắt buộc phải có

```gitignore
node_modules/
test-results/
playwright-report/
blob-report/
.env
.DS_Store
```

> ⚠️ `node_modules/` nặng hàng trăm MB — **không bao giờ** push lên GitHub

### Quy trình từng bước

```bash
# 1. Khởi tạo Git
git init

# 2. Tạo .gitignore

# 3. Commit lần đầu — bao gồm cả test-data/test-cases/
git add .
git commit -m "Initial commit: Playwright Conduit course setup"

# 4. Tạo repo trên github.com → copy 3 lệnh → paste vào terminal
git remote add origin https://github.com/username/playwright-conduit-course.git
git branch -M main
git push -u origin main
```

### Commit message chuẩn

```
feat: thêm LoginPage POM
fix: sửa locator password input
test: automation TC-LOGIN-001 đến TC-LOGIN-004
test: automation TC-ART-001, TC-ART-003
chore: cập nhật playwright config
docs: cập nhật traceability matrix
```

---

## Bài 15 — GitLab CI/CD

**Mục tiêu:** Chạy test tự động mỗi khi push code — không cần mở máy.

### File `.gitlab-ci.yml`

```yaml
image: mcr.microsoft.com/playwright:v1.49.0-jammy

stages:
  - test

cache:
  paths:
    - node_modules/

playwright-tests:
  stage: test
  script:
    - npm ci
    - npx playwright test --project=chromium
  artifacts:
    when: always
    paths:
      - playwright-report/
    expire_in: 30 days
  only:
    - main
```

### Kích hoạt Pipeline

```bash
git add .gitlab-ci.yml
git commit -m "ci: thêm GitLab CI pipeline cho Playwright"
git push origin main
```

Vào **Build → Pipelines** trên GitLab để theo dõi trạng thái.

### Đọc kết quả Pipeline — kết nối với Checklist

- 🟢 **Passed** — toàn bộ test xanh → Traceability Matrix cột Status = ✅ All Green
- 🔴 **Failed** — có test đỏ → vào Job log đọc lỗi → tải Artifacts xem báo cáo HTML → đối chiếu TC ID bị fail
- 🟡 **Running** — đang chạy

---

## 🔑 Tài khoản thực hành

> Tạo tài khoản riêng tại [conduit.bondaracademy.com](https://conduit.bondaracademy.com) để tránh xung đột dữ liệu với học viên khác.

### Biến môi trường (`.env`)

```env
CONDUIT_EMAIL=your.email@example.com
CONDUIT_PASSWORD=YourPassword123
CONDUIT_URL=https://conduit.bondaracademy.com
```

> ⚠️ File `.env` đã có trong `.gitignore` — không bao giờ commit file này

### Dùng biến môi trường trong test

```javascript
// playwright.config.js
use: {
  baseURL: process.env.CONDUIT_URL || 'https://conduit.bondaracademy.com',
}

// Trong test
const email    = process.env.CONDUIT_EMAIL    || 'default@example.com';
const password = process.env.CONDUIT_PASSWORD || 'defaultPass';
```

---

## 📊 Tổng kết lộ trình

```
Bài 1  → Môi trường + project structure
Bài 2  → Phân tích & Thiết kế Test Case + Checklist + Traceability ⭐
Bài 3  → test.describe, test(), expect(), Codegen
Bài 4  → getByRole, getByLabel, getByPlaceholder ⭐
Bài 5  → Custom input, Child Window, Filter list, Comment ⭐
Bài 6  → Hard/Soft assertions ⭐
Bài 7  → Auto-wait, không dùng waitForTimeout, Debug
Bài 8  → beforeEach, afterEach, test.describe ⭐
Bài 9  → Tư duy POM, phân chia Page Object
Bài 10 → Xây dựng POM thực tế + Data Driven ⭐
Bài 11 → Custom Fixtures, CLI commands ⭐
Bài 12 → API Testing, Token chaining, Hybrid test
Bài 13 → Screenshot, Video, Trace Viewer, HTML Report
Bài 14 → Git workflow, .gitignore, GitHub
Bài 15 → GitLab CI/CD pipeline ⭐
```

⭐ = Bài trọng tâm — học kỹ trước khi qua bài tiếp theo

---

_Khóa học sử dụng [Conduit](https://conduit.bondaracademy.com) — RealWorld App demo_  
_Playwright Documentation: [playwright.dev](https://playwright.dev)_
