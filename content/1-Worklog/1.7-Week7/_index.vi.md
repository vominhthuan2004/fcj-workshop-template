---
title: "Nhật ký Tuần 7"
date: 2026-07-20
weight: 7
pre: " <b> 1.7. </b> "
chapter: false
---


**Thời gian:** 29/05/2026 - 04/06/2026

## Mục tiêu Tuần 7

- Hiểu EC2 storage và metadata.
- Chuẩn bị quản trị an toàn EC2 backend private.

## Công việc đã thực hiện

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
|---|---|---|---|---|
| Fri - 29/05 | So sánh EC2 instance family, hình thức mua và lựa chọn t3.micro cho demo. | 29/05/2026 | 29/05/2026 | [Các loại Amazon EC2 instance](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html) |
| Sat - 30/05 | Ôn EC2 Security Group, IAM Role và kiểm soát truy cập mạng. | 30/05/2026 | 30/05/2026 | [IAM Role cho Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) |
| Sun - 31/05 | Tìm hiểu instance store, instance metadata và ảnh hưởng bảo mật. | 31/05/2026 | 31/05/2026 | [EC2 instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) |
| Mon - 01/06 | Gắn EBS volume tạm vào instance lab và quan sát vòng đời. | 01/06/2026 | 01/06/2026 | Bài lab / [Gắn EBS volume](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-attaching-volume.html) |
| Tue - 02/06 | Tìm hiểu loại EBS volume, cách gắn, snapshot và vòng đời. | 02/06/2026 | 02/06/2026 | [Amazon EBS volume](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volumes.html) / [EBS snapshot](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-snapshots.html) |
| Wed - 03/06 | So sánh Session Manager với SSH khi quản trị instance private. | 03/06/2026 | 03/06/2026 | [AWS Systems Manager Session Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html) |
| Thu - 04/06 | Tạo IAM Role cho EC2 Session Manager và rà soát cách truy cập instance private. | 04/06/2026 | 04/06/2026 | Ảnh 16-18 / [Quyền instance cho Systems Manager](https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-instance-permissions.html) |

## Kết quả đạt được

- Lựa chọn EC2 `t3.micro` phù hợp cho môi trường demo và hiểu ảnh hưởng của instance metadata đối với bảo mật.
- Thực hành gắn EBS volume, quan sát vòng đời và tìm hiểu snapshot cùng các loại volume chính.
- Tạo IAM Role cho EC2 và xác định Session Manager là phương án quản trị instance private an toàn hơn việc mở SSH công khai.
