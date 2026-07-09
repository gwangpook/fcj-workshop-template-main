---
title: "Bản đề xuất"
date: 2026-04-14
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# FLASHLEARN PLATFORM

## Nền tảng học tiếng Anh FlashLearn trên AWS Cloud

### 1. Tóm tắt điều hành

FlashLearn là nền tảng học tiếng Anh trực tuyến được xây dựng nhằm hỗ trợ người học ghi nhớ từ vựng thông qua Flashcard, Quiz và các hoạt động cộng đồng. Ngoài các chức năng học tập cơ bản, hệ thống còn tích hợp khả năng chuyển văn bản thành giọng nói để hỗ trợ luyện phát âm tiếng Anh, giúp nâng cao trải nghiệm học tập.

Để đáp ứng nhu cầu truy cập ổn định, bảo vệ dữ liệu người dùng và dễ dàng mở rộng trong tương lai, FlashLearn được triển khai trên nền tảng Amazon Web Services (AWS). Kiến trúc sử dụng các dịch vụ như Amazon EC2, Amazon RDS PostgreSQL Multi-AZ, Amazon S3, CloudFront, Application Load Balancer cùng các dịch vụ Serverless nhằm đảm bảo hiệu năng, tính sẵn sàng cao và khả năng vận hành theo tiêu chuẩn doanh nghiệp.

---

## 2. Tuyên bố vấn đề

### Vấn đề hiện tại

Các nền tảng học trực tuyến thường phải lưu trữ lượng lớn hình ảnh, nội dung học tập và dữ liệu người dùng. Nếu toàn bộ hệ thống được triển khai trên một máy chủ truyền thống, hiệu năng sẽ giảm khi số lượng người dùng tăng, đồng thời tiềm ẩn nhiều rủi ro về bảo mật và tính sẵn sàng của hệ thống.

Ngoài ra, việc lưu trữ trực tiếp các tài nguyên tĩnh trên máy chủ Backend cũng làm tăng tải xử lý, gây ảnh hưởng đến tốc độ truy cập của người học.

### Giải pháp

FlashLearn được xây dựng trên kiến trúc AWS Cloud nhiều lớp nhằm tách biệt rõ giữa phần xử lý nghiệp vụ, lưu trữ dữ liệu và phân phối nội dung.

Các hình ảnh Flashcard và tài nguyên tĩnh được lưu trữ trên Amazon S3 và phân phối thông qua Amazon CloudFront để giảm độ trễ khi truy cập. Các yêu cầu từ người dùng được tiếp nhận bởi Application Load Balancer trước khi chuyển tới các máy chủ Amazon EC2 đặt trong Private Subnets nhằm tăng cường bảo mật. Dữ liệu học tập được lưu trữ trên Amazon RDS PostgreSQL theo mô hình Multi-AZ để đảm bảo tính sẵn sàng cao.

Bên cạnh đó, Amazon Polly được tích hợp để hỗ trợ chức năng chuyển văn bản thành giọng nói phục vụ luyện phát âm. AWS EventBridge kết hợp với AWS Lambda và Amazon SES giúp tự động hóa các tác vụ nền như gửi email nhắc nhở và thông báo đến người dùng.

### Lợi ích và giá trị mang lại

Đối với người học, FlashLearn mang đến trải nghiệm học tập nhanh, ổn định và thuận tiện hơn thông qua việc truy cập nội dung với tốc độ cao, hỗ trợ luyện phát âm và đảm bảo dữ liệu luôn được lưu trữ an toàn.

Đối với hệ thống, kiến trúc AWS giúp nâng cao khả năng chịu tải, tăng cường bảo mật, dễ dàng mở rộng khi số lượng người dùng tăng lên và giảm khối lượng quản trị hạ tầng nhờ tận dụng các dịch vụ được quản lý bởi AWS.

---

## 3. Kiến trúc giải pháp

FlashLearn được triển khai bên trong một Amazon VPC trải rộng trên hai Availability Zones nhằm đảm bảo tính sẵn sàng cao và khả năng chịu lỗi khi xảy ra sự cố tại một vùng.

<p align="center">
  <img src="/fcj-workshop-template/images/2-Proposal/img4.png" width="700">
</p>


### Các dịch vụ AWS sử dụng

- Amazon Route 53
- AWS WAF
- Amazon CloudFront
- Amazon S3
- Application Load Balancer
- Amazon EC2
- Amazon RDS PostgreSQL Multi-AZ
- NAT Gateway
- Amazon EventBridge
- AWS Lambda
- Amazon SES
- Amazon Polly

### Thiết kế thành phần

- **Edge Layer:** Amazon Route 53 kết hợp AWS WAF tiếp nhận và bảo vệ lưu lượng truy cập từ Internet.
- **Content Delivery:** Amazon CloudFront phân phối hình ảnh Flashcard và các nội dung tĩnh được lưu trên Amazon S3.
- **Application Layer:** Application Load Balancer điều phối các yêu cầu đến các máy chủ Amazon EC2.
- **Business Layer:** Amazon EC2 xử lý toàn bộ nghiệp vụ của FlashLearn và kết nối tới cơ sở dữ liệu.
- **Database Layer:** Amazon RDS PostgreSQL Multi-AZ lưu trữ dữ liệu học tập và thông tin người dùng.
- **Automation Layer:** EventBridge kích hoạt Lambda để gửi email thông qua Amazon SES.
- **AI Service:** Amazon Polly cung cấp chức năng Text-to-Speech phục vụ luyện phát âm.

---

## 4. Triển khai kỹ thuật

### Các giai đoạn triển khai

Dự án được triển khai theo năm giai đoạn:

1. Xây dựng hạ tầng mạng gồm VPC, Public Subnets, Private Subnets, Route 53 và AWS WAF.
2. Triển khai Application Load Balancer, NAT Gateway và các máy chủ Amazon EC2.
3. Thiết lập Amazon RDS PostgreSQL Multi-AZ, Amazon S3 và CloudFront.
4. Tích hợp các dịch vụ Serverless gồm EventBridge, Lambda, Amazon SES và Amazon Polly.
5. Kiểm thử toàn bộ hệ thống, tối ưu hiệu năng và hoàn thiện hạ tầng.

### Yêu cầu kỹ thuật

- Amazon EC2 ARM64.
- Amazon RDS PostgreSQL Multi-AZ.
- Amazon S3.
- Amazon CloudFront.
- Application Load Balancer.
- NAT Gateway.
- AWS Lambda.
- Amazon EventBridge.
- Amazon SES.
- Amazon Polly.
- AWS WAF.
- Amazon Route 53.

---

## 5. Lộ trình triển khai

- **Tuần 1:** Thiết lập VPC, Route 53, AWS WAF và Internet Gateway.
- **Tuần 2:** Triển khai EC2, NAT Gateway và Application Load Balancer.
- **Tuần 3:** Cấu hình Amazon RDS, Amazon S3 và CloudFront.
- **Tuần 4:** Tích hợp Amazon Polly, EventBridge, Lambda và Amazon SES.
- **Tuần 5:** Kiểm thử, tối ưu hiệu năng và hoàn thiện hệ thống.

---

## 6. Ước tính ngân sách

Có thể xem chi phí trên **AWS Pricing Calculator** hoặc tệp dự toán ngân sách.

### Chi phí hạ tầng

- NAT Gateway (2): ~65.70 USD/tháng.
- Application Load Balancer: ~16.42 USD/tháng.
- Amazon RDS PostgreSQL Multi-AZ: ~46.40 USD/tháng.
- Amazon EC2 (2 máy): ~12.26 USD/tháng.
- Amazon S3, CloudFront, Route53 và AWS WAF: ~10.00 USD/tháng.
- Amazon Polly, Lambda, EventBridge và Amazon SES: ~2.00 USD/tháng.

**Tổng chi phí tham khảo:** ~152.78 USD/tháng.

---

## 7. Đánh giá rủi ro

### Các rủi ro chính

- Chi phí NAT Gateway cao.
- Application Load Balancer không thể kết nối tới EC2 do cấu hình Security Group.
- CloudFront lưu cache không đúng đối với các API động.

### Giải pháp giảm thiểu

- Trong môi trường Development chỉ sử dụng một NAT Gateway để tối ưu chi phí.
- Kiểm tra chặt chẽ Security Group giữa ALB và EC2.
- Cấu hình CloudFront Behavior riêng cho API và nội dung tĩnh nhằm tránh cache sai.

### Kế hoạch dự phòng

- Điều chỉnh kiến trúc theo từng môi trường triển khai để tối ưu chi phí.
- Khôi phục hạ tầng nhanh chóng thông qua Infrastructure as Code khi xảy ra sự cố.

---

## 8. Kết quả kỳ vọng

### Giá trị đối với người dùng

- Học từ vựng hiệu quả hơn thông qua Flashcard và Quiz.
- Luyện phát âm bằng tính năng Text-to-Speech.
- Truy cập nội dung nhanh và ổn định.
- Hệ thống luôn sẵn sàng ngay cả khi số lượng người dùng tăng.

### Giá trị kỹ thuật

- Kiến trúc Cloud bảo mật nhiều lớp.
- Hệ thống có khả năng mở rộng theo nhu cầu sử dụng.
- Tự động hóa các tác vụ nền bằng các dịch vụ Serverless.
- Có thể tái sử dụng kiến trúc cho các dự án Cloud thực tế trong tương lai.
