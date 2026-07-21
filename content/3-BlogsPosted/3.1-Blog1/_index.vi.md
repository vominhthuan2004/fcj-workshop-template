---
title: "Từ Multi-AZ sang Single-AZ – Tối ưu chi phí trên AWS"
date: 2026-07-20
weight: 1
pre: " <b> 3.1. </b> "
chapter: false
---

## Thông tin bài viết

| Thuộc tính | Nội dung |
| --- | --- |
| Chủ đề | Tối ưu chi phí khi triển khai ứng dụng trên AWS |
| Dự án liên quan | Team Task Management |
| Nền tảng đăng | AWS Study Group trên Facebook |
| Trạng thái | Đã đăng |
| Liên kết bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/share/p/1DFK3ftqfj/) |

## Bối cảnh

Bài viết chia sẻ kinh nghiệm thực tế của tôi trong quá trình thiết kế và triển khai dự án **Team Task Management** trên AWS.

Ban đầu, tôi cho rằng kiến trúc sử dụng nhiều dịch vụ và gần giống production sẽ hoàn chỉnh hơn. Vì vậy, hệ thống được thiết kế theo hướng Multi-AZ, gồm nhiều public subnet và private subnet, Application Load Balancer, API Gateway, RDS Multi-AZ, NAT Gateway, Lambda, EventBridge và SES.

Sau khi sử dụng AWS Pricing Calculator và theo dõi Billing Dashboard, tôi nhận ra một số lựa chọn chưa phù hợp với quy mô của dự án. Đây là ứng dụng học tập và demo, có ít người dùng, ít dữ liệu và chưa yêu cầu mức sẵn sàng như production. Vì vậy, tôi điều chỉnh kiến trúc theo nhu cầu thực tế.

## Chuyển từ Multi-AZ sang Single-AZ

Multi-AZ phù hợp với hệ thống production cần tiếp tục hoạt động khi một Availability Zone gặp sự cố. Với môi trường học tập, Single-AZ giúp giảm chi phí trong khi các chức năng chính vẫn hoạt động bình thường.

Quyết định này giúp tôi hiểu rằng kiến trúc tốt không phải là kiến trúc sử dụng nhiều tài nguyên nhất, mà là kiến trúc phù hợp với yêu cầu và ngân sách thực tế. Khi chuyển sang production, hệ thống vẫn cần được đánh giá lại về tính sẵn sàng, sao lưu và khả năng khôi phục.

## Chuyển frontend từ EC2 sang S3 và CloudFront

Frontend chỉ gồm HTML, CSS và JavaScript nên không cần một máy chủ EC2 hoạt động liên tục. Tôi chuyển frontend sang Amazon S3 và phân phối nội dung bằng Amazon CloudFront. Cách triển khai này:

- Không cần quản lý thêm web server.
- Giảm tải cho EC2 backend.
- Phân phối nội dung qua HTTPS.
- Tận dụng cơ chế cache của CloudFront.
- Tách frontend và backend thành hai thành phần độc lập.

## Sử dụng Lambda cho tác vụ định kỳ

Thay vì duy trì cron job trên EC2 để kiểm tra deadline, tôi sử dụng EventBridge Scheduler và AWS Lambda:

~~~text
EventBridge Scheduler
        ↓
AWS Lambda
        ↓
Deadline-check API
        ↓
Backend truy vấn Amazon RDS
        ↓
Amazon SES gửi email
~~~

Lambda chỉ chạy khi đến lịch và tự kết thúc sau khi hoàn thành. Mô hình này phù hợp với tác vụ chỉ thực hiện một hoặc vài lần mỗi ngày.

## Theo dõi chi phí NAT Gateway

NAT Gateway là tài nguyên cần đặc biệt chú ý vì phát sinh phí theo thời gian tồn tại và lượng dữ liệu xử lý. Sau trải nghiệm này, tôi hình thành các thói quen:

- Chỉ tạo NAT Gateway khi cần thiết.
- Xóa NAT Gateway sau khi hoàn thành buổi triển khai nếu không còn sử dụng.
- Release Elastic IP không còn được gắn với tài nguyên.
- Kiểm tra Billing Dashboard thường xuyên.
- Rà soát các tài nguyên còn hoạt động sau mỗi buổi thực hành.

## Bài học rút ra

- Không sử dụng Multi-AZ cho môi trường demo khi chưa có yêu cầu sẵn sàng cao.
- Dùng S3 và CloudFront cho frontend tĩnh.
- Dùng Lambda cho tác vụ định kỳ thay vì duy trì tiến trình riêng.
- Theo dõi kỹ các tài nguyên tính phí theo giờ.
- Phân biệt rõ kiến trúc học tập với kiến trúc production.

> Kiến trúc tốt không phải là kiến trúc có nhiều dịch vụ nhất, mà là kiến trúc đáp ứng đúng nhu cầu với chi phí hợp lý.

## Tài liệu tham khảo

- [AWS Pricing Calculator](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/what-is-pricing-calculator.html)
- [Tạo dự toán bằng AWS Pricing Calculator](https://docs.aws.amazon.com/pricing-calculator/latest/userguide/generate-estimate.html)
