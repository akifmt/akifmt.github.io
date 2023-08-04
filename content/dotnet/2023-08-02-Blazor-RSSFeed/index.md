---
title: "Blazor Expose a Feed as RSS"
date: 2023-08-02T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Expose a Feed as RSS
menu:
  dotnet:
    name: Blazor Expose a Feed as RSS
    identifier: blazor-expose-a-feed-as-rss
    weight: -20230802
tags: [ Dotnet, Blazor Expose a Feed as RSS]
categories: [ Dotnet, Blazor Expose a Feed as RSS]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

##### **Blazor Expose a Feed as RSS**
In this example, we will explore how to expose a feed as RSS using C# and the Blazor framework. We will create a controller that generates an RSS feed based on mock data.

RSS (Really Simple Syndication): RSS is a web feed format used to publish frequently updated content, such as blog posts, news headlines, or podcasts. It allows users to subscribe to a website's content and receive updates in a standardized format.

SyndicationFeed: The ```SyndicationFeed``` class represents an RSS or Atom feed. It contains properties like title, description, author, categories, and a collection of ```SyndicationItem``` objects.

SyndicationItem: The ```SyndicationItem``` class represents an individual item in an RSS or Atom feed. It contains properties like title, content, author, categories, and a link to the full article.

Rss20FeedFormatter: The ```Rss20FeedFormatter``` class is used to format a ```SyndicationFeed``` object as an RSS 2.0 feed.

###### **RSSController.cs**
```RSSController``` inherits from ```ControllerBase```. It exposes a single GET endpoint ```(/api/RSS)``` that returns an RSS feed.

The controller has a constructor that takes a ```MockData``` object as a parameter. This object is used to retrieve mock blog post data.

```
[Route("api/[controller]")]
[ApiController]
public class RSSController : ControllerBase
{
    private readonly MockData _mockData;

    public RSSController(MockData mockData)
    {
        _mockData = mockData;
    }

    [HttpGet]
    public IActionResult Get()
    {
        var output = new MemoryStream();
        string xml;

        var Feeds = _mockData.GetAllBlogPosts();

        List<SyndicationItem> items = new List<SyndicationItem>();
        var feed = new SyndicationFeed("RSS Feed Title", "Feed Description",
                                        new Uri("https://localhost:5001/posts"));
        // Set feed properties
        feed.ImageUrl = new Uri("https://picsum.photos/600/400");
        feed.Authors.Add(new SyndicationPerson("asd@asd.com", "asdname", "https://picsum.photos/600/600"));
        feed.BaseUri = new Uri("https://localhost:5001");
        feed.Categories.Add(new SyndicationCategory("Feed Category 1 Base"));
        feed.LastUpdatedTime = DateTime.Now;
        feed.Language = "Lang1";
        feed.Copyright = new TextSyndicationContent("Copy1");

        // Create SyndicationItems for each blog post
        foreach (var post in Feeds)
        {
            var solutionfeed = new SyndicationItem(post.Title, post.Content,
                new Uri(quot;https://localhost:5001/postsingle/{post.Id}"), post.Id.ToString(), DateTime.Now);
            solutionfeed.Authors.Add(new SyndicationPerson("post@user.com", "postuser", "https://picsum.photos/600/600"));
            solutionfeed.BaseUri = new Uri("https://localhost:5001/posts");
            solutionfeed.Categories.Add(new SyndicationCategory("Feed Category 1 feed"));
            solutionfeed.Contributors.Add(new SyndicationPerson("postCont@user.com", "postcontuser", "https://picsum.photos/600/600"));
            solutionfeed.Copyright = new TextSyndicationContent("feed copy");
            solutionfeed.ElementExtensions.Add(new XElement("enclosure",
                new XAttribute("type", ""),
                new XAttribute("url", "https://picsum.photos/600/400"),
                new XAttribute("width", 200),
                new XAttribute("height", 200)).CreateReader());
            solutionfeed.PublishDate = DateTime.Now;
            solutionfeed.Summary = new TextSyndicationContent(post.Content.Substring(0, 5));
            items.Add(solutionfeed);
        }

        // Set the items collection of the feed
        feed.Items = items;

        // Create an Rss20FeedFormatter and write the feed to a MemoryStream
        var formatter = new Rss20FeedFormatter(feed);
        var xws = new XmlWriterSettings { Encoding = Encoding.UTF8 };
        using (var xmlWriter = XmlWriter.Create(output, xws))
        {
            formatter.WriteTo(xmlWriter);
            xmlWriter.Flush();
        }

        // Read the generated XML from the MemoryStream
        using (var sr = new StreamReader(output))
        {
            output.Position = 0;
            xml = sr.ReadToEnd();
            sr.Close();
        }

        // Return the XML as a ContentResult with the appropriate content type
        ContentResult result = Content(xml, "application/xml", Encoding.UTF8);
        return result;
    }
}
```

The Get method is responsible for generating the RSS feed. Here's a breakdown of the code:

- Create a ```MemoryStream``` and a string variable to store the generated XML.
- Retrieve the mock blog post data using the ```MockData``` object.
- Create an empty list of ```SyndicationItem``` objects and a ```SyndicationFeed``` object. Set the properties of the feed, such as title, description, image URL, authors, categories, and last updated time.
- Iterate over each blog post and create a ```SyndicationItem``` for it. Set the properties of the item, such as title, content, link, author, categories, contributors, enclosure, publish date, and summary. Add the item to the list of items.
- Set the ```Items``` property of the feed to the list of items.
- Create an ```Rss20FeedFormatter``` and write the feed to the ```MemoryStream``` using an ```XmlWriter```.
- Read the generated XML from the ```MemoryStream``` into the ```xml``` variable.
- Create a ```ContentResult``` with the XML as the content and the appropriate content type.
- Return the ```ContentResult```.

###### **Source**
Full source code is available at this repository in GitHub: 
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppRSSFeed
