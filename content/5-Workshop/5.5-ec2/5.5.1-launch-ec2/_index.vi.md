---
title: "Khởi tạo EC2 Instance"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.5.1. </b>"
---


---

## 1. Tạo Key Pair

**Key Pair** là cặp khóa SSH dùng để kết nối vào EC2 an toàn.

1. **EC2** → **Key Pairs** → **Create key pair**

2. Cấu hình:

| Trường                      | Giá trị                                          |
| --------------------------- | ------------------------------------------------ |
| **Name**                    | `flashlearn-key`                                 |
| **Key pair type**           | RSA                                              |
| **Private key file format** | `.pem` (cho Linux/macOS) hoặc `.ppk` (cho PuTTY) |

3. Nhấn **Create key pair** → File `.pem` sẽ tự động tải về

4. **Bảo mật file key**:

```bash
# Trên Linux/macOS
chmod 400 flashlearn-key.pem
```

>  **Lưu file .pem cẩn thận!** Nếu mất file này, bạn sẽ không thể SSH vào EC2.

---

## 2. Khởi tạo EC2 Instance

1. **EC2** → **Instances** → **Launch instances**

2. **Name and tags**:
   - Name: `flashlearn-server`

3. **Application and OS Images (AMI)**:
   - Chọn **Ubuntu Server 22.04 LTS (HVM)**
   - Architecture: **64-bit (x86)**

![Chọn Ubuntu AMI](/images/5-Workshop/5.5-ec2/5.5.1/1.png)

4. **Instance type**:
   - Chọn **t3.micro** (Free Tier eligible)
   - 2 vCPU, 1 GiB RAM

5. **Key pair**: Chọn `flashlearn-key`

6. **Network settings** → **Edit**:

| Trường                    | Giá trị                        |
| ------------------------- | ------------------------------ |
| **VPC**                   | `flashlearn-vpc`               |
| **Subnet**                | `flashlearn-public-subnet`     |
| **Auto-assign public IP** | Enable                         |
| **Firewall**              | Select existing security group |
| **Security group**        | `flashlearn-ec2-sg`            |

7. **Configure storage**:
   - **Root volume**: 20 GiB, gp3

8. Nhấn **Launch instance**

![EC2 Instance Running](/images/5-Workshop/5.5-ec2/5.5.1/2.png)

---

## 3. Gắn Elastic IP (tùy chọn nhưng khuyến nghị)

Elastic IP đảm bảo EC2 luôn có cùng một địa chỉ IP dù khởi động lại.

1. **Elastic IPs** → **Allocate Elastic IP address** → **Allocate**

2. Chọn Elastic IP vừa tạo → **Actions** → **Associate Elastic IP address**

3. Cấu hình:
   - **Resource type**: Instance
   - **Instance**: `flashlearn-server`

4. Nhấn **Associate**

---

## 4. Kết nối vào EC2

```bash
ssh -i flashlearn-key.pem ubuntu@<EC2-Public-IP>
```

Kết quả mong đợi:
```
Welcome to Ubuntu 22.04.x LTS (GNU/Linux 6.x.x-aws x86_64)
ubuntu@ip-10-0-1-xxx:~$
```

---

## Kết quả

Sau bước này, bạn đã có:
-  EC2 Ubuntu 22.04 đang chạy trong Public Subnet
-  Key Pair `flashlearn-key.pem` để SSH
-  Kết nối SSH vào EC2 thành công
