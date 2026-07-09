# 🖼️ HƯỚNG DẪN ĐẶT ẢNH TRONG HUGO

## ❌ LỖI THƯỜNG GẶP

### Lỗi 1: Thêm `static/` vào đường dẫn
```markdown
❌ SAI:  ![Alt text](/static/images/abc.png)
✅ ĐÚNG: ![Alt text](/images/abc.png)
```

### Lỗi 2: Đặt ảnh sai thư mục
```
❌ SAI:  images/abc.png (thư mục images ở root)
✅ ĐÚNG: static/images/abc.png
```

### Lỗi 3: Đường dẫn không bắt đầu bằng `/`
```markdown
❌ SAI:  ![Alt text](images/abc.png)
✅ ĐÚNG: ![Alt text](/images/abc.png)
```

---

## ✅ QUY TẮC HUGO

### 1. Cấu trúc thư mục

```
project/
├── static/              ← Thư mục gốc cho static files
│   └── images/          ← Đặt ảnh ở đây
│       └── 5-Workshop/
│           └── 5.3-vpc/
│               └── 5.3.1/
│                   └── vpc-dashboard.png
└── content/
    └── 5-Workshop/
        └── 5.3-vpc/
            └── 5.3.1-create-vpc/
                └── _index.vi.md  ← File markdown
```

### 2. Trong file markdown

```markdown
---
title: "Khởi tạo VPC"
---

## Bước 1

Mô tả...

![VPC Dashboard](/images/5-Workshop/5.3-vpc/5.3.1/vpc-dashboard.png)
                   ↑
                   Bỏ "static/", bắt đầu với "/images/"
```

---

## 📁 CẤU TRÚC ẢNH ĐỀ XUẤT

### Cho Workshop FlashLearn:

```
static/images/5-Workshop/
├── 5.3-vpc/
│   ├── 5.3.1/
│   │   ├── vpc-dashboard.png
│   │   ├── create-vpc-form.png
│   │   ├── vpc-created.png
│   │   ├── create-public-subnet.png
│   │   ├── create-private-subnets.png
│   │   └── subnets-overview.png
│   └── 5.3.2/
│       ├── create-igw.png
│       ├── attach-igw.png
│       ├── igw-attached.png
│       ├── create-public-rt.png
│       ├── edit-public-routes.png
│       ├── associate-public-subnet.png
│       └── route-tables-overview.png
├── 5.4-rds/
│   ├── 5.4.1/
│   │   ├── rds-dashboard.png
│   │   ├── choose-postgresql.png
│   │   ├── select-template.png
│   │   ├── db-settings.png
│   │   ├── instance-config.png
│   │   ├── storage-config.png
│   │   ├── connectivity.png
│   │   ├── additional-config.png
│   │   ├── rds-creating.png
│   │   └── rds-available.png
│   └── 5.4.2/
│       ├── rds-security-group.png
│       └── rds-endpoint.png
├── 5.5-ec2/
│   ├── 5.5.1/
│   │   ├── ec2-dashboard.png
│   │   ├── name-and-ami.png
│   │   ├── instance-type.png
│   │   ├── keypair.png
│   │   ├── network-settings.png
│   │   ├── security-group.png
│   │   ├── storage.png
│   │   ├── iam-role.png
│   │   └── ec2-running.png
│   └── 5.5.2/
│       ├── connect-instance.png
│       └── app-deployed.png
├── 5.6-s3/
│   ├── 5.6.1/
│   │   ├── s3-dashboard.png
│   │   ├── create-bucket.png
│   │   └── bucket-created.png
│   └── 5.6.2/
│       ├── bucket-policy.png
│       └── cors-config.png
└── 5.7-cognito/
    ├── 5.7.1/
    │   ├── cognito-dashboard.png
    │   ├── signin-experience.png
    │   ├── security-requirements.png
    │   ├── signup-experience.png
    │   ├── message-delivery.png
    │   ├── user-pool-name.png
    │   └── user-pool-created.png
    └── 5.7.2/
        ├── app-client-settings.png
        └── create-test-user.png
```

---

## 📝 TEMPLATE MARKDOWN

### Cho mỗi section trong workshop:

```markdown
---
title: "Tên section"
date: 2026-07-09
weight: 1
chapter: false
pre: "<b>5.x.x. </b>"
---

## 1. Bước đầu tiên

Mô tả các bước...

1. Đăng nhập AWS Console
2. Chọn dịch vụ VPC

![Tên mô tả ảnh](/images/5-Workshop/5.x-service/5.x.x/ten-anh.png)

3. Tiếp tục các bước...

![Mô tả bước tiếp theo](/images/5-Workshop/5.x-service/5.x.x/ten-anh-khac.png)

---

## 2. Bước tiếp theo

...
```

---

## 🔧 TẠO THƯ MỤC NHANH

### PowerShell (Windows):

```powershell
# Tạo tất cả thư mục cho workshop
$sections = @(
    "5.3-vpc/5.3.1",
    "5.3-vpc/5.3.2",
    "5.4-rds/5.4.1",
    "5.4-rds/5.4.2",
    "5.5-ec2/5.5.1",
    "5.5-ec2/5.5.2",
    "5.6-s3/5.6.1",
    "5.6-s3/5.6.2",
    "5.7-cognito/5.7.1",
    "5.7-cognito/5.7.2"
)

foreach ($section in $sections) {
    New-Item -ItemType Directory -Force -Path "static/images/5-Workshop/$section"
    Write-Host "✓ Created static/images/5-Workshop/$section"
}
```

### Bash (Mac/Linux):

```bash
# Tạo tất cả thư mục cho workshop
sections=(
  "5.3-vpc/5.3.1"
  "5.3-vpc/5.3.2"
  "5.4-rds/5.4.1"
  "5.4-rds/5.4.2"
  "5.5-ec2/5.5.1"
  "5.5-ec2/5.5.2"
  "5.6-s3/5.6.1"
  "5.6-s3/5.6.2"
  "5.7-cognito/5.7.1"
  "5.7-cognito/5.7.2"
)

for section in "${sections[@]}"; do
  mkdir -p "static/images/5-Workshop/$section"
  echo "✓ Created static/images/5-Workshop/$section"
done
```

---

## ✅ CHECKLIST SAU KHI CHỤP ẢNH

Sau khi chụp ảnh từ AWS Console:

1. **Lưu ảnh vào đúng thư mục:**
   ```
   static/images/5-Workshop/[section]/[subsection]/[ten-anh].png
   ```

2. **Đặt tên file:**
   - ✅ Dùng: `vpc-dashboard.png`, `create-vpc-form.png`
   - ❌ Tránh: `Screenshot 2024-01-01.png`, `image1.png`

3. **Thêm vào markdown:**
   ```markdown
   ![Mô tả ngắn gọn](/images/5-Workshop/5.3-vpc/5.3.1/vpc-dashboard.png)
   ```

4. **Kiểm tra:**
   - Chạy `hugo server` local
   - Mở http://localhost:1313
   - Xem ảnh có hiển thị không

---

## 🚨 TROUBLESHOOTING

### Ảnh không hiển thị?

1. **Kiểm tra đường dẫn file:**
   ```powershell
   # Windows
   Test-Path "static/images/5-Workshop/5.3-vpc/5.3.1/vpc-dashboard.png"
   ```

2. **Kiểm tra đường dẫn trong markdown:**
   - Phải bắt đầu với `/images/` (có dấu `/` đầu tiên)
   - Không có `static/` trong đường dẫn
   - Chính xác tên file và extension

3. **Restart Hugo server:**
   ```bash
   Ctrl + C  # Dừng server
   hugo server  # Khởi động lại
   ```

4. **Xóa cache:**
   ```bash
   rm -rf public/  # Mac/Linux
   Remove-Item -Recurse -Force public/  # Windows
   hugo server
   ```

---

## 🎯 QUY ƯỚC ĐẶT TÊN ẢNH

### Format chuẩn:
```
[action]-[object]-[detail].png
```

### Ví dụ:
```
✅ create-vpc-form.png
✅ edit-security-group-rules.png
✅ rds-instance-running.png
✅ ec2-connect-terminal.png

❌ Screenshot 2024-01-01.png
❌ image1.png
❌ aws-1.png
```

### Quy tắc:
- Dùng lowercase
- Dấu gạch ngang `-` thay vì khoảng trắng
- Tên rõ ràng, dễ hiểu
- Extension: `.png` (ưu tiên) hoặc `.jpg`

---

## 📊 CÔNG CỤ HỖ TRỢ

### 1. Bulk Rename Utility (Windows)
https://www.bulkrenameutility.co.uk/

### 2. VS Code Extension
- **Hugo Helper**: Autocomplete đường dẫn ảnh
- **Markdown All in One**: Preview markdown với ảnh

### 3. Script đổi tên hàng loạt:

```powershell
# Windows PowerShell
# Đổi tên tất cả ảnh trong thư mục hiện tại
Get-ChildItem -Filter "Screenshot*.png" | ForEach-Object {
    $counter = 1
    Rename-Item $_.FullName -NewName "image-$counter.png"
    $counter++
}
```

---

## 💡 BEST PRACTICES

1. **Chụp ảnh chất lượng cao:**
   - Độ phân giải: 1920x1080 trở lên
   - Format: PNG (cho screenshots)
   - Crop bỏ phần không cần thiết

2. **Tối ưu kích thước:**
   - Dùng TinyPNG.com để compress
   - Target: < 500KB mỗi ảnh

3. **Alt text có ý nghĩa:**
   ```markdown
   ✅ ![VPC Dashboard showing create VPC button](/images/...)
   ❌ ![Image](/images/...)
   ```

4. **Tổ chức theo sections:**
   - 1 section = 1 thư mục
   - Đặt tên thư mục giống tên section trong content/

---

## ✅ TÓM TẮT

```
┌─────────────────────────────────────────────────────────┐
│  File thực tế:                                          │
│  static/images/5-Workshop/5.3-vpc/5.3.1/vpc-dash.png    │
│                                                          │
│  Trong markdown (_index.vi.md):                         │
│  ![VPC Dashboard](/images/5-Workshop/5.3-vpc/5.3.1/vpc-dash.png)
│                     ↑                                    │
│                     Bỏ "static/", bắt đầu với "/images" │
└─────────────────────────────────────────────────────────┘
```

**Nhớ:** Hugo tự động serve mọi thứ trong `static/` từ root URL!
