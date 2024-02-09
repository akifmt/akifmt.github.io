---
title: "Blazor .NET 8 Object Mapping with AutoMapper"
date: 2024-02-09T00:00:00+00:00
hero: blazor_dotnet8_automapper.jpg
description: Blazor .NET 8 Object Mapping with AutoMapper
menu:
  dotnet:
    name: Blazor .NET 8 Object Mapping with AutoMapper
    identifier: blazor-dotnet8-object-mapping-with-automapper
    weight: -20240209
tags: [dotnet8, .NET8, Blazor, Object Mapping with AutoMapper]
categories: [dotnet8, .NET8, Object Mapping with AutoMapper]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet8_automapper.jpg" alt="blazor_dotnet8_automapper" title="blazor_dotnet8_automapper" style="border-radius: 20px;"><br>
<p>

#### **Blazor .NET 8 Object Mapping with AutoMapper**

How to perform object mapping in a Blazor application using ```AutoMapper```. Object mapping is the process of converting one object type to another, which can be useful when transferring data between different layers of an application or when displaying data in a different format.

AutoMapper: AutoMapper is a popular ```object-to-object``` mapping library in the .NET ecosystem. It simplifies the process of mapping one object to another by automatically matching properties with similar names and types.

Mapping Profile: A mapping profile is a class that defines the ```mapping configuration``` between the ```source``` and ```destination``` objects. It specifies how properties should be mapped and can be customized to handle complex mapping scenarios.

To use ```AutoMapper``` in a ```Blazor .NET 8``` application, we need to follow these steps:
- Install the ```AutoMapper``` ```NuGet``` package.
- Define ```mapping``` configurations in a ```mapping``` profile class.
- Register the ```mapping profile``` in the application startup.
- Use the ```Mapper.Map``` method to perform the object mapping.

##### **Program.cs**
```
internal class Program
{
    private static void Main(string[] args)
    {
        ...
        var mapperConfiguration = new MapperConfiguration(configuration =>
        {
            var profile = new MappingProfile();
            configuration.AddProfile(profile);
        });
        var mapper = mapperConfiguration.CreateMapper();
        builder.Services.AddSingleton(mapper);
		...
    }
}

public class MappingProfile : Profile
{
    public MappingProfile()
    {
        CreateMap<BlogPost, BlogPostViewModel>().ReverseMap();
    }
}
```
In the ```Program.cs``` file, we have the ```Main``` method, which is the entry point of the application. Inside the ```Main``` method, we create an instance of ```MapperConfiguration``` and configure it by adding a ```MappingProfile``` to it. This configuration sets up the mapping between the ```BlogPost``` and ```BlogPostViewModel``` classes. Finally, we create an instance of the ```mapper``` using the configured ```MapperConfiguration``` and register it as a ```singleton``` service in the Blazor application.

The ```MappingProfile``` class extends the ```Profile``` class provided by ```AutoMapper```. Inside the ```MappingProfile``` class, we define the mapping between the ```BlogPost``` and ```BlogPostViewModel``` classes using the ```CreateMap``` method. This method specifies how properties should be mapped between the ```source``` and ```destination``` objects. In this example, we use the ```ReverseMap``` method to enable bidirectional mapping between the two classes.


##### **In the Component**
We can use the ```Mapper.Map``` method to map objects between different models.
```
...
var vm = Mapper.Map<BlogPost, BlogPostViewModel>(result);
...
var model = Mapper.Map<BlogPostViewModel, BlogPost>(blogPost);
...
```

By following the steps outlined in this article, you can easily implement ```object mapping``` in your Blazor applications using ```AutoMapper```.


#### **Source**
Full source code is available at this repository in GitHub:
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppObjectMappingwithAutoMapper
