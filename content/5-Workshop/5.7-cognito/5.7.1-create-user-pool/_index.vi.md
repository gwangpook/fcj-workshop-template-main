---
title: "Khởi tạo Cognito User Pool"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.7.1. </b>"
---


**User Pool** là nơi Cognito lưu trữ thông tin người dùng (email, mật khẩu đã hash, trạng thái xác minh...).

---

## 1. Tạo User Pool

1. Tìm kiếm **Cognito** trong AWS Console → **Create user pool**

2. **Step 1 — Configure sign-in experience**:
   - **Authentication providers**: Cognito user pool
   - **Cognito user pool sign-in options**:  Email

3. **Step 2 — Configure security requirements**:
   - **Password policy**: Minimum 8 characters, ít nhất 1 chữ hoa, 1 chữ số
   - **Multi-factor authentication**: No MFA (đơn giản cho workshop)
   - **User account recovery**:  Email only

4. **Step 3 — Configure sign-up experience**:
   - **Self-registration**:  Enable
   - **Cognito-assisted verification**:  Send email message
   - **Required attributes**: Thêm `name` (tên hiển thị người dùng)

5. **Step 4 — Configure message delivery**:
   - **Email provider**: Send email with Cognito (dùng free Cognito email)

6. **Step 5 — Integrate your app**:

| Trường              | Giá trị                        |
| ------------------- | ------------------------------ |
| **User pool name**  | `flashlearn-user-pool`         |
| **App client name** | `flashlearn-web-client`        |
| **Client secret**   | Don't generate a client secret |

   Thêm **Callback URL**: `http://<EC2-IP>:5000/signin-oidc`  
   Thêm **Sign out URL**: `http://<EC2-IP>:5000/signout-callback-oidc`

7. **Step 6 — Review and create** → **Create user pool**

---

## 2. Lưu lại thông tin quan trọng

Sau khi tạo xong, ghi lại các thông tin sau:

1. **User Pool ID**: Vào User Pool vừa tạo → Tab **Overview**
   - Ví dụ: `ap-southeast-1_AbCdEfGh`

2. **App Client ID**: Tab **App integration** → **App clients**
   - Ví dụ: `1a2b3c4d5e6f7g8h9i0j`

3. **Cognito Domain**: Tab **App integration** → **Domain**
   - Nhấn **Create Cognito domain**
   - Domain prefix: `flashlearn-auth`
   - URL đầy đủ: `https://flashlearn-auth.auth.ap-southeast-1.amazoncognito.com`

---

## 3. Cấu hình Hosted UI (tùy chọn)

Cognito cung cấp **Hosted UI** sẵn có để đăng nhập/đăng ký mà không cần tự xây dựng form:

1. Tab **App integration** → **App clients** → Chọn `flashlearn-web-client`
2. Phần **Hosted UI** → **Edit**
3. Thêm OAuth 2.0 grant types:
   -  Authorization code grant
4. **OpenID Connect scopes**:  OpenID,  Email,  Profile
5. **Save changes**

Kiểm tra Hosted UI:
```
https://flashlearn-auth.auth.ap-southeast-1.amazoncognito.com/login?
  client_id=<App-Client-ID>&
  response_type=code&
  scope=openid+email+profile&
  redirect_uri=http://<EC2-IP>:5000/signin-oidc
```

---

## Kết quả

Sau bước này, bạn đã có:
-  User Pool `flashlearn-user-pool` đã tạo
-  App Client `flashlearn-web-client` (không có secret)
-  Cognito Domain đã thiết lập
-  Đã lưu User Pool ID và App Client ID
