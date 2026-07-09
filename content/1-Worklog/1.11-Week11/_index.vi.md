---
title: "Worklog Tuần 11"
date: 2026-04-17
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

### Mục tiêu tuần 11:

* Thiết kế API cốt lõi.
* Xây dựng cơ sở dữ liệu.
* Triển khai luồng phê duyệt nội dung.
* Tối ưu hóa quản lý tài nguyên.
* Hoàn thiện tài liệu kỹ thuật.

### Các công việc cần triển khai trong tuần này:

| Ngày | Nhiệm vụ | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo |
| --- | --- | --- | --- | --- |
| 2 | Thiết kế API cho tính năng Flashcard (POST/Create) và Quiz, xác thực dữ liệu đầu vào (tối đa ký tự, định dạng câu hỏi). | 28/06/2026 | 29/06/2026 | [Xây dựng API RESTful với API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started.html) |
| 3 | Tạo bảng flashcards và quiz_results với trạng thái xử lý; xây dựng giao diện admin để quản trị nội dung flashcard. | 29/06/2026 | 30/06/2026 | |
| 4 | Xử lý trạng thái nội dung (chờ duyệt/đã duyệt); kích hoạt Lambda để cập nhật chỉ mục tìm kiếm sau khi duyệt. | 01/06/2026 | 02/06/2026 | |
| 5 | Tạo API ký số (Presigned URL) cho S3 để người dùng tải ảnh/âm thanh học tập; thiết lập URL CloudFront để truy xuất. | 02/06/2026 | 03/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| 6 | Đánh giá tài liệu kỹ thuật, hoàn thiện sơ đồ quan hệ cơ sở dữ liệu (ERD) cho người dùng, flashcard và kết quả bài thi; chuẩn bị cho tuần 12. | 03/06/2026 | 05/06/2026 | [Hướng dẫn thiết kế sơ đồ quan hệ thực thể](https://lucid.co/diagram/erd/tutorial) |

### Kết quả đạt được tuần 11:

* Đã hoàn tất thiết kế toàn bộ các API cho tính năng Flashcard và Quiz, bao gồm triển khai cơ chế xác thực đầu vào chặt chẽ nhằm đảm bảo tính toàn vẹn của dữ liệu.

* Hoàn thành thiết kế các bảng dữ liệu flashcards và quiz_results trong PostgreSQL; đồng thời phát triển thành công giao diện quản trị (admin) phục vụ việc kiểm duyệt nội dung học tập.

* Thiết lập thành công quy trình xử lý trạng thái nội dung (chờ duyệt/đã duyệt) và tích hợp AWS Lambda để tự động hóa việc cập nhật chỉ mục tìm kiếm ngay sau khi nội dung được phê duyệt.

* Triển khai cơ chế Presigned URL của Amazon S3 kết hợp với CloudFront, cho phép người dùng tải lên và truy xuất tài nguyên học tập (hình ảnh/âm thanh) một cách an toàn và tối ưu.

* Tổng kết và hoàn thiện sơ đồ quan hệ cơ sở dữ liệu (ERD) cho các thực thể quan trọng (người dùng, flashcard, kết quả học tập), tạo nền tảng vững chắc cho việc triển khai các giai đoạn tiếp theo.
