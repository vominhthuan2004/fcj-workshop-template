---
title: "Nhật ký Tuần 8"
date: 2026-07-20
weight: 8
pre: " <b> 1.8. </b> "
chapter: false
---


**Thời gian:** 05/06/2026 - 11/06/2026

## Mục tiêu Tuần 8

- Xác định các nguồn chi phí chính trong kiến trúc dự án.
- Viết bản thảo blog kỹ thuật từ kết quả tìm hiểu.

## Công việc đã thực hiện

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| Fri - 05/06 | Rà soát chi phí theo giờ/lượt dùng của EC2, RDS, ALB, NAT Gateway, S3 và Lambda. | 05/06/2026 | 05/06/2026 | [Bảng giá AWS](https://aws.amazon.com/pricing/) / Billing |
| Sat - 06/06 | Kiểm tra Billing, Cost Explorer và xác định tài nguyên đang hoạt động. | 06/06/2026 | 06/06/2026 | [AWS Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html) |
| Sun - 07/06 | So sánh Single-AZ/Multi-AZ, chọn tài nguyên cỡ demo và ghi rõ đánh đổi production. | 07/06/2026 | 07/06/2026 | [Amazon RDS Multi-AZ](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |
| Mon - 08/06 | Tạo checklist tiết kiệm chi phí cho NAT Gateway, ALB, EC2, RDS và EBS. | 08/06/2026 | 08/06/2026 | [AWS Cost Optimization Pillar](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) |
| Tue - 09/06 | Viết bản thảo blog tối ưu chi phí và đối chiếu nội dung với kiến trúc dự án. | 09/06/2026 | 09/06/2026 | Bản thảo Blog / [AWS Cost Optimization Pillar](https://docs.aws.amazon.com/wellarchitected/latest/cost-optimization-pillar/welcome.html) |
| Wed - 10/06 | Chỉnh blog và bổ sung đánh đổi giữa Single-AZ với Multi-AZ. | 10/06/2026 | 10/06/2026 | Bản thảo Blog / [Amazon RDS Multi-AZ](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) |
| Thu - 11/06 | Liệt kê số liệu còn cần xác minh bằng Pricing Calculator. | 11/06/2026 | 11/06/2026 | [AWS Pricing Calculator](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/what-is-pricing-calculator.html) |

## Kết quả đạt được

- Xác định EC2, RDS, NAT Gateway, Internal ALB và lưu trữ là các nhóm chi phí cần theo dõi trong kiến trúc.
- Biết sử dụng Billing và Cost Explorer để rà soát tài nguyên, đồng thời xây dựng checklist tiết kiệm chi phí cho môi trường demo.
- Hoàn thành bản thảo blog tối ưu chi phí, phân tích đánh đổi Single-AZ/Multi-AZ và liệt kê số liệu cần kiểm tra bằng Pricing Calculator.
