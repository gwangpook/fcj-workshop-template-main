---
title: "Set Up Development Environment"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.2.2. </b>"
---

# Set Up Development Environment

This step guides you through installing the necessary tools on your personal computer to work with AWS and the FlashLearn source code.

## 1. Install AWS CLI

**AWS CLI** allows you to interact with AWS services from your terminal.

### Windows

Download and install from the official page:

```
https://awscli.amazonaws.com/AWSCLIV2.msi
```

After installation, verify:

```bash
aws --version
# aws-cli/2.x.x Python/3.x.x Windows/x86_64
```

### Configure AWS CLI

Run the following command and enter your IAM User credentials created in step 5.2.1:

```bash
aws configure
```

```
AWS Access Key ID [None]: <IAM User Access Key>
AWS Secret Access Key [None]: <IAM User Secret Key>
Default region name [None]: ap-southeast-1
Default output format [None]: json
```

>  To get the **Access Key**: Go to IAM → Users → `flashlearn-admin` → Security credentials → **Create access key**

---

## 2. Install .NET SDK 8.0

The FlashLearn application is built with **.NET 8.0**. It is required to build and publish the project.

Download .NET SDK 8.0 at:
```
https://dotnet.microsoft.com/download/dotnet/8.0
```

Verify after installation:

```bash
dotnet --version
# 8.0.x
```

---

## 3. Install Git

**Git** is required to clone the source code and push code to the server.

```
https://git-scm.com/downloads
```

Verify:

```bash
git --version
# git version 2.x.x
```

---

## 4. Install SSH Client

SSH is required to connect to EC2 instances.

- **Windows 10/11**: OpenSSH is built-in
- Verify: Open PowerShell and type `ssh`

---

## 5. Clone the FlashLearn Source Code

```bash
git clone <your FlashLearn repository URL>
cd WebhocTiengAnhFlashLearn
```

Verify the project builds successfully:

```bash
dotnet restore
dotnet build
```

Expected result:
```
Build succeeded.
    0 Warning(s)
    0 Error(s)
```

---

## Result

After this step, you will have:
-  AWS CLI configured with your IAM User
-  .NET SDK 8.0 installed
-  Git and SSH Client ready
-  FlashLearn source code cloned locally
