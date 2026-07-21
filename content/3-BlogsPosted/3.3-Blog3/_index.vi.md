---
title: "Dashboard quản lý bản vá đa tài khoản bằng Kiro Specs"
date: 2026-07-20
weight: 3
pre: " <b> 3.3. </b> "
chapter: false
---

## Thông tin bài viết

| Thuộc tính | Nội dung |
| --- | --- |
| Chủ đề | AWS Systems Manager, Kiro, Serverless, DevOps và quản lý tuân thủ |
| Hình thức | Nghiên cứu và tổng hợp từ AWS Cloud Operations Blog |
| Nền tảng đăng | AWS Study Group trên Facebook |
| Trạng thái | Đã đăng |
| Liên kết bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/share/p/1E51kMkYdK/) |
| Nguồn nghiên cứu | [Build a Multi Account Patch Compliance Dashboard with Kiro Specs](https://aws.amazon.com/blogs/mt/build-a-multi-account-patch-compliance-dashboard-with-kiro-specs/) |

## Bài toán quản lý bản vá đa tài khoản

Trong doanh nghiệp sử dụng nhiều tài khoản AWS, theo dõi trạng thái cập nhật bản vá là nhiệm vụ quan trọng của đội ngũ DevOps và Cloud Operations. AWS Systems Manager Patch Manager hỗ trợ quét và triển khai bản vá cho managed node, nhưng việc kiểm tra thủ công từng tài khoản trở nên mất thời gian khi quy mô tăng.

Các vấn đề chính:

- Dữ liệu tuân thủ nằm ở nhiều tài khoản.
- Khó xác định máy chủ chưa được vá.
- Tổng hợp báo cáo thủ công mất nhiều thời gian.
- Dữ liệu thô trên S3 có thể rất lớn.
- Đọc lại toàn bộ dữ liệu mỗi lần mở dashboard làm tăng độ trễ.
- Dashboard quản trị không nên public ra Internet.

## Kiến trúc dashboard

Giải pháp sử dụng AWS Systems Manager Resource Data Sync để tập trung dữ liệu tuân thủ vào Amazon S3:

~~~text
AWS Accounts
      ↓
Systems Manager Patch Manager
      ↓
Resource Data Sync
      ↓
Amazon S3 – Raw Compliance Data
      ↓
Scheduled Cache Lambda
      ↓
Amazon S3 – Aggregated JSON Cache
      ↓
API Lambda
      ↓
Internal Application Load Balancer
      ↓
React Dashboard
~~~

## Truy cập riêng tư bằng Session Manager

Dashboard không dùng public endpoint. Người quản trị truy cập thông qua Systems Manager Session Manager và port forwarding:

~~~text
Developer Laptop
       ↓
Systems Manager Port Forwarding
       ↓
Private EC2 Instance
       ↓
Internal Application Load Balancer
       ↓
Dashboard Application
~~~

Cách tiếp cận này:

- Không yêu cầu public IP.
- Không cần mở SSH ra Internet.
- Giữ ALB trong mạng nội bộ.
- Kiểm soát quyền truy cập bằng IAM.
- Giảm bề mặt tấn công của dashboard.

## Chiến lược cache

EventBridge kích hoạt Cache Lambda theo lịch. Lambda đọc dữ liệu thô, tổng hợp kết quả và ghi các file JSON nhỏ hơn vào S3:

~~~text
EventBridge Schedule
        ↓
Cache Lambda
        ↓
Read Raw S3 Data
        ↓
Aggregate Compliance Results
        ↓
Write JSON Cache to S3
~~~

Khi mở dashboard, API đọc dữ liệu đã xử lý từ vùng cache thay vì quét lại toàn bộ dữ liệu Resource Data Sync. Điều này giúp giảm thời gian tải trang, giảm xử lý lặp lại và thuận tiện cho việc xây dựng báo cáo tổng quan lẫn chi tiết.

## Phát triển theo đặc tả với Kiro

Điểm nổi bật của bài viết là quy trình spec-driven development:

~~~text
Requirements
      ↓
Design
      ↓
Implementation Tasks
~~~

### Requirements

Xác định hệ thống cần làm gì: hiển thị tổng quan tuân thủ, danh sách tài khoản, chi tiết instance, các bản vá còn thiếu và tiêu chí compliant.

### Design

Mô tả kiến trúc AWS, luồng request, mô hình dữ liệu, IAM Role, cơ chế cache, Security Group và yêu cầu bảo mật.

### Implementation Tasks

Chia thiết kế thành các nhiệm vụ nhỏ như tạo Lambda, S3 bucket, internal ALB, API, frontend, CloudFormation, test và kiểm tra bảo mật. Cách làm này giúp người phát triển xem xét yêu cầu và kiến trúc trước khi AI tạo mã nguồn.

## Steering files

Kiro sử dụng các file trong thư mục **.kiro/steering/** để duy trì nguyên tắc và ngữ cảnh dự án:

~~~text
.kiro/steering/
├── architecture.md
├── data-schemas.md
├── compliance-logic.md
├── frontend-specs.md
└── security.md
~~~

- **architecture.md:** kiến trúc, subnet, ALB và luồng giao tiếp.
- **data-schemas.md:** cấu trúc JSON, kiểu và quan hệ dữ liệu.
- **compliance-logic.md:** điều kiện để instance được xem là compliant.
- **frontend-specs.md:** bố cục giao diện, bảng, bộ lọc và biểu đồ.
- **security.md:** yêu cầu mã hóa, kết nối bảo mật, S3/ALB private và IAM tối thiểu.

## Bảo mật

Dashboard chứa thông tin về trạng thái bảo mật của hệ thống nên cần:

- Bật S3 Block Public Access và mã hóa dữ liệu.
- Sử dụng internal ALB.
- Không cấp public IP cho tài nguyên ứng dụng.
- Truy cập bằng Session Manager.
- Áp dụng nguyên tắc đặc quyền tối thiểu.
- Hạn chế Security Group.
- Quét Infrastructure as Code và kiểm tra IAM policy.
- Không ghi dữ liệu nhạy cảm vào log.

## Liên hệ với dự án Team Task Management

Tư duy phát triển theo đặc tả có thể áp dụng trực tiếp vào dự án cá nhân:

~~~text
project-specs/
├── architecture.md
├── database-schema.md
├── task-business-rules.md
├── api-contracts.md
├── security-requirements.md
└── testing-criteria.md
~~~

Ví dụ:

- **Business rules:** chỉ Manager được tạo và giao nhiệm vụ; Member chỉ cập nhật nhiệm vụ được giao; task đã hoàn thành không tiếp tục gửi email; không gửi email lặp lại trong 24 giờ.
- **Security requirements:** không commit file môi trường hoặc hard-code access key; RDS chỉ nhận kết nối từ backend Security Group; internal endpoint cần xác thực; CloudFront được bảo vệ bằng WAF.
- **Testing criteria:** login hoạt động đúng; task được lưu vào RDS; Target Group Healthy; deadline-check trả HTTP 200; Lambda có CloudWatch log; SES gửi email thành công.

## Bài học rút ra

- Không nên bắt đầu viết code khi yêu cầu chưa rõ.
- AI hoạt động tốt hơn khi có schema, quy tắc và acceptance criteria cụ thể.
- Bảo mật phải được xác định từ giai đoạn thiết kế.
- Dashboard nội bộ không nhất thiết phải public ra Internet.
- Cache giúp cải thiện hiệu năng và giảm xử lý lặp lại.
- Infrastructure as Code cần được review và kiểm tra bảo mật.
- AI hỗ trợ tăng tốc nhưng không thay thế trách nhiệm của kỹ sư.

> AI có thể giúp phát triển nhanh hơn, nhưng đặc tả rõ ràng mới giúp hệ thống được xây dựng đúng hướng.

{{% notice info %}}
Đây là bài nghiên cứu và phân tích từ AWS Cloud Operations Blog. Tôi chưa trực tiếp triển khai toàn bộ dashboard đa tài khoản trong môi trường doanh nghiệp, vì vậy nội dung tập trung vào kiến trúc, quy trình và bài học có thể áp dụng.
{{% /notice %}}
