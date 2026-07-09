---
title: "Deploy Backend (AWS EC2)"
date: 2026-07-09
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---


**Amazon EC2 (Elastic Compute Cloud)** is a virtual server service on AWS. We will use EC2 to run the FlashLearn **ASP.NET Core 8.0** application.

## Role of EC2 in the Architecture

```
[Internet] → [EC2 t3.micro - Public Subnet]
                    │
                    ├─► ASP.NET Core 8.0 (port 5000)
                    ├─► SignalR Hubs (Battle mode real-time)
                    ├─► Connect to RDS PostgreSQL (port 5432)
                    └─► Connect to S3 (upload/download images)
```

## Contents

{{% children showhidden="false" /%}}
