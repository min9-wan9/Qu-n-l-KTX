cat > README.md << 'EOF'
# 🏢 Hệ thống Quản lý Ký túc xá Sinh viên

Ứng dụng web full-stack hỗ trợ quản lý ký túc xá cho trường đại học: sinh viên đăng ký phòng trực tuyến, quản trị viên duyệt đơn, quản lý hóa đơn, bảo trì và tài khoản người dùng.

> Dự án cá nhân — xây dựng nhằm thực hành kiến trúc **client - server** hiện đại với TanStack Start, React 19 và PostgreSQL.

---

## 🚀 Tính năng chính

### 👤 Dành cho Sinh viên
- Đăng ký / đăng nhập tài khoản, quản lý hồ sơ cá nhân
- Xem danh sách phòng (Tiêu chuẩn / VIP), lọc theo loại, xem cơ sở vật chất
- Đăng ký phòng theo 2 diện: **Hoàn cảnh khó khăn** hoặc **Xét GPA & Điểm rèn luyện**
- Tải lên ảnh minh chứng (giấy xác nhận hộ nghèo, bảng điểm...)
- Theo dõi trạng thái đơn đăng ký, xem hóa đơn hàng tháng, gửi yêu cầu bảo trì
- Nhận thông báo từ ban quản lý

### 🛠 Dành cho Quản trị viên (Admin)
- **Quản lý tài khoản sinh viên**: thêm / sửa / xóa (server function bảo mật, kiểm tra quyền admin)
- **Quản lý phòng**: CRUD phòng, cấu hình giá, sức chứa, cơ sở vật chất
- **Duyệt đơn đăng ký**: xem minh chứng, phê duyệt hoặc từ chối
- **Quản lý hóa đơn**: tạo, cập nhật trạng thái thanh toán
- **Quản lý yêu cầu bảo trì**: tiếp nhận và xử lý phản hồi
- Dashboard tổng quan số liệu

---

## 🏗 Kiến trúc hệ thống

Mô hình **Client - Server** ba tầng:

```
┌────────────────────┐    HTTPS/RPC    ┌────────────────────┐    SQL     ┌──────────────┐
│  Client (Browser)  │ ──────────────► │  Server (Edge FN)  │ ─────────► │  PostgreSQL  │
│  React 19 + Vite   │ ◄────────────── │  TanStack Start    │ ◄───────── │  + RLS       │
└────────────────────┘                 └────────────────────┘            └──────────────┘
```

- **Client**: React 19, TypeScript, TanStack Router (file-based routing), TanStack Query, TailwindCSS v4, shadcn/ui, React Hook Form + Zod.
- **Server**: TanStack Start `createServerFn` chạy trên Cloudflare Workers (Edge Runtime).
- **Database & Auth**: PostgreSQL + Auth, bảo mật bằng **Row Level Security (RLS)** và bảng `user_roles` riêng biệt (chống privilege escalation).
- **Storage**: Object Storage cho ảnh minh chứng đăng ký.

---

## 🛠 Công nghệ sử dụng

| Nhóm            | Công nghệ                                                  |
|-----------------|------------------------------------------------------------|
| Frontend        | React 19, TypeScript, Vite 7, TailwindCSS v4, shadcn/ui    |
| Routing / SSR   | TanStack Start, TanStack Router                            |
| Data fetching   | TanStack Query, Zod                                        |
| Backend         | TanStack Server Functions, Cloudflare Workers              |
| Database        | PostgreSQL, RLS Policies, Security Definer Functions       |
| Auth            | JWT, role-based access control                             |
| Deployment      | Cloudflare Workers Edge Runtime                            |

---

## 🔐 Bảo mật

- **Row Level Security** trên toàn bộ bảng public.
- Vai trò (`admin` / `student`) lưu ở bảng `user_roles` riêng, kiểm tra qua hàm `has_role()` `SECURITY DEFINER`.
- Server function admin xác thực token qua middleware `requireSupabaseAuth` và **assertAdmin** trước mọi thao tác nhạy cảm.
- Service role key chỉ dùng ở tầng server, không bao giờ lộ ra client.

---

## 📂 Cấu trúc thư mục

```
src/
├── routes/                # File-based routing
│   ├── admin/             # Trang quản trị: students, rooms, bills, ...
│   ├── rooms.tsx          # Danh sách & đăng ký phòng
│   ├── my-bills.tsx       # Hóa đơn của tôi
│   └── ...
├── lib/                   # Server functions (*.functions.ts) & utils
├── components/            # UI components (shadcn/ui)
├── integrations/supabase/ # DB clients (browser / server / admin)
└── styles.css             # TailwindCSS v4 tokens
```

---

## ⚙️ Cài đặt & Chạy local

```bash
# 1. Cài dependencies
bun install

# 2. Cấu hình biến môi trường trong .env

# 3. Chạy dev server
bun run dev

# 4. Build production
bun run build
```

---

## 📸 Demo

- 🔗 **Live demo**: https://app-clone-deploy.lovable.app

---

## 👨‍💻 Vai trò của tôi trong dự án

- Thiết kế toàn bộ **schema cơ sở dữ liệu**, viết migration SQL, cấu hình RLS policies.
- Xây dựng **luồng xác thực & phân quyền** an toàn (JWT + `user_roles` + `has_role`).
- Phát triển toàn bộ **UI/UX** cho hai vai trò (Sinh viên & Admin) với shadcn/ui.
- Cài đặt **server functions** cho thao tác admin (thêm/sửa/xóa sinh viên) sử dụng service role có kiểm soát.
- Tích hợp **upload ảnh minh chứng** qua Object Storage.
- Triển khai lên **Cloudflare Workers** (Edge Runtime).

---

## 📝 License

MIT © 2026
EOF
echo done