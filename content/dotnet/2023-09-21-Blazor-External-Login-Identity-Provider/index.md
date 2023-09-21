---
title: "Blazor External Login Identity Provider Google, Facebook, Microsoft, Twitter"
date: 2023-09-21T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor External Login Identity Provider Google, Facebook, Microsoft, Twitter
menu:
  dotnet:
    name: Blazor External Login Identity Provider Google, Facebook, Microsoft, Twitter
    identifier: Blazor-external-login-identity-provider
    weight: -20230921
tags: [Dotnet, Blazor External Login, Identity Provider Google Facebook Microsoft Twitter]
categories: [Dotnet, Blazor External Login, Identity Provider Google Facebook Microsoft Twitter]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet" style="border-radius: 20px;"><br>
<p>

#### **Blazor External Login Identity Provider Google, Facebook, Microsoft, Twitter**

we will explore how to configure external login identity providers in a Blazor application using the appsettings.json file. We will specifically focus on configuring ```Google, Facebook, Microsoft, and Twitter as external login providers```.

Blazor: Blazor is a web framework that allows developers to build interactive web UIs using C# instead of JavaScript. It enables full-stack development with .NET.

Authentication: Authentication is the process of verifying the ```identity of a user```. Blazor provides built-in support for authentication, allowing you to authenticate users using various identity providers.

External Login Identity Providers: These are third-party services that allow users to authenticate and log in to your application using their existing credentials from platforms like ```Google, Facebook, Microsoft, or Twitter```.

Identity Providers: Identity providers are third-party services that handle user authentication. Examples of identity providers include Google, Facebook, Microsoft, and Twitter. These providers allow users to log in to your application using their existing accounts.

appsettings.json: The ```appsettings.json``` file is a configuration file used in ASP.NET Core applications to store application settings. It allows you to separate configuration from code, making it easier to manage and modify settings without redeploying the application.

##### **appsettings.json**
To configure ```external login``` identity providers in a Blazor application, you need to modify the ```appsettings.json``` file. The ```Authentication``` section of the ```appsettings.json``` file contains the configuration for each identity provider.
```
"Authentication": {
    "Google": {
      "ClientId": "ClientId",
      "ClientSecret": "ClientSecret"
    },
    "Facebook": {
      "ClientId": "ClientId",
      "ClientSecret": "ClientSecret"
    },
    "Microsoft": {
      "ClientId": "ClientId",
      "ClientSecret": "ClientSecret"
    },
    "Twitter": {
      "ConsumerAPIKey": "ConsumerAPIKey",
      "ConsumerSecret": "ConsumerSecret"
    }
  },
```
Each identity provider (Google, Facebook, Microsoft, and Twitter) is represented as a nested object within the ```Authentication``` section. Each provider has its own ```ClientId and ClientSecret or ConsumerAPIKey and ConsumerSecret``` values, which are obtained from the respective identity provider's developer console.


##### **Program.cs**
The AddAuthentication() method is used to configure the authentication services.
```
// add external logins
var config = builder.Configuration;
builder.Services.AddAuthentication()
   .AddGoogle(options =>
   {
	   IConfigurationSection googleAuthSection = config.GetSection("Authentication:Google");
	   options.ClientId = googleAuthSection["ClientId"];
	   options.ClientSecret = googleAuthSection["ClientSecret"];
   })
   .AddFacebook(options =>
   {
	   IConfigurationSection facebookAuthSection = config.GetSection("Authentication:Facebook");
	   options.ClientId = facebookAuthSection["ClientId"];
	   options.ClientSecret = facebookAuthSection["ClientSecret"];
	   options.Scope.Add("email");
	   options.Scope.Add("public_profile");
   })
   .AddMicrosoftAccount(microsoftOptions =>
   {
	   IConfigurationSection microsoftAuthSection = config.GetSection("Authentication:Microsoft");
	   microsoftOptions.ClientId = microsoftAuthSection["ClientId"];
	   microsoftOptions.ClientSecret = microsoftAuthSection["ClientSecret"];
   })
   .AddTwitter(twitterOptions =>
   {
	   IConfigurationSection twitterAuthSection = config.GetSection("Authentication:Twitter");
	   twitterOptions.ConsumerKey = twitterAuthSection["ConsumerAPIKey"];
	   twitterOptions.ConsumerSecret = twitterAuthSection["ConsumerSecret"];
	   twitterOptions.RetrieveUserDetails = true;
   });
```
The ```AddGoogle()``` method adds ```Google``` as an external login provider. It retrieves the ```client ID and client secret``` from the configuration file.

The ```AddFacebook()``` method adds ```Facebook``` as an external login provider. It also retrieves the ```client ID and client secret``` from the configuration file. Additionally, it specifies the scopes for which the application requests access.

The ```AddMicrosoftAccount()``` method adds ```Microsoft``` as an external login provider. It retrieves the ```client ID and client secret``` from the configuration file.

The ```AddTwitter()``` method adds ```Twitter``` as an external login provider. It retrieves the ```consumer key and consumer secret``` from the configuration file. It also enables the retrieval of user details.



##### **ExternalLogin.cshtml.cs**
The ```ExternalLogin.cshtml.cs``` file, which handles the logic for confirming and creating user accounts using ```external login``` providers.
```
public async Task<IActionResult> OnPostConfirmationAsync(string returnUrl = null)
{
	returnUrl = returnUrl ?? Url.Content("~/");
	// Get the information about the user from the external login provider
	var info = await _signInManager.GetExternalLoginInfoAsync();
  
	if (ModelState.IsValid)
	{
		var user = CreateUser();

		// fill user info (name, givenname, surname ...)
		FillExternalProviderUserInfo(user, info);

		var result = await _userManager.CreateAsync(user);
		if (result.Succeeded)
		{
			result = await _userManager.AddLoginAsync(user, info);
			if (result.Succeeded)
			{
				_logger.LogInformation("User created an account using {Name} provider.", info.LoginProvider);

				// Add User role
				await _userManager.AddToRoleAsync(user, "User");

				var userId = await _userManager.GetUserIdAsync(user);
				var code = await _userManager.GenerateEmailConfirmationTokenAsync(user);
				code = WebEncoders.Base64UrlEncode(Encoding.UTF8.GetBytes(code));
				var callbackUrl = Url.Page(
					"/Account/ConfirmEmail",
					pageHandler: null,
					values: new { area = "Identity", userId = userId, code = code },
					protocol: Request.Scheme);

				await _emailSender.SendEmailAsync(Input.Email, "Confirm your email",
					$"Please confirm your account by <a href='{HtmlEncoder.Default.Encode(callbackUrl)}'>clicking here</a>.");

				await _signInManager.SignInAsync(user, isPersistent: false, info.LoginProvider);
				return LocalRedirect(returnUrl);
			}
		}
	}

	ProviderDisplayName = info.ProviderDisplayName;
	ReturnUrl = returnUrl;
	return Page();
}

public static class SupportedExternalLoginProviderNames
{
	public const string GOOGLE = "Google";
	public const string FACEBOOK = "Facebook";
	public const string MICROSOFT = "Microsoft";
	public const string TWITTER = "Twitter";
}

private void FillExternalProviderUserInfo(ApplicationUser user, ExternalLoginInfo info)
{
	if (info is null || string.IsNullOrEmpty(info.LoginProvider))
		return;

	switch (info.LoginProvider)
	{
		case SupportedExternalLoginProviderNames.GOOGLE:
			Claim gname = info.Principal.Claims.FirstOrDefault(c => c.Type == ClaimTypes.Name);
			if (gname is not null)
				user.Name = gname.Value;
			Claim ggivenName = info.Principal.Claims.FirstOrDefault(c => c.Type == ClaimTypes.GivenName);
			if (ggivenName is not null)
				user.GivenName = ggivenName.Value;
			Claim gsurname = info.Principal.Claims.FirstOrDefault(c => c.Type == ClaimTypes.Surname);
			if (gsurname is not null)
				user.Surname = gsurname.Value;
			break;

		case SupportedExternalLoginProviderNames.FACEBOOK:
			Claim fname = info.Principal.Claims.FirstOrDefault(c => c.Type == "urn:facebook:name");
			if (fname is not null)
				user.Name = fname.Value;
			Claim fgivenName = info.Principal.Claims.FirstOrDefault(c => c.Type == "urn:facebook:first_name");
			if (fgivenName is not null)
				user.Name = fgivenName.Value;
			Claim fsurname = info.Principal.Claims.FirstOrDefault(c => c.Type == "urn:facebook:last_name");
			if (fsurname is not null)
				user.Name = fsurname.Value;
			break;

		case SupportedExternalLoginProviderNames.MICROSOFT:
			Claim mname = info.Principal.Claims.FirstOrDefault(c => c.Type == "displayName");
			if (mname is not null)
				user.Name = mname.Value;
			Claim mgivenName = info.Principal.Claims.FirstOrDefault(c => c.Type == "first_name");
			if (mgivenName is not null)
				user.GivenName = mgivenName.Value;
			Claim msurname = info.Principal.Claims.FirstOrDefault(c => c.Type == "last_name");
			if (msurname is not null)
				user.Surname = msurname.Value;
			break;

		case SupportedExternalLoginProviderNames.TWITTER:
			Claim tname = info.Principal.Claims.FirstOrDefault(c => c.Type == "screen_name");
			if (tname is not null)
				user.Name = tname.Value;
			break;

		default:
			break;
	}
}
```

Configuring ```external login``` identity providers in a Blazor application is essential to provide users with the option to authenticate using their existing credentials from popular platforms like ```Google, Facebook, Microsoft, or Twitter```. By using the ```appsettings.json``` file, you can easily configure these providers by specifying the required ```client IDs and client secrets```.

#### **Source**
Full source code is available at this repository in GitHub:
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppExternalLogin
