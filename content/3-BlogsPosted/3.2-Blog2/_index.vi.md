---
title: "Blog 2"
date: 2024-04-17
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---


# KIẾN TRÚC EVENT-DRIVEN TRÊN AWS VỚI AMAZON EVENTBRIDGE, AMAZON SNS VÀ AMAZON SQS 

Event-Driven Architecture là một mô hình thiết kế hệ thống trong đó các thành phần giao tiếp với nhau thông qua Event thay vì gọi trực tiếp bằng REST API. Trên AWS, kiến trúc này có thể được triển khai hiệu quả với các dịch vụ như Amazon API Gateway, AWS Lambda, Amazon EventBridge, Amazon SNS và Amazon SQS, giúp giảm sự phụ thuộc giữa các service, tăng khả năng mở rộng và nâng cao tính ổn định của hệ thống khi xử lý bất đồng bộ. 

Các điểm chính cần nắm:

* Producer chỉ phát sinh Event: Sau khi xử lý nghiệp vụ, AWS Lambda sẽ gửi Event đến Amazon EventBridge thay vì gọi trực tiếp sang service khác, giúp giảm coupling giữa các thành phần trong hệ thống.
* Amazon EventBridge chịu trách nhiệm định tuyến Event: EventBridge tiếp nhận, lọc và chuyển tiếp Event theo các Rule đã cấu hình, đồng thời hỗ trợ truyền sự kiện giữa nhiều AWS Account.
* Amazon SNS thực hiện cơ chế Publish/Subscribe: Một Event có thể được phát tán đồng thời đến nhiều Subscriber như AWS Lambda, Amazon SQS, HTTP Endpoint hoặc Email mà không cần thay đổi Producer.
* Amazon SQS đảm bảo xử lý bất đồng bộ: SQS hoạt động như một hàng đợi (Queue), giúp lưu trữ Event tạm thời, hấp thụ lưu lượng truy cập tăng đột biến, hỗ trợ Retry và Dead Letter Queue (DLQ) nhằm hạn chế mất dữ liệu khi Consumer gặp lỗi.
* Kiến trúc dễ dàng mở rộng: Khi cần bổ sung các chức năng như gửi Email, Audit Log, Data Lake hoặc Machine Learning, chỉ cần thêm Consumer mới đăng ký nhận Event mà không ảnh hưởng đến các thành phần hiện có.
* Tối ưu chi phí và khả năng mở rộng: Việc sử dụng các dịch vụ Serverless như API Gateway, Lambda, EventBridge, SNS và SQS giúp hệ thống tự động mở rộng theo lưu lượng truy cập và chỉ phát sinh chi phí khi có yêu cầu xử lý.

Kiến trúc này đặc biệt phù hợp với các hệ thống Microservices, xử lý bất đồng bộ, tích hợp đa hệ thống hoặc môi trường đa tài khoản AWS, nơi yêu cầu khả năng mở rộng cao, giảm sự phụ thuộc giữa các dịch vụ và tăng khả năng chịu lỗi của toàn bộ hệ thống.

<p align="center">
  <img src="/images/3-BlogsPosted/Blog2/img2.png" width="700">
</p>

<p align="center">
  <em>Hình 1. Event-driven architecture using Amazon EventBridge, Amazon SNS, and Amazon SQS.</em>
</p>

## Link bài viết
[🔗 Xem bài viết trên Facebook](https://web.facebook.com/share/p/18tYHRgbgh/?_rdc=1&_rdr)
