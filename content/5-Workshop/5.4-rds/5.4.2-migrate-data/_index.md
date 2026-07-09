---
title: "Migrate & Seed Data"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.4.2. </b>"
---


After RDS is ready, we need to run **Entity Framework Core Migrations** to create the database schema and seed sample data.

---

## 1. Update Connection String

On your local machine, update `appsettings.json` in the FlashLearn project:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=flashlearn-db.xxxx.ap-southeast-1.rds.amazonaws.com;Port=5432;Database=flashlearn;Username=flashlearn_admin;Password=<your password>;SSL Mode=Require;Trust Server Certificate=true"
  }
}
```

>  Replace `flashlearn-db.xxxx.ap-southeast-1.rds.amazonaws.com` with the actual Endpoint you saved in step 5.4.1

---

## 2. Install PostgreSQL Provider

Ensure the project has the `Npgsql.EntityFrameworkCore.PostgreSQL` package:

```bash
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```

Update `Program.cs` to use PostgreSQL instead of SQLite:

```csharp
// Replace the old SQLite line:
// builder.Services.AddDbContext<ApplicationDbContext>(options =>
//     options.UseSqlite(connectionString));

// With PostgreSQL:
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection")));
```

---

## 3. Create a New Migration (if needed)

If no migration exists for PostgreSQL yet:

```bash
dotnet ef migrations add InitialPostgreSQL
```

---

## 4. Connect EC2 to RDS to Run Migration

>  Since RDS is in a Private Subnet, you must SSH into EC2 first, then run the migration from EC2.

**Step 1**: SSH into EC2 (created in step 5.5):

```bash
ssh -i flashlearn-key.pem ubuntu@<EC2-Public-IP>
```

**Step 2**: On EC2, publish the project and run the migration:

```bash
# Clone source code on EC2
git clone <repo-url>
cd WebhocTiengAnhFlashLearn

# Install .NET SDK
sudo snap install dotnet-sdk --classic --channel=8.0

# Run EF migrations
export ConnectionStrings__DefaultConnection="Host=<RDS-Endpoint>;Port=5432;Database=flashlearn;Username=flashlearn_admin;Password=<password>;SSL Mode=Require;Trust Server Certificate=true"

dotnet ef database update
```

**Step 3**: Verify migration succeeded:

```bash
# Connect to PostgreSQL from EC2
psql -h <RDS-Endpoint> -U flashlearn_admin -d flashlearn

# List created tables
\dt

# Expected result:
#  Schema |        Name         | Type  |      Owner       
# --------+---------------------+-------+------------------
#  public | AspNetRoles         | table | flashlearn_admin
#  public | AspNetUsers         | table | flashlearn_admin
#  public | Cards               | table | flashlearn_admin
#  public | Decks               | table | flashlearn_admin
#  public | UserProgress        | table | flashlearn_admin
```

---

## 5. Seed Sample Data (Optional)

If the application has a DbInitializer, run:

```bash
dotnet run --seed
```

Or insert sample data directly via psql:

```sql
-- Insert sample flashcard decks
INSERT INTO "Decks" ("Id", "Name", "Description", "CreatedAt", "UserId")
VALUES 
  (gen_random_uuid(), 'Basic Vocabulary A1', '500 basic English words', NOW(), NULL),
  (gen_random_uuid(), 'Irregular Verbs', '100 common irregular verbs', NOW(), NULL);
```

---

## Result

After this step, you will have:
-  Connection String updated to point to RDS PostgreSQL
-  Database schema created via EF Core Migrations
-  Sample data seeded into the database
