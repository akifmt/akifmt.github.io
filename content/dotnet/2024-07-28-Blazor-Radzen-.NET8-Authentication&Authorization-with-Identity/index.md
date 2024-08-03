---
title: "Blazor Radzen .NET 8 Authentication & Authorization with Identity"
date: 2024-07-28T00:00:00+00:00
hero: blazor_radzen_dotnet8.jpg
description: Blazor Radzen .NET 8 Authentication & Authorization with Identity
menu:
  dotnet:
    name: Blazor Radzen .NET 8 Authentication & Authorization with Identity
    identifier: blazor-radzen-dotnet8-authentication-authorization-with-identity
    weight: -20240728
tags: [dotnet8, .NET8, Blazor, Radzen, Authentication, Authorization, Identity]
categories: [dotnet8, .NET8, Blazor, Radzen, Authentication, Authorization, Identity]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_radzen_dotnet8.jpg" alt="blazor_radzen_dotnet8" title="blazor_radzen_dotnet8" style="border-radius: 20px;"><br>
<p>


#### **Blazor Radzen .NET 8 Authentication & Authorization with Identity**

Blazor: ```Blazor``` is a framework for building interactive web UIs using C# instead of JavaScript. It allows developers to create web applications using .NET.  
Radzen: ```Radzen``` is a low-code development tool that helps in building web applications quickly and efficiently.  
.NET 8: ```.NET 8``` is a software framework developed by Microsoft for building different types of applications, including web applications.  
Authentication: ```Authentication``` is the process of verifying the identity of users accessing an application.  
Authorization: ```Authorization``` determines what actions users are allowed to perform within an application.  
Identity: ```Identity``` is a membership system in ```ASP.NET``` that provides user authentication and authorization functionality.  


##### **BlazorAppRadzenAuthwithIdentity.csproj**
One fundamental component in this ecosystem is ```Microsoft.AspNetCore.Identity.EntityFrameworkCore```. This package plays a pivotal role in managing user authentication and authorization within a Blazor application.

<kbd>BlazorAppRadzenAuthwithIdentity.csproj</kbd>
```
...  
<PackageReference Include="Microsoft.AspNetCore.Identity.EntityFrameworkCore" Version="8.0.5" />
...  
```


##### **Program.cs**

```Program.cs``` provided configures services and settings related to ```Identity```, authentication, and authorization in a Blazor Radzen application.

<kbd>Program.cs</kbd>
```
...

builder.Services.AddCascadingAuthenticationState();
builder.Services.AddScoped<IdentityUserAccessor>();
builder.Services.AddScoped<IdentityRedirectManager>();
builder.Services.AddScoped<AuthenticationStateProvider, IdentityRevalidatingAuthenticationStateProvider>();

builder.Services.AddAuthentication(options =>
{
	options.DefaultScheme = IdentityConstants.ApplicationScheme;
	options.DefaultSignInScheme = IdentityConstants.ExternalScheme;
})
	.AddIdentityCookies();

	
builder.Services.AddIdentityCore<ApplicationUser>()
	.AddEntityFrameworkStores<ApplicationDbContext>()
	.AddSignInManager()
	.AddDefaultTokenProviders();
	
	
builder.Services.AddSingleton<IEmailSender<ApplicationUser>, IdentityNoOpEmailSender>();
		
...

// Add additional endpoints required by the Identity /Account Razor components.
app.MapAdditionalIdentityEndpoints();

...
```

```AddCascadingAuthenticationState()```: Registers the cascading authentication state service.  
```AddScoped<IdentityUserAccessor>()```: Registers the IdentityUserAccessor service.  
```AddScoped<IdentityRedirectManager>()```: Registers the IdentityRedirectManager service.  
```AddScoped<AuthenticationStateProvider, IdentityRevalidatingAuthenticationStateProvider>()```: Registers the IdentityRevalidatingAuthenticationStateProvider service for managing authentication state.  


##### **ApplicationDbContext.cs**

```ApplicationDbContext``` class, which inherits from ```IdentityDbContext<ApplicationUser>```. This class is responsible for managing the database context for the application, specifically related to user authentication and authorization.

<kbd>ApplicationDbContext.cs</kbd>
```
using BlazorAppRadzenAuthwithIdentity.Models;
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore;

namespace BlazorAppRadzenAuthwithIdentity.Data;

public class ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : IdentityDbContext<ApplicationUser>(options)
{
    public DbSet<BlogPost> BlogPosts => Set<BlogPost>();

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
    }
}
```

The ```ApplicationDbContext``` class takes ```DbContextOptions<ApplicationDbContext>``` as a parameter in its constructor.  
It defines a ```DbSet<BlogPost>``` property named BlogPosts to interact with the BlogPost entity in the database.  
The ```OnModelCreating``` method is overridden to configure the model for the database context.  


##### **Login.razor**
<kbd>Login.razor</kbd>
```
@page "/Account/Login"
@layout AccountLayout

@using System.ComponentModel.DataAnnotations
@using Microsoft.AspNetCore.Authentication
@using Microsoft.AspNetCore.Identity
@using BlazorAppRadzenAuthwithIdentity.Models

@inject SignInManager<ApplicationUser> SignInManager
@inject ILogger<Login> Logger
@inject NavigationManager NavigationManager
@inject IdentityRedirectManager RedirectManager

<PageTitle>Log in</PageTitle>

<h1>Log in</h1>
<div class="row">
    <div class="col-md-4">
        <section>
            <StatusMessage Message="@errorMessage" />
            <EditForm Model="Input" method="post" OnValidSubmit="LoginUser" FormName="login">
                <DataAnnotationsValidator />
                <h2>Use a local account to log in.</h2>
                <hr />
                <ValidationSummary class="text-danger" role="alert" />
                <div class="form-floating mb-3">
                    <InputText @bind-Value="Input.Email" class="form-control" autocomplete="username" aria-required="true" placeholder="name@example.com" />
                    <label for="email" class="form-label">Email</label>
                    <ValidationMessage For="() => Input.Email" class="text-danger" />
                </div>
                <div class="form-floating mb-3">
                    <InputText type="password" @bind-Value="Input.Password" class="form-control" autocomplete="current-password" aria-required="true" placeholder="password" />
                    <label for="password" class="form-label">Password</label>
                    <ValidationMessage For="() => Input.Password" class="text-danger" />
                </div>
                <div class="checkbox mb-3">
                    <label class="form-label">
                        <InputCheckbox @bind-Value="Input.RememberMe" class="darker-border-checkbox form-check-input" />
                        Remember me
                    </label>
                </div>
                <div>
                    <button type="submit" class="w-100 btn btn-lg btn-primary">Log in</button>
                </div>
                <div>
                    <p>
                        <a href="Account/ForgotPassword">Forgot your password?</a>
                    </p>
                    <p>
                        <a href="@(NavigationManager.GetUriWithQueryParameters("Account/Register", new Dictionary<string, object?> { ["ReturnUrl"] = ReturnUrl }))">Register as a new user</a>
                    </p>
                    <p>
                        <a href="Account/ResendEmailConfirmation">Resend email confirmation</a>
                    </p>
                </div>
            </EditForm>
        </section>
    </div>
    <div class="col-md-6 col-md-offset-2">
        <section>
            <h3>Use another service to log in.</h3>
            <hr />
            <ExternalLoginPicker />
        </section>
    </div>
</div>

@code {
    private string? errorMessage;

    [CascadingParameter]
    private HttpContext HttpContext { get; set; } = default!;

    [SupplyParameterFromForm]
    private InputModel Input { get; set; } = new();

    [SupplyParameterFromQuery]
    private string? ReturnUrl { get; set; }

    protected override async Task OnInitializedAsync()
    {
        if (HttpMethods.IsGet(HttpContext.Request.Method))
        {
            // Clear the existing external cookie to ensure a clean login process
            await HttpContext.SignOutAsync(IdentityConstants.ExternalScheme);
        }
    }

    public async Task LoginUser()
    {
        // This doesn't count login failures towards account lockout
        // To enable password failures to trigger account lockout, set lockoutOnFailure: true
        var result = await SignInManager.PasswordSignInAsync(Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: false);
        if (result.Succeeded)
        {
            Logger.LogInformation("User logged in.");
            RedirectManager.RedirectTo(ReturnUrl);
        }
        else if (result.RequiresTwoFactor)
        {
            RedirectManager.RedirectTo(
                "Account/LoginWith2fa",
                new() { ["returnUrl"] = ReturnUrl, ["rememberMe"] = Input.RememberMe });
        }
        else if (result.IsLockedOut)
        {
            Logger.LogWarning("User account locked out.");
            RedirectManager.RedirectTo("Account/Lockout");
        }
        else
        {
            errorMessage = "Error: Invalid login attempt.";
        }
    }

    private sealed class InputModel
    {
        [Required]
        [EmailAddress]
        public string Email { get; set; } = "";

        [Required]
        [DataType(DataType.Password)]
        public string Password { get; set; } = "";

        [Display(Name = "Remember me?")]
        public bool RememberMe { get; set; }
    }
}
```

The ```@inject``` directives inject dependencies into the page, such as the ```SignInManager```, ```Logger```, ```NavigationManager```, and ```RedirectManager```.


##### **Register.razor**
<kbd>Register.razor</kbd>
```
@page "/Account/Register"
@layout AccountLayout

@using System.ComponentModel.DataAnnotations
@using System.Text
@using System.Text.Encodings.Web
@using Microsoft.AspNetCore.Identity
@using Microsoft.AspNetCore.WebUtilities
@using BlazorAppRadzenAuthwithIdentity.Models

@inject UserManager<ApplicationUser> UserManager
@inject IUserStore<ApplicationUser> UserStore
@inject SignInManager<ApplicationUser> SignInManager
@inject IEmailSender<ApplicationUser> EmailSender
@inject ILogger<Register> Logger
@inject NavigationManager NavigationManager
@inject IdentityRedirectManager RedirectManager

<PageTitle>Register</PageTitle>

<h1>Register</h1>

<div class="row">
    <div class="col-md-4">
        <StatusMessage Message="@Message" />
        <EditForm Model="Input" asp-route-returnUrl="@ReturnUrl" method="post" OnValidSubmit="RegisterUser" FormName="register">
            <DataAnnotationsValidator />
            <h2>Create a new account.</h2>
            <hr />
            <ValidationSummary class="text-danger" role="alert" />
            <div class="form-floating mb-3">
                <InputText @bind-Value="Input.Email" class="form-control" autocomplete="username" aria-required="true" placeholder="name@example.com" />
                <label for="email">Email</label>
                <ValidationMessage For="() => Input.Email" class="text-danger" />
            </div>
            <div class="form-floating mb-3">
                <InputText type="password" @bind-Value="Input.Password" class="form-control" autocomplete="new-password" aria-required="true" placeholder="password" />
                <label for="password">Password</label>
                <ValidationMessage For="() => Input.Password" class="text-danger" />
            </div>
            <div class="form-floating mb-3">
                <InputText type="password" @bind-Value="Input.ConfirmPassword" class="form-control" autocomplete="new-password" aria-required="true" placeholder="password" />
                <label for="confirm-password">Confirm Password</label>
                <ValidationMessage For="() => Input.ConfirmPassword" class="text-danger" />
            </div>
            <button type="submit" class="w-100 btn btn-lg btn-primary">Register</button>
        </EditForm>
    </div>
    <div class="col-md-6 col-md-offset-2">
        <section>
            <h3>Use another service to register.</h3>
            <hr />
            <ExternalLoginPicker />
        </section>
    </div>
</div>

@code {
    private IEnumerable<IdentityError>? identityErrors;

    [SupplyParameterFromForm]
    private InputModel Input { get; set; } = new();

    [SupplyParameterFromQuery]
    private string? ReturnUrl { get; set; }

    private string? Message => identityErrors is null ? null : $"Error: {string.Join(", ", identityErrors.Select(error => error.Description))}";

    public async Task RegisterUser(EditContext editContext)
    {
        var user = CreateUser();

        await UserStore.SetUserNameAsync(user, Input.Email, CancellationToken.None);
        var emailStore = GetEmailStore();
        await emailStore.SetEmailAsync(user, Input.Email, CancellationToken.None);
        var result = await UserManager.CreateAsync(user, Input.Password);

        if (!result.Succeeded)
        {
            identityErrors = result.Errors;
            return;
        }

        Logger.LogInformation("User created a new account with password.");

        var userId = await UserManager.GetUserIdAsync(user);
        var code = await UserManager.GenerateEmailConfirmationTokenAsync(user);
        code = WebEncoders.Base64UrlEncode(Encoding.UTF8.GetBytes(code));
        var callbackUrl = NavigationManager.GetUriWithQueryParameters(
            NavigationManager.ToAbsoluteUri("Account/ConfirmEmail").AbsoluteUri,
            new Dictionary<string, object?> { ["userId"] = userId, ["code"] = code, ["returnUrl"] = ReturnUrl });

        await EmailSender.SendConfirmationLinkAsync(user, Input.Email, HtmlEncoder.Default.Encode(callbackUrl));

        if (UserManager.Options.SignIn.RequireConfirmedAccount)
        {
            RedirectManager.RedirectTo(
                "Account/RegisterConfirmation",
                new() { ["email"] = Input.Email, ["returnUrl"] = ReturnUrl });
        }

        await SignInManager.SignInAsync(user, isPersistent: false);
        RedirectManager.RedirectTo(ReturnUrl);
    }

    private ApplicationUser CreateUser()
    {
        try
        {
            return Activator.CreateInstance<ApplicationUser>();
        }
        catch
        {
            throw new InvalidOperationException($"Can't create an instance of '{nameof(ApplicationUser)}'. " +
                $"Ensure that '{nameof(ApplicationUser)}' is not an abstract class and has a parameterless constructor.");
        }
    }

    private IUserEmailStore<ApplicationUser> GetEmailStore()
    {
        if (!UserManager.SupportsUserEmail)
        {
            throw new NotSupportedException("The default UI requires a user store with email support.");
        }
        return (IUserEmailStore<ApplicationUser>)UserStore;
    }

    private sealed class InputModel
    {
        [Required]
        [EmailAddress]
        [Display(Name = "Email")]
        public string Email { get; set; } = "";

        [Required]
        [StringLength(100, ErrorMessage = "The {0} must be at least {2} and at max {1} characters long.", MinimumLength = 6)]
        [DataType(DataType.Password)]
        [Display(Name = "Password")]
        public string Password { get; set; } = "";

        [DataType(DataType.Password)]
        [Display(Name = "Confirm password")]
        [Compare("Password", ErrorMessage = "The password and confirmation password do not match.")]
        public string ConfirmPassword { get; set; } = "";
    }
}
```

The ```RegisterUser``` method handles the user registration process. It creates a new user, sets the user's email and password, and calls the ```CreateAsync``` method of the ```UserManager``` to create the user in the database. If the registration is successful, it generates an email confirmation token, sends a confirmation email to the user, and signs in the user. Finally, it redirects the user to the specified ```URL```.  
The ```InputModel``` class represents the input fields for the user registration form. It includes properties for the email, password, and confirm password fields, along with validation attributes.  


##### **Auth.razor**
<kbd>Auth.razor</kbd>
```
@page "/auth"

@using Microsoft.AspNetCore.Authorization

@attribute [Authorize]

<PageTitle>Auth</PageTitle>

<h1>You are authenticated</h1>

<AuthorizeView>
    Hello @context.User.Identity?.Name!
</AuthorizeView>
```

```@attribute [Authorize]```: Decorates the page with the ```[Authorize]``` attribute, restricting access to authenticated users only.

```Hello @context.User.Identity?.Name!```: Displays a greeting message with the ```authenticated``` user's name if available.

#### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppRadzenAuthwithIdentity
