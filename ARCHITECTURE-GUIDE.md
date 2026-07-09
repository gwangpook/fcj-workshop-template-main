# 🏗️ Hướng dẫn vẽ kiến trúc AWS FlashLearn chi tiết trong Draw.io

## 📋 Checklist các thành phần cần vẽ

### 1. Internet Layer
- [ ] Icon User (desktop)
- [ ] Icon Mobile User  
- [ ] Label "HTTPS Port 443"

### 2. AWS Cloud Boundary
- [ ] Rectangle lớn bao bọc toàn bộ
- [ ] Label "AWS Cloud - Singapore Region (ap-southeast-1)"
- [ ] Màu: Orange border (#FF9900)

### 3. Amazon Cognito (bên ngoài VPC)
- [ ] Icon Cognito
- [ ] Label chi tiết:
  ```
  Amazon Cognito User Pool
  - Email verification
  - JWT Token (Access, ID, Refresh)
  - Brute-force protection
  ```

### 4. Amazon S3 (bên ngoài VPC)
- [ ] Icon S3 Bucket
- [ ] Label chi tiết:
  ```
  S3 Bucket: flashlearn-images
  - Flashcard images
  - Audio files  
  - Versioning: Enabled
  - Storage Class: Standard
  ```

### 5. VPC Container
- [ ] Rectangle màu xanh lá (#248814)
- [ ] Label "VPC: 10.0.0.0/16 (FlashLearn-VPC)"

### 6. Internet Gateway (trong VPC, trên cùng)
- [ ] Icon Internet Gateway
- [ ] Label "IGW: igw-flashlearn"
- [ ] Connection line từ Internet vào IGW

### 7. Public Subnet (10.0.1.0/24)
- [ ] Rectangle màu xanh lá nhạt (#E9F3E6)
- [ ] Label "Public Subnet: 10.0.1.0/24 | AZ: ap-southeast-1a"

#### 7.1. Route Table (Public)
- [ ] Icon Route Table trong Public Subnet
- [ ] Label chi tiết:
  ```
  Route Table: Public-RT
  ┌─────────────────┬──────────────────┐
  │ Destination     │ Target           │
  ├─────────────────┼──────────────────┤
  │ 10.0.0.0/16     │ local            │
  │ 0.0.0.0/0       │ igw-flashlearn   │
  └─────────────────┴──────────────────┘
  ```

#### 7.2. Security Group EC2
- [ ] Rectangle dotted màu đỏ quanh EC2
- [ ] Label "Security Group: EC2-Web-SG"
- [ ] Text box chi tiết rules:
  ```
  Inbound Rules:
  • HTTP  : 0.0.0.0/0 → Port 80
  • HTTPS : 0.0.0.0/0 → Port 443  
  • SSH   : YOUR_IP/32 → Port 22
  
  Outbound Rules:
  • All Traffic : 0.0.0.0/0
  ```

#### 7.3. EC2 Instance
- [ ] Icon EC2 Instance (màu cam #ED7100)
- [ ] Label rất chi tiết:
  ```
  EC2 Instance
  ┌───────────────────────────────────┐
  │ Type: t3.micro                    │
  │ vCPU: 2 | RAM: 1GB                │
  │ OS: Amazon Linux 2                │
  │ Public IP: Yes                    │
  │ Private IP: 10.0.1.x              │
  │ Storage: 8GB gp3                  │
  ├───────────────────────────────────┤
  │ 📦 Software Stack:                │
  │ • Nginx (reverse proxy)           │
  │ • ASP.NET Core 8.0 (Port 5000)    │
  │ • SignalR Hubs                    │
  │ • .NET 8 Runtime                  │
  └───────────────────────────────────┘
  ```

#### 7.4. IAM Role
- [ ] Icon IAM Role
- [ ] Connection line đứt nét từ IAM Role → EC2
- [ ] Label "Attached"
- [ ] Label IAM Role details:
  ```
  IAM Role: EC2-FlashLearn-Role
  ┌────────────────────────────────────┐
  │ Policies:                          │
  │ ✓ AmazonS3FullAccess               │
  │   - s3:PutObject                   │
  │   - s3:GetObject                   │
  │   - s3:DeleteObject                │
  │ ✓ CloudWatchAgentServerPolicy      │
  │   - logs:CreateLogGroup            │
  │   - logs:PutLogEvents              │
  └────────────────────────────────────┘
  ```

### 8. Private Subnet (10.0.2.0/24)
- [ ] Rectangle màu xanh dương nhạt (#E6F2F8)
- [ ] Label "Private Subnet: 10.0.2.0/24 | AZ: ap-southeast-1a"

#### 8.1. Route Table (Private)
- [ ] Icon Route Table trong Private Subnet
- [ ] Label:
  ```
  Route Table: Private-RT
  ┌─────────────────┬──────────────────┐
  │ Destination     │ Target           │
  ├─────────────────┼──────────────────┤
  │ 10.0.0.0/16     │ local            │
  └─────────────────┴──────────────────┘
  (No Internet access)
  ```

#### 8.2. Security Group RDS
- [ ] Rectangle dotted màu đỏ quanh RDS
- [ ] Label "Security Group: RDS-DB-SG"
- [ ] Text box:
  ```
  Inbound Rules:
  • PostgreSQL: EC2-Web-SG → Port 5432
  
  Outbound Rules:
  • None (stateful response only)
  ```

#### 8.3. RDS PostgreSQL
- [ ] Icon RDS PostgreSQL (màu xanh #3334B9)
- [ ] Label chi tiết:
  ```
  Amazon RDS PostgreSQL
  ┌────────────────────────────────────┐
  │ Instance: db.t3.micro              │
  │ Engine: PostgreSQL 15              │
  │ Storage: 20GB gp3 SSD              │
  │ Multi-AZ: No (Single-AZ)           │
  │ Backup Retention: 7 days           │
  │ Encryption: Yes (AWS KMS)          │
  │ Private IP: 10.0.2.x               │
  ├────────────────────────────────────┤
  │ 📊 Database: flashlearn_db         │
  │ Tables:                            │
  │ • Users (authentication data)      │
  │ • Decks (flashcard collections)    │
  │ • Cards (flashcard content)        │
  │ • Progress (learning analytics)    │
  │ • Battles (game sessions)          │
  └────────────────────────────────────┘
  ```

### 9. Network ACL
- [ ] Icon Network ACL
- [ ] Position: góc VPC
- [ ] Label:
  ```
  Network ACL: Default
  • Inbound: Allow All
  • Outbound: Allow All
  ```

## 🔗 Connections (Mũi tên với labels)

### Connection 1: User → Cognito
```
User ──────────────────────> Cognito
        1️⃣ Authentication
        POST /auth/register
        POST /auth/login
        Protocol: HTTPS
        Port: 443
        
Cognito ──────────────────> User
        Response: JWT Tokens
        • Access Token (1h)
        • ID Token (1h)
        • Refresh Token (30 days)
```

### Connection 2: User → EC2
```
User ──────────────────────> IGW ──────────> EC2
        2️⃣ HTTPS Requests
        Header: Authorization Bearer {JWT}
        Port: 443
        
        Nginx forwards to →  ASP.NET Core :5000
```

### Connection 3: EC2 → RDS
```
EC2 ──────────────────────────────────> RDS
        3️⃣ Database Connection
        Protocol: PostgreSQL Wire Protocol
        Port: 5432
        Connection String:
        Host=10.0.2.x
        Port=5432
        Database=flashlearn_db
        SSL=require
        
RDS ──────────────────────────────────> EC2
        Response: Query Results (JSON)
```

### Connection 4: EC2 → S3
```
EC2 ────────────────────────────────────> S3
        4️⃣ S3 Operations
        AWS SDK S3 Client
        Methods:
        • PutObject (upload images)
        • GetObject (download images)
        • DeleteObject (remove images)
        Protocol: HTTPS
        Port: 443
        Via: Internet Gateway
        Authentication: IAM Role
        
S3 ─────────────────────────────────────> EC2
        Response: Pre-signed URLs
        Image/Audio binary data
```

## 🎨 Màu sắc chuẩn AWS

```
AWS Services:
- EC2: #ED7100 (Orange)
- RDS: #3334B9 (Blue)
- S3: #7AA116 (Green)
- Cognito: #DD344C (Red)
- VPC: #248814 (Dark Green)
- IAM: #DD344C (Red)
- Security Group: #DD344C (Red dotted border)

Subnets:
- Public: #E9F3E6 (Light Green)
- Private: #E6F2F8 (Light Blue)

Text:
- Titles: Bold, 14pt
- Details: Regular, 10-11pt
- Labels: Bold, 11pt
```

## 📐 Layout Suggestions

```
┌─────────────────────────────────────────────────────────────┐
│ Internet                                                     │
│   👤 User     📱 Mobile                                      │
│      │           │                                           │
│      └───────┬───┘                                           │
│              │ HTTPS                                         │
│ ┌────────────▼─────────────────────────────────────────────┐│
│ │ AWS Cloud (ap-southeast-1)                   🔐 Cognito  ││
│ │  ┌──────────────────────────────────────────┐    │       ││
│ │  │ VPC 10.0.0.0/16                           │    │       ││
│ │  │  🌍 IGW                                    │    │       ││
│ │  │   │                                        │    │       ││
│ │  │ ┌─▼─────────────────────┐                  │    │       ││
│ │  │ │ Public Subnet         │                  │    │       ││
│ │  │ │ 10.0.1.0/24           │                  │    │       ││
│ │  │ │  ┌─────────────────┐  │                  │    │       ││
│ │  │ │  │ 🛡️ EC2-Web-SG   │  │                  │    │       ││
│ │  │ │  │  🖥️ EC2 Instance│──┼──────────────────┼────┘       ││
│ │  │ │  │  + IAM Role     │  │                  │            ││
│ │  │ │  └────────┬────────┘  │                  │            ││
│ │  │ │           │            │                  │            ││
│ │  │ └───────────┼────────────┘                  │   💾 S3   ││
│ │  │             │                               │     Bucket││
│ │  │ ┌───────────▼────────────┐                  │      │    ││
│ │  │ │ Private Subnet         │                  │      │    ││
│ │  │ │ 10.0.2.0/24            │◄─────────────────┼──────┘    ││
│ │  │ │  ┌─────────────────┐  │                  │            ││
│ │  │ │  │ 🛡️ RDS-DB-SG    │  │                  │            ││
│ │  │ │  │  🗄️ RDS          │  │                  │            ││
│ │  │ │  │  PostgreSQL     │  │                  │            ││
│ │  │ │  └─────────────────┘  │                  │            ││
│ │  │ └────────────────────────┘                  │            ││
│ │  └──────────────────────────────────────────────┘            ││
│ └──────────────────────────────────────────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

## ✅ Best Practices Notes (Vẽ ở góc dưới)

```
┌──────────────────────────────────────────────────────┐
│ 💡 Best Practice Improvements                        │
│ (Not included in basic workshop)                     │
├──────────────────────────────────────────────────────┤
│ ✓ Add VPC Gateway Endpoint for S3                    │
│   → Save NAT Gateway costs                           │
│   → Direct private connection to S3                  │
│                                                      │
│ ✓ Deploy Multi-AZ with ALB                          │
│   → Add second AZ (ap-southeast-1b)                  │
│   → Application Load Balancer across AZs            │
│   → Auto Scaling Group for EC2                      │
│                                                      │
│ ✓ Enable RDS Multi-AZ                               │
│   → Synchronous replication                         │
│   → Automatic failover                              │
│                                                      │
│ ✓ Add NAT Gateway for Private Subnet                │
│   → Enable outbound internet for private resources  │
│   → Required for software updates                   │
│                                                      │
│ ✓ Add CloudFront CDN                                │
│   → Cache S3 static assets globally                 │
│   → Reduce S3 egress costs                          │
└──────────────────────────────────────────────────────┘
```

## 🚀 Cách sử dụng guide này trong Draw.io

1. **Mở draw.io**: https://app.diagrams.net
2. **Chọn AWS Architecture template** hoặc blank diagram
3. **Kéo thả AWS icons** từ sidebar bên trái:
   - Search "AWS" để tìm shape library
   - Enable "AWS 19" library
4. **Vẽ theo checklist** từ ngoài vào trong:
   - AWS Cloud boundary first
   - VPC container
   - Subnets
   - Services trong mỗi subnet
5. **Thêm labels chi tiết** từ guide này (copy-paste)
6. **Vẽ connections** với mũi tên và labels
7. **Apply màu sắc** theo AWS color scheme

## 📦 Export Options

- **PNG**: Cho presentations và documents
- **SVG**: Cho web và scaling
- **PDF**: Cho documentation
- **XML**: Để chia sẻ và edit later
