---
title: "Giới thiệu"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---


## FlashLearn là gì?

**FlashLearn** là ứng dụng web học tiếng Anh thông minh được xây dựng bằng **ASP.NET Core 8.0**, cho phép người dùng:

-  **Tạo bộ Flashcard** (Deck) với từ vựng tiếng Anh — tiếng Việt
-  **Luyện tập** theo hệ thống lặp lại ngắt quãng (Spaced Repetition)
-  **Đính kèm hình ảnh** minh họa cho từng flashcard
-  **Thi đấu** từ vựng với người chơi khác theo thời gian thực (SignalR)
-  **Theo dõi tiến độ** và chuỗi ngày học liên tục (Streak)

Ứng dụng hiện chạy với **SQLite** trên môi trường local. Workshop này sẽ hướng dẫn bạn chuyển toàn bộ hệ thống lên **AWS Cloud** với cơ sở dữ liệu **PostgreSQL** được quản lý hoàn toàn bởi AWS.

---

## Kiến trúc hệ thống

Hệ thống được thiết kế theo mô hình **3 lớp (3-Tier Architecture)** đặt trong một **Virtual Private Cloud (VPC)** có phân chia subnet rõ ràng:

![Kiến trúc FlashLearn AWS](/images/5-Workshop/5.1-Workshop-overview/aws_whiteboard.png)

### Các thành phần chính

| Lớp                | Thành phần                    | Dịch vụ AWS                            |
| ------------------ | ----------------------------- | -------------------------------------- |
| **Presentation**   | Giao diện web, SignalR        | EC2 (Public Subnet)                    |
| **Authentication** | Đăng ký / Đăng nhập           | Amazon Cognito                         |
| **Business Logic** | ASP.NET Core Controllers      | EC2 (Public Subnet)                    |
| **Database**       | Dữ liệu người dùng, flashcard | Amazon RDS PostgreSQL (Private Subnet) |
| **Storage**        | Ảnh flashcard                 | Amazon S3                              |

### Luồng hoạt động

```
Người dùng
    │
    ├─[1]─► Amazon Cognito ──► Xác thực, trả JWT Token
    │
    ├─[2]─► EC2 (ASP.NET Core) ──► Xử lý request với JWT
    │              │
    │              ├─► RDS PostgreSQL ──► Đọc/ghi dữ liệu flashcard
    │              │
    │              └─► Amazon S3 ──► Upload/Download ảnh
    │
    └─[3]─► SignalR Hub ──► Battle mode real-time
```

---

## Mục tiêu workshop

Sau khi hoàn thành workshop, bạn sẽ:

Nắm được cách thiết lập mạng lưới ảo (VPC) với các lớp bảo mật  
Triển khai ứng dụng ASP.NET Core 8.0 lên EC2  
Chuyển đổi database từ SQLite sang RDS PostgreSQL  
Lưu trữ media người dùng trên Amazon S3  
Tích hợp xác thực người dùng với Amazon Cognito  
Biết cách dọn dẹp tài nguyên để tránh phát sinh chi phí  

---

## Thời gian thực hiện

| Bước     | Nội dung             | Thời gian ước tính |
| -------- | -------------------- | ------------------ |
| 5.2      | Chuẩn bị môi trường  | ~30 phút           |
| 5.3      | Thiết lập VPC        | ~20 phút           |
| 5.4      | Triển khai RDS       | ~25 phút           |
| 5.5      | Triển khai EC2 + App | ~45 phút           |
| 5.6      | Cấu hình S3          | ~20 phút           |
| 5.7      | Tích hợp Cognito     | ~30 phút           |
| 5.8      | Dọn dẹp              | ~15 phút           |
| **Tổng** |                      | **~3 giờ**         |

---

## Chi phí ước tính


| Dịch vụ         | Cấu hình              | Chi phí/tháng  |
| --------------- | --------------------- | -------------- |
| EC2 t3.micro    | 1 instance, On-Demand | ~$8.47         |
| RDS db.t3.micro | PostgreSQL, Single-AZ | ~$15.33        |
| Amazon S3       | 5GB standard          | ~$0.12         |
| Amazon Cognito  | < 50,000 MAU          | $0.00          |
| **Tổng**        |                       | **~$24/tháng** |

