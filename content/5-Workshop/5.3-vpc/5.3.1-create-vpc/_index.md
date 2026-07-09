---
title: "Create VPC & Subnets"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.3.1. </b>"
---


In this step, we will create a new VPC and divide it into specific subnets for each application tier.

---

## 1. Create VPC

1. Log in to **AWS Console** → Search for **VPC** → Select **VPC**

2. Click **Create VPC**

![VPC Dashboard](/images/5-Workshop/5.3-S3-vpc/5.3.1.1.png)

3. Configure the VPC:

| Field                   | Value              |
| ----------------------- | ------------------ |
| **Resources to create** | VPC only           |
| **Name tag**            | `flashlearn-vpc`   |
| **IPv4 CIDR block**     | `10.0.0.0/16`      |
| **IPv6 CIDR block**     | No IPv6 CIDR block |
| **Tenancy**             | Default            |

4. Click **Create VPC**

![Create VPC](/images/5-Workshop/5.3-S3-vpc/5.3.1-vpc-created.png)

---

## 2. Create Public Subnet

The Public Subnet will host the EC2 instance running the FlashLearn application.

1. In the VPC Console → **Subnets** → **Create subnet**

2. Configure:

| Field                 | Value                      |
| --------------------- | -------------------------- |
| **VPC ID**            | Select `flashlearn-vpc`    |
| **Subnet name**       | `flashlearn-public-subnet` |
| **Availability Zone** | `ap-southeast-1a`          |
| **IPv4 CIDR block**   | `10.0.1.0/24`              |

3. Click **Create subnet**

![Create Public Subnet](/images/5-Workshop/5.3-S3-vpc/5.3.1.2.png)

4. After creation, **enable Auto-assign public IP**:
   - Select subnet `flashlearn-public-subnet`
   - **Actions** → **Edit subnet settings**
   - Check **Enable auto-assign public IPv4 address**
   - Click **Save**

---

## 3. Create Private Subnets

Private Subnets will host the RDS database, with no direct Internet connection.

1. **Subnets** → **Create subnet**

2. Configure the first subnet:

| Field                 | Value                         |
| --------------------- | ----------------------------- |
| **VPC ID**            | Select `flashlearn-vpc`       |
| **Subnet name**       | `flashlearn-private-subnet-1` |
| **Availability Zone** | `ap-southeast-1a`             |
| **IPv4 CIDR block**   | `10.0.2.0/24`                 |

3. Click **Add new subnet** to add a second subnet (RDS requires at least 2 AZs):

| Field                 | Value                         |
| --------------------- | ----------------------------- |
| **Subnet name**       | `flashlearn-private-subnet-2` |
| **Availability Zone** | `ap-southeast-1b`             |
| **IPv4 CIDR block**   | `10.0.3.0/24`                 |

4. Click **Create subnet**

![Create Private Subnets](/images/5-Workshop/5.3-S3-vpc/5.3.1.3.png)
![Create Private Subnets](/images/5-Workshop/5.3-S3-vpc/5.3.1.4.png)
---

## Result

After this step, you will have:
-  VPC `flashlearn-vpc` with CIDR `10.0.0.0/16`
-  Public Subnet `10.0.1.0/24` in `ap-southeast-1a`
-  Private Subnet 1 `10.0.2.0/24` in `ap-southeast-1a`
-  Private Subnet 2 `10.0.3.0/24` in `ap-southeast-1b`
