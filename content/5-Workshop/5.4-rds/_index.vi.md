---
title: "Triển khai Cơ sở dữ liệu (AWS RDS)"
date: 2026-07-09
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---


**Amazon RDS (Relational Database Service)** là dịch vụ cơ sở dữ liệu quan hệ được AWS quản lý hoàn toàn — tự động backup, vá lỗi bảo mật, và giám sát.

## Tại sao chuyển từ SQLite sang RDS?

|                   | SQLite (Local)          | Amazon RDS (PostgreSQL) |
| ----------------- | ----------------------- | ----------------------- |
| **Vị trí**        | File trên máy chủ       | Dịch vụ riêng biệt      |
| **Backup**        | Thủ công                | Tự động, 7 ngày         |
| **Mở rộng**       | Khó                     | Dễ dàng nâng cấp        |
| **Tính sẵn sàng** | Single point of failure | Multi-AZ tùy chọn       |
| **Quản lý**       | Tự quản                 | AWS quản lý hoàn toàn   |

## Nội dung

{{% children showhidden="false" /%}}
