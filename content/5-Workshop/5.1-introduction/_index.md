---
title: "Introduction"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---


## What is FlashLearn?

**FlashLearn** is a smart English learning web application built with **ASP.NET Core 8.0**, allowing users to:

-  **Create Flashcard Decks** with English — Vietnamese vocabulary
-  **Practice** using a Spaced Repetition algorithm
-  **Attach images** to illustrate each flashcard
-  **Battle** other players in real-time vocabulary challenges (SignalR)
-  **Track progress** and daily learning streaks

The application currently runs with **SQLite** on a local environment. This workshop will guide you through migrating the entire system to **AWS Cloud** with a **PostgreSQL** database fully managed by AWS.

---

## System Architecture

The system is designed following a **3-Tier Architecture** deployed within a **Virtual Private Cloud (VPC)** with clearly separated subnets:

![FlashLearn AWS Architecture](/images/5-Workshop/5.1-Workshop-overview/aws_whiteboard.png)

### Core Components

| Layer              | Component                | AWS Service                            |
| ------------------ | ------------------------ | -------------------------------------- |
| **Presentation**   | Web UI, SignalR          | EC2 (Public Subnet)                    |
| **Authentication** | Register / Login         | Amazon Cognito                         |
| **Business Logic** | ASP.NET Core Controllers | EC2 (Public Subnet)                    |
| **Database**       | User data, flashcards    | Amazon RDS PostgreSQL (Private Subnet) |
| **Storage**        | Flashcard images         | Amazon S3                              |

### Request Flow

```
User
    │
    ├─[1]─► Amazon Cognito ──► Authenticate, return JWT Token
    │
    ├─[2]─► EC2 (ASP.NET Core) ──► Handle request with JWT
    │              │
    │              ├─► RDS PostgreSQL ──► Read/write flashcard data
    │              │
    │              └─► Amazon S3 ──► Upload/Download images
    │
    └─[3]─► SignalR Hub ──► Battle mode real-time
```

---

## Workshop Objectives

After completing this workshop, you will be able to:

 Set up a Virtual Private Cloud (VPC) with security layers  
 Deploy an ASP.NET Core 8.0 application on EC2  
 Migrate the database from SQLite to RDS PostgreSQL  
 Store user media on Amazon S3  
 Integrate user authentication with Amazon Cognito  
 Clean up resources to avoid unexpected charges  

---

## Estimated Duration

| Step      | Content              | Estimated Time |
| --------- | -------------------- | -------------- |
| 5.2       | Environment Setup    | ~30 minutes    |
| 5.3       | VPC Setup            | ~20 minutes    |
| 5.4       | RDS Deployment       | ~25 minutes    |
| 5.5       | EC2 + App Deployment | ~45 minutes    |
| 5.6       | S3 Configuration     | ~20 minutes    |
| 5.7       | Cognito Integration  | ~30 minutes    |
| 5.8       | Cleanup              | ~15 minutes    |
| **Total** |                      | **~3 hours**   |

---

## Estimated Cost


| Service         | Configuration         | Monthly Cost   |
| --------------- | --------------------- | -------------- |
| EC2 t3.micro    | 1 instance, On-Demand | ~$8.47         |
| RDS db.t3.micro | PostgreSQL, Single-AZ | ~$15.33        |
| Amazon S3       | 5GB standard          | ~$0.12         |
| Amazon Cognito  | < 50,000 MAU          | $0.00          |
| **Total**       |                       | **~$24/month** |

