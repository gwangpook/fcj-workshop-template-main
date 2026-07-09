---
title: "Install Environment & Deploy Application"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.5.2. </b>"
---


---

## 1. Install .NET 8.0 Runtime on EC2

SSH into EC2 and run the following commands:

```bash
# Update package list
sudo apt-get update && sudo apt-get upgrade -y

# Add Microsoft package repository
wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
rm packages-microsoft-prod.deb

# Install .NET SDK 8.0
sudo apt-get update
sudo apt-get install -y dotnet-sdk-8.0

# Verify
dotnet --version
# 8.0.x
```

---

## 2. Install Required Tools

```bash
# Install Git
sudo apt-get install -y git

# Install EF Core Tools
dotnet tool install --global dotnet-ef

# Add to PATH
echo 'export PATH="$PATH:$HOME/.dotnet/tools"' >> ~/.bashrc
source ~/.bashrc
```

---

## 3. Clone and Configure the Application

```bash
# Clone source code
git clone <FlashLearn repository URL>
cd WebhocTiengAnhFlashLearn
```

Create the environment configuration file:

```bash
cat > appsettings.Production.json << 'EOF'
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=<RDS-Endpoint>;Port=5432;Database=flashlearn;Username=flashlearn_admin;Password=<password>;SSL Mode=Require;Trust Server Certificate=true"
  },
  "AWS": {
    "Region": "ap-southeast-1",
    "BucketName": "<your S3 bucket name>"
  },
  "Cognito": {
    "Authority": "https://cognito-idp.ap-southeast-1.amazonaws.com/<UserPoolId>",
    "ClientId": "<App Client ID>"
  }
}
EOF
```

---

## 4. Run EF Core Migration

```bash
export ASPNETCORE_ENVIRONMENT=Production

# Run migration to create schema
dotnet ef database update

echo "Migration completed!"
```

---

## 5. Build and Publish the Application

```bash
# Publish in Release mode
dotnet publish -c Release -o /var/www/flashlearn

# Verify
ls /var/www/flashlearn/
```

---

## 6. Configure systemd Service

To automatically start the application when EC2 restarts, create a systemd service:

```bash
sudo nano /etc/systemd/system/flashlearn.service
```

File contents:

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

Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable flashlearn
sudo systemctl start flashlearn

# Check status
sudo systemctl status flashlearn
```

Expected result:
```
● flashlearn.service - FlashLearn ASP.NET Core Application
     Loaded: loaded (/etc/systemd/system/flashlearn.service; enabled)
     Active: active (running)
```

---

## 7. Verify the Application

```bash
# Check the application is listening
curl http://localhost:5000

# View logs
sudo journalctl -u flashlearn -f
```

Open a browser and navigate to:
```
http://<EC2-Public-IP>:5000
```



---

## Result

After this step, you will have:
-  .NET 8.0 Runtime installed on EC2
-  FlashLearn application running on port 5000
-  systemd service auto-restarts when the server reboots
-  Successfully connected to RDS PostgreSQL
