---
title: "Tạo S3 Bucket & Phân quyền"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.6.1. </b>"
---


---

## 1. Tạo S3 Bucket

1. Tìm kiếm **S3** trong AWS Console → **Create bucket**

2. Cấu hình bucket:

| Trường                  | Giá trị                               |
| ----------------------- | ------------------------------------- |
| **Bucket name**         | `flashlearn-media-<tên-của-bạn>`      |
| **AWS Region**          | `ap-southeast-1` (Singapore)          |
| **Object Ownership**    | ACLs disabled                         |
| **Block Public Access** | **Bỏ tích** "Block all public access" |
| **Bucket Versioning**   | Disable                               |



1. Tích xác nhận bỏ chặn public access → Nhấn **Create bucket**

![Tạo S3 Bucket](/images/5-Workshop/5.6-s3/5.6.1/1.png)

---

## 2. Thêm Bucket Policy

Cấu hình policy để cho phép đọc file công khai (ảnh flashcard có thể xem được không cần đăng nhập):

1. Chọn bucket → tab **Permissions** → **Bucket policy** → **Edit**

2. Dán policy sau:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::flashlearn-media-quangphuc/*"
    }
  ]
}
```


1. Nhấn **Save changes**

---

## 3. Tạo CORS Configuration

Cần cấu hình CORS để browser có thể upload file trực tiếp lên S3:

1. Tab **Permissions** → **Cross-origin resource sharing (CORS)** → **Edit**

2. Dán cấu hình:

```json
[
  {
    "AllowedHeaders": ["*"],
    "AllowedMethods": ["GET", "PUT", "POST", "DELETE"],
    "AllowedOrigins": [
      "http://localhost:5000",
      "http://<EC2-Public-IP>:5000"
    ],
    "ExposeHeaders": ["ETag"]
  }
]
```

3. Nhấn **Save changes**

---

## 4. Tạo IAM Role cho EC2

EC2 cần quyền để đọc/ghi vào S3. Cách tốt nhất là dùng **IAM Role** thay vì Access Key.

1. Tìm kiếm **IAM** → **Roles** → **Create role**

2. **Trusted entity type**: AWS service → **EC2**

3. **Permissions policies**: Tìm và thêm:
   - `AmazonS3FullAccess` (hoặc custom policy giới hạn chỉ bucket flashlearn)

4. **Role name**: `flashlearn-ec2-role`

5. Nhấn **Create role**

6. **Gắn Role vào EC2**:
   - EC2 Console → Chọn `flashlearn-server`
   - **Actions** → **Security** → **Modify IAM role**
   - Chọn `flashlearn-ec2-role` → **Update IAM role**

![Gắn IAM Role vào EC2](/images/5-Workshop/5.6-s3/5.6.1/2.png)

---

## Kết quả

Sau bước này, bạn đã có:
-  S3 Bucket `flashlearn-media-xxx` đã tạo
-  Bucket Policy cho phép đọc file công khai
-  CORS cấu hình cho phép upload từ browser
-  IAM Role gắn vào EC2 để có quyền S3
