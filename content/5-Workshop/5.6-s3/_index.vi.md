---
title: "Lưu trữ tệp phương tiện (Amazon S3)"
date: 2026-07-09
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---


**Amazon S3 (Simple Storage Service)** là dịch vụ lưu trữ đối tượng của AWS. Trong FlashLearn, S3 được dùng để lưu trữ **ảnh minh họa** của các flashcard do người dùng tải lên.

## Tại sao dùng S3 thay vì lưu trực tiếp trên EC2?

|             | Lưu trên EC2       | Amazon S3              |
| ----------- | ------------------ | ---------------------- |
| **Độ bền**  | Mất khi terminate  | 99.999999999%          |
| **Chi phí** | Tốn dung lượng EBS | Rẻ hơn (~$0.023/GB)    |
| **Mở rộng** | Giới hạn disk      | Không giới hạn         |
| **CDN**     | Không              | Dễ tích hợp CloudFront |
| **Backup**  | Thủ công           | Tự động với Versioning |

## Nội dung

{{% children showhidden="false" /%}}
