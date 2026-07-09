---
title: "Update Application to Use S3"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.6.2. </b>"
---


---

## 1. Install AWS SDK for .NET

On EC2, in the project directory:

```bash
cd ~/WebhocTiengAnhFlashLearn

# Install AWS SDK for .NET
dotnet add package AWSSDK.S3
dotnet add package AWSSDK.Extensions.NETCore.Setup
```

---

## 2. Configure AWS SDK in the Application

Update `appsettings.Production.json`:

```json
{
  "AWS": {
    "Region": "ap-southeast-1",
    "BucketName": "flashlearn-media-quangphuc"
  }
}
```

In `Program.cs`, register AWS services:

```csharp
// Add AWS SDK
builder.Services.AddDefaultAWSOptions(builder.Configuration.GetAWSOptions());
builder.Services.AddAWSService<IAmazonS3>();

// Register S3 service
builder.Services.AddScoped<IS3Service, S3Service>();
```

---

## 3. Create S3Service

Create file `Services/S3Service.cs`:

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

## 4. Update the Controller Handling Image Uploads

Find the flashcard controller and update the image upload logic:

```csharp
// Before: saving image to wwwroot
// var fileName = Path.Combine(_env.WebRootPath, "uploads", file.FileName);
// await using var stream = new FileStream(fileName, FileMode.Create);
// await file.CopyToAsync(stream);

// After: upload to S3
var imageUrl = await _s3Service.UploadFileAsync(file, "flashcard-images");
card.ImageUrl = imageUrl;
```

---

## 5. Rebuild and Restart the Service

```bash
# Republish the application
dotnet publish -c Release -o /var/www/flashlearn

# Restart the service
sudo systemctl restart flashlearn

# Check logs
sudo journalctl -u flashlearn -f
```

---

## 6. Verify S3 Integration

1. Access the application at `http://<EC2-IP>:5000`
2. Create a new flashcard and upload an image
3. Verify the image appears in the S3 bucket:

```bash
aws s3 ls s3://flashlearn-media-quangphuc/flashcard-images/
```

Verify the image URL is publicly accessible:
```
https://flashlearn-media-quangphuc.s3.ap-southeast-1.amazonaws.com/flashcard-images/<filename>
```

---

## Result

After this step, you will have:
-  AWS SDK integrated into ASP.NET Core
-  S3Service handling file upload and deletion
-  Flashcard images stored on S3 instead of EC2 local disk
-  Image URLs publicly accessible from the Internet
