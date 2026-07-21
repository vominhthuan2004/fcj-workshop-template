---
title: "Nhật ký Tuần 6"
date: 2026-07-20
weight: 6
pre: " <b> 1.6. </b> "
chapter: false
---


**Thời gian:** 22/05/2026 - 28/05/2026

## Mục tiêu Tuần 6

- Xây dựng VPC và sơ đồ subnet của dự án.
- Cấu hình định tuyến public/private cho bước triển khai sau.

## Công việc đã thực hiện

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| Fri - 22/05 | Tạo VPC dự án và bật các thiết lập DNS cần thiết. | 22/05/2026 | 22/05/2026 | Tài liệu deploy / [Thuộc tính DNS của VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-dns.html) |
| Sat - 23/05 | Tạo public subnet, kiểm tra Availability Zone và dải CIDR. | 23/05/2026 | 23/05/2026 | Ảnh 01-02 / [Subnet trong VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) |
| Sun - 24/05 | Tạo private app subnet và private DB subnet cho EC2/RDS. | 24/05/2026 | 24/05/2026 | Ảnh 03-05 / [Subnet trong VPC](https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html) |
| Mon - 25/05 | Tạo Security Group ban đầu cho ALB, backend và database với rule theo nguồn. | 25/05/2026 | 25/05/2026 | Ảnh 14-15 / [Security Group rule](https://docs.aws.amazon.com/vpc/latest/userguide/security-group-rules.html) |
| Tue - 26/05 | Tạo/gắn Internet Gateway và cấu hình public route table. | 26/05/2026 | 26/05/2026 | Ảnh 06-10 / [Internet Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html) |
| Wed - 27/05 | Gắn tag thống nhất và rà tài nguyên VPC trước khi cấu hình private route. | 27/05/2026 | 27/05/2026 | Danh sách VPC / [Gắn tag tài nguyên AWS](https://docs.aws.amazon.com/tag-editor/latest/userguide/tagging.html) |
| Thu - 28/05 | Tạo NAT Gateway và private route table, sau đó kiểm tra outbound route. | 28/05/2026 | 28/05/2026 | Ảnh 11-13 / [NAT Gateway](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html) |

## Kết quả đạt được

- Xây dựng VPC dự án với DNS, public subnet, private application subnet và private database subnet theo kế hoạch CIDR.
- Cấu hình Internet Gateway cùng public route table và NAT Gateway cùng private route table cho đúng luồng mạng.
- Tạo Security Group ban đầu cho Internal ALB, EC2 và RDS, đồng thời áp dụng tag thống nhất cho tài nguyên VPC.
