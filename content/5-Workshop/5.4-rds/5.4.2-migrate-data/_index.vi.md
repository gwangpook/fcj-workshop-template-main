---
title: "Migrate & Seed dữ liệu"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.4.2. </b>"
---


Sau khi RDS đã sẵn sàng, chúng ta cần chạy **Entity Framework Core Migrations** để tạo schema database và seed dữ liệu mẫu.

---

## 1. Cập nhật Connection String

Trên máy tính local, cập nhật file `appsettings.json` của project FlashLearn:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=flashlearn-db.xxxx.ap-southeast-1.rds.amazonaws.com;Port=5432;Database=flashlearn;Username=flashlearn_admin;Password=<mật khẩu của bạn>;SSL Mode=Require;Trust Server Certificate=true"
  }
}
```

>  Thay `flashlearn-db.xxxx.ap-southeast-1.rds.amazonaws.com` bằng Endpoint thực tế bạn đã lưu ở bước 5.4.1

---

## 2. Cài đặt PostgreSQL Provider

Đảm bảo project đã có package `Npgsql.EntityFrameworkCore.PostgreSQL`:

```bash
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

Cập nhật `Program.cs` để dùng PostgreSQL thay vì SQLite:

```csharp
// Thay thế dòng SQLite cũ:
// builder.Services.AddDbContext<ApplicationDbContext>(options =>
//     options.UseSqlite(connectionString));

// Bằng PostgreSQL:
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 3. Tạo Migration mới (nếu cần)

Nếu chưa có Migration cho PostgreSQL:

```bash
dotnet ef migrations add InitialPostgreSQL
```

---

## 4. Kết nối EC2 vào RDS để chạy Migration

>  Vì RDS ở Private Subnet, bạn phải SSH vào EC2 trước rồi mới chạy migration từ EC2.

**Bước 1**: SSH vào EC2 (sẽ tạo ở bước 5.5):

```bash
ssh -i flashlearn-key.pem ubuntu@<EC2-Public-IP>
```

**Bước 2**: Trên EC2, publish project và chạy migration:

```bash
# Clone source code trên EC2
git clone <repo-url>
cd WebhocTiengAnhFlashLearn

# Cài .NET SDK
sudo snap install dotnet-sdk --classic --channel=8.0

# Chạy EF migrations
export ConnectionStrings__DefaultConnection="Host=<RDS-Endpoint>;Port=5432;Database=flashlearn;Username=flashlearn_admin;Password=<password>;SSL Mode=Require;Trust Server Certificate=true"

dotnet ef database update
```

**Bước 3**: Kiểm tra migration thành công:

```bash
# Kết nối vào PostgreSQL từ EC2
psql -h <RDS-Endpoint> -U flashlearn_admin -d flashlearn

# Xem danh sách tables đã tạo
\dt

# Kết quả mong đợi:
#  Schema |        Name         | Type  |      Owner       
# --------+---------------------+-------+------------------
#  public | AspNetRoles         | table | flashlearn_admin
#  public | AspNetUsers         | table | flashlearn_admin
#  public | Cards               | table | flashlearn_admin
#  public | Decks               | table | flashlearn_admin
#  public | UserProgress        | table | flashlearn_admin
```

---

## 5. Seed dữ liệu mẫu (tùy chọn)

Nếu ứng dụng có DbInitializer, chạy lệnh sau để seed dữ liệu ban đầu:

```bash
dotnet run --seed
```

Hoặc chèn dữ liệu mẫu trực tiếp qua psql:

```sql
-- Chèn bộ flashcard mẫu
INSERT INTO "Decks" ("Id", "Name", "Description", "CreatedAt", "UserId")
VALUES 
  (gen_random_uuid(), 'Từ vựng cơ bản A1', '500 từ vựng tiếng Anh cơ bản', NOW(), NULL),
  (gen_random_uuid(), 'Động từ bất quy tắc', '100 động từ bất quy tắc thông dụng', NOW(), NULL);
```

---

## Kết quả

Sau bước này, bạn đã có:
-  Connection String đã cập nhật trỏ đến RDS PostgreSQL
-  Schema database đã được tạo thông qua EF Core Migrations
-  Dữ liệu mẫu đã được seed vào database
