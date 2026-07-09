---
title: "Integrate Cognito into the Application"
date: 2026-07-09
weight: 2
chapter: false
pre: "<b>5.7.2. </b>"
---


---

## 1. Install Authentication Packages

On EC2, in the project directory:

```bash
dotnet add package Microsoft.AspNetCore.Authentication.OpenIdConnect
dotnet add package Amazon.AspNetCore.Identity.Cognito
```

---

## 2. Update Configuration

Update `appsettings.Production.json`:

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

## 3. Configure Authentication in Program.cs

Update `Program.cs` to add Cognito authentication:

```csharp
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

// Ensure middleware is added in the correct order
app.UseAuthentication();
app.UseAuthorization();
```

---

## 4. Update Login/Logout Controller

Replace the old AccountController with Cognito integration:

```csharp
[Controller]
public class AccountController : Controller
{
    // Login: redirect to Cognito Hosted UI
    [HttpGet]
    public IActionResult Login(string returnUrl = "/")
    {
        return Challenge(new AuthenticationProperties 
        { 
            RedirectUri = returnUrl 
        }, 
        OpenIdConnectDefaults.AuthenticationScheme);
    }

    // Logout
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

    // Get user info from Claims
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

## 5. Protect Routes Requiring Authentication

```csharp
// Add [Authorize] to controllers/actions requiring login
[Authorize]
public class DeckController : Controller
{
    // Only authenticated users can create a Deck
    [HttpPost]
    public async Task<IActionResult> Create(CreateDeckViewModel model)
    {
        var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
        // ...
    }
}
```

---

## 6. Rebuild and Verify

```bash
dotnet publish -c Release -o /var/www/flashlearn
sudo systemctl restart flashlearn
```

Test the authentication flow:
1. Access `http://<EC2-IP>:5000`
2. Click **Sign In** → Redirected to Cognito Hosted UI
3. Register a new account → Receive verification email
4. Confirm email → Successfully signed in
5. Access features requiring login (create Deck, Battle...)

---

## Result

After this step, you will have:
-  Cognito fully integrated into ASP.NET Core
-  Sign-in/Sign-up via Cognito Hosted UI
-  JWT Token validated automatically
-  Cognito User ID used as the key in the database
