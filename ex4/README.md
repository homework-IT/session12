# Báo cáo Thực hành DevOps - Session 12: Bài 4

## Tự động triển khai ứng dụng Spring Boot (Container) lên VPS từ xa qua SSH

### 1. Giới thiệu tổng quan
Bài thực hành xây dựng đường ống phân phối và triển khai liên tục (CD Pipeline) tự động từ GitHub Actions tới máy chủ thực tế (VPS từ xa). Quy trình bao gồm 2 giai đoạn:
1. **Job `build-image`:** Biên dịch ứng dụng Spring Boot Gradle, đóng gói Dockerfile và đẩy container image lên GitHub Container Registry (`ghcr.io`).
2. **Job `deploy-vps`:** Sử dụng action an toàn `appleboy/ssh-action@v1.0.3` để mở phiên kết nối SSH mã hóa tới máy chủ VPS, tự động đăng nhập GHCR, kéo image mới nhất về và tái khởi động container ứng dụng phục vụ người dùng.

### 2. Cấu trúc thư mục bài nộp
```text
SS12_HW04_DaoTrongTri_PTIT062/
├── .github/
│   └── workflows/
│       └── deploy-vps.yml     # Kịch bản CI/CD Build Image & SSH Deploy VPS
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── src/
│   ├── main/
│   │   ├── java/com/ptit/devops_ss12/Application.java
│   │   └── resources/application.properties
│   └── test/
│       └── java/com/ptit/devops_ss12/ApplicationTests.java
├── .gitignore
├── build.gradle
├── Dockerfile                 # Đóng gói ứng dụng chạy trên Temurin 17 JRE Alpine
├── gradlew
├── gradlew.bat
├── settings.gradle
├── README.md                  # Hướng dẫn tổng quan bài tập
└── REPORT.md                  # Báo cáo kỹ thuật chi tiết phân tích SSH Action & Auto Deployment
```

### 3. Tóm tắt các biến Secrets cần thiết
- `SSH_HOST`: Địa chỉ IP máy chủ VPS (`103.72.57.148`).
- `SSH_USER`: Tên tài khoản người dùng (`devopsg5`).
- `SSH_KEY` / `SSH_PASSWORD`: Khóa bí mật SSH hoặc mật khẩu đăng nhập VPS.
- `REGISTRY_TOKEN`: Personal Access Token dùng để kéo private package từ GHCR.

Chi tiết phân tích kỹ thuật và hướng dẫn kiểm tra triển khai xem tại [REPORT.md](REPORT.md).
