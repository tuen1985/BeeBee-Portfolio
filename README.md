# BeeTho - Nền Tảng Bee Tho (Service Delivery Platform)
![BeeTho Logo](assets/BeeTho-Logo.png)

BeeTho là một nền tảng kết nối người dùng với các kỹ thuật viên chuyên nghiệp trong nhiều lĩnh vực khác nhau, từ sửa chữa điện dân dụng, điện lạnh đến các dịch vụ bảo trì kỹ thuật khác. Dự án bao gồm ứng dụng di động cho khách hàng và thợ, cùng với hệ thống quản lý backend mạnh mẽ.

---

## 🚀 Tính Năng Chính

- **Đa vai trò**: Hỗ trợ đồng thời hai loại người dùng: Khách hàng (Client) và Kỹ thuật viên (Technician).
- **Xác thực bảo mật**: Hệ thống đăng nhập/đăng ký tích hợp OTP qua Email và Firebase Authentication.
- **Dashboard thông minh**: Giao diện trực quan dành cho kỹ thuật viên để quản lý công việc và khách hàng để theo dõi dịch vụ.
- **Giao tiếp thời gian thực**: Sử dụng Socket.io để cập nhật trạng thái đơn hàng và thông báo tức thì.
- **Giao diện hiện đại**: Thiết kế Cyber-style, tối ưu trải nghiệm người dùng với Flutter.

---

## 🛠 Công Nghệ Sử Dụng

### Frontend (Mobile App/Web)
- **Framework**: Flutter (Dart)
- **Quản lý trạng thái**: GetX
- **Xác thực**: Firebase Auth
- **UI/UX**: Google Fonts, Flutter SVG, Shared Preferences.

### Backend (Server)
- **Runtime**: Node.js (Express)
- **Database**: MongoDB (với Mongoose)
- **Bảo mật**: JWT (JSON Web Tokens), Bcryptjs.
- **Giao tiếp**: Socket.io (Real-time), Nodemailer (Email/OTP).
- **Môi trường**: Dotenv.

---

## 📂 Cấu Trúc Thư Mục

```text
BeeTho/
├── frontend/             # Mã nguồn ứng dụng Flutter
│   ├── lib/              # Logic cốt lõi (Controllers, Views, Services)
│   ├── assets/           # Hình ảnh, biểu tượng (SVG/PNG)
│   └── pubspec.yaml      # Quản lý thư viện Flutter
├── backend/              # Mã nguồn máy chủ Node.js
│   ├── models/           # Định nghĩa các Schema (User, Technician, Task)
│   ├── routes/           # Các endpoint API (Auth, User API)
│   ├── services/         # Logic xử lý nghiệp vụ (OTP, Email service)
│   └── server.js         # Tệp cấu hình chính của server
└── README.md             # Tài liệu dự án
```

---

## ⚙️ Hướng Dẫn Cài Đặt

### 1. Chuẩn bị
Đảm bảo bạn đã cài đặt:
- Flutter SDK (phiên bản mới nhất)
- Node.js (v14+)
- MongoDB (Local hoặc Atlas)

### 2. Thiết lập Backend
```bash
cd backend
npm install
# Tạo tệp .env với các biến: MONGO_URI, JWT_SECRET, EMAIL_USER, EMAIL_PASS
node server.js
```

### 3. Thiết lập Frontend
```bash
cd frontend
flutter pub get
# Cấu hình Firebase (tệp firebase_options.dart)
flutter run
```

---

## 📄 Giấy Phép (License)
Dự án được phát triển dưới giấy phép ISC.

---

## 👥 Nhóm Thực Hiện
- **Duy trì bởi**: tuen1985
- **Tình trạng**: Đang trong quá trình hoàn thiện các tính năng cốt lõi.
## SaaS repair-facility refactor notes

BeeTho now models three system roles: `customer`, `owner`, and `staff`.
Users store `roles: []` and `selectedRole`; legacy `client` and
`technician` values remain only for old UI compatibility.

Main API groups:
- Auth: `/api/auth/register-or-login-phone`, `/api/auth/verify-otp`, `/api/auth/me`, `/api/auth/profile`, `/api/auth/switch-role`.
- Facilities: `/api/facilities`, `/api/facilities/nearby`, admin facility actions under `/api/admin/facilities/*`.
- Services: `/api/facilities/:facilityId/services`, `/api/services/:id`.
- Staff: `/api/facilities/:facilityId/staff/*`, `/api/staff/online-status`.
- Bookings/jobs: `/api/bookings/quote`, `/api/bookings`, `/api/bookings/my`, `/api/jobs/:id/*`.
- Emergency: `/api/emergency/facilities-nearby`, `/api/emergency/book`.
- Admin Web: `/api/admin/auth/*`, `/api/admin/*`.

Admin is a separate system role for `BeeTho-Admin/` only. The mobile app remains limited to
`customer`, `owner`, and `staff`.

Backend quick start:
```bash
cd backend
npm install
npm run seed
npm run dev
```

LAN web quick start on Windows:
```powershell
.\run_lan.ps1
```

The script starts the backend on `0.0.0.0:3000` and Flutter Web on
your LAN IP at port `8080`, then prints the LAN URL for other devices on the same network.
If another device cannot open the URL, allow Node.js and Flutter/Dart through
Windows Firewall.

If Windows has VPN/virtual adapters and the script picks the wrong IP, pass your
Wi-Fi/LAN IPv4 manually:
```powershell
.\run_lan.ps1 -LanIp 192.168.0.132
```

Customer warnings:
- Khi đơn chuyển sang chờ thanh toán, khách thanh toán trực tiếp cho thợ/cơ sở theo thỏa thuận.
- Mọi thay đổi giá cần được xác nhận trong app.
- Lịch sử sửa chữa được lưu để hỗ trợ khiếu nại và bảo hành.
