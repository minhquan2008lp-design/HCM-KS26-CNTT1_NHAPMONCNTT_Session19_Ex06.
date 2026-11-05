Bảo mật mã nguồn trước khi đưa lên GitHub Public
Mục tiêu là xây dựng một repository mẫu cho dự án thương mại điện tử, đảm bảo không công khai API Key, mật khẩu, thông tin Database và các dữ liệu bí mật.

Mình sẽ chia bài thành 4 phần:
1. Pre-push Checklist – kiểm tra trước khi Push.
2. .env.example – cấu trúc biến môi trường Dev, Test, Production.
3. GitHub 2FA – xác thực hai lớp cho thành viên.
4. Báo cáo rủi ro – đánh giá hậu quả nếu thiếu .gitignore.
1. Cấu trúc repository mẫu
ecommerce-secure/
│
├── .github/
│   └── workflows/
│       └── secret-scan.yml
│
├── src/
│   └── README.md
│
├── .env.example
├── .gitignore
├── .gitleaks.toml
├── PRE-PUSH-CHECKLIST.md
├── SECURITY.md
└── RISK-REPORT.md


Nguyên tắc bảo mật:
- .env: chứa thông tin bí mật, không được đưa lên GitHub.
- .env.example: chỉ chứa tên biến và giá trị mẫu giả.
- .gitignore: ngăn Git theo dõi các tệp nhạy cảm.
- Secret scanning: tự động kiểm tra mã nguồn để phát hiện thông tin bí mật.
2. Nội dung các tệp quan trọng
A. Pre-push Checklist
Trước mỗi lần chạy git push, thành viên cần kiểm tra:
Kiểm tra	Yêu cầu
.env	Không được commit
API Key	Không hardcode trong source code
Password	Không ghi trực tiếp trong code
Private Key	Không được đưa lên GitHub
Git history	Kiểm tra cả những commit cũ
Gitleaks	Không có cảnh báo chưa xử lý
2FA	Tất cả thành viên đã bật

B. Cấu trúc .env.example
Tệp mẫu đã được chia thành ba môi trường:
Môi trường	Mục đích
DEV	Lập trình trên máy cá nhân
TEST	Kiểm thử ứng dụng
PRODUCTION	Chạy ứng dụng thực tế

Ví dụ:
# DEV
APP_ENV=development
DEV_DATABASE_URL=your_local_database
DEV_PAYMENT_API_KEY=replace_with_test_key

# TEST
TEST_DATABASE_URL=your_test_database
TEST_PAYMENT_API_KEY=replace_with_test_key

# PRODUCTION
PROD_DATABASE_URL=inject_from_secret_store
PROD_PAYMENT_API_KEY=inject_from_secret_store


Không đưa giá trị bí mật thật vào tệp này. Production secrets nên được lưu trong kho quản lý bí mật riêng hoặc GitHub Environments.
C. Thiết lập 2FA bắt buộc
Có hai cấp độ cần phân biệt:

Cấp độ 1 – Từng tài khoản
1. Đăng nhập GitHub.
2. Vào Settings.
3. Chọn Password and authentication.
4. Tìm Two-factor authentication.
5. Thiết lập ứng dụng Authenticator hoặc Passkey.
6. Lưu Recovery Codes ở nơi an toàn.
Cấp độ 2 – Bắt buộc toàn team

Repository thuộc GitHub Organization có thể áp dụng chính sách bắt buộc 2FA:
1. Vào trang Organization.
2. Chọn Settings.
3. Chọn Authentication security.
4. Bật Require two-factor authentication for everyone in your organization.
5. Xác nhận thay đổi.
GitHub hỗ trợ chính sách này trên Organization Free, Team và các gói Enterprise phù hợp. Thành viên không đáp ứng yêu cầu có thể mất quyền truy cập tài nguyên cho đến khi bật 2FA. 
GitHub Docs+1


Lưu ý: Nếu repository thuộc tài khoản cá nhân, không thể dùng thiết lập này để bắt buộc 2FA cho tất cả cộng tác viên ở cấp repository. Nên đưa dự án vào Organization.
3. Báo cáo đánh giá rủi ro khi thiếu .gitignore
Mức độ rủi ro: CAO – NGHIÊM TRỌNG
Nguy cơ	Hậu quả
Lộ API Key	Người khác có thể sử dụng trái phép API
Lộ Database Password	Có nguy cơ truy cập dữ liệu khách hàng
Lộ Payment Secret	Có nguy cơ lạm dụng dịch vụ thanh toán
Lộ Private Key	Có nguy cơ giả mạo dịch vụ
Lộ file .env	Tiết lộ cấu hình và thông tin hệ thống