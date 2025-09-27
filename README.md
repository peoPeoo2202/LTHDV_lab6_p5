# Token Auth (Express + JWT)

Ứng dụng mẫu minh họa xác thực bằng token (JWT) sử dụng Express, Mongoose, bcrypt và jsonwebtoken.

## Mô tả

Project cung cấp API đăng ký, đăng nhập và lấy thông tin profile (route bảo vệ bằng JWT). Đây là một ví dụ nhỏ phục vụ mục đích học tập.

## Yêu cầu

- Node.js (>=12)
- MongoDB (local hoặc remote)

## Cài đặt

Mở PowerShell ở thư mục project và chạy:

```powershell
npm install
```

## Cách chạy

Hiện tại project không có script `start` trong `package.json`, bạn có thể khởi động trực tiếp bằng lệnh:

```powershell
node app.js
```

Sau khi chạy, server mặc định lắng nghe ở: http://localhost:3000

### Lưu ý về cấu hình

Mã nguồn hiện đang dùng giá trị cứng cho MongoDB và JWT secret:

- MongoDB URI: `mongodb://127.0.0.1:27017/tokenAuthApp` (được đặt trực tiếp trong `app.js`)
- JWT secret: `'secretKey'` (được đặt trực tiếp trong `routes/auth.js`)

Đề xuất: thay thế bằng biến môi trường (`MONGO_URI`, `JWT_SECRET`, `PORT`) hoặc dùng `.env` + `dotenv` để bảo mật và linh hoạt.

Ví dụ chạy tạm với biến môi trường trong PowerShell:

```powershell
$env:MONGO_URI = "mongodb://127.0.0.1:27017/tokenAuthApp"; $env:JWT_SECRET = "replace_with_a_strong_secret"; $env:PORT = "3000"; node app.js
```

(Tuy nhiên hiện code chưa đọc các biến này — cần chỉnh sửa code để sử dụng `process.env`.)

## API Endpoints

Base path: `/api/auth`

- POST /register
  - Mô tả: Tạo người dùng mới
  - Body (JSON): { "username": "...", "email": "...", "password": "..." }
  - Trả về: { message: 'User registered successfully!' }

- POST /login
  - Mô tả: Đăng nhập, nhận JWT
  - Body (JSON): { "email": "...", "password": "..." }
  - Trả về: { token: "<jwt>" }

- GET /profile
  - Mô tả: Lấy thông tin người dùng (route bảo vệ)
  - Header: Authorization: Bearer <token>
  - Trả về: Thông tin user (không bao gồm password)

Ví dụ dùng curl (PowerShell):

```powershell
# Đăng ký
curl -Method POST -Uri http://localhost:3000/api/auth/register -Body (@{username='user';email='a@b.com';password='pass'} | ConvertTo-Json) -ContentType 'application/json'

# Đăng nhập
curl -Method POST -Uri http://localhost:3000/api/auth/login -Body (@{email='a@b.com';password='pass'} | ConvertTo-Json) -ContentType 'application/json'

# Lấy profile (thay <token> bằng token trả về)
curl -Method GET -Uri http://localhost:3000/api/auth/profile -Headers @{ Authorization = "Bearer <token>" }
```

## Cấu trúc thư mục chính

- `app.js` - Entry point của ứng dụng
- `package.json` - Dependencies
- `models/User.js` - Mongoose model cho User
- `routes/auth.js` - Các route liên quan đến xác thực

## Cải tiến đề xuất

- Di chuyển `secretKey` và MongoDB URI sang biến môi trường
- Thêm `start` script trong `package.json` (ví dụ: `"start": "node app.js"`)
- Dùng `dotenv` để load biến môi trường từ `.env`
- Thêm validation (ví dụ Joi hoặc express-validator)
- Thêm logging và tests

## Liên hệ

Bạn có thể chỉnh sửa phần tác giả trong `package.json` hoặc thêm thông tin liên hệ ở đây.
