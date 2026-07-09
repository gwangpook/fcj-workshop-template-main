---
title: "Cập nhật ứng dụng dùng S3"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.6.2. </b>"
---


---

## 1. Cài đặt AWS SDK cho .NET

Trên EC2, trong thư mục project:

```bash
cd ~/WebhocTiengAnhFlashLearn

# Cài AWS SDK for .NET
dotnet add package AWSSDK.S3
dotnet add package AWSSDK.Extensions.NETCore.Setup
```

---

## 2. Cấu hình AWS SDK trong ứng dụng

Cập nhật `appsettings.Production.json`:

```json
{
  "AWS": {
    "Region": "ap-southeast-1",
    "BucketName": "flashlearn-media-quangphuc"
  }
}
```

Trong `Program.cs`, đăng ký AWS services:

```csharp
// Thêm AWS SDK
builder.Services.AddDefaultAWSOptions(builder.Configuration.GetAWSOptions());
builder.Services.AddAWSService<IAmazonS3>();

// Đăng ký S3 service
builder.Services.AddScoped<IS3Service, S3Service>();
```

---

## 3. Tạo S3Service

Tạo file `Services/S3Service.cs`:

```csharp
using Amazon.S3;
using Amazon.S3.Transfer;

public interface IS3Service
{
    Task<string> UploadFileAsync(IFormFile file, string folder);
    Task DeleteFileAsync(string fileKey);
}

public class S3Service : IS3Service
{
    private readonly IAmazonS3 _s3Client;
    private readonly string _bucketName;

    public S3Service(IAmazonS3 s3Client, IConfiguration config)
    {
        _s3Client = s3Client;
        _bucketName = config["AWS:BucketName"]!;
    }

    public async Task<string> UploadFileAsync(IFormFile file, string folder)
    {
        var fileKey = $"{folder}/{Guid.NewGuid()}{Path.GetExtension(file.FileName)}";
        
        using var stream = file.OpenReadStream();
        var uploadRequest = new TransferUtilityUploadRequest
        {
            BucketName = _bucketName,
            Key = fileKey,
            InputStream = stream,
            ContentType = file.ContentType,
            CannedACL = S3CannedACL.PublicRead
        };

        var transferUtility = new TransferUtility(_s3Client);
        await transferUtility.UploadAsync(uploadRequest);

        return $"https://{_bucketName}.s3.ap-southeast-1.amazonaws.com/{fileKey}";
    }

    public async Task DeleteFileAsync(string fileKey)
    {
        await _s3Client.DeleteObjectAsync(_bucketName, fileKey);
    }
}
```

---

## 4. Cập nhật Controller xử lý upload ảnh

Tìm controller xử lý flashcard và cập nhật phần upload ảnh:

```csharp
// Trước đây: lưu ảnh vào wwwroot
// var fileName = Path.Combine(_env.WebRootPath, "uploads", file.FileName);
// await using var stream = new FileStream(fileName, FileMode.Create);
// await file.CopyToAsync(stream);

// Thay bằng: upload lên S3
var imageUrl = await _s3Service.UploadFileAsync(file, "flashcard-images");
card.ImageUrl = imageUrl;
```

---

## 5. Rebuild và khởi động lại service

```bash
# Publish lại ứng dụng
dotnet publish -c Release -o /var/www/flashlearn

# Khởi động lại service
sudo systemctl restart flashlearn

# Kiểm tra logs
sudo journalctl -u flashlearn -f
```

---

## 6. Kiểm tra tích hợp S3

1. Truy cập ứng dụng tại `http://<EC2-IP>:5000`
2. Tạo một flashcard mới và upload ảnh
3. Kiểm tra ảnh đã xuất hiện trong S3 bucket:

```bash
aws s3 ls s3://flashlearn-media-quangphuc/flashcard-images/
```

Kiểm tra URL ảnh có thể truy cập công khai:
```
https://flashlearn-media-quangphuc.s3.ap-southeast-1.amazonaws.com/flashcard-images/<filename>
```

---

## Kết quả

Sau bước này, bạn đã có:
-  AWS SDK tích hợp vào ứng dụng ASP.NET Core
-  S3Service xử lý upload/delete file
-  Ảnh flashcard lưu trên S3 thay vì EC2 local
-  URL ảnh có thể truy cập công khai từ Internet
