---
title: "User Authentication (Amazon Cognito)"
date: 2026-07-09
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---

# User Authentication (Amazon Cognito)

**Amazon Cognito** is a fully managed user authentication service by AWS. Instead of building your own registration/login system (which is prone to security vulnerabilities), Cognito provides:

-  Secure Sign-up / Sign-in
-  Automatic email verification
-  JWT Token issuance (Access Token, ID Token, Refresh Token)
-  Brute-Force attack protection

## Authentication Flow in FlashLearn

```
[User]
    │
    ├─[1]─► POST /register → Cognito creates user, sends verification email
    ├─[2]─► Confirm email with 6-digit code
    ├─[3]─► POST /login → Cognito returns JWT Token
    └─[4]─► Call API with JWT Token in Authorization header
                │
                ▼
            [EC2 ASP.NET Core]
            Validate JWT → Process request
```

## Contents

{{% children showhidden="false" /%}}
