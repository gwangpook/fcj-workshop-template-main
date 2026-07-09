---
title: "Blog 1"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# ENHANCING SECURITY WITH SESSION POLICIES IN AMAZON EKS POD IDENTITY

Amazon EKS Pod Identity enables Pods to securely access AWS services without relying on long-term access keys. Recently, Amazon EKS introduced **Session Policies**, allowing inline IAM policies to be applied whenever a Pod assumes an IAM Role. This feature provides a flexible way to restrict each Pod's permissions while continuing to use existing IAM Roles, simplifying permission management across large-scale Kubernetes clusters.

Key highlights include:

* **Session Policies are applied when a Pod assumes an IAM Role:** These policies are enforced during the AssumeRole process to restrict a Pod's access to AWS resources without requiring additional IAM Roles.
* **Effective permissions are the intersection of the IAM Role and the Session Policy:** A Pod can perform only the actions that are permitted by both the IAM Role and the Session Policy. Session Policies cannot grant new permissions; they can only further restrict existing ones.
* **Simplified IAM management:** Multiple Pods can share the same IAM Role, while Session Policies define different permission scopes for individual workloads, reducing the number of IAM Roles that need to be maintained.
* **Improved security through the principle of least privilege:** Each Pod receives only the permissions required for its specific workload, minimizing security risks if the Pod or application is compromised.
* **Flexible deployment and management:** Session Policies are configured through Pod Identity Associations and can be managed using the AWS Management Console, AWS CLI, or AWS SDK, making them easy to integrate into deployment and automation workflows.
* **Well suited for large-scale Amazon EKS environments:** Reducing the number of IAM Roles simplifies administration, helps avoid IAM quota limitations, and improves scalability as the number of Pods and applications grows.

Session Policies are particularly valuable in microservices architectures, where multiple Pods share the same IAM Role but require different levels of access to AWS resources. For example, one Pod may only need read access to a specific Amazon S3 bucket, while another may also require access to Amazon DynamoDB or Amazon SQS. By combining Amazon EKS Pod Identity with Session Policies, organizations can centrally manage IAM Roles while ensuring that each Pod has only the permissions necessary to perform its intended tasks.

## Illustration

<p align="center">
  <img src="/images/3-BlogsPosted/Blog1/img1.png" width="700">
</p>

<p align="center">
  <em>Figure 1. Amazon EKS Pod Identity architecture with Session Policies.</em>
</p>

## Original Post

[🔗 View the original Facebook post](https://www.facebook.com/share/p/1Bf61McbW7/)
