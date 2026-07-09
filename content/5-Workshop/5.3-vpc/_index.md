---
title: "Set Up Virtual Network (AWS VPC)"
date: 2026-07-09
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---


**Amazon Virtual Private Cloud (VPC)** allows you to create an isolated virtual network on AWS Cloud, completely separated from other resources.

## Network Architecture to Build

```
VPC: 10.0.0.0/16 (FlashLearn VPC)
│
├── Public Subnet: 10.0.1.0/24 (ap-southeast-1a)
│   ├── Internet Gateway
│   └── EC2 (ASP.NET Core App)
│
└── Private Subnet: 10.0.2.0/24 (ap-southeast-1a)
    └── RDS PostgreSQL
```

### Why Separate Public and Private Subnets?

|                     | Public Subnet    | Private Subnet |
| ------------------- | ---------------- | -------------- |
| **Internet Access** | Yes              | No             |
| **Place here**      | EC2 (Web Server) | RDS (Database) |
| **Security**        | Medium           | High           |

>  **Best Practice**: The database should not be exposed directly to the Internet. Placing RDS in a Private Subnet provides complete isolation — only EC2 can connect to it.

## Contents

{{% children showhidden="false" /%}}
