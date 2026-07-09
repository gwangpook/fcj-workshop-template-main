---
title: "Clean Up Resources"
date: 2026-07-09
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---



Follow the deletion order below to avoid dependency errors.

---

## Deletion Order

```
1. Cognito User Pool
2. S3 Bucket (empty contents first, then delete bucket)
3. RDS Instance
4. EC2 Instance & Elastic IP
5. VPC & Network Resources
6. IAM Role & Key Pair
```

---

## 1. Delete Amazon Cognito

1. **Cognito** → **User pools** → Select `flashlearn-user-pool`
2. **Actions** → **Delete user pool**
3. Enter the user pool name to confirm → **Delete**

---

## 2. Delete S3 Bucket

>  You must **empty all contents** before deleting the bucket!

```bash
# Delete all objects in the bucket
aws s3 rm s3://flashlearn-media-<your-name> --recursive

# Delete the bucket
aws s3 rb s3://flashlearn-media-<your-name>
```

Or via the Console:
1. **S3** → Select `flashlearn-media-xxx` → **Empty** → Confirm
2. After emptying → **Delete** → Enter bucket name → **Delete bucket**

---

## 3. Delete RDS Instance

1. **RDS** → **Databases** → Select `flashlearn-db`
2. **Actions** → **Delete**
3. Uncheck **Create final snapshot** (no backup needed)
4. Check acknowledgment and type `delete me` → **Delete**

> ⏱️ RDS deletion takes approximately 5–10 minutes

After RDS is deleted, delete the **DB Subnet Group**:
1. **Subnet groups** → `flashlearn-db-subnet-group` → **Delete**

---

## 4. Delete EC2 Instance & Elastic IP

**Delete EC2**:
1. **EC2** → **Instances** → Select `flashlearn-server`
2. **Instance state** → **Terminate instance** → **Terminate**

**Release Elastic IP** (if created):
1. **Elastic IPs** → Select the created IP
2. **Actions** → **Release Elastic IP addresses** → **Release**

**Delete Key Pair**:
1. **Key Pairs** → Select `flashlearn-key` → **Actions** → **Delete** → **Delete**

---

## 5. Delete VPC & Network Resources

Delete in the following order:

**Delete Security Groups**:
1. **Security Groups** → Select `flashlearn-ec2-sg` → **Actions** → **Delete security groups**
2. Repeat for `flashlearn-rds-sg`

**Delete Internet Gateway**:
1. **Internet gateways** → Select `flashlearn-igw`
2. **Actions** → **Detach from VPC** → Detach
3. **Actions** → **Delete internet gateway** → Delete

**Delete Route Table**:
1. **Route tables** → Select `flashlearn-public-rt`
2. **Subnet associations** tab → Remove all associations
3. **Actions** → **Delete route table**

**Delete VPC**:
1. **Your VPCs** → Select `flashlearn-vpc`
2. **Actions** → **Delete VPC** → Type `delete` → **Delete**

---

## 6. Delete IAM Role

1. **IAM** → **Roles** → Search for `flashlearn-ec2-role`
2. Select → **Delete** → Enter role name → **Delete**

---

## 7. Final Verification

After deletion, verify no resources remain:

```bash
# Check EC2
aws ec2 describe-instances --filters "Name=tag:Name,Values=flashlearn*" --query "Reservations[*].Instances[*].InstanceId"

# Check RDS
aws rds describe-db-instances --query "DBInstances[?starts_with(DBInstanceIdentifier,'flashlearn')].DBInstanceIdentifier"

# Check S3
aws s3 ls | grep flashlearn
```

All commands above should return **empty results** if cleanup is complete.

---





| Step        | Achievement                                                    |
| ----------- | -------------------------------------------------------------- |
| **VPC**     | Set up an isolated virtual network with Public/Private Subnets |
| **RDS**     | Deployed a fully managed PostgreSQL database                   |
| **EC2**     | Deployed an ASP.NET Core 8.0 application to the Cloud          |
| **S3**      | Stored flashcard images with high durability                   |
| **Cognito** | Implemented enterprise-grade user authentication               |

