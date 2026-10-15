# Báo cáo Thực hành DevOps - Session 12: Bài 2

## Tự động hóa đóng gói và phát hành Docker Image khi gắn Git Tag (v*.*.*)

### 1. Giới thiệu tổng quan
Bài thực hành cấu hình luồng Chuyển giao Liên tục (Continuous Delivery - CD) tự động phát hành phiên bản Docker Image chính thức khi có sự kiện gán nhãn phát hành (Git Tag dạng `v*.*.*`). Quy trình sử dụng `docker/metadata-action@v5` để tự động phân tích và trích xuất số phiên bản Semantic Versioning (SemVer) từ Git Tag để gắn thẻ tương ứng cho Docker Image trên GitHub Container Registry (`ghcr.io`).

### 2. Cấu trúc thư mục bài nộp
```text
SS12_HW02_DaoTrongTri_PTIT062/
├── .github/
│   └── workflows/
│       └── release.yml        # Kịch bản CD kích hoạt khi push Git Tag v*.*.*
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
└── REPORT.md                  # Báo cáo kỹ thuật chi tiết phân tích Git Tag & SemVer
```

### 3. Tóm tắt nội dung kịch bản CD (`.github/workflows/release.yml`)
- **Sự kiện kích hoạt (`on.push.tags`):** Mẫu `v*.*.*` (ví dụ: `v1.0.0`, `v1.0.1`).
- **Môi trường chạy:** `ubuntu-latest`.
- **Hành động cốt lõi:**
  1. `actions/checkout@v5`: Kéo mã nguồn đúng tại commit được gán tag.
  2. `setup-java@v5` & Gradle: Chạy Unit Test và đóng gói Fat JAR (`./gradlew bootJar`).
  3. `docker/metadata-action@v5`: Trích xuất tag `v1.0.0`, `1.0.0` và `latest`.
  4. `docker/login-action@v3`: Xác thực bảo mật vào `ghcr.io`.
  5. `docker/build-push-action@v5`: Đóng gói Dockerfile và đẩy Image lên GHCR.

Chi tiết phân tích cơ chế SemVer và hướng dẫn kiểm tra gói Image trên GitHub Packages xem tại [REPORT.md](REPORT.md).
