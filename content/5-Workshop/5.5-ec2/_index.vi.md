---
title: "Triển khai Backend (AWS EC2)"
date: 2026-07-09
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---


**Amazon EC2 (Elastic Compute Cloud)** là dịch vụ máy chủ ảo trên AWS. Chúng ta sẽ sử dụng EC2 để chạy ứng dụng **ASP.NET Core 8.0** của FlashLearn.

## Vai trò của EC2 trong kiến trúc

```
[Internet] → [EC2 t3.micro - Public Subnet]
                    │
                    ├─► ASP.NET Core 8.0 (port 5000)
                    ├─► SignalR Hubs (Battle mode real-time)
                    ├─► Kết nối RDS PostgreSQL (port 5432)
                    └─► Kết nối S3 (upload/download ảnh)
```

## Nội dung

{{% children showhidden="false" /%}}
