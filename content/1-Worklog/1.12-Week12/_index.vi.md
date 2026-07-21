---
title: "Nhật ký Tuần 12"
date: 2026-07-20
weight: 12
pre: " <b> 1.12. </b> "
chapter: false
---


**Thời gian:** 03/07/2026 - 09/07/2026

## Mục tiêu Tuần 12

- Hoàn thiện backend Node.js/PostgreSQL và triển khai kiến trúc AWS private.
- Phân phối frontend, tự động hóa, bảo mật, monitoring và kiểm thử end-to-end.

## Công việc đã thực hiện

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| Fri - 03/07 | Hoàn thiện backend route, truy cập PostgreSQL, validation và logic deadline-check. | 03/07/2026 | 03/07/2026 | Mã nguồn / [API Gateway HTTP API](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html) |
| Sat - 04/07 | Tạo RDS PostgreSQL private, khởi tạo schema và test kết nối database. | 04/07/2026 | 04/07/2026 | Ảnh 19 / [Tạo RDS DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html) |
| Sun - 05/07 | Khởi tạo EC2 backend private, deploy Node.js và cấu hình systemd service. | 05/07/2026 | 05/07/2026 | Ảnh 20 / [Bắt đầu với EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html) |
| Mon - 06/07 | Tạo Target Group và Internal ALB làm điểm vào private, sau đó kiểm tra target health. | 06/07/2026 | 06/07/2026 | Ảnh 21-23 / [ALB target health check](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) |
| Tue - 07/07 | Tạo VPC Link và API Gateway HTTP API, deploy API và test private integration. | 07/07/2026 | 07/07/2026 | Ảnh 24-28 / [HTTP API private integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-integrations-private.html) |
| Wed - 08/07 | Upload frontend lên S3/CloudFront; cấu hình SES, Lambda và EventBridge nhắc hạn. | 08/07/2026 | 08/07/2026 | Ảnh 29-45 / [CloudFront với S3 origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html) / [EventBridge Scheduler](https://docs.aws.amazon.com/scheduler/latest/UserGuide/getting-started.html) |
| Thu - 09/07 | Gắn WAF, test end-to-end, rà log, viết cleanup và tổng kết kết quả. | 09/07/2026 | 09/07/2026 | Ảnh 46 / [Gắn WAF Web ACL](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-associating-aws-resource.html) / [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) |

## Kết quả đạt được

- Hoàn thiện backend Node.js, RDS PostgreSQL và systemd service trên EC2 private; Target Group ghi nhận backend ở trạng thái healthy.
- Triển khai đường API private qua Internal ALB, VPC Link và API Gateway; phân phối frontend bằng S3/CloudFront và kiểm tra luồng Manager/Member.
- Hoàn thành luồng nhắc hạn EventBridge–Lambda–SES, bật AWS WAF, rà CloudWatch log và kiểm thử end-to-end toàn hệ thống.
