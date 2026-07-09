---
title: "Launch EC2 Instance"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.5.1. </b>"
---


---

## 1. Create Key Pair

A **Key Pair** is an SSH key used to securely connect to EC2.

1. **EC2** → **Key Pairs** → **Create key pair**

2. Configure:

| Field                       | Value                                  |
| --------------------------- | -------------------------------------- |
| **Name**                    | `flashlearn-key`                       |
| **Key pair type**           | RSA                                    |
| **Private key file format** | `.pem` (Linux/macOS) or `.ppk` (PuTTY) |

3. Click **Create key pair** → The `.pem` file will automatically download

4. **Secure the key file**:

```bash
# On Linux/macOS
chmod 400 flashlearn-key.pem
```

>  **Keep the .pem file safe!** If you lose it, you will not be able to SSH into EC2.

---

## 2. Launch EC2 Instance

1. **EC2** → **Instances** → **Launch instances**

2. **Name and tags**:
   - Name: `flashlearn-server`

3. **Application and OS Images (AMI)**:
   - Select **Ubuntu Server 22.04 LTS (HVM)**
   - Architecture: **64-bit (x86)**

![Select Ubuntu AMI](/images/5-Workshop/5.5-ec2/5.5.1/1.png)

4. **Instance type**:
   - Select **t3.micro** (Free Tier eligible)
   - 2 vCPU, 1 GiB RAM

5. **Key pair**: Select `flashlearn-key`

6. **Network settings** → **Edit**:

| Field                     | Value                          |
| ------------------------- | ------------------------------ |
| **VPC**                   | `flashlearn-vpc`               |
| **Subnet**                | `flashlearn-public-subnet`     |
| **Auto-assign public IP** | Enable                         |
| **Firewall**              | Select existing security group |
| **Security group**        | `flashlearn-ec2-sg`            |

7. **Configure storage**:
   - **Root volume**: 20 GiB, gp3

8. Click **Launch instance**

![EC2 Instance Running](/images/5-Workshop/5.5-ec2/5.5.1/2.png)

---

## 3. Allocate Elastic IP (Optional but Recommended)

An Elastic IP ensures the EC2 instance always has the same IP address even after restarts.

1. **Elastic IPs** → **Allocate Elastic IP address** → **Allocate**

2. Select the newly created Elastic IP → **Actions** → **Associate Elastic IP address**

3. Configure:
   - **Resource type**: Instance
   - **Instance**: `flashlearn-server`

4. Click **Associate**

---

## 4. Connect to EC2

```bash
ssh -i flashlearn-key.pem ubuntu@<EC2-Public-IP>
```

Expected result:
```
Welcome to Ubuntu 22.04.x LTS (GNU/Linux 6.x.x-aws x86_64)
ubuntu@ip-10-0-1-xxx:~$
```

---

## Result

After this step, you will have:
-  EC2 Ubuntu 22.04 running in the Public Subnet
-  Key Pair `flashlearn-key.pem` for SSH access
-  Successful SSH connection to EC2
