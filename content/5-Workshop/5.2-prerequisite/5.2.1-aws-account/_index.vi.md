---
title: "Tạo tài khoản AWS"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.2.1. </b>"
---


Để thực hiện workshop này, bạn cần có một tài khoản AWS. Nếu chưa có, hãy làm theo hướng dẫn bên dưới.

## 1. Đăng ký tài khoản AWS

1. Truy cập **[https://aws.amazon.com](https://aws.amazon.com)** và chọn **Create an AWS Account**


2. Điền thông tin đăng ký:
   - **Email address**: Địa chỉ email của bạn
   - **AWS account name**: Tên tài khoản (ví dụ: `flashlearn-workshop`)

3. Chọn gói **Free Tier** khi được hỏi về loại tài khoản

4. Điền thông tin thẻ tín dụng (AWS sẽ tính phí $1 để xác minh, sẽ hoàn lại ngay sau đó)

5. Xác minh danh tính qua điện thoại

6. Chọn **Basic support plan** (miễn phí)

---

## 2. Bật tính năng bảo mật (MFA)

Sau khi đăng ký, **bắt buộc bật MFA** cho tài khoản root để bảo vệ tài khoản:

1. Đăng nhập vào **AWS Console**
2. Nhấn vào tên tài khoản (góc trên phải) → **Security credentials**
3. Tại mục **Multi-factor authentication (MFA)** → **Assign MFA device**
4. Chọn **Authenticator app** và làm theo hướng dẫn


---

## 3. Tạo IAM User cho workshop

>  **Không nên** dùng tài khoản root cho công việc hằng ngày. Hãy tạo IAM User với quyền hạn phù hợp.

1. Tìm kiếm **IAM** trong thanh tìm kiếm → **Users** → **Create user**

2. Cấu hình user:
   - **User name**: `flashlearn-admin`
   - Tích chọn **Provide user access to the AWS Management Console**
   - Chọn **I want to create an IAM user**
   - **Console password**: Tự đặt mật khẩu

3. Gán quyền → **Attach policies directly** → tìm và chọn **AdministratorAccess**


4. Nhấn **Create user** và lưu lại thông tin đăng nhập

---

## 4. Thiết lập AWS Budget cảnh báo chi phí

Để tránh phát sinh chi phí ngoài ý muốn, hãy cài đặt cảnh báo ngân sách:

1. Tìm kiếm **Billing and Cost Management** → **Budgets** → **Create budget**

2. Chọn **Use a template** → **Zero spend budget**

3. Điền email nhận cảnh báo → **Create budget**

>  **Zero spend budget** sẽ gửi email ngay khi bất kỳ dịch vụ nào phát sinh chi phí (kể cả $0.01), giúp bạn phát hiện kịp thời.

![Cấu hình Budget](/images/5-Workshop/5.2-Prerequisite/budget.png)

---

## Kết quả

Sau bước này, bạn đã có:
-  Tài khoản AWS với MFA được bật
-  IAM User `flashlearn-admin` với quyền Administrator
-  Cảnh báo ngân sách $0 để bảo vệ chi phí
