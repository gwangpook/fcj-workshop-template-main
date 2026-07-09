---
title: "Xác thực người dùng (Amazon Cognito)"
date: 2026-07-09
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---


**Amazon Cognito** là dịch vụ xác thực người dùng được AWS quản lý hoàn toàn. Thay vì tự xây dựng hệ thống đăng ký/đăng nhập (dễ có lỗ hổng bảo mật), Cognito cung cấp:

-  Đăng ký / Đăng nhập an toàn
-  Xác minh email tự động
-  Cấp phát JWT Token (Access Token, ID Token, Refresh Token)
-  Bảo vệ chống tấn công Brute-Force

## Luồng xác thực trong FlashLearn

```
[Người dùng]
    │
    ├─[1]─► POST /register → Cognito tạo user, gửi email xác nhận
    ├─[2]─► Xác nhận email qua mã 6 số
    ├─[3]─► POST /login → Cognito trả về JWT Token
    └─[4]─► Gọi API với JWT Token trong Authorization header
                │
                ▼
            [EC2 ASP.NET Core]
            Xác thực JWT → Xử lý request
```

## Nội dung

{{% children showhidden="false" /%}}
