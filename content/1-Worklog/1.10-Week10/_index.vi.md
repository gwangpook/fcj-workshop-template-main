---
title: "Worklog Tuần 10"
date: 2026-04-17
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

### Mục tiêu tuần 10:

* Tối ưu hóa hiệu năng hệ thống: Hoàn thiện cơ chế caching trên Amazon CloudFront và tối ưu hóa các truy vấn cơ sở dữ liệu trên RDS PostgreSQL để tăng tốc độ tải trang và phản hồi API.
* Tự động hóa xử lý nội dung: Triển khai hàm AWS Lambda để tự động xử lý (resize, nén) hình ảnh người dùng tải lên Amazon S3, giúp tối ưu dung lượng lưu trữ.
* Kiểm chứng bảo mật và luồng API: Hoàn tất kiểm thử các phương thức CRUD của API thông qua API Gateway, đảm bảo cấu hình bảo mật IAM và CORS được thiết lập chính xác.
* Thiết lập hệ thống giám sát: Cấu hình Amazon CloudWatch để theo dõi hiệu năng của EC2, đặt cảnh báo cho RDS và sử dụng AWS X-Ray để theo dõi, phân tích các cuộc gọi API nội bộ nhằm phát hiện sớm sự cố.
* Phát triển tính năng cốt lõi: Hoàn thiện luồng dữ liệu cho tính năng gợi ý học tập thông minh dựa trên lịch sử của người dùng.

### Các công việc cần triển khai trong tuần này:

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | Họp nhóm lên kế hoạch tối ưu hóa truy vấn trên RDS PostgreSQL và thiết lập cơ chế caching cho CloudFront để cải thiện tốc độ tải nội dung. | 21/06/2026 | 22/06/2026 | |
| 3 | Thực hành tối ưu hóa xử lý ảnh, triển khai Lambda để tự động resize và nén ảnh người dùng tải lên S3 nhằm giảm dung lượng lưu trữ. | 22/06/2026 | 23/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 4 | Kiểm thử API sử dụng Postman để kiểm tra các phương thức CRUD của API Gateway, đảm bảo cấu hình CORS và bảo mật IAM hoạt động chính xác. | 23/06/2026 | 24/06/2026 | |
| 5 | Họp nhóm phát triển tính năng gợi ý học tập thông minh dựa trên lịch sử học của người dùng và hoàn thiện luồng dữ liệu cho tính năng này. | 24/06/2026 | 26/06/2026 | |
| 6 | Thiết lập giám sát hệ thống cấu hình CloudWatch để theo dõi hiệu năng của EC2 (ARM64), cài đặt cảnh báo cho RDS và sử dụng X-Ray để theo dõi các cuộc gọi API nội bộ. | 26/06/2026 | 28/06/2026 | https://cloudjourney.awsstudygroup.com/ |

### Kết quả đạt được tuần 10:

* Đã hoàn thiện cơ chế caching trên Amazon CloudFront và tinh chỉnh các truy vấn cơ sở dữ liệu trên RDS PostgreSQL, giúp cải thiện đáng kể tốc độ phản hồi của hệ thống.

* Triển khai thành công hàm AWS Lambda để tự động hóa quy trình xử lý hình ảnh (resize, nén) tải lên Amazon S3, tối ưu hiệu quả lưu trữ.

* Hoàn tất kiểm thử các phương thức CRUD qua API Gateway; đảm bảo cấu hình bảo mật IAM và CORS vận hành ổn định.

* Cấu hình thành công Amazon CloudWatch để theo dõi hiệu năng EC2 và đặt cảnh báo cho RDS; tích hợp AWS X-Ray để phân tích luồng cuộc gọi API nội bộ, giúp phát hiện và khắc phục sự cố nhanh chóng.

* Hoàn thiện thành công luồng dữ liệu cho tính năng gợi ý học tập thông minh dựa trên lịch sử tương tác của người dùng trên nền tảng.
