---
title: "Deploy Database (AWS RDS)"
date: 2026-07-09
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---


**Amazon RDS (Relational Database Service)** is a fully managed relational database service — with automatic backups, security patching, and monitoring.

## Why Migrate from SQLite to RDS?

|                  | SQLite (Local)          | Amazon RDS (PostgreSQL)    |
| ---------------- | ----------------------- | -------------------------- |
| **Location**     | File on the server      | Separate managed service   |
| **Backup**       | Manual                  | Automatic, 7-day retention |
| **Scaling**      | Difficult               | Easy instance upgrade      |
| **Availability** | Single point of failure | Multi-AZ optional          |
| **Management**   | Self-managed            | Fully managed by AWS       |

## Contents

{{% children showhidden="false" /%}}
