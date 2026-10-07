# Postman API Testing - JSONPlaceholder

## 1. Giới thiệu

Dự án này thực hiện kiểm thử REST API bằng công cụ **Postman** trên API **JSONPlaceholder**.

Mục tiêu của dự án là thực hành quy trình kiểm thử API từ việc gửi HTTP Request, kiểm tra Response, xây dựng các Test Script bằng JavaScript, sử dụng Environment Variables và chạy tự động nhiều test case bằng **Postman Collection Runner**.

Trong bài thực hành, các phương thức HTTP chính được sử dụng gồm:

- GET
- POST
- PUT
- DELETE

Ngoài các test case kiểm thử chức năng thông thường, dự án cũng thực hiện **Negative Testing** để kiểm tra trường hợp truy vấn một tài nguyên không tồn tại.

API được sử dụng:

```text
https://jsonplaceholder.typicode.com
```

JSONPlaceholder là một REST API giả lập được sử dụng cho mục đích học tập, thực hành và kiểm thử API.

---

## 2. Mục tiêu

Dự án được thực hiện nhằm đạt các mục tiêu sau:

1. Làm quen với giao diện và các chức năng cơ bản của Postman.
2. Thực hiện HTTP Request bằng Postman.
3. Kiểm thử các phương thức HTTP:
   - GET
   - POST
   - PUT
   - DELETE
4. Sử dụng Environment Variable để quản lý Base URL.
5. Viết Test Script bằng JavaScript trong Postman.
6. Kiểm tra HTTP Status Code.
7. Kiểm tra định dạng JSON của Response.
8. Kiểm tra dữ liệu trả về trong Response Body.
9. Thực hiện Positive Testing.
10. Thực hiện Negative Testing.
11. Sử dụng Collection Runner để chạy tự động toàn bộ test case.
12. Tổng hợp và đánh giá kết quả kiểm thử.
13. Quản lý và lưu trữ project trên GitHub.

---

## 3. Công cụ sử dụng

| Công cụ | Mục đích |
|---|---|
| **Postman** | Gửi HTTP Request và thực hiện API Testing |
| **JSONPlaceholder** | REST API giả lập dùng để thực hành |
| **JavaScript** | Viết Test Script trong Postman |
| **GitHub** | Lưu trữ và quản lý project |
| **Markdown** | Viết tài liệu README |

---

## 4. API được kiểm thử

### 4.1. Base URL

API được sử dụng trong project:

```text
https://jsonplaceholder.typicode.com
```

Để quản lý Base URL thuận tiện hơn, project sử dụng Environment Variable:

```text
{{base_url}}
```

Giá trị của biến:

```text
https://jsonplaceholder.typicode.com
```

Ví dụ:

Thay vì sử dụng:

```text
https://jsonplaceholder.typicode.com/users
```

project sử dụng:

```text
{{base_url}}/users
```

Việc sử dụng Environment Variable giúp giảm việc lặp lại Base URL và thuận tiện khi cần thay đổi môi trường kiểm thử.

### 4.2. Các API Endpoint

| Test Case | Method | Endpoint | Mục đích |
|---|---|---|---|
| TC01 | GET | `/users` | Lấy danh sách users |
| TC02 | GET | `/users/1` | Lấy thông tin user có ID = 1 |
| TC03 | POST | `/users` | Tạo user mới |
| TC04 | PUT | `/users/1` | Cập nhật user có ID = 1 |
| TC05 | DELETE | `/users/1` | Xóa user có ID = 1 |
| TC06 | GET | `/users/9999` | Kiểm thử user không tồn tại |

---

## 5. Cấu hình Environment

### 5.1. Environment

Tên Environment:

```text
JSONPlaceholder Environment
```

Environment Variable:

| Variable | Value |
|---|---|
| `base_url` | `https://jsonplaceholder.typicode.com` |

Environment này được sử dụng cho toàn bộ request trong Collection.

### 5.2. Collection

Tên Collection:

```text
Postman API Testing - JSONPlaceholder
```

Collection bao gồm 6 request:

```text
01 - GET All Users
02 - GET User by ID
03 - POST Create User
04 - PUT Update User
05 - DELETE User
06 - Negative Testing
```

---

## 6. Các Test Case

### 6.1. TC01 - GET All Users

#### Mục đích

Kiểm tra API có thể trả về danh sách users hay không.

#### Request

**Method:**

```text
GET
```

**URL:**

```text
{{base_url}}/users
```

#### Expected Result

- HTTP Status Code = `200`.
- Response có định dạng JSON.
- Response Body là một Array.
- Array chứa ít nhất một user.
- User đầu tiên có các trường:
  - `id`
  - `name`
  - `email`

#### Test Script

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("Response contains users", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData).to.be.an("array");
    pm.expect(jsonData.length).to.be.above(0);
});

pm.test("First user has required fields", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData[0]).to.have.property("id");
    pm.expect(jsonData[0]).to.have.property("name");
    pm.expect(jsonData[0]).to.have.property("email");
});
```

#### Kết quả

```text
4/4 tests passed
```

#### Screenshot

![TC01 - GET All Users](screenshots/01_get_all_users.png)

---

### 6.2. TC02 - GET User by ID

#### Mục đích

Kiểm tra khả năng lấy thông tin của một user cụ thể thông qua ID.

#### Request

**Method:**

```text
GET
```

**URL:**

```text
{{base_url}}/users/1
```

#### Expected Result

- HTTP Status Code = `200`.
- Response có định dạng JSON.
- User trả về có `id = 1`.
- User có trường `name`.
- User có trường `email`.

#### Test Script

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("User ID is 1", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.id).to.eql(1);
});

pm.test("User has name", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.name).to.not.be.empty;
});

pm.test("User has email", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.email).to.not.be.empty;
});
```

#### Kết quả

```text
5/5 tests passed
```

#### Screenshot

![TC02 - GET User by ID](screenshots/02_get_user_by_id.png)

---

### 6.3. TC03 - POST Create User

#### Mục đích

Kiểm tra API có thể tiếp nhận request POST để tạo một user mới hay không.

#### Request

**Method:**

```text
POST
```

**URL:**

```text
{{base_url}}/users
```

#### Request Body

Body sử dụng định dạng JSON:

```json
{
  "name": "Nguyen Van A",
  "username": "nguyenvana",
  "email": "nguyenvana@example.com"
}
```

#### Expected Result

- HTTP Status Code = `201`.
- Response có định dạng JSON.
- Response trả về đúng `name`.
- Response trả về đúng `username`.
- Response trả về đúng `email`.
- Response có trường `id`.

#### Test Script

```javascript
pm.test("Status code is 201", function () {
    pm.response.to.have.status(201);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("Created user has correct name", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.name).to.eql("Nguyen Van A");
});

pm.test("Created user has correct username", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.username).to.eql("nguyenvana");
});

pm.test("Created user has correct email", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.email).to.eql("nguyenvana@example.com");
});

pm.test("Created user has an ID", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData).to.have.property("id");
});
```

#### Kết quả

```text
6/6 tests passed
```

#### Screenshot

![TC03 - POST Create User](screenshots/03_post_create_user.png)

---

### 6.4. TC04 - PUT Update User

#### Mục đích

Kiểm tra API có thể cập nhật thông tin của một user hiện có hay không.

#### Request

**Method:**

```text
PUT
```

**URL:**

```text
{{base_url}}/users/1
```

#### Request Body

```json
{
  "id": 1,
  "name": "Nguyen Van B",
  "username": "nguyenvanb",
  "email": "nguyenvanb@example.com"
}
```

#### Expected Result

- HTTP Status Code = `200`.
- Response có định dạng JSON.
- User ID = `1`.
- Name được cập nhật thành `Nguyen Van B`.
- Username được cập nhật thành `nguyenvanb`.
- Email được cập nhật thành `nguyenvanb@example.com`.

#### Test Script

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("User ID is 1", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.id).to.eql(1);
});

pm.test("User name was updated", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.name).to.eql("Nguyen Van B");
});

pm.test("Username was updated", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.username).to.eql("nguyenvanb");
});

pm.test("Email was updated", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData.email).to.eql("nguyenvanb@example.com");
});
```

#### Kết quả

```text
6/6 tests passed
```

#### Screenshot

![TC04 - PUT Update User](screenshots/04_put_update_user.png)

---

### 6.5. TC05 - DELETE User

#### Mục đích

Kiểm tra API có thể xử lý request DELETE đối với user hay không.

#### Request

**Method:**

```text
DELETE
```

**URL:**

```text
{{base_url}}/users/1
```

#### Request Body

Không có Request Body.

#### Expected Result

- HTTP Status Code = `200`.
- Response có định dạng JSON.
- Response Body là object rỗng `{}`.

#### Test Script

```javascript
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response body is empty object", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData).to.eql({});
});
```

#### Kết quả

```text
2/2 tests passed
```

#### Ghi chú

Trong quá trình kiểm thử, Response của JSONPlaceholder sau request DELETE trả về:

```json
{}
```

Do đó Test Script được xây dựng để kiểm tra Response Body là một object rỗng thay vì kiểm tra chuỗi rỗng.

#### Screenshot

![TC05 - DELETE User](screenshots/05_delete_user.png)

---

### 6.6. TC06 - Negative Testing

#### Mục đích

Kiểm tra cách API xử lý trường hợp client yêu cầu một user không tồn tại.

#### Request

**Method:**

```text
GET
```

**URL:**

```text
{{base_url}}/users/9999
```

ID `9999` được sử dụng để kiểm tra trường hợp tài nguyên không tồn tại.

#### Expected Result

- HTTP Status Code = `404`.
- Response có định dạng JSON.
- Response Body là object rỗng `{}`.

#### Test Script

```javascript
pm.test("Status code is 404", function () {
    pm.response.to.have.status(404);
});

pm.test("Response is JSON", function () {
    pm.response.to.be.json;
});

pm.test("Response body is empty object", function () {
    const jsonData = pm.response.json();

    pm.expect(jsonData).to.eql({});
});
```

#### Kết quả

```text
3/3 tests passed
```

#### Screenshot

![TC06 - Negative Testing](screenshots/06_negative_testing.png)

---

## 7. Test Automation

### 7.1. Collection Runner

Sau khi hoàn thành các test case, toàn bộ request được chạy tự động bằng **Postman Collection Runner**.

**Collection:**

```text
Postman API Testing - JSONPlaceholder
```

**Environment:**

```text
JSONPlaceholder Environment
```

**Iterations:**

```text
1
```

Các request được chạy tự động theo thứ tự:

```text
01 - GET All Users
02 - GET User by ID
03 - POST Create User
04 - PUT Update User
05 - DELETE User
06 - Negative Testing
```

### 7.2. Test Automation Result

Kết quả chạy Collection Runner:

| Metric | Result |
|---|---:|
| Iterations | 1 |
| Total Tests | 26 |
| Passed | 26 |
| Failed | 0 |
| Skipped | 0 |
| Errors | 0 |
| Pass Rate | 100% |

Toàn bộ các Test Script đã được thực thi thành công.

#### Screenshot

![Collection Runner Result](screenshots/07_collection_runner.png)

---

## 8. Kết quả kiểm thử

### 8.1. Tổng hợp kết quả

| ID | Test Case | Method | Expected Status | Tests | Result |
|---|---|---|---:|---:|---|
| TC01 | GET All Users | GET | 200 | 4/4 | PASS |
| TC02 | GET User by ID | GET | 200 | 5/5 | PASS |
| TC03 | POST Create User | POST | 201 | 6/6 | PASS |
| TC04 | PUT Update User | PUT | 200 | 6/6 | PASS |
| TC05 | DELETE User | DELETE | 200 | 2/2 | PASS |
| TC06 | Negative Testing | GET | 404 | 3/3 | PASS |
| **Total** | | | | **26/26** | **PASS** |

### 8.2. Tổng kết

Tổng số Test Assertion:

```text
26
```

Số assertion Passed:

```text
26
```

Số assertion Failed:

```text
0
```

Tỷ lệ Passed:

```text
100%
```

Kết quả cuối cùng:

```text
26/26 tests passed
```

---

## 9. Hình ảnh minh họa

Các hình ảnh minh họa kết quả kiểm thử được lưu trong thư mục:

```text
screenshots/
```

| STT | File | Nội dung |
|---:|---|---|
| 1 | `01_get_all_users.png` | Kết quả GET toàn bộ users |
| 2 | `02_get_user_by_id.png` | Kết quả GET user theo ID |
| 3 | `03_post_create_user.png` | Kết quả POST tạo user |
| 4 | `04_put_update_user.png` | Kết quả PUT cập nhật user |
| 5 | `05_delete_user.png` | Kết quả DELETE user |
| 6 | `06_negative_testing.png` | Kết quả Negative Testing |
| 7 | `07_collection_runner.png` | Kết quả chạy toàn bộ Collection |

---

## 10. Cấu trúc Project

Project được tổ chức như sau:

```text
Postman-API-Testing/
│
├── README.md
│
├── postman/
│   ├── Postman_API_Testing.postman_collection.json
│   └── JSONPlaceholder_Environment.postman_environment.json
│
└── screenshots/
    ├── 01_get_all_users.png
    ├── 02_get_user_by_id.png
    ├── 03_post_create_user.png
    ├── 04_put_update_user.png
    ├── 05_delete_user.png
    ├── 06_negative_testing.png
    └── 07_collection_runner.png
```

---

## 11. Phân tích kết quả

### 11.1. GET All Users

API `/users` trả về HTTP Status Code `200` và dữ liệu ở dạng JSON Array.

Test Script kiểm tra:

- Status Code.
- JSON Response.
- Response là Array.
- Array không rỗng.
- User đầu tiên có các trường `id`, `name` và `email`.

Tất cả các assertion đều Passed.

### 11.2. GET User by ID

API `/users/1` trả về thông tin user có ID bằng `1`.

Test Script kiểm tra:

- Status Code = `200`.
- Response là JSON.
- `id = 1`.
- User có `name`.
- User có `email`.

Kết quả:

```text
5/5 Passed
```

### 11.3. POST Create User

API `/users` được sử dụng để mô phỏng việc tạo user mới.

Request Body:

```json
{
  "name": "Nguyen Van A",
  "username": "nguyenvana",
  "email": "nguyenvana@example.com"
}
```

Response được kiểm tra để đảm bảo:

- Status Code = `201`.
- Response là JSON.
- `name` đúng với dữ liệu gửi lên.
- `username` đúng với dữ liệu gửi lên.
- `email` đúng với dữ liệu gửi lên.
- Response có `id`.

Kết quả:

```text
6/6 Passed
```

### 11.4. PUT Update User

API `/users/1` được sử dụng để mô phỏng việc cập nhật user.

Các trường được cập nhật:

```text
name     = Nguyen Van B
username = nguyenvanb
email    = nguyenvanb@example.com
```

Test Script kiểm tra các giá trị trong Response có đúng với dữ liệu cập nhật hay không.

Kết quả:

```text
6/6 Passed
```

### 11.5. DELETE User

API `/users/1` được sử dụng để kiểm thử thao tác DELETE.

API trả về:

```json
{}
```

Do đó test case kiểm tra:

- Status Code = `200`.
- Response Body là object rỗng.

Kết quả:

```text
2/2 Passed
```

### 11.6. Negative Testing

API `/users/9999` được sử dụng để kiểm tra trường hợp truy vấn một user không tồn tại.

Expected Result:

```text
404 Not Found
```

Test Script kiểm tra:

- Status Code = `404`.
- Response là JSON.
- Response Body là `{}`.

Kết quả:

```text
3/3 Passed
```

Negative Testing giúp kiểm tra cách API xử lý một trường hợp lỗi thay vì chỉ kiểm tra các request hợp lệ.

---

## 12. Những kiến thức đã thực hành

Thông qua project này, các kiến thức sau đã được áp dụng:

### API Testing

- REST API Testing.
- HTTP Request.
- HTTP Response.
- Request Body.
- Response Body.

### HTTP Methods

- GET.
- POST.
- PUT.
- DELETE.

### HTTP Status Codes

- `200 OK`
- `201 Created`
- `404 Not Found`

### Postman

- Collection.
- Environment.
- Environment Variables.
- Request.
- Test Script.
- Collection Runner.

### Automated Testing

- Viết assertion bằng JavaScript.
- Kiểm tra Status Code.
- Kiểm tra Response Format.
- Kiểm tra Response Data.
- Kiểm tra dữ liệu cụ thể.
- Positive Testing.
- Negative Testing.
- Chạy tự động nhiều test case.

### Project Management

- Quản lý source trên GitHub.
- Tổ chức thư mục project.
- Viết tài liệu bằng Markdown.
- Lưu trữ Postman Collection và Environment.

---

## 13. Kết luận

Project đã thực hiện thành công việc kiểm thử REST API bằng Postman trên JSONPlaceholder.

Tổng cộng có **6 test case** được xây dựng:

1. GET All Users.
2. GET User by ID.
3. POST Create User.
4. PUT Update User.
5. DELETE User.
6. Negative Testing.

Mỗi test case đều có Test Script để kiểm tra Status Code, Response Format và dữ liệu trả về.

Sau khi chạy toàn bộ Collection bằng Postman Collection Runner, kết quả thu được:

```text
Total Tests : 26
Passed      : 26
Failed      : 0
Pass Rate   : 100%
```

Kết quả cho thấy toàn bộ các assertion trong project đều đạt yêu cầu.

Project cũng sử dụng Environment Variable `base_url`, giúp quản lý endpoint dễ dàng và có khả năng mở rộng khi cần kiểm thử trên các môi trường khác.

Bên cạnh việc kiểm tra các request thành công, project còn thực hiện Negative Testing với trường hợp user không tồn tại. Điều này giúp mở rộng phạm vi kiểm thử và đánh giá cách API xử lý các tình huống không hợp lệ.

Thông qua bài thực hành, quy trình cơ bản của API Testing đã được thực hiện theo các bước:

```text
Create Request
      ↓
Send Request
      ↓
Receive Response
      ↓
Write Test Script
      ↓
Validate Response
      ↓
Run Collection
      ↓
Analyze Test Result
      ↓
Document Result
```

JSONPlaceholder là API phục vụ mục đích học tập và mô phỏng. Các thao tác POST, PUT và DELETE được sử dụng để thực hành request/response và không nên hiểu là dữ liệu nghiệp vụ thực tế được lưu trữ lâu dài trên server.

---

## 14. Tài liệu tham khảo

1. **Postman Documentation**  
   https://learning.postman.com/docs/

2. **Postman - Test Scripts**  
   https://learning.postman.com/docs/tests-and-scripts/write-scripts/test-scripts/

3. **Postman - Collections**  
   https://learning.postman.com/docs/collections/collections-overview/

4. **Postman - Managing Environments**  
   https://learning.postman.com/docs/sending-requests/variables/managing-environments/

5. **JSONPlaceholder**  
   https://jsonplaceholder.typicode.com/

6. **Video hướng dẫn Postman API Testing**  
   https://www.youtube.com/watch?v=MFxk5BZulVU
