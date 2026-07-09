---
title: "Create Cognito User Pool"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.7.1. </b>"
---


A **User Pool** is where Cognito stores user information (email, hashed password, verification status, etc.).

---

## 1. Create User Pool

1. Search for **Cognito** in AWS Console → **Create user pool**

2. **Step 1 — Configure sign-in experience**:
   - **Authentication providers**: Cognito user pool
   - **Cognito user pool sign-in options**:  Email

3. **Step 2 — Configure security requirements**:
   - **Password policy**: Minimum 8 characters, at least 1 uppercase, 1 number
   - **Multi-factor authentication**: No MFA (simplified for workshop)
   - **User account recovery**:  Email only

4. **Step 3 — Configure sign-up experience**:
   - **Self-registration**:  Enable
   - **Cognito-assisted verification**:  Send email message
   - **Required attributes**: Add `name` (user display name)

5. **Step 4 — Configure message delivery**:
   - **Email provider**: Send email with Cognito (uses free Cognito email)

6. **Step 5 — Integrate your app**:

| Field               | Value                          |
| ------------------- | ------------------------------ |
| **User pool name**  | `flashlearn-user-pool`         |
| **App client name** | `flashlearn-web-client`        |
| **Client secret**   | Don't generate a client secret |

   Add **Callback URL**: `http://<EC2-IP>:5000/signin-oidc`  
   Add **Sign out URL**: `http://<EC2-IP>:5000/signout-callback-oidc`

7. **Step 6 — Review and create** → **Create user pool**

---

## 2. Save Important Information

After creation, record the following:

1. **User Pool ID**: Go to the newly created User Pool → **Overview** tab
   - Example: `ap-southeast-1_AbCdEfGh`

2. **App Client ID**: **App integration** tab → **App clients**
   - Example: `1a2b3c4d5e6f7g8h9i0j`

3. **Cognito Domain**: **App integration** tab → **Domain**
   - Click **Create Cognito domain**
   - Domain prefix: `flashlearn-auth`
   - Full URL: `https://flashlearn-auth.auth.ap-southeast-1.amazoncognito.com`

---

## 3. Configure Hosted UI (Optional)

Cognito provides a ready-made **Hosted UI** for sign-in/sign-up without building your own forms:

1. **App integration** tab → **App clients** → Select `flashlearn-web-client`
2. Under **Hosted UI** → **Edit**
3. Add OAuth 2.0 grant types:
   -  Authorization code grant
4. **OpenID Connect scopes**:  OpenID,  Email,  Profile
5. **Save changes**

Test the Hosted UI:
```
https://flashlearn-auth.auth.ap-southeast-1.amazoncognito.com/login?
  client_id=<App-Client-ID>&
  response_type=code&
  scope=openid+email+profile&
  redirect_uri=http://<EC2-IP>:5000/signin-oidc
```

---

## Result

After this step, you will have:
-  User Pool `flashlearn-user-pool` created
-  App Client `flashlearn-web-client` (no client secret)
-  Cognito Domain configured
-  User Pool ID and App Client ID saved
