# Hệ thống bán tài khoản & dịch vụ

## 1. Công nghệ sử dụng

- **Backend Framework:** Laravel (PHP >= 8.2)
- **Database:** MySQL / MariaDB
- **Frontend:** Blade Templates (Laravel), HTML5, CSS3, JavaScript (Alpine.js / jQuery tùy theo theme)
- **Xác thực:** Session-based authentication, Laravel Socialite
- **Thanh toán tích hợp:** PayPal, Banking (Chuyển khoản ngân hàng nội địa), Card (Thẻ cào)
- **Queue & Cronjob:** Laravel Scheduler + Database queue driver
- **API:** Laravel Sanctum or Passport (dựa theo route api)

## 2. Tính năng chính của hệ thống

### 2.1. Module người dùng (User)

| Chức năng | Mô tả |
|-----------|-------|
| Đăng nhập / Đăng ký | Hỗ trợ login qua Google, Telegram |
| Quên mật khẩu | Gửi link reset qua email |
| Xem trang chủ | Hiển thị sản phẩm nổi bật, mới cập nhật |
| Danh mục sản phẩm | Xem theo slug category |
| Chi tiết sản phẩm | Xem thông tin, mua tài khoản |
| Blog & bài viết | Xem danh mục bài viết, chi tiết article |
| Profile người bán | Xem thông tin người bán theo slug |
| Tra cứu bảo hành | Form nhập mã để tra thông tin bảo hành |
| Giỏ hàng / Đơn hàng | Xem lịch sử đơn hàng, chi tiết order |
| Tải tài khoản | Download file tài khoản sau khi mua |
| Nhắn tin / Inbox | Liên hệ với người bán |
| Đánh dấu người bán | Bookmark seller yêu thích |
| Xem lịch sử đăng nhập | Login history |

### 2.2. Module Apple ID (Thuê tài khoản Apple)

| Chức năng | Mô tả |
|-----------|-------|
| Danh sách gói thuê | Xem các gói Apple ID theo plan |
| Chi tiết Apple ID | Hiển thị thông tin chi tiết |
| Thuê Apple ID | Người dùng đặt thuê và gia hạn |
| Lịch sử thuê | Admin xem danh sách rent history |

### 2.3. Module thanh toán (Payment)

- **PayPal:** Tạo order, capture payment qua API
- **Banking:** Tạo đơn chuyển khoản, kiểm tra trạng thái giao dịch
- **Card (thẻ cào):** Nạp thẻ cào điện thoại, callback xử lý

### 2.4. Module quản trị admin (tb-admin)

Admin có toàn quyền quản lý qua prefix `/tb-admin`

| Nhóm chức năng | Các chức năng cụ thể |
|----------------|----------------------|
| Quản lý tài khoản | Category, Product, Stock (kho), Account |
| Quản lý ngân hàng | Danh sách, chỉnh sửa thông tin bank |
| Quản lý blog | Category, Article |
| Quản lý booking | Danh sách booking, chỉnh sửa |
| Quản lý người dùng | Users, User Contact |
| Quản lý thanh toán | Payment lists, chỉnh sửa |
| Quản lý giảm giá | Discount lists, edit |
| Quản lý bảo hành | Warranties, edit |
| Quản lý sản phẩm | Product, Order, Category, Sub category |
| Báo cáo | Report list, edit |
| Đánh giá | Reviews list |
| Hỗ trợ | Support list, edit |
| Cài đặt chung | Setting general |
| Apple ID quản lý | Apple list, Rent history |

### 2.5. Module API (Dành cho tích hợp & cron)

| Endpoint | Mục đích |
|----------|----------|
| POST /service | Service chính cho người dùng |
| POST /auth/service | Service có xác thực user |
| POST /paypal/create-order | Tạo order PayPal |
| POST /paypal/capture-order | Capture thanh toán PayPal |
| POST /create-banking | Tạo đơn banking |
| POST /create-card | Nạp thẻ cào |
| POST /rent/appleid | Thuê Apple ID |
| POST /renew/appleid | Gia hạn Apple ID |
| POST /tb-admin/service | API dành riêng cho admin |
| POST /forget-password | API quên mật khẩu (rate limit 3 lần/phút) |
| POST /v2/payments | Cronjob xử lý thanh toán |
| POST /cron-transfaction | Cronjob xử lý giao dịch |
| GET /cron-reset | Reset số dư seller |

### 2.6. Middleware đặc biệt

- `guest`: Dành cho route chưa đăng nhập
- `auth`: Yêu cầu đăng nhập
- `tb.isAdmin`: Kiểm tra quyền admin
- `tb.isAdminAPI`: Kiểm tra quyền admin qua API
- `checking.user`: Kiểm tra user hợp lệ trước khi vào API
- `throttle:100,1`: Giới hạn 100 request/phút
- `csrf.check`: Kiểm tra CSRF token

## 3. Cài đặt hệ thống

### 3.1. Yêu cầu máy chủ

- PHP >= 8.1
- Composer
- MySQL >= 5.7 hoặc MariaDB >= 10.2
- Node.js (nếu cần compile frontend)
- Web server: Nginx / Apache
