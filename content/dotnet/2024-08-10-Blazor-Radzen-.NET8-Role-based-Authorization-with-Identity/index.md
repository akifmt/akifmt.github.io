---
title: "Blazor Radzen .NET 8 Role-based Authorization with Identity"
date: 2024-08-10T00:00:00+00:00
hero: blazor_radzen_dotnet8.jpg
description: Blazor Radzen .NET 8 Role-based Authorization with Identity
menu:
  dotnet:
    name: Blazor Radzen .NET 8 Role-based Authorization with Identity
    identifier: blazor-radzen-dotnet8-role-based-authorization-with-identity
    weight: -20240810
tags: [dotnet8, .NET8, Blazor, Radzen, Role-based Authorization, Identity]
categories: [dotnet8, .NET8, Blazor, Radzen, Role-based Authorization, Identity]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_radzen_dotnet8.jpg" alt="blazor_radzen_dotnet8" title="blazor_radzen_dotnet8" style="border-radius: 20px;"><br>
<p>


#### **Blazor Radzen .NET 8 Role-based Authorization with Identity**

Blazor: ```Blazor``` is a web framework for building interactive client-side web applications using C# instead of JavaScript. It allows you to write code that runs on the client-side in the browser.  
Radzen: ```Radzen``` is a low-code development tool for building web applications. It provides a visual interface for designing UI components and generating code.  
.NET 8: ```.NET 8``` is the latest version of the .NET framework, which is a software development platform for building applications.  
Role-based Authorization: ```Role-based authorization``` is a security mechanism that allows you to control access to certain parts of your application based on the roles assigned to users. Users with specific roles are granted access to specific resources or actions.  
Identity: ```Identity``` is a membership system in ASP.NET that provides user authentication and authorization functionality. It allows you to manage user accounts, roles, and permissions.  
DbContext: The ```DbContext``` class is a fundamental part of Entity Framework, responsible for managing the connection and communication with the database. It provides an abstraction layer for interacting with the database and performing CRUD operations on the entities.  
ClaimsPrincipal: Represents the ```user's identity``` and associated ```claims```.  
AuthenticationStateProvider: Provides access to the current ```authentication state```.  


<b>Test Users:</b>  
| Role | Username | Password |
| ---- | -------- | -------- |
| ```Admin``` | ```admin@admin.com``` | ```123456``` |
| ```User``` | ```register with any email``` | ```register with any password``` |


##### **BlazorAppRadzenRoleBasedAuthwithIdentity.csproj**
<kbd>BlazorAppRadzenRoleBasedAuthwithIdentity.csproj</kbd>
```
...  
<PackageReference Include="Microsoft.AspNetCore.Identity.EntityFrameworkCore" Version="8.0.5" />
...  
```
One fundamental component in this ecosystem is ```Microsoft.AspNetCore.Identity.EntityFrameworkCore```. This package plays a pivotal role in managing user authentication and authorization within a Blazor application.  


##### **Program.cs**
```Program.cs``` file in a Blazor Radzen application. It configures the services and options required for ```role-based authorization``` using ```Identity```.  
<kbd>Program.cs</kbd>
```
...
// Add services to the container.
builder.Services.AddCascadingAuthenticationState();
builder.Services.AddScoped<IdentityUserAccessor>();
builder.Services.AddScoped<IdentityRedirectManager>();
builder.Services.AddScoped<AuthenticationStateProvider, IdentityRevalidatingAuthenticationStateProvider>();

builder.Services.AddAuthentication(options =>
{
	options.DefaultScheme = IdentityConstants.ApplicationScheme;
	options.DefaultSignInScheme = IdentityConstants.ExternalScheme;
});

builder.Services.AddDbContext<ApplicationDbContext>(options =>
	options.UseInMemoryDatabase("ConnectionInMemory"));
builder.Services.AddDatabaseDeveloperPageExceptionFilter();

builder.Services.AddIdentity<ApplicationUser, ApplicationRole>()
		.AddSignInManager<SignInManager<ApplicationUser>>()
		.AddUserStore<UserStore<ApplicationUser, ApplicationRole, ApplicationDbContext, string, ApplicationUserClaim, ApplicationUserRole, ApplicationUserLogin, ApplicationUserToken, ApplicationRoleClaim>>()
		.AddRoleStore<RoleStore<ApplicationRole, ApplicationDbContext, string, ApplicationUserRole, ApplicationRoleClaim>>()
		.AddUserManager<UserManager<ApplicationUser>>()
		.AddRoles<ApplicationRole>()
		.AddRoleManager<RoleManager<ApplicationRole>>()
		.AddEntityFrameworkStores<ApplicationDbContext>()
		.AddDefaultTokenProviders();
...
```


##### **ApplicationDbContext.cs**
The ```ApplicationDbContext.cs``` file is a class that extends the ```IdentityDbContext``` class, which is provided by the Identity framework. It defines the database context for the application and includes the configuration for the database tables and their relationships.  
<kbd>ApplicationDbContext.cs</kbd>
```
...
public DbSet<ApplicationUser> ApplicationUsers => Set<ApplicationUser>();
public DbSet<ApplicationRole> ApplicationRoles => Set<ApplicationRole>();

public DbSet<ApplicationUserClaim> ApplicationUserClaims => Set<ApplicationUserClaim>();
public DbSet<ApplicationUserRole> ApplicationUserRoles => Set<ApplicationUserRole>();
public DbSet<ApplicationUserLogin> ApplicationUserLogins => Set<ApplicationUserLogin>();
public DbSet<ApplicationRoleClaim> ApplicationRoleClaims => Set<ApplicationRoleClaim>();
public DbSet<ApplicationUserToken> ApplicationUserTokens => Set<ApplicationUserToken>();

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
	base.OnModelCreating(modelBuilder);

	//models
	modelBuilder.Entity<ApplicationUser>(b =>
	{
		// Primary key
		b.HasKey(u => u.Id);

		// Indexes for "normalized" username and email, to allow efficient lookups
		b.HasIndex(u => u.NormalizedUserName).HasDatabaseName("UserNameIndex").IsUnique();
		b.HasIndex(u => u.NormalizedEmail).HasDatabaseName("EmailIndex");

		// Maps to the AspNetUsers table
		b.ToTable("AspNetUsers");

		// A concurrency token for use with the optimistic concurrency checking
		b.Property(u => u.ConcurrencyStamp).IsConcurrencyToken();

		// Limit the size of columns to use efficient database types
		b.Property(u => u.UserName).HasMaxLength(256);
		b.Property(u => u.NormalizedUserName).HasMaxLength(256);
		b.Property(u => u.Email).HasMaxLength(256);
		b.Property(u => u.NormalizedEmail).HasMaxLength(256);

		// The relationships between User and other entity types
		// Note that these relationships are configured with no navigation properties

		// Each User can have many UserClaims
		b.HasMany<ApplicationUserClaim>().WithOne().HasForeignKey(uc => uc.UserId).IsRequired();

		// Each User can have many UserLogins
		b.HasMany<ApplicationUserLogin>().WithOne().HasForeignKey(ul => ul.UserId).IsRequired();

		// Each User can have many UserTokens
		b.HasMany<ApplicationUserToken>().WithOne().HasForeignKey(ut => ut.UserId).IsRequired();

		// Each User can have many entries in the UserRole join table
		b.HasMany<ApplicationUserRole>().WithOne().HasForeignKey(ur => ur.UserId).IsRequired();
	});

	modelBuilder.Entity<ApplicationUserClaim>(b =>
	{
		// Primary key
		b.HasKey(uc => uc.Id);

		// Maps to the AspNetUserClaims table
		b.ToTable("AspNetUserClaims");
	});

	modelBuilder.Entity<ApplicationUserLogin>(b =>
	{
		// Composite primary key consisting of the LoginProvider and the key to use
		// with that provider
		b.HasKey(l => new { l.LoginProvider, l.ProviderKey });

		// Limit the size of the composite key columns due to common DB restrictions
		b.Property(l => l.LoginProvider).HasMaxLength(128);
		b.Property(l => l.ProviderKey).HasMaxLength(128);

		// Maps to the AspNetUserLogins table
		b.ToTable("AspNetUserLogins");
	});

	modelBuilder.Entity<ApplicationUserToken>(b =>
	{
		// Composite primary key consisting of the UserId, LoginProvider and Name
		b.HasKey(t => new { t.UserId, t.LoginProvider, t.Name });

		// Limit the size of the composite key columns due to common DB restrictions
		//b.Property(t => t.LoginProvider).HasMaxLength(maxKeyLength);
		//b.Property(t => t.Name).HasMaxLength(maxKeyLength);

		// Maps to the AspNetUserTokens table
		b.ToTable("AspNetUserTokens");
	});

	modelBuilder.Entity<ApplicationRole>(b =>
	{
		// Primary key
		b.HasKey(r => r.Id);

		// Index for "normalized" role name to allow efficient lookups
		b.HasIndex(r => r.NormalizedName).HasDatabaseName("RoleNameIndex").IsUnique();

		// Maps to the AspNetRoles table
		b.ToTable("AspNetRoles");

		// A concurrency token for use with the optimistic concurrency checking
		b.Property(r => r.ConcurrencyStamp).IsConcurrencyToken();

		// Limit the size of columns to use efficient database types
		b.Property(u => u.Id).HasMaxLength(50);
		b.Property(u => u.Name).HasMaxLength(256);
		b.Property(u => u.NormalizedName).HasMaxLength(256);

		// The relationships between Role and other entity types
		// Note that these relationships are configured with no navigation properties

		// Each Role can have many entries in the UserRole join table
		b.HasMany<ApplicationUserRole>().WithOne().HasForeignKey(ur => ur.RoleId).IsRequired();

		// Each Role can have many associated RoleClaims
		b.HasMany<ApplicationRoleClaim>().WithOne().HasForeignKey(rc => rc.RoleId).IsRequired();
	});

	modelBuilder.Entity<ApplicationRoleClaim>(b =>
	{
		// Primary key
		b.HasKey(rc => rc.Id);

		// Maps to the AspNetRoleClaims table
		b.ToTable("AspNetRoleClaims");
	});

	modelBuilder.Entity<ApplicationUserRole>(b =>
	{
		// Primary key
		b.HasKey(r => new { r.UserId, r.RoleId });

		// Maps to the AspNetUserRoles table
		b.ToTable("AspNetUserRoles");
	});

	// navigation props
	modelBuilder.Entity<ApplicationUser>(b =>
	{
		// Each User can have many UserClaims
		b.HasMany(e => e.Claims)
			.WithOne(e => e.User)
			.HasForeignKey(uc => uc.UserId)
			.IsRequired();

		// Each User can have many UserLogins
		b.HasMany(e => e.Logins)
			.WithOne(e => e.User)
			.HasForeignKey(ul => ul.UserId)
			.IsRequired();

		// Each User can have many UserTokens
		b.HasMany(e => e.Tokens)
			.WithOne(e => e.User)
			.HasForeignKey(ut => ut.UserId)
			.IsRequired();

		// Each User can have many entries in the UserRole join table
		b.HasMany(e => e.UserRoles)
			.WithOne(e => e.User)
			.HasForeignKey(ur => ur.UserId)
			.IsRequired();
	});

	modelBuilder.Entity<ApplicationRole>(b =>
	{
		// Each Role can have many entries in the UserRole join table
		b.HasMany(e => e.UserRoles)
			.WithOne(e => e.Role)
			.HasForeignKey(ur => ur.RoleId)
			.IsRequired();

		// Each Role can have many associated RoleClaims
		b.HasMany(e => e.RoleClaims)
			.WithOne(e => e.Role)
			.HasForeignKey(rc => rc.RoleId)
			.IsRequired();
	});
}
...
```
The ```ApplicationDbContext.cs``` file is a crucial part of a Blazor application with Radzen and .NET 8 that implements ```role-based authorization``` using the ```Identity``` framework. It defines the ```database context``` for the application and includes the configuration for the database tables and their relationships.  


##### **Login.razor**
The code for the ```login page``` in a Blazor Radzen application with .NET 8 and role-based authorization using Identity.  
<kbd>Login.razor</kbd>
```
@page "/Account/Login"
@layout AccountLayout

@using System.ComponentModel.DataAnnotations
@using BlazorAppRadzenRoleBasedAuthwithIdentity.Models.Identity
@using Microsoft.AspNetCore.Authentication
@using Microsoft.AspNetCore.Identity
@using BlazorAppRadzenRoleBasedAuthwithIdentity.Models

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

                <div class="mb-3">
                    <p>
                        <b>Admin User</b> <br />
                        email: admin@admin.com <br />
                        pass: 123456
                    </p>
                </div>

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
The ```InputModel``` class represents the model for the ```login form```. It includes properties for ```email, password, and remember me checkbox```.  
The ```LoginUser method``` is called when the user submits the ```login form```. It uses the ```SignInManager``` service to authenticate the user based on the provided ```email and password```.  


##### **Register.razor**
The ```Register page``` in a Blazor Radzen application with Role-based Authorization using Identity.  
<kbd>Register.razor</kbd>
```
@page "/Account/Register"
@layout AccountLayout

@using System.ComponentModel.DataAnnotations
@using System.Text
@using System.Text.Encodings.Web
@using BlazorAppRadzenRoleBasedAuthwithIdentity.Models.Identity
@using Microsoft.AspNetCore.Identity
@using Microsoft.AspNetCore.WebUtilities
@using BlazorAppRadzenRoleBasedAuthwithIdentity.Models

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

        await UserManager.AddToRoleAsync(user, "User");
        Logger.LogInformation("User added to 'User' role.");

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
Creates a new user using the ```CreateUser``` method.    
Creates the user with the provided password using the ```UserManager.CreateAsync``` method.  
Adds the user to the ```"User"``` role using the ```UserManager.AddToRoleAsync``` method.  
Signs in the ```user``` and redirects to the specified return URL.  


##### **Auth.razor**
```Auth.razor```, role-based authorization in a Blazor application using Radzen, .NET 8, and Identity. By checking the user's roles, you can control access to different parts of your application based on specific permissions.  
<kbd>Auth.razor</kbd>
```
@page "/auth"

@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@using System.Security.Claims;

@inject AuthenticationStateProvider authenticationStateProvider


@attribute [Authorize]

<PageTitle>Auth</PageTitle>

<h1>You are authenticated</h1>

<AuthorizeView>
    User: @context.User.Identity?.Name <br />
    User: @user?.Identity?.Name <br />
    Roles: @(string.Join(", ", userRoleNames)) <br />
</AuthorizeView>


@code {
    ClaimsPrincipal? user;
    string[]? userRoleNames;

    protected override async Task OnInitializedAsync()
    {
        var auth = await authenticationStateProvider.GetAuthenticationStateAsync();
        user = auth.User as ClaimsPrincipal;
        //var userIdentity = auth.User.Identity as ClaimsIdentity;

        
        userRoleNames = user.FindAll(ClaimTypes.Role).Select(x => x.Value).ToArray();

    }

}
```
The ```Authorize``` attribute is used to restrict access to the page to authenticated users only.  


##### **AuthAdminRole.razor**
By utilizing the ```Authorize``` attribute and ```ClaimsPrincipal```, the application restricts access to ```specific roles```, ensuring that only users with the ```"Admin"``` role can view the content of the page.  
<kbd>AuthAdminRole.razor</kbd>
```
@page "/auth-admin-role"

@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@using System.Security.Claims;

@inject AuthenticationStateProvider authenticationStateProvider


@attribute [Authorize(Roles = "Admin")]

<PageTitle>Auth</PageTitle>

<h1>You are authenticated (Admin)</h1>

<AuthorizeView>
    <Authorized>
        This is admin authorize page. <br />
        User: @context.User.Identity?.Name <br />
        User: @user?.Identity?.Name <br />
        Roles: @(string.Join(", ", userRoleNames)) <br />
    </Authorized>

    <NotAuthorized>
        You don't have permission to access this page. (Need Admin Role)
    </NotAuthorized>
</AuthorizeView>


@code {
    ClaimsPrincipal? user;
    string[]? userRoleNames;

    protected override async Task OnInitializedAsync()
    {
        var auth = await authenticationStateProvider.GetAuthenticationStateAsync();
        user = auth.User as ClaimsPrincipal;
        //var userIdentity = auth.User.Identity as ClaimsIdentity;


        userRoleNames = user.FindAll(ClaimTypes.Role).Select(x => x.Value).ToArray();

    }

}
```
Only users with the ```"Admin"``` role can access the content of the page.  



#### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppRadzenRoleBasedAuthwithIdentity
