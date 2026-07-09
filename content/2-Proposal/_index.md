---
title: "Project Proposal"
date: 2026-04-14
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# FLASHLEARN PLATFORM

## FlashLearn: An English Learning Platform on AWS Cloud

### 1. Executive Summary

FlashLearn is an online English learning platform designed to help learners improve vocabulary retention through Flashcards, Quizzes, and community-based learning activities. In addition to its core learning features, the platform integrates text-to-speech technology to support English pronunciation practice, providing a more engaging and interactive learning experience.

To ensure reliable accessibility, secure data management, and seamless scalability, FlashLearn is deployed on Amazon Web Services (AWS). The solution leverages services such as Amazon EC2, Amazon RDS PostgreSQL Multi-AZ, Amazon S3, Amazon CloudFront, Application Load Balancer, and various Serverless services to achieve high performance, high availability, and enterprise-grade operational standards.

---

## 2. Problem Statement

### Current Challenges

Modern online learning platforms are required to store large volumes of multimedia content, educational resources, and user data. Deploying the entire application on a single traditional server can lead to performance degradation as the number of concurrent users increases, while also introducing significant risks related to security, availability, and system reliability.

Furthermore, storing static resources directly on the backend server increases processing overhead, resulting in slower response times and a less efficient user experience.

### Proposed Solution

FlashLearn adopts a multi-tier AWS Cloud architecture that clearly separates business logic, data storage, and content delivery.

Flashcard images and other static assets are stored in Amazon S3 and distributed globally through Amazon CloudFront to reduce latency and improve content delivery speed. User requests are first processed by an Application Load Balancer before being routed to Amazon EC2 instances deployed within Private Subnets, thereby strengthening the application's security posture. Learning data and user information are stored in Amazon RDS PostgreSQL with a Multi-AZ deployment to ensure high availability and fault tolerance.

In addition, Amazon Polly is integrated to provide text-to-speech functionality for pronunciation practice. AWS EventBridge works together with AWS Lambda and Amazon SES to automate background processes such as sending reminder emails and user notifications.

### Benefits and Business Value

For learners, FlashLearn provides a faster, more stable, and more convenient learning experience by delivering content with low latency, supporting pronunciation practice, and ensuring secure data storage.

From a technical perspective, the AWS-based architecture improves scalability, strengthens security, increases system availability, and reduces infrastructure management overhead by utilizing fully managed cloud services.

---

## 3. Solution Architecture

FlashLearn is deployed within an Amazon VPC spanning two Availability Zones to provide high availability and fault tolerance in the event of infrastructure failures.

<p align="center">
  <img src="/images/2-Proposal/img4.png" width="700">
</p>

### AWS Services Used

- Amazon Route 53
- AWS WAF
- Amazon CloudFront
- Amazon S3
- Application Load Balancer
- Amazon EC2
- Amazon RDS PostgreSQL Multi-AZ
- NAT Gateway
- Amazon EventBridge
- AWS Lambda
- Amazon SES
- Amazon Polly

### Architecture Components

- **Edge Layer:** Amazon Route 53 and AWS WAF receive incoming traffic and protect the application against common web threats.
- **Content Delivery Layer:** Amazon CloudFront distributes Flashcard images and other static assets stored in Amazon S3.
- **Application Layer:** Application Load Balancer distributes incoming requests across Amazon EC2 instances.
- **Business Layer:** Amazon EC2 processes application logic and communicates with the database.
- **Database Layer:** Amazon RDS PostgreSQL Multi-AZ stores learning content and user information while providing high availability.
- **Automation Layer:** Amazon EventBridge triggers AWS Lambda functions to send emails through Amazon SES.
- **AI Service Layer:** Amazon Polly provides Text-to-Speech functionality for English pronunciation practice.

---

## 4. Technical Implementation

### Implementation Phases

The project is implemented in five major phases:

1. Build the network infrastructure, including Amazon VPC, Public Subnets, Private Subnets, Amazon Route 53, and AWS WAF.
2. Deploy the Application Load Balancer, NAT Gateway, and Amazon EC2 instances.
3. Configure Amazon RDS PostgreSQL Multi-AZ, Amazon S3, and Amazon CloudFront.
4. Integrate Serverless services, including Amazon EventBridge, AWS Lambda, Amazon SES, and Amazon Polly.
5. Perform system testing, optimize application performance, and finalize the cloud infrastructure.

### Technical Requirements

- Amazon EC2 ARM64
- Amazon RDS PostgreSQL Multi-AZ
- Amazon S3
- Amazon CloudFront
- Application Load Balancer
- NAT Gateway
- AWS Lambda
- Amazon EventBridge
- Amazon SES
- Amazon Polly
- AWS WAF
- Amazon Route 53

---

## 5. Implementation Roadmap

- **Week 1:** Configure Amazon VPC, Amazon Route 53, AWS WAF, and Internet Gateway.
- **Week 2:** Deploy Amazon EC2, NAT Gateway, and Application Load Balancer.
- **Week 3:** Configure Amazon RDS, Amazon S3, and Amazon CloudFront.
- **Week 4:** Integrate Amazon Polly, Amazon EventBridge, AWS Lambda, and Amazon SES.
- **Week 5:** Conduct system testing, optimize performance, and complete the deployment.

---

## 6. Budget Estimation

The estimated deployment cost can be calculated using the **AWS Pricing Calculator** or referenced from the accompanying cost estimation document.

### Infrastructure Cost

- NAT Gateway (2): approximately **USD 65.70/month**
- Application Load Balancer: approximately **USD 16.42/month**
- Amazon RDS PostgreSQL Multi-AZ: approximately **USD 46.40/month**
- Amazon EC2 (2 instances): approximately **USD 12.26/month**
- Amazon S3, Amazon CloudFront, Amazon Route 53, and AWS WAF: approximately **USD 10.00/month**
- Amazon Polly, AWS Lambda, Amazon EventBridge, and Amazon SES: approximately **USD 2.00/month**

**Estimated Total Cost:** approximately **USD 152.78 per month**

---

## 7. Risk Assessment

### Key Risks

- High operating cost associated with NAT Gateway.
- Application Load Balancer may fail to communicate with Amazon EC2 due to incorrect Security Group configuration.
- Improper CloudFront cache behavior for dynamic API requests.

### Mitigation Strategies

- Use a single NAT Gateway in the development environment to reduce infrastructure costs.
- Carefully validate Security Group rules between the Application Load Balancer and Amazon EC2 instances.
- Configure separate CloudFront behaviors for API endpoints and static assets to prevent incorrect caching.

### Contingency Plan

- Adjust the infrastructure architecture according to deployment environments to optimize operational costs.
- Restore infrastructure rapidly using Infrastructure as Code (IaC) in the event of system failures.

---

## 8. Expected Outcomes

### User Benefits

- Improve vocabulary learning through Flashcards and Quizzes.
- Enhance English pronunciation using Text-to-Speech technology.
- Access learning content quickly and reliably.
- Maintain a highly available learning platform capable of supporting increasing numbers of users.

### Technical Benefits

- A secure, multi-layer cloud architecture.
- Scalable infrastructure capable of accommodating future growth.
- Automated background operations powered by Serverless services.
- A reusable cloud architecture suitable for future enterprise-scale AWS projects.
