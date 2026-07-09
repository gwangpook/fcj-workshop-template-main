---
title: "Cài đặt môi trường"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.2.2. </b>"
---


Bước này hướng dẫn cài đặt các công cụ cần thiết trên máy tính cá nhân để có thể làm việc với AWS và source code FlashLearn.

## 1. Cài đặt AWS CLI

**AWS CLI** cho phép bạn tương tác với các dịch vụ AWS từ terminal.

### Windows

Tải và cài đặt từ trang chính thức:

```
https://awscli.amazonaws.com/AWSCLIV2.msi
```

Sau khi cài xong, kiểm tra:

```bash
aws --version
# aws-cli/2.x.x Python/3.x.x Windows/x86_64
```

### Cấu hình AWS CLI

Chạy lệnh sau và điền thông tin IAM User đã tạo ở bước 5.2.1:

```bash
aws configure
```

```
AWS Access Key ID [None]: <Access Key của IAM User>
AWS Secret Access Key [None]: <Secret Key của IAM User>
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

> 💡 Để lấy **Access Key**: Vào IAM → Users → `flashlearn-admin` → Security credentials → **Create access key**

---

## 2. Cài đặt .NET SDK 8.0

Ứng dụng FlashLearn được xây dựng bằng **.NET 8.0**. Cần cài đặt để build và publish project.

Tải .NET SDK 8.0 tại:
```
https://dotnet.microsoft.com/download/dotnet/8.0
```

Kiểm tra sau khi cài:

```bash
dotnet --version
# 8.0.x
```

---

## 3. Cài đặt Git

**Git** cần thiết để clone source code và đẩy code lên server.

```
https://git-scm.com/downloads
```

Kiểm tra:

```bash
git --version
# git version 2.x.x
```

---

## 4. Cài đặt SSH Client

Cần SSH để kết nối vào EC2 instance.

- **Windows 10/11**: OpenSSH đã được tích hợp sẵn
- Kiểm tra: Mở PowerShell và gõ `ssh`

---

## 5. Clone source code FlashLearn

```bash
git clone <đường dẫn repo FlashLearn của bạn>
cd WebhocTiengAnhFlashLearn
```

Kiểm tra project có thể build thành công:

```bash
dotnet restore
dotnet build
```

Kết quả mong đợi:
```
Build succeeded.
    0 Warning(s)
    0 Error(s)
```

---

## Kết quả

Sau bước này, bạn đã có:
-  AWS CLI đã cấu hình với IAM User
- .NET SDK 8.0 đã cài đặt
-  Git và SSH Client sẵn sàng
-  Source code FlashLearn đã clone về máy
