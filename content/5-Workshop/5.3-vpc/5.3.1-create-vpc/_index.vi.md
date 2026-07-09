---
title: "Khởi tạo VPC & Subnets"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.3.1. </b>"
---



Trong bước này, chúng ta sẽ tạo một VPC mới và phân chia thành các subnet cụ thể cho từng lớp ứng dụng.

---

## 1. Tạo VPC

1. Đăng nhập **AWS Console** → Tìm kiếm **VPC** → Chọn **VPC**

2. Chọn **Create VPC**

![VPC Dashboard](/images/5-Workshop/5.3-S3-vpc/5.3.1.1.png)


1. Cấu hình VPC:

| Trường                  | Giá trị            |
| ----------------------- | ------------------ |
| **Resources to create** | VPC only           |
| **Name tag**            | `flashlearn-vpc`   |
| **IPv4 CIDR block**     | `10.0.0.0/16`      |
| **IPv6 CIDR block**     | No IPv6 CIDR block |
| **Tenancy**             | Default            |

4. Nhấn **Create VPC**

![Tạo VPC](/images/5-Workshop/5.3-S3-vpc/5.3.1-vpc-created.png)

---

## 2. Tạo Public Subnet

Public Subnet sẽ chứa EC2 instance chạy ứng dụng FlashLearn.

1. Trong VPC Console → **Subnets** → **Create subnet**

2. Cấu hình:

| Trường                | Giá trị                    |
| --------------------- | -------------------------- |
| **VPC ID**            | Chọn `flashlearn-vpc`      |
| **Subnet name**       | `flashlearn-public-subnet` |
| **Availability Zone** | `ap-southeast-1a`          |
| **IPv4 CIDR block**   | `10.0.1.0/24`              |

3. Nhấn **Create subnet**

![Tạo Public Subnet](/images/5-Workshop/5.3-S3-vpc/5.3.1.2.png)

1. Sau khi tạo xong, **bật tính năng Auto-assign public IP**:
   - Chọn subnet `flashlearn-public-subnet`
   - **Actions** → **Edit subnet settings**
   - Tích chọn **Enable auto-assign public IPv4 address**
   - Nhấn **Save**

---

## 3. Tạo Private Subnet

Private Subnet sẽ chứa RDS database, không có kết nối Internet trực tiếp.

1. **Subnets** → **Create subnet**

2. Cấu hình subnet thứ nhất:

| Trường                | Giá trị                       |
| --------------------- | ----------------------------- |
| **VPC ID**            | Chọn `flashlearn-vpc`         |
| **Subnet name**       | `flashlearn-private-subnet-1` |
| **Availability Zone** | `ap-southeast-1a`             |
| **IPv4 CIDR block**   | `10.0.2.0/24`                 |

3. Nhấn **Add new subnet** để thêm subnet thứ 2 (RDS yêu cầu tối thiểu 2 AZ):

| Trường                | Giá trị                       |
| --------------------- | ----------------------------- |
| **Subnet name**       | `flashlearn-private-subnet-2` |
| **Availability Zone** | `ap-southeast-1b`             |
| **IPv4 CIDR block**   | `10.0.3.0/24`                 |

4. Nhấn **Create subnet**

![Tạo Private Subnets](/images/5-Workshop/5.3-S3-vpc/5.3.1.3.png)
![Create Private Subnets](/images/5-Workshop/5.3-S3-vpc/5.3.1.4.png)

---

## Kết quả

Sau bước này, bạn đã có:
-  VPC `flashlearn-vpc` với CIDR `10.0.0.0/16`
-  Public Subnet `10.0.1.0/24` tại `ap-southeast-1a`
-  Private Subnet 1 `10.0.2.0/24` tại `ap-southeast-1a`
-  Private Subnet 2 `10.0.3.0/24` tại `ap-southeast-1b`
