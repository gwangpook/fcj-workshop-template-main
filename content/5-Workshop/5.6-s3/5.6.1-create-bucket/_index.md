---
title: "Create S3 Bucket & Configure Permissions"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.6.1. </b>"
---


---

## 1. Create S3 Bucket

1. Search for **S3** in AWS Console → **Create bucket**

2. Configure the bucket:

| Field                   | Value                                 |
| ----------------------- | ------------------------------------- |
| **Bucket name**         | `flashlearn-media-<your-name>`        |
| **AWS Region**          | `ap-southeast-1` (Singapore)          |
| **Object Ownership**    | ACLs disabled                         |
| **Block Public Access** | **Uncheck** "Block all public access" |
| **Bucket Versioning**   | Disable                               |



1. Confirm unchecking public access block → Click **Create bucket**

![Create S3 Bucket](/images/5-Workshop/5.6-s3/5.6.1/1.png)

---

## 2. Add Bucket Policy

Configure a policy to allow public read access to flashcard images:

1. Select the bucket → **Permissions** tab → **Bucket policy** → **Edit**

2. Paste the following policy:

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


1. Click **Save changes**

---

## 3. Configure CORS

CORS must be configured to allow browsers to upload files directly to S3:

1. **Permissions** tab → **Cross-origin resource sharing (CORS)** → **Edit**

2. Paste the configuration:

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

3. Click **Save changes**

---

## 4. Create IAM Role for EC2

EC2 needs permissions to read and write to S3. The best practice is to use an **IAM Role** instead of Access Keys.

1. Search for **IAM** → **Roles** → **Create role**

2. **Trusted entity type**: AWS service → **EC2**

3. **Permissions policies**: Search and add:
   - `AmazonS3FullAccess` (or a custom policy scoped to the flashlearn bucket only)

4. **Role name**: `flashlearn-ec2-role`

5. Click **Create role**

6. **Attach the Role to EC2**:
   - EC2 Console → Select `flashlearn-server`
   - **Actions** → **Security** → **Modify IAM role**
   - Select `flashlearn-ec2-role` → **Update IAM role**

![Attach IAM Role to EC2](/images/5-Workshop/5.6-s3/5.6.1/2.png)

---

## Result

After this step, you will have:
-  S3 Bucket `flashlearn-media-xxx` created
-  Bucket Policy allowing public read access
-  CORS configured to allow browser uploads
-  IAM Role attached to EC2 with S3 permissions
