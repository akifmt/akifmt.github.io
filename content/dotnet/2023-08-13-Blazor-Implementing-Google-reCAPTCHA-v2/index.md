---
title: "Blazor Implementing Google reCAPTCHA v2"
date: 2023-08-13T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Implementing Google reCAPTCHA v2
menu:
  dotnet:
    name: Blazor Implementing Google reCAPTCHA v2
    identifier: blazor-implementing-google-reCAPTCHA-v2
    weight: -20230813
tags: [ Dotnet, Implementing Google reCAPTCHA v2]
categories: [ Dotnet, Implementing Google reCAPTCHA v2]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

#### **Blazor Implementing Google reCAPTCHA v2**
How to implement Google reCAPTCHA v2 in a Blazor application using C#. Google reCAPTCHA is a free service provided by Google that helps protect websites from spam and abuse. It uses advanced risk analysis techniques to distinguish between humans and bots.

Site Key: A unique key provided by Google when you register your website to use reCAPTCHA. It is used to identify your website when making API requests.

Secret Key: A unique key provided by Google when you register your website to use reCAPTCHA. It is used to authenticate your server-side API requests.

reCAPTCHA Response: The response generated by the reCAPTCHA widget when a user completes the reCAPTCHA challenge. This response is sent to the server for verification.

Verification Response: The response received from the Google reCAPTCHA API after verifying the reCAPTCHA response. It contains information about the success of the verification and any error codes, if applicable.

##### **GooglereCAPTCHAv3Service.cs**
```GooglereCAPTCHAv2Service``` class is responsible for verifying the reCAPTCHA response using the Google reCAPTCHA API. Let's break down the code structure:

```
namespace BlazorAppreCAPTCHAv2.Data;

public class GooglereCAPTCHAv2Service
{
    public async Task<(bool Success, string[]? ErrorCodes)> Post(string reCAPTCHAResponse)
    {
        var url = "https://www.google.com/recaptcha/api/siteverify";
        var content = new FormUrlEncodedContent(new Dictionary<string, string>
            {
                {"secret", GooglereCAPTCHAv2Settings.SecretKey},
                {"response", reCAPTCHAResponse}
            });

        GooglereCAPTCHAv2Response? verificationResponse;
        using (HttpClient httpClient = new HttpClient())
        {
            try
            {
                var response = await httpClient.PostAsync(url, content);
                response.EnsureSuccessStatusCode();
                verificationResponse = await response.Content.ReadFromJsonAsync<GooglereCAPTCHAv2Response>();
            }
            catch (Exception)
            {
                throw;
            }
        }

        if (verificationResponse is null || !verificationResponse.Success)
            return (false, verificationResponse?.ErrorCodes.Select(err => err.Replace('-', ' ')).ToArray());

        return (Success: true, ErrorCodes: null);
    }
}
```

The ```Post``` method takes the reCAPTCHA response as input and returns a tuple containing a boolean value indicating the success of the verification and an array of error codes, if any.

The ```url``` variable stores the URL of the Google reCAPTCHA API endpoint.

The ```content``` variable is created using the ```FormUrlEncodedContent``` class, which represents key-value pairs encoded in the ```application/x-www-form-urlencoded``` format. It contains the ```secret key``` (your server-side secret key) and the ```response key``` (the reCAPTCHA response).

The ```verificationResponse``` variable is used to store the response received from the Google reCAPTCHA API after verifying the reCAPTCHA response.

The ```HttpClient``` class is used to send an HTTP POST request to the Google reCAPTCHA API endpoint. The ```PostAsync``` method is called with the URL and content as parameters.

The ```response.EnsureSuccessStatusCode()``` method ensures that the HTTP response is successful (status code 200).

The ```response.Content.ReadFromJsonAsync<GooglereCAPTCHAv2Response>()``` method reads the response content and deserializes it into an instance of the ```GooglereCAPTCHAv2Response``` class.

If the verification response is null or the verification was not successful, the method returns a tuple with the success value set to false and the error codes extracted from the verification response.

If the verification was successful, the method returns a tuple with the success value set to true and the error codes set to null.

##### **GooglereCAPTCHAv2Settings.cs**
To implement Google reCAPTCHA v2 in Blazor, we need to create a class called ```GooglereCAPTCHAv2Settings``` in the ```BlazorAppreCAPTCHAv2.Data``` namespace. This class will contain the site key and secret key provided by Google reCAPTCHA.
```
namespace BlazorAppreCAPTCHAv2.Data;

public class GooglereCAPTCHAv2Settings
{
    public const string SiteKey = "YOUR_GOOGLE_reCAPTCHAv2_SITEKEY";
    public const string SecretKey = "YOUR_GOOGLE_reCAPTCHAv2_SECRETKEY";
}
```

##### **GooglereCAPTCHAv2Response.cs**
 ```GooglereCAPTCHAv2Response.cs``` file represents a C# class that models the response received from the Google reCAPTCHA API. Let's break down the code structure:
```
using System.Text.Json.Serialization;

namespace BlazorAppreCAPTCHAv2.Data;

public class GooglereCAPTCHAv2Response
{
    public bool Success { get; set; }

    // timestamp of the challenge load (ISO format yyyy-MM-dd'T'HH:mm:ssZZ)
    [JsonPropertyName("challenge_ts")]
    public DateTimeOffset ChallengeTimestamp { get; set; }

    // the hostname of the site where the reCAPTCHA was solved
    public string Hostname { get; set; }

    [JsonPropertyName("error-codes")]
    public string[] ErrorCodes { get; set; } = new string[0];
}
```
The ```GooglereCAPTCHAv2Response``` class has the following properties:

Success: A boolean property indicating whether the reCAPTCHA challenge was successfully completed by the user.

ChallengeTimestamp: A ```DateTimeOffset``` property representing the timestamp of the challenge load in ISO format.

Hostname: A string property representing the hostname of the site where the reCAPTCHA was solved.

ErrorCodes: An array of strings representing any error codes returned by the reCAPTCHA API. This property is initialized with an empty array by default.

##### **script.js**
a JavaScript module that provides functions for initializing and rendering the reCAPTCHA widget, as well as getting the user's response.
```
var My;
(function (My) {
    var reCAPTCHA;
    (function (reCAPTCHA) {
        let scriptLoaded = null;
        function waitScriptLoaded(resolve) {
            if (typeof (grecaptcha) !== 'undefined' && typeof (grecaptcha.render) !== 'undefined')
                resolve();
            else
                setTimeout(() => waitScriptLoaded(resolve), 100);
        }
        function init() {
            const scripts = Array.from(document.getElementsByTagName('script'));
            if (!scripts.some(s => (s.src || '').startsWith('https://www.google.com/recaptcha/api.js'))) {
                const script = document.createElement('script');
                script.src = 'https://www.google.com/recaptcha/api.js?render=explicit';
                script.async = true;
                script.defer = true;
                document.head.appendChild(script);
            }
            if (scriptLoaded === null)
                scriptLoaded = new Promise(waitScriptLoaded);
            return scriptLoaded;
        }
        reCAPTCHA.init = init;
        function render(dotNetObj, selector, siteKey) {
            return grecaptcha.render(selector, {
                'sitekey': siteKey,
                'callback': (response) => { dotNetObj.invokeMethodAsync('CallbackOnSuccess', response); },
                'expired-callback': () => { dotNetObj.invokeMethodAsync('CallbackOnExpired'); }
            });
        }
        reCAPTCHA.render = render;
        function getResponse(widgetId) {
            return grecaptcha.getResponse(widgetId);
        }
        reCAPTCHA.getResponse = getResponse;
    })(reCAPTCHA = My.reCAPTCHA || (My.reCAPTCHA = {}));
})(My || (My = {}));
```

waitScriptLoaded: This function checks if the reCAPTCHA script has been loaded. If not, it waits for it to be loaded before resolving the promise.

init: This function initializes the reCAPTCHA script by adding it to the HTML document if it hasn't been added already. It returns a promise that resolves when the script has been loaded.

render: This function renders the reCAPTCHA widget using the provided ```dotNetObj, selector, and siteKey```. It also handles the callback functions for success and expiration.

getResponse: This function gets the user's response from the reCAPTCHA widget using the provided ```widgetId```.

##### **ReCAPTCHAv2Component.razor**
It is responsible for rendering the reCAPTCHA widget and handling the user's response. Let's break down the code structure:
```
@using System.ComponentModel
@inject IJSRuntime JS


<span>@loadMessage</span>
<div id="@UniqueId"></div>


@code {

    [Parameter]
    public string SiteKey { get; set; }

    [Parameter]
    public EventCallback<string> OnSuccess { get; set; }

    [Parameter]
    public EventCallback OnExpired { get; set; }

    private bool isLoadedScript = false;
    private string loadMessage = "Loading...";

    private string UniqueId = Guid.NewGuid().ToString();

    private int WidgetId;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            try
            {
                var timeoutTimeSpan = new TimeSpan(0, 0, 5); // 5 secs
                var ress = await JS.InvokeAsync<object>("My.reCAPTCHA.init", timeout: timeoutTimeSpan);
                this.WidgetId = await JS.InvokeAsync<int>("My.reCAPTCHA.render", timeout: timeoutTimeSpan, DotNetObjectReference.Create(this), UniqueId, SiteKey);
                isLoadedScript = true;
                loadMessage = "";
            }
            catch (Exception ex)
            {
                loadMessage = "Error on Load Captcha.";
                isLoadedScript = false;
            }

            StateHasChanged();
        }
    }

    [JSInvokable, EditorBrowsable(EditorBrowsableState.Never)]
    public void CallbackOnSuccess(string response)
    {
        if (OnSuccess.HasDelegate)
        {
            OnSuccess.InvokeAsync(response);
        }
    }

    [JSInvokable, EditorBrowsable(EditorBrowsableState.Never)]
    public void CallbackOnExpired()
    {
        if (OnExpired.HasDelegate)
        {
            OnExpired.InvokeAsync(null);
        }
    }

    public ValueTask<string> GetResponseAsync()
    {
        return JS.InvokeAsync<string>("My.reCAPTCHA.getResponse", WidgetId);
    }
}
```

@using System.ComponentModel and @inject IJSRuntime JS: These directives import the necessary namespaces and inject the JavaScript runtime service into the component.

@code block: This block contains the C# code for the component.

SiteKey property: This property is used to pass the site key value to the component. The site key is required to initialize the reCAPTCHA widget.

OnSuccess and OnExpired event callbacks: These event callbacks are used to handle the success and expiration events of the reCAPTCHA widget.

UniqueId field: This field stores a unique identifier generated using the ```Guid.NewGuid().ToString()``` method. It is used to identify the reCAPTCHA widget on the web page.

OnAfterRenderAsync method: This method is called after the component has been rendered on the web page. It initializes the reCAPTCHA widget by invoking JavaScript functions and sets the ```isLoadedScript and loadMessage``` variables based on the success or failure of the initialization.

CallbackOnSuccess and CallbackOnExpired methods: These methods are invoked by the reCAPTCHA widget's JavaScript API when the user successfully completes the challenge or when the challenge expires. They invoke the respective event callbacks ```(OnSuccess and OnExpired)``` if they are subscribed to.

GetResponseAsync method: This method is used to retrieve the user's response from the reCAPTCHA widget. It invokes a JavaScript function to get the response and returns it as a ```ValueTask<string>```.

##### **ReCAPTCHAv2.razor**
```
@page "/ReCAPTCHAv2"

@using BlazorAppreCAPTCHAv2.Data;
@using BlazorAppreCAPTCHAv2.ViewModels;
@using BlazorAppreCAPTCHAv2.Components;

@inject IJSRuntime JSRuntime
@inject GooglereCAPTCHAv2Service GooglereCAPTCHAv2Service

<PageTitle>reCAPTCHAv2</PageTitle>

<EditForm Model="@blogPostViewModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <div class="form-group">
        <label class="control-label">@nameof(BlogPostViewModel.Title)</label>
        <InputText @bind-Value="blogPostViewModel.Title" class="form-control" />
        <ValidationMessage For="@(() => blogPostViewModel.Title)" class="text-danger" />
    </div>

    <div class="form-group mt-3">
        <label class="control-label">@nameof(BlogPostViewModel.Content)</label>
        <InputText @bind-Value="blogPostViewModel.Content" class="form-control" />
        <ValidationMessage For="@(() => blogPostViewModel.Content)" class="text-danger" />
    </div>

    <div class="mt-3">
        <ReCAPTCHAv2Component @ref="reCAPTCHAv2Component" SiteKey="@GooglereCAPTCHAv2Settings.SiteKey" OnSuccess="OnSuccess" OnExpired="OnExpired" />
    </div>

    <div class="form-group mt-3">
        <input type="submit" value="Send" disabled="@DisablePostButton" class="btn btn-primary" />
    </div>

</EditForm>


<div class="form-group mt-3">
    <label class="control-label text-primary"><strong>
            Result: @resultMessage
    </strong></label>
</div>


@code {
    BlogPostViewModel? blogPostViewModel;

    private ReCAPTCHAv2Component? reCAPTCHAv2Component;

    private bool ValidReCAPTCHA = false;

    private bool DisablePostButton => !ValidReCAPTCHA;

    private void OnSuccess() => ValidReCAPTCHA = true;

    private void OnExpired() => ValidReCAPTCHA = false;

    private string resultMessage = "";

    protected override void OnInitialized()
    {
        blogPostViewModel = new();
    }

    private async Task HandleValidSubmit()
    {
        // verify ReCAPTCHAv2
        bool verifiedResult = await CheckReCAPTCHA();
        if (!verifiedResult)
            return;

        // verified actions
        resultMessage = "Success...";
        StateHasChanged();
    }

    private async Task<bool> CheckReCAPTCHA()
    {
        bool serverVerified = false;
        if (ValidReCAPTCHA)
        {
            var response = await reCAPTCHAv2Component.GetResponseAsync();
            try
            {
                StateHasChanged();
                var result = await GooglereCAPTCHAv2Service.Post(response);
                if (result.Success)
                {
                    serverVerified = true;
                }
                else
                {
                    await JSRuntime.InvokeAsync<object>("alert", string.Join(", ", result.ErrorCodes));
                    serverVerified = false;
                    StateHasChanged();
                }
            }
            catch (HttpRequestException e)
            {
                await JSRuntime.InvokeAsync<object>("alert", e.Message);
                serverVerified = false;
                StateHasChanged();
            }
        }
        return serverVerified;
    }

}
```
The ```OnSuccess``` method is called when the user successfully completes the reCAPTCHA challenge. It sets the ```ValidReCAPTCHA``` flag to true.

The ```OnExpired``` method is called when the reCAPTCHA challenge expires. It sets the ```ValidReCAPTCHA``` flag to false.

The ```HandleValidSubmit``` method is called when the form is submitted and passes the validation. It first verifies the reCAPTCHA response by calling the ```CheckReCAPTCHA``` method. If the verification fails, the method returns. Otherwise, it performs the desired actions and updates the ```resultMessage``` variable.

The ```CheckReCAPTCHA``` method checks if the reCAPTCHA response is valid. If it is, it retrieves the response token from the reCAPTCHA component and sends it to the Google reCAPTCHA service for verification. If the verification is successful, the ```serverVerified``` flag is set to true. Otherwise, an alert is displayed with the error codes returned by the service.

We have learned how to implement Google reCAPTCHA v2 in a Blazor application. By following the code examples and understanding the key concepts, you can enhance the security of your forms and protect your website from spam and abuse. Remember to obtain a site key from Google reCAPTCHA and replace the placeholder in the code with your actual site key. Happy coding!


#### **Source**
Full source code is available at this repository in GitHub: 
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppreCAPTCHAv2