---
title: "Blog 1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# TỐI ƯU HÓA BẢO MẬT VỚI SESSION POLICIES TRONG AMAZON EKS POD IDENTITY

Amazon EKS Pod Identity cung cấp cơ chế cho phép Pod truy cập an toàn đến các dịch vụ AWS mà không cần quản lý khóa truy cập tĩnh. Gần đây, Amazon EKS đã bổ sung Session Policies, cho phép áp dụng các chính sách IAM nội tuyến trong suốt quá trình Pod đảm nhận IAM Role. Tính năng này giúp giới hạn quyền truy cập của từng Pod một cách linh hoạt mà vẫn tận dụng được IAM Role hiện có, từ đó đơn giản hóa việc quản lý quyền trong các cụm Kubernetes quy mô lớn.

Các điểm chính cần nắm:

* Session Policies được áp dụng khi Pod đảm nhận IAM Role: Chính sách được sử dụng trong quá trình AssumeRole để giới hạn quyền truy cập của Pod đối với các tài nguyên AWS mà không cần tạo thêm IAM Role mới.
* Quyền truy cập hiệu lực là giao của IAM Role và Session Policy: Pod chỉ được phép thực hiện những hành động được cho phép đồng thời bởi cả IAM Role và Session Policy. Session Policy không thể cấp thêm quyền mà chỉ có thể thu hẹp phạm vi quyền hiện có.
* Đơn giản hóa việc quản lý IAM: Nhiều Pod có thể cùng sử dụng một IAM Role, trong khi Session Policies được sử dụng để áp dụng các mức quyền khác nhau cho từng workload, giúp giảm số lượng IAM Role cần quản lý.
* Tăng cường bảo mật theo nguyên tắc Least Privilege: Quyền của từng Pod được giới hạn đúng với yêu cầu nghiệp vụ, giảm thiểu rủi ro khi Pod hoặc ứng dụng bị khai thác.
* Triển khai linh hoạt: Session Policies được cấu hình thông qua Pod Identity Association và có thể quản lý bằng AWS Management Console, AWS CLI hoặc AWS SDK, giúp tích hợp thuận lợi vào quy trình triển khai và tự động hóa.
* Phù hợp với môi trường Amazon EKS quy mô lớn: Việc giảm số lượng IAM Role giúp đơn giản hóa quản trị, hạn chế vượt giới hạn IAM và tăng khả năng mở rộng khi số lượng Pod hoặc ứng dụng ngày càng nhiều.

Session Policies đặc biệt hữu ích trong các hệ thống Microservices, nơi nhiều Pod sử dụng chung một IAM Role nhưng yêu cầu phạm vi truy cập tài nguyên AWS khác nhau. Ví dụ, một Pod chỉ cần quyền đọc dữ liệu từ một Amazon S3 Bucket cụ thể, trong khi Pod khác cần truy cập thêm Amazon DynamoDB hoặc Amazon SQS. Bằng cách kết hợp Pod Identity với Session Policies, hệ thống vừa duy trì được khả năng quản lý tập trung vừa đảm bảo mỗi Pod chỉ có đúng những quyền cần thiết để thực hiện chức năng của mình.

## Hình ảnh minh họa

<p align="center">
  <img src="/images/3-BlogsPosted/Blog1/img1.png" width="700">
</p>

<p align="center">
  <em>Hình 1. Kiến trúc Amazon EKS Pod Identity và Session Policies.</em>
</p>

## Link bài viết

[🔗 Xem bài viết trên Facebook](https://www.facebook.com/share/p/1Bf61McbW7/)
