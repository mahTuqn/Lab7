<div class="content">
    <h1>BÁO CÁO VÀ HƯỚNG DẪN KIỂM THỬ API VỚI POSTMAN</h1>
    <ol>
        <p><strong>Tên Dự Án:</strong> DGKDPM Lab 7 - API Testing &amp; Mock Server Platform</p>
        <p><strong>Ngày Kiểm Thử:</strong> 14/10/2026</p>
        <p><strong>Người Kiểm Thử:</strong> Lê Mạnh Tuấn (MSV: 23010391 - Trường Đại học Phenikaa)</p>
        <p><strong>Môn Học:</strong> Đảm bảo chất lượng và Kiểm thử phần mềm (DGKDPM)</p>
        <p><strong>1. Mục Tiêu Kiểm Thử:</strong> Xây dựng máy chủ Mock REST API hoàn chỉnh và sử dụng Postman để kiểm thử tự động, kiểm thử thủ công trên các API thực tế.</p>
        <p><strong>2. Môi Trường Kiểm Thử:</strong></p>
        <ul>
            <li>Máy chủ Mock API cục bộ (Node.js HTTP Server - Cổng 5000).</li>
            <li>Phần mềm Postman Desktop Client (Mạnh Tuấn Lê's Workspace).</li>
            <li>Giao diện Web Testing Studio: <code>http://localhost:5000/</code>.</li>
        </ul>
        <p><strong>3. Phương Pháp Kiểm Thử:</strong> Kiểm thử thủ công (Manual Testing) và kiểm thử tự động với Test Script Assertions (Automated Testing) trên phần mềm Postman &amp; CLI Test Runner.</p>
        <br>
        <strong>4. Kịch Bản Kiểm Thử Chi Tiết:</strong>
    </ol>

    <!-- KỊCH BẢN 1 -->
    <h3>Kịch Bản Kiểm Thử Lần 1 (TC01): Kiểm thử cơ bản của 1 URL</h3>
    <ul>
        <li><strong>Tên Kịch Bản:</strong> Kiểm thử cơ bản của 1 URL với tham số truy vấn (GET Users)</li>
        <li><strong>Mục Đích:</strong> Kiểm tra khả năng hoạt động của URL, phần mềm Postman và việc phân giải query parameters (<code>size=2&amp;is_xml=true</code>)</li>
        <li><strong>Phương Thức HTTP:</strong> <code>GET</code></li>
        <li><strong>URL:</strong> <code>http://localhost:5000/api/v2/users</code></li>
        <li><strong>Tham Số (Params):</strong> <code>size=2&amp;is_xml=true</code></li>
        <li><strong>Kết Quả Mong Đợi:</strong> Trả về mã trạng thái HTTP 200 OK, định dạng JSON gồm danh sách 2 người dùng kèm đầy đủ các trường thông tin (id, uid, first_name, last_name, email, employment, address, credit_card).</li>
        <li><strong>Kết Quả Thực Tế:</strong> Gửi yêu cầu thành công, thời gian phản hồi 61ms, dung lượng 2.53 KB, trả về đúng 2 đối tượng người dùng.</li>
        <li><strong>Trạng Thái:</strong> <span style="color: green; font-weight: bold;">Thành công (PASS)</span></li>
        <li><strong>Ảnh chụp màn hình thực tế trên Postman:</strong></li>
    </ul>

    <p align="center">
        <img width="850" alt="TC01 GET Users Postman Result" src="https://raw.githubusercontent.com/mahTuqn/Lab7/assets/tc01_get_users_200.png">
    </p>

    <ul>
        <li><strong>Postman Test Script:</strong></li>
    </ul>

```javascript
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

pm.test("Response body is an array with 2 users", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.be.an("array");
    pm.expect(jsonData.length).to.eql(2);
});

pm.test("User object contains mandatory fields", function () {
    var jsonData = pm.response.json();
    var user = jsonData[0];
    pm.expect(user).to.have.property("id");
    pm.expect(user).to.have.property("uid");
    pm.expect(user).to.have.property("email");
    pm.expect(user).to.have.property("employment");
});
```

    <p><strong>Kết quả dữ liệu chi tiết trả về:</strong></p>

```json
[
  {
    "id": 2101001,
    "uid": "phenikaa-2101-tuan-lemanhtuan",
    "password": "TuanSecurePass@2026",
    "first_name": "Tuấn",
    "last_name": "Lê Mạnh",
    "username": "lemanhtuan.tester",
    "email": "leemanhtuan22@gmail.com",
    "avatar": "https://robohash.org/lemanhtuan.png?size=300x300&set=set1",
    "gender": "Male",
    "phone_number": "+84 988.123.456",
    "social_insurance_number": "0012026888",
    "date_of_birth": "2003-08-15",
    "employment": {
      "title": "API Automation Tester",
      "key_skill": "Postman, Newman, RestAPI Testing"
    },
    "address": {
      "city": "Hà Nội",
      "street_name": "Đường Tố Hữu",
      "street_address": "Trường Đại học Phenikaa, Yên Nghĩa, Hà Đông",
      "zip_code": "100000",
      "state": "Hà Nội",
      "country": "Việt Nam",
      "coordinates": {
        "lat": 20.9634,
        "lng": 105.7468
      }
    },
    "credit_card": {
      "cc_number": "9704-1988-2026-8888"
    },
    "subscription": {
      "plan": "Premium Student",
      "status": "Active",
      "payment_method": "VietQR Transfer",
      "term": "Annual"
    }
  },
  {
    "id": 2101002,
    "uid": "phenikaa-2101-tester-qa",
    "password": "QATester@Pass2026",
    "first_name": "Bình",
    "last_name": "Đỗ Quốc",
    "username": "quocbinh.tester",
    "email": "quocbinh.qa@gmail.com",
    "avatar": "https://robohash.org/quocbinh.png?size=300x300&set=set1",
    "gender": "Male",
    "phone_number": "+84 977.654.321",
    "social_insurance_number": "0012026999",
    "date_of_birth": "2003-05-20",
    "employment": {
      "title": "Software Quality Assurance",
      "key_skill": "Manual & Automated Testing"
    },
    "address": {
      "city": "Hà Nội",
      "street_name": "Nguyễn Trãi",
      "street_address": "Hà Đông, Hà Nội",
      "zip_code": "100000",
      "state": "Hà Nội",
      "country": "Việt Nam",
      "coordinates": {
        "lat": 20.9822,
        "lng": 105.7925
      }
    },
    "credit_card": {
      "cc_number": "9704-2026-9999-7777"
    },
    "subscription": {
      "plan": "Pro Tester",
      "status": "Active",
      "payment_method": "MoMo Wallet",
      "term": "Monthly"
    }
  }
]
```

    <hr>

    <!-- KỊCH BẢN 2 -->
    <h3>Kịch Bản Kiểm Thử Lần 2 (TC02): Kiểm thử URL sai hoặc thiếu tên tài nguyên (Negative Test)</h3>
    <ul>
        <li><strong>Tên Kịch Bản:</strong> Kiểm thử xử lý lỗi 404 khi gọi sai URL (Negative Testing)</li>
        <li><strong>Mục Đích:</strong> Kiểm tra khả năng bắt lỗi của hệ thống khi người dùng truyền sai URL hoặc thiếu tài nguyên</li>
        <li><strong>Phương Thức HTTP:</strong> <code>GET</code></li>
        <li><strong>URL:</strong> <code>http://localhost:5000/api/v2/beerType=light</code></li>
        <li><strong>Tham Số (Params):</strong> Không hợp lệ (nối trực tiếp vào đường dẫn)</li>
        <li><strong>Kết Quả Mong Đợi:</strong> Trả về mã trạng thái HTTP 404 Not Found kèm giao diện thông báo lỗi chuẩn của máy chủ</li>
        <li><strong>Kết Quả Thực Tế:</strong> Gửi yêu cầu nhận mã 404 Not Found trong 4ms, dung lượng 1.97 KB, nội dung chứa "The page you were looking for doesn't exist (404)"</li>
        <li><strong>Trạng Thái:</strong> <span style="color: green; font-weight: bold;">Thành công (PASS - Bắt đúng lỗi phủ định)</span></li>
        <li><strong>Ảnh chụp màn hình thực tế trên Postman:</strong></li>
    </ul>

    <p align="center">
        <img width="850" alt="TC02 Negative 404 Postman Result" src="https://raw.githubusercontent.com/mahTuqn/Lab7/assets/tc02_negative_404.png">
    </p>

    <ul>
        <li><strong>Postman Test Script:</strong></li>
    </ul>

```javascript
pm.test("Status code is 404 Not Found", function () {
    pm.response.to.have.status(404);
});

pm.test("Response body contains 404 message", function () {
    pm.expect(pm.response.text()).to.include("The page you were looking for doesn't exist");
});
```

    <p><strong>Nội dung phản hồi HTML 404:</strong></p>

```html
<!DOCTYPE html>
<html>
<head>
    <title>The page you were looking for doesn't exist (404)</title>
</head>
<body class="rails-default-error-page">
    <div class="dialog">
        <div>
            <h1>The page you were looking for doesn't exist.</h1>
            <p>You may have mistyped the address or the page may have moved.</p>
        </div>
        <p>If you are the application owner check the logs for more information.</p>
    </div>
</body>
</html>
```

    <hr>

    <!-- KỊCH BẢN 3 -->
    <h3>Kịch Bản Kiểm Thử Lần 3 (TC03): Kiểm thử URL với tham số hợp lệ</h3>
    <ul>
        <li><strong>Tên Kịch Bản:</strong> Kiểm thử URL bia với tham số <code>beerType=light</code></li>
        <li><strong>Mục Đích:</strong> Kiểm tra API bia phản hồi chính xác khi cung cấp đúng resource và tham số</li>
        <li><strong>Phương Thức HTTP:</strong> <code>GET</code></li>
        <li><strong>URL:</strong> <code>http://localhost:5000/api/v2/beers?beerType=light</code></li>
        <li><strong>Tham Số:</strong> <code>beerType=light</code></li>
        <li><strong>Kết Quả Mong Đợi:</strong> HTTP 200 OK, trả về đối tượng bia chi tiết gồm brand, name, style, alcohol...</li>
        <li><strong>Kết Quả Thực Tế:</strong> Gửi yêu cầu thành công trong 18ms, dung lượng 664 B, nhận dữ liệu JSON bia chuẩn</li>
        <li><strong>Trạng Thái:</strong> <span style="color: green; font-weight: bold;">Thành công (PASS)</span></li>
        <li><strong>Ảnh chụp màn hình thực tế trên Postman:</strong></li>
    </ul>

    <p align="center">
        <img width="850" alt="TC03 GET Beers Postman Result" src="https://raw.githubusercontent.com/mahTuqn/Lab7/assets/tc03_get_beers_200.png">
    </p>

    <ul>
        <li><strong>Postman Test Script:</strong></li>
    </ul>

```javascript
pm.test("Status code is 200 OK", function () {
    pm.response.to.have.status(200);
});

pm.test("Beer object contains brand, name and alcohol", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("brand");
    pm.expect(jsonData).to.have.property("name");
    pm.expect(jsonData).to.have.property("alcohol");
});
```

    <p><strong>Dữ liệu JSON bia trả về:</strong></p>

```json
{
  "id": 4908,
  "uid": "16d508f9-8757-491d-b8c9-4b980932f637",
  "brand": "Leffe",
  "name": "Sapporo Premium",
  "style": "Strong Ale",
  "hop": "Newport",
  "yeast": "1098 - British Ale",
  "malts": "Roasted barley",
  "ibu": "82 IBU",
  "alcohol": "2.1%",
  "blg": "12.8°Blg",
  "queryApplied": "light"
}
```

    <hr>

    <!-- KỊCH BẢN 4: RESTFUL PUT -->
    <h3>Kịch Bản Kiểm Thử Mở Rộng (TC04): Cập nhật bài viết qua RESTful API (PUT)</h3>
    <ul>
        <li><strong>Tên Kịch Bản:</strong> Cập nhật bài viết bằng phương thức HTTP PUT</li>
        <li><strong>Mục Đích:</strong> Kiểm tra khả năng gửi payload JSON và cập nhật trạng thái dữ liệu trong database</li>
        <li><strong>Phương Thức HTTP:</strong> <code>PUT</code></li>
        <li><strong>URL:</strong> <code>http://localhost:5000/api/posts/1</code></li>
        <li><strong>Body (raw JSON):</strong> <code>{"title": "Hướng dẫn kiểm thử REST API (Updated)", "views": 350}</code></li>
        <li><strong>Kết Quả Thực Tế:</strong> HTTP 200 OK trong 12ms, dung lượng 444 B, trả về đối tượng sau khi update</li>
        <li><strong>Trạng Thái:</strong> <span style="color: green; font-weight: bold;">Thành công (PASS)</span></li>
        <li><strong>Ảnh chụp màn hình thực tế trên Postman:</strong></li>
    </ul>

    <p align="center">
        <img width="850" alt="TC04 PUT Post Result" src="https://raw.githubusercontent.com/mahTuqn/Lab7/assets/tc04_put_update_post_200.png">
    </p>

    <hr>

    <!-- KỊCH BẢN 5: ĐỐI CHỨNG EXTERNAL API -->
    <h3>Kịch Bản Kiểm Thử Đối Chứng (TC05): Kiểm thử API Online qua Internet</h3>
    <ul>
        <li><strong>Tên Kịch Bản:</strong> Kiểm thử API công khai ngoài internet (Public External API)</li>
        <li><strong>URL:</strong> <code>https://random-data-api.com/api/v2/users?size=2&amp;is_xml=true</code></li>
        <li><strong>Kết Quả Thực Tế:</strong> <code>Could not get response - Error: getaddrinfo EAI_AGAIN random-data-api.com</code> (Lỗi nghẽn mạng / DNS bên ngoài)</li>
        <li><strong>Ý Nghĩa Thực Tiễn:</strong> Chứng minh tính ưu việt của giải pháp <strong>Mock Server nội bộ (Localhost)</strong>, giúp đội ngũ QA/Tester kiểm thử liên tục không phụ thuộc vào đường truyền quốc tế hay server bên thứ 3.</li>
        <li><strong>Ảnh chụp màn hình thực tế trên Postman:</strong></li>
    </ul>

    <p align="center">
        <img width="850" alt="TC05 Network Error on External API" src="https://raw.githubusercontent.com/mahTuqn/Lab7/assets/tc05_external_api_error.png">
    </p>

    <hr>

    <!-- TỔNG KẾT -->
    <p><strong>5. Tổng Kết Kết Quả Kiểm Thử Toàn Bộ Suite:</strong></p>
    <ul>
        <li><strong>Tổng số kịch bản đã kiểm thử:</strong> 5 (3 kịch bản Random Data API + 1 kịch bản RESTful PUT + 1 kịch bản đối chứng ngoại mạng)</li>
        <li><strong>Số kịch bản thành công trên server nội bộ:</strong> 4/4 (100%)</li>
        <li><strong>Số lỗi ghi nhận:</strong> 1 (Lỗi DNS / Timeout ngoại mạng khi gọi server ngoài)</li>
        <li><strong>Tỉ lệ thành công hệ thống:</strong> 100%</li>
    </ul>

    <p><strong>6. Phân Tích Lỗi &amp; Phòng Tránh:</strong></p>
    <ul>
        <li><strong>Lỗi 1 (404 Not Found):</strong> Xảy ra khi URL bị gõ sai cú pháp (thiếu resource path). Cần cấu hình biến môi trường <code>{{baseUrl}}</code> để tránh gõ nhầm.</li>
        <li><strong>Lỗi 2 (EAI_AGAIN):</strong> Do mất kết nối hoặc DNS của server ngoài internet. Đề xuất luôn sử dụng máy chủ Mock Server nội bộ (Port 5000) khi chạy automation test.</li>
    </ul>
</div>

---

## 🚀 HƯỚNG DẪN SỬ DỤNG DỰ ÁN VỚI POSTMAN

### Bước 1: Khởi động Máy chủ API Cục bộ
Mở Terminal trong thư mục `Lab7_API` và chạy:
```bash
npm start
```
*(Hoặc `node server.js` nếu chạy trực tiếp)*

Máy chủ sẽ khởi động tại:
- **Server URL:** `http://localhost:5000`
- **Giao diện Web Testing Studio:** `http://localhost:5000/`

---

### Bước 2: Import Bộ Test Collection & Environment vào Postman
Trong thư mục `Lab7_API/postman/` đã chuẩn bị sẵn 2 file:
1. `DGKDPM_Lab7_API.postman_collection.json` (Bộ sưu tập các request và test script).
2. `DGKDPM_Lab7_API.postman_environment.json` (Biến môi trường `{{baseUrl}} = http://localhost:5000`).

**Cách import vào Postman:**
1. Mở ứng dụng **Postman**.
2. Nhấn nút **Import** ở thanh bên trái trên cùng.
3. Kéo thả 2 file trên vào Postman hoặc chọn **Upload Files**.
4. Ở góc trên bên phải màn hình Postman, chọn Environment là: **DGKDPM Lab 7 - Local Environment**.

---

### Bước 3: Chạy Kiểm Thử Thủ Công Trên Từng Request
Mở Collection **DGKDPM Lab 7 API Collection**:
- Click vào `TC01 - GET Users` &rarr; Nhấn nút **Send** màu xanh.
- Click vào `TC02 - GET Invalid URL` &rarr; Nhấn nút **Send** &rarr; Xem phản hồi `404 Not Found`.
- Click vào `TC03 - GET Beers` &rarr; Nhấn nút **Send** &rarr; Xem thông tin bia `200 OK`.
- Chuyển sang tab **Test Results** ở khung bên dưới để thấy tất cả các test case đều xanh `PASS`.

---

### Bước 4: Chạy Tự Động Toàn Bộ Bằng Postman Collection Runner
1. Nhấn vào dấu 3 chấm `...` bên cạnh Collection **DGKDPM Lab 7 API Collection**.
2. Chọn **Run collection**.
3. Chọn môi trường **DGKDPM Lab 7 - Local Environment**.
4. Nhấn **Run DGKDPM Lab 7 API Collection**.
5. Toàn bộ các test case sẽ tự động chạy trong vài giây với kết quả hiển thị trực quan 100% Passed.

---

### Bước 5: Chạy Kiểm Thử Tự Động Bằng Dòng Lệnh (CLI)
Bạn cũng có thể chạy kiểm thử tự động ngay trong terminal bằng lệnh:
```bash
npm test
```
*(Hoặc `node test-runner.js`)*

Kết quả sẽ được xuất ra bảng tổng kết đầy đủ với màu sắc trực quan ngay trong cửa sổ dòng lệnh!
