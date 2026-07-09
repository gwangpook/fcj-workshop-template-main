---
title: "Workshop"
date: 2026-07-09
weight: 5
chapter: false
pre: "<b>5. </b>"
---


**FlashLearn** is a smart English learning web application that allows users to create and review vocabulary through a flashcard system combined with a Spaced Repetition algorithm.

In this workshop, you will learn how to deploy an **ASP.NET Core 8.0** application onto **Amazon Web Services (AWS)** infrastructure using a standard enterprise 3-tier architecture.

---

## What You Will Build

```
[Users]
     │ HTTPS
     ▼
[Amazon Cognito]        ← Authentication & Authorization
     │
     ▼
[EC2 - ASP.NET Core]   ← Application Server (Public Subnet)
     │              │
     ▼              ▼
[RDS PostgreSQL]  [S3 Bucket]
(Private Subnet)  (Image Storage)
```

---

## AWS Services Used

| Service            | Purpose                                                |
| ------------------ | ------------------------------------------------------ |
| **AWS VPC**        | Isolated network with Public/Private Subnet separation |
| **Amazon EC2**     | Server running ASP.NET Core 8.0 application            |
| **Amazon RDS**     | AWS-managed PostgreSQL database                        |
| **Amazon S3**      | Storage for flashcard images uploaded by users         |
| **Amazon Cognito** | User authentication with JWT Token                     |

---

## Workshop Contents

{{% children showhidden="false" /%}}
