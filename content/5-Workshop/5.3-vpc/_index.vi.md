---
title: "Thiết lập mạng lưới ảo (VPC)"
date: 2026-07-09
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---


**Amazon Virtual Private Cloud (VPC)** cho phép bạn tạo một mạng riêng ảo trên AWS Cloud, hoàn toàn cô lập với các tài nguyên khác.

## Kiến trúc mạng sẽ xây dựng

```
VPC: 10.0.0.0/16 (FlashLearn VPC)
│
├── Public Subnet: 10.0.1.0/24 (ap-southeast-1a)
│   ├── Internet Gateway
│   └── EC2 (ASP.NET Core App)
│
└── Private Subnet: 10.0.2.0/24 (ap-southeast-1a)
    └── RDS PostgreSQL
```

### Tại sao cần phân chia Public/Private Subnet?

|                      | Public Subnet    | Private Subnet |
| -------------------- | ---------------- | -------------- |
| **Kết nối Internet** | Có               | Không          |
| **Đặt gì ở đây**     | EC2 (Web Server) | RDS (Database) |
| **Bảo mật**          | Trung bình       | Cao            |

>  **Best Practice**: Database không nên tiếp xúc trực tiếp với Internet. Đặt RDS trong Private Subnet giúp cô lập hoàn toàn, chỉ EC2 mới có thể kết nối vào.

## Nội dung

{{% children showhidden="false" /%}}
