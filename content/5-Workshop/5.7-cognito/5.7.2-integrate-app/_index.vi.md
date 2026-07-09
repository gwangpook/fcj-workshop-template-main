---
title: "Tích hợp Cognito vào ứng dụng"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.7.2. </b>"
---


---

## 1. Cài đặt package xác thực

Trên EC2, trong thư mục project:

```bash
dotnet add package Microsoft.AspNetCore.Authentication.OpenIdConnect
dotnet add package Amazon.AspNetCore.Identity.Cognito
```

---

## 2. Cập nhật cấu hình

Cập nhật `appsettings.Production.json`:

```json
{
  "Cognito": {
    "Authority": "https://cognito-idp.ap-southeast-1.amazonaws.com/<User-Pool-ID>",
    "ClientId": "<App-Client-ID>",
    "MetadataAddress": "https://cognito-idp.ap-southeast-1.amazonaws.com/<User-Pool-ID>/.well-known/openid-configuration",
    "Domain": "https://flashlearn-auth.auth.ap-southeast-1.amazoncognito.com"
  }
}
```

---

## 3. Cấu hình Authentication trong Program.cs

Cập nhật `Program.cs` để thêm Cognito authentication:

```csharp
// Thêm sau builder.Services.AddDbContext(...)

builder.Services.AddAuthentication(options =>
{
    options.DefaultAuthenticateScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultSignInScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
})
.AddCookie()
.AddOpenIdConnect(options =>
{
    var cognito = builder.Configuration.GetSection("Cognito");
    
    options.Authority = cognito["Authority"];
    options.ClientId = cognito["ClientId"];
    options.ResponseType = OpenIdConnectResponseType.Code;
    options.MetadataAddress = cognito["MetadataAddress"];
    
    options.Scope.Add("openid");
    options.Scope.Add("email");
    options.Scope.Add("profile");
    
    options.CallbackPath = "/signin-oidc";
    options.SignedOutCallbackPath = "/signout-callback-oidc";
    
    options.SaveTokens = true;
    options.GetClaimsFromUserInfoEndpoint = true;
    
    options.TokenValidationParameters = new TokenValidationParameters
    {
        ValidateIssuer = true,
        NameClaimType = "name",
        RoleClaimType = "cognito:groups"
    };
});

// Đảm bảo thêm middleware theo đúng thứ tự
app.UseAuthentication();
app.UseAuthorization();
```

---

## 4. Cập nhật Controller đăng nhập/đăng xuất

Thay thế AccountController cũ bằng Cognito:

```csharp
[Controller]
public class AccountController : Controller
{
    // Đăng nhập: chuyển hướng đến Cognito Hosted UI
    [HttpGet]
    public IActionResult Login(string returnUrl = "/")
    {
        return Challenge(new AuthenticationProperties 
        { 
            RedirectUri = returnUrl 
        }, 
        OpenIdConnectDefaults.AuthenticationScheme);
    }

    // Đăng xuất
    [HttpPost]
    [ValidateAntiForgeryToken]
    public IActionResult Logout()
    {
        return SignOut(
            new AuthenticationProperties { RedirectUri = "/" },
            CookieAuthenticationDefaults.AuthenticationScheme,
            OpenIdConnectDefaults.AuthenticationScheme
        );
    }

    // Lấy thông tin user từ Claims
    [Authorize]
    [HttpGet]
    public IActionResult Profile()
    {
        var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
        var email = User.FindFirst(ClaimTypes.Email)?.Value;
        var name = User.FindFirst("name")?.Value;
        
        return View(new ProfileViewModel 
        { 
            UserId = userId, 
            Email = email, 
            Name = name 
        });
    }
}
```

---

## 5. Bảo vệ các route cần đăng nhập

```csharp
// Thêm [Authorize] vào các controller/action cần đăng nhập
[Authorize]
public class DeckController : Controller
{
    // Chỉ user đăng nhập mới tạo được Deck
    [HttpPost]
    public async Task<IActionResult> Create(CreateDeckViewModel model)
    {
        var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
        // ...
    }
}
```

---

## 6. Rebuild và kiểm tra

```bash
dotnet publish -c Release -o /var/www/flashlearn
sudo systemctl restart flashlearn
```

Kiểm tra luồng đăng nhập:
1. Truy cập `http://<EC2-IP>:5000`
2. Nhấn **Đăng nhập** → Chuyển hướng đến Cognito Hosted UI
3. Đăng ký tài khoản mới → Nhận email xác nhận
4. Xác nhận email → Đăng nhập thành công
5. Truy cập các tính năng cần đăng nhập (tạo Deck, Battle...)

---

## Kết quả

Sau bước này, bạn đã có:
-  Cognito tích hợp hoàn toàn vào ASP.NET Core
-  Đăng nhập/Đăng ký qua Cognito Hosted UI
-  JWT Token được validate tự động
-  User ID từ Cognito được dùng làm khóa trong database
