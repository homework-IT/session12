# Báo cáo Thực hành DevOps - Session 12: Bài 1

## Xây dựng quy trình CI đa phiên bản JDK (Matrix Strategy) cho Java Spring Boot Gradle

### 1. Giới thiệu tổng quan
Bài thực hành triển khai kỹ thuật Ma trận kiểm thử tự động (Matrix Strategy) trong GitHub Actions cho ứng dụng Java Spring Boot sử dụng Gradle. Thông qua chiến lược này, hệ thống sẽ tự động rẽ nhánh và khởi chạy song song các Job kiểm thử độc lập trên nhiều phiên bản JDK khác nhau (`JDK 17` và `JDK 21`), đảm bảo tính tương thích ngược và ổn định của ứng dụng trên các môi trường runtime khác nhau mà không cần viết lặp lại các kịch bản workflow.

### 2. Cấu trúc thư mục bài nộp
```text
SS12_HW01_DaoTrongTri_PTIT062/
├── .github/
│   └── workflows/
│       └── ci-matrix.yml      # Kịch bản CI Matrix Strategy (Java 17 & 21)
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
├── gradlew
├── gradlew.bat
├── settings.gradle
├── README.md                  # Hướng dẫn tổng quan bài tập
└── REPORT.md                  # Báo cáo kỹ thuật chi tiết kèm log và minh chứng
```

### 3. Tóm tắt nội dung kịch bản CI (`.github/workflows/ci-matrix.yml`)
- **Tên Workflow:** `Java Spring Boot CI Matrix`
- **Sự kiện kích hoạt:** `push` lên nhánh `main`.
- **Chiến lược ma trận (`strategy.matrix`):** `java-version: ['17', '21']`.
- **Tối ưu hóa:** Bật `cache: 'gradle'` trong action `setup-java@v5` giúp tái sử dụng dependencies giữa các sub-job và giữa các lần chạy.

Chi tiết báo cáo và hướng dẫn xác nhận kết quả kiểm thử trên GitHub Actions vui lòng xem tại [REPORT.md](REPORT.md).
