---
title: "Nhật ký Tuần 9"
date: 2026-07-20
weight: 9
pre: " <b> 1.9. </b> "
chapter: false
---


**Thời gian:** 12/06/2026 - 18/06/2026

## Mục tiêu Tuần 9

- Xác định luồng nghiệp vụ Manager/Member.
- Vẽ và cải thiện kiến trúc AWS qua phản hồi cộng đồng.

## Công việc đã thực hiện

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| Fri - 12/06 | Phân tích bài toán quản lý task, vai trò, trạng thái và yêu cầu nhắc deadline. | 12/06/2026 | 12/06/2026 | Ghi chú yêu cầu / [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |
| Sat - 13/06 | Vẽ kiến trúc đầu tiên gồm CloudFront, S3, API Gateway, VPC Link, ALB, EC2, RDS, Lambda và SES. | 13/06/2026 | 13/06/2026 | Sơ đồ kiến trúc / [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| Sun - 14/06 | Liệt kê lỗi target unhealthy, IAM và giới hạn SES Sandbox. | 14/06/2026 | 14/06/2026 | [ALB health check](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) / [SES Sandbox](https://docs.aws.amazon.com/ses/latest/dg/request-production-access.html) |
| Mon - 15/06 | Xin nhận xét kiến trúc từ các anh chị trong cộng đồng AWS FCAJ. | 15/06/2026 | 15/06/2026 | Ghi chú review / [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |
| Tue - 16/06 | Rà soát trách nhiệm dịch vụ và luồng request người dùng/tự động. | 16/06/2026 | 16/06/2026 | Review luồng / [AWS Architecture Center](https://aws.amazon.com/architecture/) |
| Wed - 17/06 | Chỉnh subnet, quan hệ Security Group, truy cập API private và luồng tự động hóa. | 17/06/2026 | 17/06/2026 | [API Gateway private integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-develop-integrations-private.html) / [Security Group](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) |
| Thu - 18/06 | Hoàn thiện kiến trúc đã review để lập kế hoạch triển khai. | 18/06/2026 | 18/06/2026 | Sơ đồ hoàn thiện / [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) |

## Kết quả đạt được

- Xác định rõ vai trò Manager/Member, trạng thái công việc và yêu cầu tự động nhắc deadline của Team Task Management.
- Hoàn thành sơ đồ kiến trúc gồm CloudFront, S3, API Gateway, VPC Link, Internal ALB, EC2, RDS, Lambda, EventBridge và SES.
- Tiếp nhận góp ý từ các anh chị trong cộng đồng AWS FCAJ và điều chỉnh subnet, Security Group, private API cùng hai luồng request chính.
