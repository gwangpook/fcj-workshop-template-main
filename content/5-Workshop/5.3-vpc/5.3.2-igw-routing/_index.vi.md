---
title: "Cấu hình Internet Gateway & Route Tables"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.3.2. </b>"
---


Sau khi tạo VPC và các Subnet, chúng ta cần cấu hình **Internet Gateway** để cho phép Public Subnet kết nối ra Internet, và **Route Tables** để định tuyến lưu lượng mạng đúng hướng.

---

## 1. Tạo Internet Gateway

**Internet Gateway (IGW)** là cổng kết nối giữa VPC và Internet.

1. Trong VPC Console → **Internet gateways** → **Create internet gateway**

2. Cấu hình:

| Trường       | Giá trị          |
| ------------ | ---------------- |
| **Name tag** | `flashlearn-igw` |

3. Nhấn **Create internet gateway**

![Tạo Internet Gateway](/images/5-Workshop/5.3-vpc/5.3.2/1.png)

4. Sau khi tạo, **gắn IGW vào VPC**:
   - Chọn `flashlearn-igw` → **Actions** → **Attach to VPC**
   - Chọn `flashlearn-vpc`
   - Nhấn **Attach internet gateway**

![Gắn IGW vào VPC](/images/5-Workshop/5.3-vpc/5.3.2/2.png)

---

## 2. Tạo Route Table cho Public Subnet

**Route Table** xác định lưu lượng mạng sẽ đi đâu.

1. **Route tables** → **Create route table**

2. Cấu hình:

| Trường   | Giá trị                |
| -------- | ---------------------- |
| **Name** | `flashlearn-public-rt` |
| **VPC**  | `flashlearn-vpc`       |

3. Nhấn **Create route table**

4. **Thêm route đến Internet**: Chọn `flashlearn-public-rt` → tab **Routes** → **Edit routes** → **Add route**

| Destination | Target           |
| ----------- | ---------------- |
| `0.0.0.0/0` | `flashlearn-igw` |

5. Nhấn **Save changes**

![Thêm Route Internet](/images/5-Workshop/5.3-vpc/5.3.2/3.png)

6. **Gắn vào Public Subnet**: Tab **Subnet associations** → **Edit subnet associations** → Chọn `flashlearn-public-subnet` → **Save associations**

---

## 3. Tạo Security Group cho EC2

**Security Group** hoạt động như tường lửa ảo, kiểm soát lưu lượng vào/ra EC2.

1. **Security Groups** → **Create security group**

2. Cấu hình:

| Trường                  | Giá trị                             |
| ----------------------- | ----------------------------------- |
| **Security group name** | `flashlearn-ec2-sg`                 |
| **Description**         | `Security group for FlashLearn EC2` |
| **VPC**                 | `flashlearn-vpc`                    |

3. **Inbound rules** — Thêm các rule sau:

| Type       | Protocol | Port | Source    | Mô tả                 |
| ---------- | -------- | ---- | --------- | --------------------- |
| HTTP       | TCP      | 80   | 0.0.0.0/0 | HTTP từ Internet      |
| HTTPS      | TCP      | 443  | 0.0.0.0/0 | HTTPS từ Internet     |
| Custom TCP | TCP      | 5000 | 0.0.0.0/0 | ASP.NET Core app port |
| SSH        | TCP      | 22   | My IP     | SSH từ máy của bạn    |

4. Nhấn **Create security group**

---

## 4. Tạo Security Group cho RDS

1. **Create security group**

2. Cấu hình:

| Trường                  | Giá trị                             |
| ----------------------- | ----------------------------------- |
| **Security group name** | `flashlearn-rds-sg`                 |
| **Description**         | `Security group for FlashLearn RDS` |
| **VPC**                 | `flashlearn-vpc`                    |

3. **Inbound rules**:

| Type       | Protocol | Port | Source              | Mô tả                    |
| ---------- | -------- | ---- | ------------------- | ------------------------ |
| PostgreSQL | TCP      | 5432 | `flashlearn-ec2-sg` | Chỉ cho phép EC2 kết nối |

>  **Lưu ý bảo mật**: Source của rule PostgreSQL phải là **Security Group của EC2** (không phải IP), đảm bảo chỉ EC2 mới truy cập được database.

4. Nhấn **Create security group**

---

## Kết quả

Sau bước này, bạn đã có:
-  Internet Gateway `flashlearn-igw` gắn vào VPC
-  Route Table `flashlearn-public-rt` định tuyến traffic ra Internet
-  Security Group `flashlearn-ec2-sg` cho phép HTTP/HTTPS/SSH
-  Security Group `flashlearn-rds-sg` chỉ cho phép EC2 kết nối vào PostgreSQL
