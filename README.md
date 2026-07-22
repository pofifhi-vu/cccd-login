# CCCDLogin - Đăng nhập Windows bằng thẻ CCCD (Smart Card)

[English Version Below](#english-version)

CCCDLogin là một giải pháp mã nguồn mở bằng C++ giúp biến thẻ Căn cước công dân gắn chíp (CCCD) của Việt Nam thành chiếc chìa khóa bảo mật vật lý để đăng nhập và bảo vệ máy tính Windows của bạn qua khe đọc thẻ Smart Card (PCSC).

---

## 🌟 Các tính năng nổi bật

1. **Nhận diện Realtime**: Chỉ cần cắm thẻ CCCD vào khe đọc khi đang ở màn hình khóa, Windows sẽ ngay lập tức nhận diện và xử lý đăng nhập mà không cần click chuột.
2. **Đăng nhập tự động không cần mật khẩu (Instant Auto-Login)**: Lưu mật khẩu đăng nhập Windows dưới dạng mã hóa bằng công nghệ **DPAPI** của Microsoft ở cấp độ phần cứng. Khi cắm đúng thẻ, máy tự động mở khóa vào thẳng Desktop.
3. **Tự động khóa máy khi rút thẻ**: Dịch vụ Windows Service chạy ngầm giám sát khe đọc thẻ. Khi bạn rút thẻ CCCD ra, máy tính sẽ lập tức tự động khóa màn hình (`Win + L`) để bảo vệ dữ liệu.
4. **An toàn & Riêng tư**: Ứng dụng chỉ đọc các thông tin định danh không đổi trên chip (CPLC/Card Serial) để tạo dấu vân tay số (fingerprint SHA-256) xác thực thẻ, hoàn toàn không truy cập hay lưu trữ thông tin cá nhân của bạn.
5. **Đăng nhập phụ trợ**: Tùy chọn đăng nhập bằng CCCD được thêm vào màn hình khóa song song, bạn và người thân vẫn có thể đăng nhập bằng mật khẩu Windows thông thường.

---

## 🛠️ Yêu cầu hệ thống

- Máy tính chạy **Windows 10 / Windows 11 (Pro/Enterprise)**.
- Có khe đọc thẻ Smart Card tích hợp (ví dụ khe SC trên các dòng Dell Latitude, ThinkPad doanh nghiệp) hoặc đầu đọc thẻ SC cắm cổng USB.
- Thẻ Căn cước công dân gắn chíp của Việt Nam.

---

## 📦 Hướng dẫn cài đặt nhanh

1. Tải thư mục ứng dụng đã được biên dịch sẵn.
2. Nhấp chuột phải vào file `CCCDSetup.exe` ➔ Chọn **Run as administrator**.
3. Cắm thẻ CCCD của bạn vào khe đọc thẻ khi chương trình yêu cầu.
4. Chọn **`y`** khi được hỏi muốn bật tính năng tự động mở máy không, sau đó nhập mật khẩu Windows của bạn (các ký tự sẽ hiển thị dưới dạng ẩn `****`).
5. Đợi chương trình thông báo cài đặt hoàn tất và **Khởi động lại máy (Restart)**.

*Để gỡ cài đặt hoàn toàn:* Nhấp chuột phải vào file `uninstall.bat` ➔ Chọn **Run as administrator**.

---

## 📐 Kiến trúc dự án

Dự án được xây dựng từ 4 thành phần chính:
- **`Shared`**: Thư viện dùng chung quản lý kết nối PCSC Smart Card (`WinSCard`), mã hóa SHA-256 (`BCrypt`), và bảo mật thông tin (`DPAPI`).
- **`CCCDCredentialProvider`**: COM DLL đăng ký trực tiếp vào hệ thống giao diện đăng nhập của Windows (`LogonUI`).
- **`CCCDCardMonitor`**: Windows Service chạy nền bằng tài khoản `SYSTEM` để giám sát sự kiện rút thẻ và phát lệnh khóa máy vượt Session 0.
- **`CCCDSetup`**: Tiện ích dòng lệnh hỗ trợ thiết lập ban đầu.

---

## 🛠️ Biên dịch từ mã nguồn

Dự án sử dụng **CMake** và yêu cầu bộ công cụ phát triển **C++** của **Visual Studio 2022** (Desktop Development với C++):

```bash
# Cấu hình build
cmake -S . -B build -G "Visual Studio 17 2022" -A x64

# Biên dịch bản Release
cmake --build build --config Release
```

Các file thực thi sau khi build thành công sẽ nằm trong thư mục `build/*/Release/`.

---

<a name="english-version"></a>
# CCCDLogin - Windows Smart Card Logon via CCCD (Vietnamese ID Card)

CCCDLogin is an open-source C++ solution that turns your chip-based Vietnamese Identity Card (CCCD) into a physical security key to unlock and protect your Windows workstation via a Smart Card reader (PCSC).

## 🌟 Key Features

- **Realtime Detection**: Plug in your CCCD at the Windows logon screen, and the system instantly detects the card without any clicks.
- **Instant Auto-Login**: Safely encrypts and stores your Windows credentials using Microsoft's **DPAPI**. Inserting the card automatically authenticates and logs you directly into the Desktop.
- **Auto-Lock on Removal**: A background Windows Service monitors the smart card state. Pulling the card out immediately locks the workstation (`Win + L`).
- **Security & Privacy**: Only reads permanent hardware chips identifier (CPLC/Card Serial) to create a SHA-256 fingerprint for card authentication. No personal information is read or stored.
- **Co-existence**: Runs alongside standard Windows login methods (Password, PIN, Windows Hello).

## 📦 Setup & Usage

1. Download the pre-compiled binaries folder.
2. Right-click `CCCDSetup.exe` ➔ Select **Run as administrator**.
3. Insert your CCCD card into the reader when prompted.
4. Type `y` to enable instant auto-login and input your Windows password (masked with `****`).
5. Restart your computer.

*To uninstall:* Right-click `uninstall.bat` ➔ Select **Run as administrator**.

---

## 📄 License
Project is licensed under the [MIT License](LICENSE).
