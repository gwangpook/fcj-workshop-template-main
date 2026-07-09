---
title: "Tạo RDS PostgreSQL"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.4.1. </b>"
---


---

## 1. Tạo DB Subnet Group

Trước khi tạo RDS, cần tạo **Subnet Group** để chỉ định RDS sẽ chạy trong các Private Subnet nào.

1. Tìm kiếm **RDS** → **Subnet groups** → **Create DB subnet group**

2. Cấu hình:

| Trường          | Giá trị                           |
| --------------- | --------------------------------- |
| **Name**        | `flashlearn-db-subnet-group`      |
| **Description** | `Subnet group for FlashLearn RDS` |
| **VPC**         | `flashlearn-vpc`                  |

3. **Add subnets** — Thêm cả 2 private subnet:
   - `ap-southeast-1a` → `flashlearn-private-subnet-1` (`10.0.2.0/24`)
   - `ap-southeast-1b` → `flashlearn-private-subnet-2` (`10.0.3.0/24`)

4. Nhấn **Create**

![Tạo DB Subnet Group](/images/5-Workshop/5.4-rds/5.4.1/1.png)

---

## 2. Tạo RDS PostgreSQL Instance

1. **RDS** → **Databases** → **Create database**

2. **Engine options**:
   - Engine type: **PostgreSQL**
   - Engine version: **PostgreSQL 16.x** (bản mới nhất)

3. **Templates**: Chọn **Free tier** (nếu tài khoản còn Free Tier) hoặc **Dev/Test**

4. **Settings**:

| Trường                     | Giá trị                      |
| -------------------------- | ---------------------------- |
| **DB instance identifier** | `flashlearn-db`              |
| **Master username**        | `flashlearn_admin`           |
| **Master password**        | Đặt mật khẩu mạnh (lưu lại!) |
| **Confirm password**       | Nhập lại mật khẩu            |

5. **Instance configuration**:

| Trường                  | Giá trị       |
| ----------------------- | ------------- |
| **DB instance class**   | `db.t3.micro` |
| **Storage type**        | `gp2`         |
| **Allocated storage**   | `20 GB`       |
| **Storage autoscaling** | Tắt           |

6. **Connectivity**:

| Trường                    | Giá trị                      |
| ------------------------- | ---------------------------- |
| **Virtual private cloud** | `flashlearn-vpc`             |
| **DB subnet group**       | `flashlearn-db-subnet-group` |
| **Public access**         | **No** ← Rất quan trọng!     |
| **VPC security group**    | `flashlearn-rds-sg`          |
| **Availability Zone**     | `ap-southeast-1a`            |

>  **Public access = No** là bắt buộc! Database không được phép truy cập từ Internet.

7. **Additional configuration**:

| Trường                      | Giá trị      |
| --------------------------- | ------------ |
| **Initial database name**   | `flashlearn` |
| **Backup retention period** | `7 days`     |
| **Encryption**              | Enable       |

8. Nhấn **Create database** và chờ khoảng **5-10 phút** để database khởi tạo

![RDS Đang Khởi Tạo](/images/5-Workshop/5.4-rds/5.4.1/2.png)

---

## 3. Lưu lại thông tin kết nối

Sau khi RDS ở trạng thái **Available**, ghi lại **Endpoint** để dùng ở các bước sau:

1. Chọn database `flashlearn-db` → tab **Connectivity & security**
2. Sao chép **Endpoint** (ví dụ: `flashlearn-db.xxxx.ap-southeast-1.rds.amazonaws.com`)

![RDS Endpoint](/images/5-Workshop/5.4-rds/5.4.1/3.png)

---

## Kết quả

Sau bước này, bạn đã có:
-  DB Subnet Group trong Private Subnets
-  RDS PostgreSQL `flashlearn-db` ở trạng thái Available
-  Đã lưu thông tin kết nối (Endpoint, Username, Password)
