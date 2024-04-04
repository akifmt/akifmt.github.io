---
title: "Blazor Resize and Upload Images"
date: 2023-08-25T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Resize and Upload Images
menu:
  dotnet:
    name: Blazor Resize and Upload Images
    identifier: blazor-resize-and-upload-images
    weight: -20230825
tags: [Dotnet, Blazor Resize and Upload Images]
categories: [Dotnet, Blazor Resize and Upload Images]

author:
  name: Akif T.
---

<p class="d-flex justify-content-center">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

#### **Blazor Resize and Upload Images**
Upload and save images in a Blazor application using C#. We will cover the process of ```uploading an image```, ```resizing``` it if necessary, ```generating a random file name```, and saving it to the server's ```wwwroot``` directory.

InputFile: The ```InputFile``` component in Blazor allows users to select files from their local system. It triggers an event when the file selection changes.

File Size Limit: We can set a maximum file size limit to restrict the size of the uploaded image. In our example, we have set the limit to 800KB.

Image Resizing: We can resize the uploaded image to a specific width and height if needed. In our code, we have commented out the image resizing logic, but you can uncomment it and modify it according to your requirements.

Random File Name: To avoid naming conflicts, we generate a random file name for the uploaded image. We use the ```Guid.NewGuid().ToString("N")``` method to generate a unique identifier and combine it with the file extension.

Saving to wwwroot: We save the uploaded image to the ```wwwroot``` directory of the Blazor application. This allows us to access the image using a relative path in the application.

##### **Create.razor**
Blazor component that handles the creation of a blog post. It includes an ```EditForm``` component that binds to a ```BlogPostViewModel``` object.

HTML Markup: The HTML markup defines the form elements for creating a blog post. It includes an ```InputFile``` component for selecting the image, a preview section to display the selected image, and a submit button.

Code-behind: The code-behind section contains the C# logic for handling the image upload and saving process. It includes event handlers for the ```OnChange``` event of the ```InputFile``` component and the submit button's ```OnValidSubmit``` event.

```
@page "/BlogPost/Create"

<PageTitle>Create</PageTitle>

<h1>Create</h1>
<h4>BlogPost</h4>
<hr />
@if (blogPost == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <div class="row">
        <div class="col-md-4">
            <EditForm Model="@blogPost" OnValidSubmit="@HandleValidSubmit" Context="createBlogPost">
                <DataAnnotationsValidator />
                <ValidationSummary />
                <div class="form-group">
                    <label class="control-label">Preview</label>
                    @if (blogPost is not null && !string.IsNullOrEmpty(blogPost.PostImage))
                    {
                        <div class="form-control">
                            @*<img src="data:@selectedImage.ContentType;base64,@selectedImage.Base64data" height="200" />*@
                            <img src="/images/@blogPost.PostImage"
                                 height="200" />
                        </div>
                    }
                </div>
                <div class="form-group">
                    <label class="control-label">@nameof(BlogPostViewModel.PostImage)</label>
                    <InputFile OnChange="OnChange"
                               id="upload"
                               class="form-control"
                               accept="image/png, image/jpeg" />
                    <InputText @bind-Value="blogPost.PostImage" readonly class="form-control" />
                    <p class="text-muted mb-0">Allowed JPG, GIF or PNG. Max size of 800K (1024x1024 pixels)</p>
                </div>
                <div class="form-group">
                    <label class="control-label">@nameof(BlogPostViewModel.Title)</label>
                    <InputText @bind-Value="blogPost.Title" class="form-control" />
                    <ValidationMessage For="@(() => blogPost.Title)" class="text-danger" />
                </div>
                <div class="form-group">
                    <label class="control-label">@nameof(BlogPostViewModel.Content)</label>
                    <InputText @bind-Value="blogPost.Content" class="form-control" />
                    <ValidationMessage For="@(() => blogPost.Content)" class="text-danger" />
                </div>
                <div class="form-group">
                    <input type="submit" value="Create" class="btn btn-primary" />
                </div>
            </EditForm>
        </div>
    </div>
    <div>
        <a href="/BlogPost">Back to List</a>
    </div>
}

@code {
    private BlogPostViewModel? blogPost;
    protected override void OnInitialized()
    {
        blogPost = new();
    }
    async Task OnChange(InputFileChangeEventArgs e)
    {
        if (blogPost is null) return;

        var files = e.GetMultipleFiles();
        if (files.Count > 0)
        {
            var file = files[0];
            if (file.Size > (1024 * 800)) // MAX FILE SIZE: < 800KB
                return;

            // resize image
            //var resizedFile = await file.RequestImageFileAsync(file.ContentType, 1024, 1024); // resize the image file
            var resizedFile = file;

            //generate random imagename
            string imagename = Guid.NewGuid().ToString("N") + Path.GetExtension(file.Name);
            // save image to directory
            string imagePath = Path.Combine(System.Environment.CurrentDirectory, "wwwroot", "images");
            string imageFullName = Path.Combine(imagePath, imagename);

            await using FileStream fs = new(imageFullName, FileMode.Create);
            await resizedFile.OpenReadStream().CopyToAsync(fs);
            //await File.WriteAllBytesAsync(imageFullName, selectedImage.Data);

            blogPost.PostImage = imagename;

            StateHasChanged();
        }
    }
    private async void HandleValidSubmit()
    {
        if (blogPost is null) return;

        var model = Mapper.Map<BlogPostViewModel, BlogPost>(blogPost);
        bool result = await BlogPostService.AddBlogPostAsync(model);
        if (result)
            NavigationManager.NavigateTo("/BlogPost");
    }
}
```

We explored how to ```upload and save images``` in a ```Blazor application``` using C#. We covered the key concepts of image uploading, resizing, generating random file names, and saving images to the ```wwwroot``` directory. By following the code examples provided, you can implement image uploading functionality in your own Blazor projects.


#### **Source**
Full source code is available at this repository in GitHub:  
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppResizeUploadImages
