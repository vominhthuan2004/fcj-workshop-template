---
title: "Tìm hiểu Apache Celeborn trên Amazon EMR"
date: 2026-07-20
weight: 2
pre: " <b> 3.2. </b> "
chapter: false
---

## Thông tin bài viết

| Thuộc tính | Nội dung |
| --- | --- |
| Chủ đề | Big Data, Apache Spark, Amazon EMR và tối ưu chi phí |
| Hình thức | Nghiên cứu và tổng hợp từ AWS Big Data Blog |
| Nền tảng đăng | AWS Study Group trên Facebook |
| Trạng thái | Đã đăng |
| Liên kết bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/share/p/18VT4KGFLT/) |
| Nguồn nghiên cứu | [High-performance Remote Shuffle Service on Amazon EMR with Apache Celeborn](https://aws.amazon.com/blogs/big-data/high-performance-remote-shuffle-service-on-amazon-emr-with-apache-celeborn/) |

## Bài toán shuffle trong Apache Spark

Bài viết tập trung vào quá trình **shuffle dữ liệu** trong các workload Apache Spark quy mô lớn. Shuffle xảy ra khi dữ liệu phải được phân phối lại giữa các executor để thực hiện các thao tác như:

- groupBy
- join
- reduceByKey
- Sắp xếp dữ liệu
- Tổng hợp dữ liệu theo khóa

Quá trình này tiêu tốn băng thông mạng, bộ nhớ và dung lượng lưu trữ, đồng thời ảnh hưởng trực tiếp đến thời gian hoàn thành Spark job.

## Hạn chế của cơ chế shuffle truyền thống

### Mất dữ liệu khi Spot Instance bị thu hồi

EC2 Spot Instances giúp giảm chi phí compute, nhưng có thể bị thu hồi. Nếu Spark executor lưu shuffle data trên local disk, dữ liệu trên node đó có thể bị mất khi instance dừng. Spark phải tính lại các stage trước để tái tạo dữ liệu, làm tăng thời gian xử lý và giảm hiệu quả tiết kiệm.

### Cấp dư dung lượng lưu trữ

Với External Shuffle Service truyền thống, mỗi worker node phải có đủ dung lượng để lưu shuffle data. Không phải tất cả các node đều dùng hết phần lưu trữ được cấp, khiến hệ thống trả chi phí cho tài nguyên chưa được sử dụng hiệu quả.

### Khó scale-in cụm Spark

Một node có thể đã hoàn thành công việc tính toán nhưng vẫn phải hoạt động vì còn giữ shuffle data cho task khác. Cụm vì vậy khó thu nhỏ và tiếp tục phát sinh chi phí dù một số node gần như không còn xử lý.

## Apache Celeborn

Apache Celeborn là một **Remote Shuffle Service** mã nguồn mở. Thay vì lưu shuffle data trực tiếp trên Spark executor, dữ liệu được gửi đến một cụm Celeborn riêng biệt.

Các thành phần chính:

- **Leader/Primary:** quản lý metadata và trạng thái.
- **Worker:** lưu trữ và phục vụ shuffle data.
- **Client:** tích hợp với Spark và gửi dữ liệu đến Celeborn.

Luồng kiến trúc tổng quát:

~~~text
Amazon EMR on EC2 / Amazon EMR on EKS
                    ↓
          Internal Network Load Balancer
                    ↓
         Apache Celeborn on Amazon EKS
             ├── Leader/Primary nodes
             └── Worker nodes
~~~

## Cơ chế push-based shuffle

Spark executor chủ động đẩy shuffle data đến Celeborn Worker thay vì để reducer kết nối đến nhiều executor để lấy dữ liệu. Cách tiếp cận này có thể:

- Giảm số lượng kết nối mạng trong giai đoạn đọc.
- Hạn chế phụ thuộc vào local disk của executor.
- Tăng khả năng sử dụng Spot Instance.
- Cho phép Spark và Celeborn scale độc lập.
- Tăng khả năng chịu lỗi cho Spark job.

## Khả năng chịu lỗi và cấu hình

Các Leader node có thể dùng Raft để duy trì trạng thái điều phối. Worker có thể lưu dữ liệu trên local NVMe để đạt hiệu năng cao và sao chép shuffle partition sang nhiều worker để giảm nguy cơ mất dữ liệu.

Một số cấu hình Spark quan trọng:

~~~properties
spark.shuffle.service.enabled=false
spark.shuffle.manager=org.apache.spark.shuffle.celeborn.SparkShuffleManager
spark.shuffle.sort.io.plugin.class=org.apache.spark.shuffle.celeborn.CelebornShuffleDataIO
spark.celeborn.master.endpoints=<NLB_DNS_NAME>:9097
spark.celeborn.client.push.replicate.enabled=true
spark.sql.adaptive.localShuffleReader.enabled=false
~~~

Các giá trị này phải được đánh giá theo phiên bản Spark, Celeborn và môi trường EMR thực tế trước khi áp dụng.

## Monitoring và Observability

Giải pháp trong bài nguồn sử dụng AWS Distro for OpenTelemetry để thu thập Prometheus metrics từ Celeborn và hiển thị bằng Grafana. Những chỉ số cần theo dõi gồm:

- Active shuffle và push failure.
- Trạng thái worker và thay đổi Leader.
- Dung lượng ổ đĩa và mức sử dụng bộ nhớ.
- JVM heap và Garbage Collection.

## Liên hệ với dự án Team Task Management

Dự án cá nhân không sử dụng Amazon EMR hoặc Apache Spark, nhưng nguyên tắc tách các thành phần có vòng đời khác nhau vẫn có thể áp dụng:

- Frontend được tách khỏi EC2 và đưa lên S3.
- Tác vụ kiểm tra deadline được tách khỏi backend bằng Lambda.
- Dữ liệu được tách khỏi compute bằng Amazon RDS.
- Monitoring được xem là một thành phần của kiến trúc.

## Bài học rút ra

- Tối ưu Big Data không chỉ là chọn instance rẻ hơn.
- Spot Instance chỉ hiệu quả khi hệ thống chịu được interruption.
- Tách compute và shuffle storage giúp hệ thống scale linh hoạt hơn.
- Tăng độ tin cậy thường đi kèm chi phí và độ phức tạp.
- Monitoring cần được thiết kế ngay từ đầu.
- Kiến trúc phải được đánh giá dựa trên workload thực tế.

{{% notice info %}}
Đây là bài nghiên cứu và phân tích dựa trên AWS Big Data Blog. Tôi chưa trực tiếp triển khai một cụm Apache Celeborn hoàn chỉnh trong dự án cá nhân, vì vậy không khẳng định kết quả hiệu năng chưa được kiểm chứng.
{{% /notice %}}
