---
title: "Workshop"
date: 2026-07-09
weight: 5
chapter: false
pre: "<b>5. </b>"
---


**FlashLearn** là ứng dụng web học tiếng Anh thông minh, cho phép người dùng tạo và ôn luyện từ vựng thông qua hệ thống thẻ ghi nhớ (Flashcard) kết hợp thuật toán lặp lại ngắt quãng (Spaced Repetition).

Trong workshop này, bạn sẽ học cách triển khai ứng dụng **ASP.NET Core 8.0** lên hạ tầng **Amazon Web Services (AWS)** theo kiến trúc 3 lớp chuẩn doanh nghiệp.

---

## Những gì bạn sẽ xây dựng

```
[Người dùng]
     │ HTTPS
     ▼
[Amazon Cognito]        ← Xác thực & phân quyền
     │
     ▼
[EC2 - ASP.NET Core]   ← Máy chủ ứng dụng (Public Subnet)
     │              │
     ▼              ▼
[RDS PostgreSQL]  [S3 Bucket]
(Private Subnet)  (Lưu trữ ảnh)
```

---

## Các dịch vụ AWS sử dụng

| Dịch vụ            | Mục đích                                          |
| ------------------ | ------------------------------------------------- |
| **AWS VPC**        | Mạng lưới cô lập, phân chia Public/Private Subnet |
| **Amazon EC2**     | Máy chủ chạy ứng dụng ASP.NET Core 8.0            |
| **Amazon RDS**     | Cơ sở dữ liệu PostgreSQL được quản lý bởi AWS     |
| **Amazon S3**      | Lưu trữ ảnh flashcard do người dùng tải lên       |
| **Amazon Cognito** | Xác thực người dùng, cấp JWT Token                |

---

## Nội dung workshop

{{% children showhidden="false" /%}}
