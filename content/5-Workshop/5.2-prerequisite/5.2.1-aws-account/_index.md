---
title: "Create an AWS Account"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.2.1. </b>"
---


To complete this workshop, you need an AWS account. If you don't have one yet, follow the steps below.

## 1. Register an AWS Account

1. Go to **[https://aws.amazon.com](https://aws.amazon.com)** and click **Create an AWS Account**


2. Fill in the registration details:
   - **Email address**: Your email address
   - **AWS account name**: Account name (e.g., `flashlearn-workshop`)

3. Select the **Free Tier** plan when asked about account type

4. Enter credit card information (AWS will charge $1 for verification, which is immediately refunded)

5. Verify your identity via phone number

6. Select **Basic support plan** (free)

---

## 2. Enable Security (MFA)

After registering, **enabling MFA** on the root account is mandatory to protect your account:

1. Log in to the **AWS Console**
2. Click on your account name (top right) → **Security credentials**
3. Under **Multi-factor authentication (MFA)** → **Assign MFA device**
4. Select **Authenticator app** and follow the instructions


---

## 3. Create an IAM User for the Workshop

>  **Do not** use the root account for day-to-day tasks. Create an IAM User with appropriate permissions.

1. Search for **IAM** in the search bar → **Users** → **Create user**

2. Configure the user:
   - **User name**: `flashlearn-admin`
   - Check **Provide user access to the AWS Management Console**
   - Select **I want to create an IAM user**
   - **Console password**: Set a strong password

3. Attach permissions → **Attach policies directly** → search and select **AdministratorAccess**


4. Click **Create user** and save the login credentials

---

## 4. Set Up AWS Budget Alert

To avoid unexpected charges, configure a budget alert:

1. Search for **Billing and Cost Management** → **Budgets** → **Create budget**

2. Select **Use a template** → **Zero spend budget**

3. Enter your alert email → **Create budget**

>  **Zero spend budget** sends an email as soon as any service incurs a charge (even $0.01), helping you detect issues promptly.

![Budget Setup](/images/5-Workshop/5.2-Prerequisite/budget.png)

---

## Result

After this step, you will have:
-  An AWS account with MFA enabled
-  IAM User `flashlearn-admin` with Administrator permissions
-  A $0 budget alert to protect against unexpected costs
