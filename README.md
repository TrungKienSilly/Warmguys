[//]: # (Warmguys — README phiên bản chi tiết)

# Warmguys — Ứng dụng quản lý phòng tập

Phiên bản: code PHP thuần với giao diện Admin & User và MySQL làm database.

README này dựa trên mẫu cấu trúc tài liệu bạn cung cấp và đã được tinh chỉnh phù hợp với dự án Warmguys (PHP, MVC thuần).

## ✨ Tính năng chính

- Quản lý thiết bị (thêm, sửa, xóa, cập nhật trạng thái, hình ảnh)
- Quản lý gói tập và khuyến mãi
- Quản lý thành viên (thông tin, theo dõi tập luyện)
- Quản lý hóa đơn, gia hạn, và thống kê doanh thu
- Gửi thông báo / nhắc nhở bằng email (PHPMailer)
- Giao diện Admin/Người dùng tách biệt, nhiều trang và assets (JS/CSS/images)

## Công nghệ sử dụng

- Backend: PHP (mysqli)
- Database: MySQL / MariaDB
- Frontend: HTML, CSS, JavaScript (jQuery), template PHP
- Email: PHPMailer (đã có sẵn trong `View/Admin/assets/PHPMailer-master/`)
- Tài nguyên: nhiều thư viện vendor (morris, apex, owlcarousel, summernote, v.v.)

## Yêu cầu môi trường

- PHP 7.0+ (mysqli enabled) — nếu có phiên bản mới hơn thì càng tốt
- MySQL / MariaDB
- Apache (XAMPP/WAMP/Laragon) trên Windows (hoặc LAMP trên Linux)

## Cài đặt nhanh (Windows — dùng XAMPP)

1. Cài XAMPP (hoặc WAMP/Laragon) và bật Apache + MySQL.
2. Copy thư mục `Warmguys` vào thư mục web root (ví dụ `C:\xampp\htdocs\Warmguys`).
3. Tạo database MySQL tên `warmguys` (hoặc tên khác — nếu đổi thì chỉnh `Model/connect.php`).
   - Nếu bạn có file .sql, import vào database. Nếu chưa có, tôi có thể sinh schema cơ bản dựa trên code.
4. Cấu hình kết nối DB: mở `Model/connect.php` và chỉnh thông số nếu cần.

Ví dụ (mặc định hiện tại) trong `Model/connect.php`:

 - host: `localhost`
 - user: `root`
 - password: `` (rỗng)
 - database: `warmguys`

Nếu sử dụng XAMPP và user/password khác, cập nhật tương ứng.

## Chạy ứng dụng

1. Khởi động Apache + MySQL (XAMPP Control Panel).
2. Truy cập các đường dẫn:
   - Giao diện người dùng: http://localhost/Warmguys/View/User/index.html
   - Giao diện quản trị: http://localhost/Warmguys/View/Admin/login.php

## Cấu trúc thư mục chính (tóm tắt)

```
Warmguys/
├─ Controller/           # Controllers xử lý request
│  ├─ DeviceController.php
│  ├─ DeviceQL.php
│  └─ c_* (các controller chức năng)
├─ Model/                # Models & DB helpers
│  ├─ connect.php        # Kết nối DB (mysqli)
│  ├─ Modeldevice.php
│  └─ quanly*.php        # Các lớp quản lý khác
├─ View/
│  ├─ Admin/             # Giao diện quản trị (PHP + assets)
│  │  ├─ *.php
│  │  └─ assets/         # vendor, PHPMailer, CSS/JS/images
│  └─ User/              # Giao diện người dùng (HTML, assets)
└─ README.md
```

## PHPMailer

- Thư viện nằm ở `View/Admin/assets/PHPMailer-master/`.
- Để cấu hình SMTP (gmail hoặc SMTP khác), chỉnh file xử lý gửi mail trong `View/Admin/` (hoặc tạo 1 file config riêng) và cung cấp: host, port, username, password, secure.
- KHÔNG commit thông tin nhạy cảm (username/password) lên repo công khai — lưu vào biến môi trường hoặc file cấu hình gitignored.

## Gợi ý bảo mật & nâng cấp (quan trọng)

1. Mật khẩu:
   - Hiện code so sánh mật khẩu trực tiếp với trường `Password` trong DB (không hash). Nên dùng password_hash() khi lưu và password_verify() khi kiểm tra.
2. SQL Injection:
   - Nhiều nơi đang xây dựng câu SQL bằng nối chuỗi (ví dụ: "SELECT ... WHERE id = $id"). Chuyển sang prepared statements (mysqli_prepare / bind_param) để tránh injection.
3. Quyền DB:
   - Tránh dùng `root` với mật khẩu rỗng. Tạo user DB riêng với quyền giới hạn cho ứng dụng.
4. XSS / Output escaping:
   - Khi echo dữ liệu từ DB ra HTML, dùng hàm escape (htmlspecialchars) để tránh XSS.
5. Đặt cấu hình nhạy cảm vào file ngoài repo (vd. `config.php` trong .gitignore) hoặc biến môi trường.

## Kiểm thử & phát triển

- Đây là ứng dụng PHP thuần; không có package manager mặc định cho backend. Để phát triển:
  - Mở file tương ứng trong `Controller/`, `Model/`, `View/` và sửa.
  - Các asset JS/CSS nằm trong `View/Admin/assets/` và `View/User/assets/`.

## Gợi ý hành động tôi có thể làm tiếp (chọn 1 hoặc nhiều)

1. Sinh file SQL (schema) cơ bản từ các truy vấn tìm thấy trong code để bạn import vào MySQL.
2. Tạo một file cấu hình mẫu `.env.example` hoặc `config.sample.php` và hướng dẫn cách dùng biến môi trường để lưu thông tin DB/SMTP an toàn.
3. Quét nhanh code (Controller/Model) để liệt kê chỗ dùng SQL string concatenation — tôi sẽ tạo một danh sách cần sửa để chuyển sang prepared statements.
4. Viết small script PHP để migrate mật khẩu hiện có (nếu bạn muốn hash mật khẩu cũ).

## Đóng góp

1. Fork repository
2. Tạo branch: `git checkout -b feature/ten-tinh-nang`
3. Commit & push
4. Tạo Pull Request

## License

Dự án được phát hành theo MIT License.

## Tác giả

- Repository owner: Devteam

---
## <a=href"https://tienchung21.github.io/warmguys/View/User/about.html">Trang Chủ</a>


Nếu bạn muốn tôi tiếp tục, chọn một trong các tác vụ ở phần "Gợi ý hành động tôi có thể làm tiếp" — tôi có thể bắt đầu sinh file SQL hoặc quét các điểm nguy cơ bảo mật ngay lập tức.


