---
title: "Dọn dẹp tài nguyên"
date: 2026-07-09
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---



Thực hiện xóa theo đúng thứ tự bên dưới để tránh lỗi phụ thuộc.

---

## Thứ tự xóa

```
1. Cognito User Pool
2. S3 Bucket (xóa nội dung trước, rồi xóa bucket)
3. RDS Instance
4. EC2 Instance & Elastic IP
5. VPC & các tài nguyên mạng
6. IAM Role & Key Pair
```

---

## 1. Xóa Amazon Cognito

1. **Cognito** → **User pools** → Chọn `flashlearn-user-pool`
2. **Actions** → **Delete user pool**
3. Nhập tên user pool để xác nhận → **Delete**

---

## 2. Xóa S3 Bucket

>  Phải **xóa hết nội dung** trước khi xóa bucket!

```bash
# Xóa tất cả object trong bucket
aws s3 rm s3://flashlearn-media-<tên-của-bạn> --recursive

# Xóa bucket
aws s3 rb s3://flashlearn-media-<tên-của-bạn>
```

Hoặc qua Console:
1. **S3** → Chọn `flashlearn-media-xxx` → **Empty** → Xác nhận
2. Sau khi empty → **Delete** → Nhập tên bucket → **Delete bucket**

---

## 3. Xóa RDS Instance

1. **RDS** → **Databases** → Chọn `flashlearn-db`
2. **Actions** → **Delete**
3. Bỏ tích **Create final snapshot** (không cần backup)
4. Tích xác nhận và nhập `delete me` → **Delete**

>  Quá trình xóa RDS mất khoảng 5-10 phút

Sau khi xóa RDS, xóa **DB Subnet Group**:
1. **Subnet groups** → `flashlearn-db-subnet-group` → **Delete**

---

## 4. Xóa EC2 Instance & Elastic IP

**Xóa EC2**:
1. **EC2** → **Instances** → Chọn `flashlearn-server`
2. **Instance state** → **Terminate instance** → **Terminate**

**Xóa Elastic IP** (nếu đã tạo):
1. **Elastic IPs** → Chọn IP đã tạo
2. **Actions** → **Release Elastic IP addresses** → **Release**

**Xóa Key Pair**:
1. **Key Pairs** → Chọn `flashlearn-key` → **Actions** → **Delete** → **Delete**

---

## 5. Xóa VPC & Tài nguyên mạng

Xóa theo thứ tự:

**Xóa Security Groups**:
1. **Security Groups** → Chọn `flashlearn-ec2-sg` → **Actions** → **Delete security groups**
2. Lặp lại cho `flashlearn-rds-sg`

**Xóa Subnets** (tùy chọn — sẽ tự xóa khi xóa VPC):
1. **Subnets** → Chọn tất cả 3 subnet flashlearn → **Actions** → **Delete subnet**

**Xóa Internet Gateway**:
1. **Internet gateways** → Chọn `flashlearn-igw`
2. **Actions** → **Detach from VPC** → Detach
3. **Actions** → **Delete internet gateway** → Delete

**Xóa Route Table**:
1. **Route tables** → Chọn `flashlearn-public-rt`
2. Tab **Subnet associations** → Remove all associations
3. **Actions** → **Delete route table**

**Xóa VPC**:
1. **Your VPCs** → Chọn `flashlearn-vpc`
2. **Actions** → **Delete VPC** → Nhập `delete` → **Delete**

---

## 6. Xóa IAM Role

1. **IAM** → **Roles** → Tìm `flashlearn-ec2-role`
2. Chọn → **Delete** → Nhập tên role → **Delete**

---

## 7. Kiểm tra cuối cùng

Sau khi xóa xong, kiểm tra không còn tài nguyên nào:

```bash
# Kiểm tra EC2
aws ec2 describe-instances --filters "Name=tag:Name,Values=flashlearn*" --query "Reservations[*].Instances[*].InstanceId"

# Kiểm tra RDS
aws rds describe-db-instances --query "DBInstances[?starts_with(DBInstanceIdentifier,'flashlearn')].DBInstanceIdentifier"

# Kiểm tra S3
aws s3 ls | grep flashlearn
```

Tất cả lệnh trên nên trả về **rỗng** nếu đã xóa sạch.

---

Hoàn thành **Workshop FlashLearn trên AWS**!

### Những gì đã làm được:

| Bước        | Thành tựu                                        |
| ----------- | ------------------------------------------------ |
| **VPC**     | Thiết lập mạng lưới ảo với Public/Private Subnet |
| **RDS**     | Triển khai PostgreSQL được AWS quản lý           |
| **EC2**     | Deploy ứng dụng ASP.NET Core 8.0 lên Cloud       |
| **S3**      | Lưu trữ ảnh flashcard với độ bền cao             |
| **Cognito** | Xác thực người dùng chuẩn doanh nghiệp           |

