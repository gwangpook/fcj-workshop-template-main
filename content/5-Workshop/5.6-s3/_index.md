---
title: "Media Storage (Amazon S3)"
date: 2026-07-09
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---


**Amazon S3 (Simple Storage Service)** is AWS's object storage service. In FlashLearn, S3 is used to store **illustration images** for flashcards uploaded by users.

## Why Use S3 Instead of Storing Directly on EC2?

|                | Store on EC2                   | Amazon S3                   |
| -------------- | ------------------------------ | --------------------------- |
| **Durability** | Lost if instance is terminated | 99.999999999%               |
| **Cost**       | Consumes EBS storage           | Cheaper (~$0.023/GB)        |
| **Scaling**    | Limited by disk size           | Unlimited                   |
| **CDN**        | Not available                  | Easy CloudFront integration |
| **Backup**     | Manual                         | Automatic with Versioning   |

## Contents

{{% children showhidden="false" /%}}
