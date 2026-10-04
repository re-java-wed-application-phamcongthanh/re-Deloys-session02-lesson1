# Bài 1: Khởi tạo Droplet trên DigitalOcean

## 🎯 1. Mục Tiêu
- Thực hành đăng ký tài khoản, lựa chọn cấu hình phần cứng phù hợp và khởi tạo thành công Droplet chạy hệ điều hành Ubuntu Server trên hạ tầng đám mây DigitalOcean.
- Cấu hình xác thực an toàn bằng cặp khóa SSH (SSH Keypair) thay vì mật khẩu thô và thực hiện kết nối thành công từ máy cá nhân.

---

## 📋 2. Yêu Cầu Chi Tiết (Requirements)
- **Bối cảnh**: Nhóm DevOps cần tạo một máy chủ thử nghiệm trên hạ tầng đám mây DigitalOcean để chuẩn bị chạy các dự án Web.
- **Ràng buộc**:
  1. Tạo Droplet chạy hệ điều hành **Ubuntu 22.04 LTS** (hoặc bản mới nhất).
  2. Lựa chọn cấu hình tối giản tiết kiệm chi phí: **Basic plan, Regular SSD, CPU Shared** ($4/tháng hoặc $6/tháng).
  3. Chọn datacenter region gần Việt Nam nhất (**Singapore**).
  4. Tại mục Authentication, bắt buộc chọn **SSH Keys**. Tạo mới một cặp khóa SSH từ máy cá nhân, thêm khóa công khai (Public Key) vào tài khoản DigitalOcean và gán vào Droplet khi khởi tạo.
  5. Kết nối thành công tới Droplet thông qua SSH từ máy tính cá nhân bằng tài khoản mặc định `root`.

---

## 🛠️ 3. Hướng Dẫn Các Bước Thực Hiện (Step-by-Step Guide)

### Bước 1: Tạo khóa SSH trên máy cá nhân
Mở Terminal (trên Linux/macOS) hoặc Git Bash/PowerShell (trên Windows) và chạy lệnh:
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- Nhấn `Enter` để lưu vào đường dẫn mặc định (`~/.ssh/id_ed25519`).
- Đọc nội dung file Public Key:
```bash
cat ~/.ssh/id_ed25519.pub
```

### Bước 2: Khởi tạo Droplet trên DigitalOcean
1. Đăng nhập trang quản trị DigitalOcean Console -> Click nút **Create** -> chọn **Droplets**.
2. **Region**: Chọn **Singapore**.
3. **Choose an image**: Chọn **Ubuntu 22.04 LTS (x64)**.
4. **Choose Size**:
   - Chọn loại **Basic**.
   - CPU options: chọn **Regular SSD** ($4/tháng hoặc $6/tháng).
5. **Authentication**:
   - Chọn **SSH Keys**.
   - Bấm **New SSH Key**, dán toàn bộ nội dung file `.pub` vừa copy ở Bước 1 vào.
   - Đặt tên SSH Key (ví dụ: `my-devops-key`) và bấm **Add SSH Key**.
6. **Finalize Details**:
   - Đặt tên Hostname: `devops-ubuntu-droplet`.
7. Nhấn **Create Droplet** và chờ hệ thống cấp IP công cộng (Public IP).

### Bước 3: Kết nối SSH tới Droplet
Mở Terminal máy cá nhân và chạy lệnh:
```bash
ssh -i ~/.ssh/id_ed25519 root@<IP_ADDRESS_DROPLET>
```

---

## ✅ 4. Lệnh Kiểm Tra & Kết Quả Mong Đợi (Verification)
- **Lệnh kiểm tra**:
  ```bash
  ssh -i ~/.ssh/id_ed25519 root@<IP_ADDRESS_DROPLET>
  ```
- **Kết quả mong đợi**: Truy cập thành công vào giao diện dòng lệnh của Droplet mà không cần nhập mật khẩu. Terminal hiển thị thông tin chào mừng của Ubuntu Server.
  ```text
  Welcome to Ubuntu 22.04 LTS (GNU/Linux x86_64)
  ...
  root@devops-ubuntu-droplet:~#
  ```

---

## 💡 5. Gợi Ý & Best Practices
- Khóa công khai (Public Key) nằm ở file có đuôi `.pub` (`~/.ssh/id_ed25519.pub`). Copy toàn bộ nội dung dán vào DigitalOcean Console.
- Tuyệt đối giữ bí mật file Private Key (`id_ed25519`) và không bao giờ commit Private Key lên GitHub.
