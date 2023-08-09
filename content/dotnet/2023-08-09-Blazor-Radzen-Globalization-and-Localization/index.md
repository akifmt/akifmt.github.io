---
title: "Blazor Radzen Globalization and Localization"
date: 2023-08-09T00:00:00+00:00
hero: blazor_radzen_dotnet.jpg
description: Blazor Radzen Globalization and Localization
menu:
  dotnet:
    name: Blazor Radzen Globalization and Localization
    identifier: Blazor Radzen Globalization and Localization
    weight: -20230809
tags: [ Dotnet, Blazor Radzen Globalization and Localization]
categories: [ Dotnet, Blazor Radzen Globalization and Localization]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_radzen_dotnet.jpg" alt="blazor_radzen_dotnet" title="blazor_radzen_dotnet"><br>
<p>

##### **Blazor Radzen Globalization and Localization**
The concept of globalization and localization in Blazor, a popular framework for building web applications. We will specifically focus on how to implement globalization and localization using Radzen, a set of UI components for Blazor. We will explore the code provided and understand its functionality.

Globalization: Globalization refers to the process of designing and developing applications that can be adapted to various cultures and languages. It involves making your application culturally aware and providing support for different languages, date formats, number formats, and more.

Localization: Localization is the process of adapting an application to a specific culture or locale. It involves translating the user interface, date formats, number formats, and other elements of the application to match the preferences of a particular culture or language.

Culture: A culture represents a specific set of conventions, such as language, date format, and number format, associated with a particular region or group of people.

###### **CulturePicker.razor**
The ```CulturePicker``` component is a partial class that extends the ```ComponentBase``` class provided by Blazor. It contains several injected services and properties for language, currency, and dropdown data. The ```OnInitialized``` method is overridden to set the initial values for language and currency based on the current culture settings. The ```ChangeCulture``` method is called when the user selects a new language or currency, and it updates the culture settings and redirects the user to the appropriate page.

```
@inject Microsoft.Extensions.Localization.IStringLocalizer<CulturePicker> L

@*<RadzenDropDown @bind-Value="@culture" TValue="string" Data="@(cultureDrowDownData)"
                TextProperty="Text" ValueProperty="Value" Change="@ChangeCulture" />
*@

<RadzenText Text="Language" class="rz-my-1 rz-mx-2" />
<RadzenDropDown @bind-Value="@language" TValue="string" Data="@(languageDrowDownData)"
                TextProperty="Text" ValueProperty="Value" Change="@ChangeCulture" />

<RadzenText Text="Currency" class="rz-my-1 rz-ml-4 rz-mr-2" />
<RadzenDropDown @bind-Value="@currency" TValue="string" Data="@(currencyDrowDownData)"
                TextProperty="Text" ValueProperty="Value" Change="@ChangeCulture"
                class="rz-mr-2" />
```

The code begins with an ```@inject``` directive, which is used to inject an instance of the ```IStringLocalizer``` interface into the ```CulturePicker``` component. The ```IStringLocalizer``` interface provides a way to retrieve localized strings for the current culture.

Next, we have a commented out code block that represents a ```RadzenDropDown``` component. This component is used to display a dropdown list of cultures, allowing the user to select their preferred language. The ```@bind-Value``` directive is used to bind the selected value to the culture variable. The ```TValue``` parameter specifies the type of the selected value, which in this case is a string. The ```Data``` parameter provides the data source for the dropdown list, and the ```TextProperty and ValueProperty``` parameters specify the properties of the data source that should be used for displaying the text and storing the selected value, respectively. The ```Change``` parameter specifies the event handler that should be called when the selected value changes.

After the commented out code block, we have two ```RadzenText``` components. These components are used to display labels for the dropdown lists. The ```Text``` parameter specifies the text to be displayed, and the ```class``` parameter is used to apply CSS classes for styling.

Finally, we have two more ```RadzenDropDown``` components. These components are similar to the commented out code block, but they are used to display dropdown lists for selecting the preferred language and currency. The ```@bind-Value``` directive is used to bind the selected values to the ```language and currency``` variables, respectively. The ```class``` parameter is used to apply CSS classes for styling.

###### **CulturePicker.razor.cs**
Initializing language and currency:
```
protected override void OnInitialized()
{
    language = CultureInfo.CurrentCulture.TwoLetterISOLanguageName;
    currency = CultureInfo.CurrentCulture.Name.ReplaceFirst(quot;{CultureInfo.CurrentCulture.TwoLetterISOLanguageName}-", "");
}
```

Changing the culture:
```
protected void ChangeCulture()
{
    var redirect = new Uri(NavigationManager.Uri).GetComponents(UriComponents.PathAndQuery | UriComponents.Fragment, UriFormat.UriEscaped);

    string culture = quot;{language}-{currency}";

    var query = quot;?culture={Uri.EscapeDataString(culture)}&redirectUri={redirect}";

    NavigationManager.NavigateTo("Culture/SetCulture" + query, forceLoad: true);
}
```

###### **CultureController.cs**
The ```CultureController.cs``` file is a C# code file that contains a controller class responsible for handling culture-related operations in a Blazor application. It provides a method called ```SetCulture``` that allows the user to set the culture of the application and redirect to a specified URI.

The ```CultureController.cs``` file is a partial class that extends the ```Controller``` class provided by the ```Microsoft.AspNetCore.Mvc``` namespace. It is decorated with the ```[Route]``` attribute, which specifies the route prefix for the controller's actions.

The controller contains a single action method called ```SetCulture```, which takes two parameters: ```culture and redirectUri```. The ```culture``` parameter represents the desired culture to be set, and the ```redirectUri``` parameter represents the URI to which the user should be redirected after setting the culture.

```
using Microsoft.AspNetCore.Localization;
using Microsoft.AspNetCore.Mvc;

namespace BlazorAppRadzenGlobalizationLocalization.Controllers
{
    [Route("Culture/[action]")]
    public partial class CultureController : Controller
    {
        public IActionResult SetCulture(string culture, string redirectUri)
        {
            if (culture != null)
            {
                Response.Cookies.Append(
                    CookieRequestCultureProvider.DefaultCookieName,
                    CookieRequestCultureProvider.MakeCookieValue(new RequestCulture(culture)));
            }

            return LocalRedirect(redirectUri);
        }
    }
}
```

In this code, we import the necessary namespaces ```Microsoft.AspNetCore.Localization``` and ```Microsoft.AspNetCore.Mvc``` to access the required classes and methods.

The ```CultureController``` class is defined as a partial class and inherits from the ```Controller``` class provided by the ```Microsoft.AspNetCore.Mvc``` namespace.

The ```[Route]``` attribute is used to specify the route prefix for the controller's actions. In this case, the route prefix is set to "Culture/[action]". This means that the ```SetCulture``` action can be accessed using the URL ```"/Culture/SetCulture"```.

The ```SetCulture``` action method takes two parameters: ```culture and redirectUri```. The ```culture``` parameter represents the desired culture to be set, and the ```redirectUri``` parameter represents the URI to which the user should be redirected after setting the culture.

Inside the ```SetCulture``` method, there is a conditional statement that checks if the ```culture``` parameter is not null. If it is not null, it means that a valid culture value has been provided.

The ```Response.Cookies.Append``` method is then called to add a cookie to the response. This cookie is used to store the selected culture value. The ```CookieRequestCultureProvider.DefaultCookieName``` property is used to specify the name of the cookie, and the ```CookieRequestCultureProvider.MakeCookieValue``` method is used to convert the culture value into a string representation that can be stored in the cookie.

Finally, the ```LocalRedirect``` method is called to redirect the user to the specified ```redirectUri``` after setting the culture.


##### **Source**
Full source code is available at this repository in GitHub: 
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppRadzenGlobalizationLocalization