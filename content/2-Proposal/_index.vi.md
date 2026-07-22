---
title: "Đề xuất dự án"
date: 2026-07-20
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Đề xuất này xác định định hướng, kiến trúc, giai đoạn triển khai, rủi ro, hạng mục chi phí và kết quả mong đợi của Team Task Management. Các lệnh triển khai chi tiết được đặt trong Workshop để phần này tập trung vào quyết định và kế hoạch.

- **Dự án:** Hệ thống quản lý công việc nhóm và tự động nhắc hạn trên AWS
- **Mã nguồn:** [GitHub – vominhthuan2004/AWSPROJECT](https://github.com/vominhthuan2004/AWSPROJECT)
- **Người dùng mục tiêu:** nhóm nhỏ với vai trò Manager và Member
- **Region:** ap-southeast-1 (Singapore)
- **Cấu trúc Proposal:** tóm tắt, vấn đề, kiến trúc, triển khai, timeline, ngân sách, rủi ro và kết quả

## Bản đồ nội dung

| Phần | Trọng tâm |
|---|---|
| [1. Tóm tắt chính](#1-tóm-tắt-chính) | Giá trị dự án và giải pháp tổng quát |
| [2. Vấn đề và giải pháp](#2-vấn-đề-và-giải-pháp) | Hạn chế hiện tại và cách xử lý đề xuất |
| [3. Kiến trúc giải pháp](#3-kiến-trúc-giải-pháp) | Luồng người dùng, backend private, dữ liệu và tự động hóa |
| [4. Kế hoạch kỹ thuật](#4-kế-hoạch-kỹ-thuật) | Các giai đoạn và phương pháp triển khai |
| [5. Timeline và cột mốc](#5-timeline-và-cột-mốc) | Đồng bộ với 12 mục Worklog |
| [6. Ước tính chi phí](#6-ước-tính-chi-phí) | Lượng AWS Credit dự kiến dùng trong một tháng |
| [7. Đánh giá rủi ro](#7-đánh-giá-rủi-ro) | Rủi ro kỹ thuật, bảo mật, vận hành và chi phí |
| [8. Kết quả mong đợi](#8-kết-quả-mong-đợi) | Tiêu chí thành công có thể kiểm chứng |

## 1. Tóm tắt chính

Ứng dụng giúp nhóm nhỏ tạo, giao và theo dõi nhiệm vụ tập trung. Frontend tĩnh được phân phối bằng Amazon S3 và CloudFront. Backend Node.js/Express chạy trên EC2 trong private subnet và lưu dữ liệu tại Amazon RDS for PostgreSQL. EventBridge Scheduler kích hoạt Lambda gọi API kiểm tra deadline; Amazon SES gửi email nhắc hạn. Thiết kế ưu tiên bảo mật cơ bản, khả năng mở rộng, vận hành và chi phí hợp lý cho môi trường học tập.

## 2. Vấn đề và giải pháp

Giao việc qua tin nhắn hoặc bảng tính làm dữ liệu phân tán, khó theo dõi tiến độ và dễ quên hạn. Nền tảng đề xuất tập trung vai trò Manager/Member, trạng thái nhiệm vụ, deadline, email tự động và log vận hành.

## 3. Kiến trúc giải pháp

![Kiến trúc Team Task Management trên AWS](/images/2-Proposal/team-task-management-architecture.png)

{{% notice note %}}
Trong sơ đồ, bước 5 ghi “Load balanced Backend Request” được hiểu là **private backend request được định tuyến qua Internal ALB listener**. Target Group hiện chỉ có một EC2 instance nên ALB không được dùng để phân phối traffic cho nhiều backend.
{{% /notice %}}

Kiến trúc gồm hai luồng chính:

1. **Luồng request người dùng:** Manager hoặc Member gửi HTTPS request đến CloudFront. CloudFront trả file frontend tĩnh từ S3 và chuyển API request đến API Gateway. VPC Link dùng listener của Internal ALB làm điểm vào private bên trong VPC. ALB định tuyến request đến EC2 Target Group, hiện chỉ có một backend instance. Backend thực hiện tạo, đọc, cập nhật và xóa dữ liệu task trong RDS for PostgreSQL.
2. **Luồng nhắc deadline tự động:** EventBridge kích hoạt Lambda theo lịch. Lambda gọi deadline-check API được bảo vệ thông qua API Gateway. Backend truy vấn RDS để tìm task sắp đến hạn và gọi Amazon SES qua kết nối outbound do NAT Gateway cung cấp. SES gửi email nhắc đến người dùng.

AWS WAF Web ACL được gắn với CloudFront distribution để kiểm tra và lọc request trước khi request đến các origin của ứng dụng.

| Lớp | Dịch vụ AWS | Trách nhiệm |
|---|---|---|
| Biên | AWS WAF, CloudFront | HTTPS, cache và lọc request |
| Frontend | Amazon S3 | Lưu nội dung web tĩnh |
| API | API Gateway HTTP API, VPC Link | Điểm vào công khai và tích hợp private |
| Ứng dụng | Internal ALB, EC2 | Điểm vào private trong VPC, định tuyến request và backend Node.js/Express |
| Dữ liệu | RDS for PostgreSQL | User, task, phân công, trạng thái và deadline |
| Tự động hóa | EventBridge Scheduler, Lambda, SES | Kiểm tra định kỳ và gửi email |
| Vận hành | IAM, Security Groups, CloudWatch | Phân quyền, cô lập mạng, log, metric và alarm |

## 4. Kế hoạch kỹ thuật

1. Phân tích yêu cầu và rà soát kiến trúc.
2. Phát triển, kiểm thử frontend, backend và database cục bộ.
3. Triển khai lần lượt mạng, bảo mật, dữ liệu, compute, API, frontend và tự động hóa.
4. Kiểm thử end-to-end, xử lý lỗi, viết tài liệu và tối ưu chi phí.

## 5. Timeline và cột mốc

Kế hoạch gồm 12 kỳ Worklog liên tiếp, mỗi kỳ 7 ngày, từ 17/04 đến 09/07/2026: nền tảng AWS và lab (Tuần 1-4), networking và compute (Tuần 5-7), chi phí và review kiến trúc (Tuần 8-9), lập kế hoạch nhóm (Tuần 10), phát triển frontend (Tuần 11), hoàn thiện backend, triển khai AWS, tự động hóa, bảo mật và kiểm thử (Tuần 12). Giai đoạn 10/07-31/07 dành cho hoàn thiện báo cáo, xin xác nhận và thực hiện thủ tục nộp.

## 6. Ước tính chi phí

Ước tính dưới đây sử dụng giá On-Demand tại region `ap-southeast-1` (Singapore), được kiểm tra ngày 20/07/2026. Khi lập ngân sách, 1 AWS Credit được xem xấp xỉ 1 USD chi phí dịch vụ AWS đủ điều kiện.

**Giả định tải trong một tháng:** hệ thống hoạt động 730 giờ; một EC2 Linux `t3.micro` với EBS gp3 8 GB; một RDS PostgreSQL Single-AZ `db.t3.micro` với gp3 20 GB; một NAT Gateway dùng một địa chỉ IPv4 công khai và xử lý 1 GB dữ liệu; một Internal ALB có LCU thấp; S3 lưu 1 GB; CloudFront phân phối 5 GB; 10.000 HTTP API request; 1.000 email nhắc hạn; một WAF Web ACL gồm ba AWS managed rule group và một rate-based rule.

| Dịch vụ AWS | Cách tính trong một tháng | Credit/tháng dự kiến |
|---|---:|---:|
| EC2 | `t3.micro`: 730 giờ × $0,0132 | 9,64 |
| EBS | gp3 8 GB × $0,096/GB-tháng | 0,77 |
| RDS for PostgreSQL | `db.t3.micro`: 730 giờ × $0,028 + gp3 20 GB × $0,138 | 23,20 |
| NAT Gateway | 730 giờ × $0,059 + 1 GB × $0,059 | 43,13 |
| Public IPv4 | Một Elastic IP của NAT Gateway: 730 giờ × $0,005 | 3,65 |
| Internal ALB | 730 giờ × $0,0252 + LCU thấp | 18,40 |
| S3 | Lưu 1 GB và số request thấp | 0,03 |
| CloudFront | Khoảng 5 GB truyền tải và 10.000 request | 0,61 |
| API Gateway HTTP API | 10.000 request × $1,00/triệu request | 0,01 |
| Lambda và EventBridge Scheduler | Khoảng 30 lần chạy theo lịch, nằm trong hạn mức miễn phí tháng | 0,00 |
| Amazon SES | 1.000 email gửi đi × $0,10/1.000 email | 0,10 |
| AWS WAF | Một Web ACL + bốn rule/rule group + 10.000 request | 9,01 |
| CloudWatch | Dưới hạn mức Logs miễn phí 5 GB/tháng | 0,00 |
| **Tổng dự kiến** |  | **108,55 credit/tháng** |
| **Ngân sách đề xuất có khoảng 10% dự phòng** |  | **120 credit/tháng** |

Vì vậy, dự án nên chuẩn bị **khoảng 120 AWS Credit để chạy liên tục trong một tháng**. Mức sử dụng dự kiến gần **109 credit**, 11 credit còn lại dùng để dự phòng khi traffic hoặc log tăng nhẹ và cho sai số làm tròn. NAT Gateway, RDS và Internal ALB chiếm khoảng 78% tổng dự toán. Mặc dù ALB hiện là điểm vào private VPC cho API Gateway và chỉ có một EC2 target, ALB vẫn bị tính phí theo từng giờ tồn tại.

Dự toán chưa gồm thuế, tên miền, Route 53 Hosted Zone, gói hỗ trợ trả phí, RDS snapshot vượt hạn mức backup đi kèm và các khoản truyền dữ liệu Internet hoặc cross-AZ phát sinh. AWS Credit khuyến mãi và quyền lợi Free Tier riêng của tài khoản chưa được trừ; AWS sẽ tự áp dụng credit đủ điều kiện vào hóa đơn thực tế. Để giảm chi phí workshop, cần xóa NAT Gateway và Internal ALB khi không sử dụng, dừng EC2, đồng thời dừng hoặc xóa RDS sau khi đã lưu đủ minh chứng.

Nguồn giá tham khảo: [Amazon EC2](https://aws.amazon.com/ec2/pricing/on-demand/), [Amazon RDS](https://aws.amazon.com/rds/postgresql/pricing/), [NAT Gateway và public IPv4](https://aws.amazon.com/vpc/pricing/), [Elastic Load Balancing](https://aws.amazon.com/elasticloadbalancing/pricing/), [API Gateway](https://aws.amazon.com/api-gateway/pricing/), [AWS WAF](https://aws.amazon.com/waf/pricing/) và [Amazon SES](https://aws.amazon.com/ses/pricing/).

## 7. Đánh giá rủi ro

| Rủi ro | Biện pháp |
|---|---|
| Sai route hoặc Security Group | Test từng chặng mạng và áp dụng least privilege |
| API Gateway 503 hoặc target unhealthy | Kiểm tra route, listener, target health, VPC Link và log |
| IAM thiếu quyền | Dùng IAM Role giới hạn phạm vi và đọc lỗi CloudTrail/CloudWatch |
| SES Sandbox hoặc email vào spam | Verify identity, test recipient và kiểm tra suppression/spam |
| Lộ secret | Không commit `.env`, dùng role và dự kiến chuyển sang Secrets Manager |
| Chi phí tăng | Tạo AWS Budgets và xóa tài nguyên tính phí theo giờ sau workshop |

## 8. Kết quả mong đợi

- Frontend HTTPS và API hoạt động qua đúng điểm vào.
- EC2/RDS là private và các luồng quản lý task chạy đúng.
- Lambda, Scheduler và SES gửi được nhắc hạn, có log CloudWatch.
- Workshop đủ để người khác triển khai, test, giám sát và cleanup.
