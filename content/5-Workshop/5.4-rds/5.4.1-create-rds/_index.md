---
title: "Create RDS PostgreSQL"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.4.1. </b>"
---


---

## 1. Create DB Subnet Group

Before creating RDS, you need to create a **Subnet Group** to specify which Private Subnets the RDS instance will run in.

1. Search for **RDS** → **Subnet groups** → **Create DB subnet group**

2. Configure:

| Field           | Value                             |
| --------------- | --------------------------------- |
| **Name**        | `flashlearn-db-subnet-group`      |
| **Description** | `Subnet group for FlashLearn RDS` |
| **VPC**         | `flashlearn-vpc`                  |

3. **Add subnets** — Add both private subnets:
   - `ap-southeast-1a` → `flashlearn-private-subnet-1` (`10.0.2.0/24`)
   - `ap-southeast-1b` → `flashlearn-private-subnet-2` (`10.0.3.0/24`)

4. Click **Create**

![Create DB Subnet Group](/images/5-Workshop/5.4-rds/5.4.1/1.png)

---

## 2. Create RDS PostgreSQL Instance

1. **RDS** → **Databases** → **Create database**

2. **Engine options**:
   - Engine type: **PostgreSQL**
   - Engine version: **PostgreSQL 16.x** (latest)

3. **Templates**: Select **Free tier** (if account has Free Tier) or **Dev/Test**

4. **Settings**:

| Field                      | Value                            |
| -------------------------- | -------------------------------- |
| **DB instance identifier** | `flashlearn-db`                  |
| **Master username**        | `flashlearn_admin`               |
| **Master password**        | Set a strong password (save it!) |
| **Confirm password**       | Re-enter the password            |

5. **Instance configuration**:

| Field                   | Value         |
| ----------------------- | ------------- |
| **DB instance class**   | `db.t3.micro` |
| **Storage type**        | `gp2`         |
| **Allocated storage**   | `20 GB`       |
| **Storage autoscaling** | Disabled      |

6. **Connectivity**:

| Field                     | Value                        |
| ------------------------- | ---------------------------- |
| **Virtual private cloud** | `flashlearn-vpc`             |
| **DB subnet group**       | `flashlearn-db-subnet-group` |
| **Public access**         | **No** ← Very important!     |
| **VPC security group**    | `flashlearn-rds-sg`          |
| **Availability Zone**     | `ap-southeast-1a`            |

>  **Public access = No** is mandatory! The database must not be accessible from the Internet.

7. **Additional configuration**:

| Field                       | Value        |
| --------------------------- | ------------ |
| **Initial database name**   | `flashlearn` |
| **Backup retention period** | `7 days`     |
| **Encryption**              | Enable       |

8. Click **Create database** and wait approximately **5–10 minutes** for the database to initialize

![RDS Creating](/images/5-Workshop/5.4-rds/5.4.1/2.png)

---

## 3. Save Connection Information

After RDS reaches **Available** status, record the **Endpoint** for use in later steps:

1. Select database `flashlearn-db` → **Connectivity & security** tab
2. Copy the **Endpoint** (e.g., `flashlearn-db.xxxx.ap-southeast-1.rds.amazonaws.com`)

![RDS Endpoint](/images/5-Workshop/5.4-rds/5.4.1/3.png)

---

## Result

After this step, you will have:
-  DB Subnet Group configured in Private Subnets
-  RDS PostgreSQL `flashlearn-db` in Available status
-  Connection details saved (Endpoint, Username, Password)
