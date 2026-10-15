# Báo cáo Thực hành DevOps - Session 12: Bài 3

## Tách biệt luồng CI/CD đa môi trường (Staging & Production) với GitHub Environments

### 1. Giới thiệu tổng quan
Bài thực hành triển khai mô hình quản lý vòng đời phát hành phần mềm đa môi trường thông qua tính năng GitHub Environments. Hệ thống phân định ranh giới rõ ràng giữa môi trường Thử nghiệm (`staging`) được triển khai tự động từ nhánh `develop`, và môi trường Thực tế (`production`) được bảo vệ nghiêm ngặt bằng Environment Protection Rules (yêu cầu phê duyệt thủ công - Manual Approval / Required Reviewers) khi kích hoạt từ nhánh chính `main`.

### 2. Cấu trúc thư mục bài nộp
```text
SS12_HW03_DaoTrongTri_PTIT062/
├── .github/
│   └── workflows/
│       └── cd-multi-env.yml   # Kịch bản CD đa môi trường Staging & Production
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
└── REPORT.md                  # Báo cáo kỹ thuật chi tiết về GitHub Environments & Deployment Gates
```

### 3. Tóm tắt cấu hình Jobs trong Workflow
1. **Job `build-and-test`:** Biên dịch, chạy JUnit test và đóng gói `bootJar` cho tất cả các nhánh.
2. **Job `deploy-staging`:** Gán nhãn `environment: staging`, kích hoạt tự động khi push nhánh `develop`.
3. **Job `deploy-prod`:** Gán nhãn `environment: production` với URL `https://api.prod.example.com`, kích hoạt khi push nhánh `main` và yêu cầu xác thực phê duyệt bởi người có thẩm quyền.

Chi tiết phân tích cơ chế bảo vệ môi trường và hướng dẫn kiểm tra phê duyệt deployment xem tại [REPORT.md](REPORT.md).
