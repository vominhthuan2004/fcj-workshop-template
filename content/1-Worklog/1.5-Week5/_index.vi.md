---
title: "Nhật ký Tuần 5"
date: 2026-07-20
weight: 5
pre: " <b> 1.5. </b> "
chapter: false
---


**Thời gian:** 15/05/2026 - 21/05/2026

## Mục tiêu Tuần 5

- Hiểu thành phần VPC và cách hoạch định CIDR.
- Phân tích luồng public/private resource kết nối Internet.

## Công việc đã thực hiện

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| Fri - 15/05 | Học Module 03 và xác định vai trò của VPC, subnet, route table, Security Group. | 15/05/2026 | 15/05/2026 | Module 03 / [Kiến thức cơ bản Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-subnet-basics.html) |
| Sat - 16/05 | Ôn cách áp dụng mô hình trách nhiệm chung vào cấu hình VPC. | 16/05/2026 | 16/05/2026 | [Mô hình trách nhiệm chung AWS](https://aws.amazon.com/compliance/shared-responsibility-model/) |
| Sun - 17/05 | Ôn cách tính IPv4 CIDR và lập kế hoạch public, app, DB subnet không trùng nhau. | 17/05/2026 | 17/05/2026 | [Địa chỉ IP cho VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html) |
| Mon - 18/05 | So sánh Security Group và Network ACL, xác định vị trí áp dụng. | 18/05/2026 | 18/05/2026 | [Bảo mật hạ tầng VPC](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html) |
| Tue - 19/05 | Phân tích luồng Internet Gateway và NAT Gateway cho public/private subnet. | 19/05/2026 | 19/05/2026 | [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) / [NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html) |
| Wed - 20/05 | Kiểm tra dải CIDR dự kiến và xác nhận subnet không trùng nhau. | 20/05/2026 | 20/05/2026 | Bảng tính CIDR / [Địa chỉ IP cho VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-ip-addressing.html) |
| Thu - 21/05 | Vẽ nháp luồng VPC và liệt kê ranh giới bảo mật cho EC2/RDS. | 21/05/2026 | 21/05/2026 | Bản vẽ kiến trúc / [Security Group](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-security-groups.html) |

## Kết quả đạt được

- Lập được kế hoạch CIDR không trùng nhau cho public subnet, private application subnet và private database subnet.
- Phân biệt vai trò của Security Group, Network ACL, Internet Gateway và NAT Gateway trong luồng public/private.
- Hoàn thành bản nháp luồng mạng và xác định ranh giới truy cập giữa ALB, EC2 backend và RDS.
