---
title: "Cài đặt môi trường & Deploy ứng dụng"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.5.2. </b>"
---


---

## 1. Cài đặt .NET 8.0 Runtime trên EC2

SSH vào EC2 và chạy các lệnh sau:

```bash
# Cập nhật package list
sudo apt-get update && sudo apt-get upgrade -y

# Thêm Microsoft package repository
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Cài .NET SDK 8.0
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0

# Kiểm tra
dotnet --version
# 8.0.x
```

---

## 2. Cài đặt các công cụ cần thiết

```bash
# Cài Git
sudo apt-get install -y git

# Cài EF Core Tools
dotnet tool install --global dotnet-ef

# Thêm vào PATH
echo 'export PATH="$PATH:$HOME/.dotnet/tools"' >> ~/.bashrc
source ~/.bashrc
```

---

## 3. Clone và cấu hình ứng dụng

```bash
# Clone source code
git clone <đường dẫn repo FlashLearn>
cd WebhocTiengAnhFlashLearn
```

Tạo file cấu hình môi trường:

```bash
cat > appsettings.Production.json << 'EOF'
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=<RDS-Endpoint>;Port=5432;Database=flashlearn;Username=flashlearn_admin;Password=<password>;SSL Mode=Require;Trust Server Certificate=true"
  },
  "AWS": {
    "Region": "ap-southeast-1",
    "BucketName": "<tên S3 bucket>"
  },
  "Cognito": {
    "Authority": "https://cognito-idp.ap-southeast-1.amazonaws.com/<UserPoolId>",
    "ClientId": "<App Client ID>"
  }
}
EOF
```

---

## 4. Chạy EF Core Migration

```bash
export ASPNETCORE_ENVIRONMENT=Production

# Chạy migration để tạo schema
dotnet ef database update

# Kiểm tra migration thành công
echo "Migration hoàn thành!"
```

---

## 5. Build và Publish ứng dụng

```bash
# Publish ở chế độ Release
dotnet publish -c Release -o /var/www/flashlearn

# Kiểm tra
ls /var/www/flashlearn/
```

---

## 6. Cấu hình systemd Service

Để ứng dụng tự động khởi động khi EC2 restart, tạo systemd service:

```bash
sudo nano /etc/systemd/system/flashlearn.service
```

Nội dung file:

```ini
[Unit]
Description=FlashLearn ASP.NET Core Application
After=network.target

[Service]
WorkingDirectory=/var/www/flashlearn
ExecStart=/usr/bin/dotnet /var/www/flashlearn/WebhocTiengAnhFlashLearn.dll
Restart=always
RestartSec=10
KillSignal=SIGINT
SyslogIdentifier=flashlearn
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=ASPNETCORE_URLS=http://+:5000

[Install]
WantedBy=multi-user.target
```

Kích hoạt và khởi động service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable flashlearn
sudo systemctl start flashlearn

# Kiểm tra trạng thái
sudo systemctl status flashlearn
```

Kết quả mong đợi:
```
● flashlearn.service - FlashLearn ASP.NET Core Application
     Loaded: loaded (/etc/systemd/system/flashlearn.service; enabled)
     Active: active (running)
```

---

## 7. Kiểm tra ứng dụng

```bash
# Kiểm tra ứng dụng đang lắng nghe
curl http://localhost:5000

# Xem logs
sudo journalctl -u flashlearn -f
```

Mở trình duyệt và truy cập:
```
http://<EC2-Public-IP>:5000
```



---

## Kết quả

Sau bước này, bạn đã có:
-  .NET 8.0 Runtime đã cài đặt trên EC2
-  Ứng dụng FlashLearn đang chạy tại port 5000
-  systemd service tự động restart khi server khởi động lại
-  Kết nối thành công đến RDS PostgreSQL
