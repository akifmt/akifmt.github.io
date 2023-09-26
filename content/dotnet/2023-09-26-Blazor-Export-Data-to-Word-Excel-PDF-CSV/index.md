---
title: "Blazor Export Data to Word, Excel, PDF, CSV"
date: 2023-09-26T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Export Data to Word, Excel, PDF, CSV
menu:
  dotnet:
    name: Blazor Export Data to Word, Excel, PDF, CSV
    identifier: blazor-export-data-to-word-excel-pdf-csv
    weight: -20230926
tags: [Dotnet, Blazor Export Data to Word Excel PDF CSV]
categories: [Dotnet, Blazor Export Data to Word Excel PDF CSV]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet" style="border-radius: 20px;"><br>
<p>

#### **Blazor Export Data to Word, Excel, PDF, CSV**
We will explore how to export data to various file formats such as Word, Excel, PDF, and CSV using Blazor, a web framework for building interactive user interfaces.

Blazor: Blazor is a web framework that allows developers to build interactive web applications using C# instead of JavaScript. It enables the creation of rich UI components and provides a seamless integration between the client and server.

Exporting Data: Exporting data refers to the process of converting data from one format to another, such as exporting data from a web application to a file format like Word, Excel, PDF, or CSV. This allows users to save or share the data in a more convenient and portable manner.

PdfSharp: PdfSharp is a popular open-source library for creating and manipulating PDF documents in C#. It provides a comprehensive set of features for generating PDF files, including text formatting, images, tables, and more.

FileFontResolver: The FileFontResolver class is a custom implementation of the IFontResolver interface provided by PdfSharp. It is responsible for resolving font information when exporting data to PDF. It maps font names to the corresponding font files in the application's file system.

##### **Program.cs**

```
...
// add PdfSharp FontResolver
PdfSharp.Fonts.GlobalFontSettings.FontResolver = new FileFontResolver();
...
```

##### **FileFontResolver.cs**
The ```FileFontResolver``` class implements the ```IFontResolver``` interface, which requires the implementation of two methods: ```GetFont``` and ```ResolveTypeface```.
```
using PdfSharp.Fonts;

namespace BlazorAppExport.Helpers;

public class FileFontResolver : IFontResolver
{
    public string DefaultFontName => throw new NotImplementedException();

    public byte[] GetFont(string faceName)
    {
        using (var ms = new MemoryStream())
        {
            using (var fs = File.Open(faceName, FileMode.Open))
            {
                fs.CopyTo(ms);
                ms.Position = 0;
                return ms.ToArray();
            }
        }
    }

    public FontResolverInfo ResolveTypeface(string familyName, bool isBold, bool isItalic)
    {
        if (familyName.Equals("Arial", StringComparison.CurrentCultureIgnoreCase))
        {
            if (isBold && isItalic)
            {
                return new FontResolverInfo("Fonts/arialbi.ttf");
            }
            else if (isBold)
            {
                return new FontResolverInfo("Fonts/arialbd.ttf");
            }
            else if (isItalic)
            {
                return new FontResolverInfo("Fonts/ariali.ttf");
            }
            else
            {
                return new FontResolverInfo("Fonts/arial.ttf");
            }
        }
        if (familyName.Equals("Times New Roman", StringComparison.CurrentCultureIgnoreCase))
        {
            if (isBold && isItalic)
            {
                return new FontResolverInfo("Fonts/timesbi.ttf");
            }
            else if (isBold)
            {
                return new FontResolverInfo("Fonts/timesbd.ttf");
            }
            else if (isItalic)
            {
                return new FontResolverInfo("Fonts/timesi.ttf");
            }
            else
            {
                return new FontResolverInfo("Fonts/times.ttf");
            }
        }
        if (familyName.Equals("Tahoma", StringComparison.CurrentCultureIgnoreCase))
        {
            if (isBold && isItalic)
            {
                return new FontResolverInfo("Fonts/tahoma.ttf");
            }
            else if (isBold)
            {
                return new FontResolverInfo("Fonts/tahomabd.ttf");
            }
            else if (isItalic)
            {
                return new FontResolverInfo("Fonts/tahoma.ttf");
            }
            else
            {
                return new FontResolverInfo("Fonts/tahoma.ttf");
            }
        }
        if (familyName.Equals("Verdana", StringComparison.CurrentCultureIgnoreCase))
        {
            if (isBold && isItalic)
            {
                return new FontResolverInfo("Fonts/verdanaz.ttf");
            }
            else if (isBold)
            {
                return new FontResolverInfo("Fonts/verdanab.ttf");
            }
            else if (isItalic)
            {
                return new FontResolverInfo("Fonts/verdanai.ttf");
            }
            else
            {
                return new FontResolverInfo("Fonts/verdana.ttf");
            }
        }
        if (familyName.Equals("Courier New", StringComparison.CurrentCultureIgnoreCase))
        {
            if (isBold && isItalic)
            {
                return new FontResolverInfo("Fonts/courbi.ttf");
            }
            else if (isBold)
            {
                return new FontResolverInfo("Fonts/courbd.ttf");
            }
            else if (isItalic)
            {
                return new FontResolverInfo("Fonts/couri.ttf");
            }
            else
            {
                return new FontResolverInfo("Fonts/cour.ttf");
            }
        }
        return null;
    }
}
```
The ```GetFont``` method is responsible for retrieving the ```font data``` based on the provided faceName. It uses the ```File.Open``` method to open the font file and then copies its contents to a ```MemoryStream```. Finally, it returns the```byte array``` representation of the font data.

The ```ResolveTypeface``` method is responsible for resolving the font information based on the provided ```familyName, isBold, and isItalic``` parameters. It checks the ```familyName``` against a set of predefined font names and returns the corresponding ```FontResolverInfo``` object.


##### **MigraDocDocumentHelper.cs**
The code provided includes a ```MigraDocDocumentHelper``` class, which contains a static method ```CreateDocument```. This method takes two parameters: ```headerColumnNames and dataColumns```. It also has an optional parameter ```dataSplitChar```, which defaults to ```'|'```.
```
using MigraDoc.DocumentObjectModel;
using MigraDoc.DocumentObjectModel.Shapes.Charts;
using MigraDoc.DocumentObjectModel.Tables;

namespace BlazorAppExport.Data;

public class MigraDocDocumentHelper
{
    public static Document CreateDocument(List<string> headerColumnNames, List<string> dataColumns, char dataSplitChar = '|')
    {
        //GlobalFontSettings.FontResolver = new FileFontResolver();

        // Create a new MigraDoc document
        Document document = new Document();
        document.Info.Title = "title";
        document.Info.Subject = "subject";
        document.Info.Author = "author";

        MigraDocStylesHelper.DefineStyles(document);

        MigraDocCoverHelper.DefineCover(document);
        MigraDocTableOfContentsHelper.DefineTableOfContents(document);

        MigraDocContentSectionHelper.DefineContentSection(document);

        //MigraDocParagraphsHelper.DefineParagraphs(document);
        //MigraDocTablesHelper.DefineTables(document);
        //MigraDocChartsHelper.DefineCharts(document);

        MigraDocTablesExportedDataHelper.DefineTablesExportedData(document, headerColumnNames, dataColumns, dataSplitChar);

        return document;
    }
}
```
The ```CreateDocument``` method is responsible for creating a new ```MigraDoc``` document and setting its ```title, subject, and author```. It then calls various helper methods to define the document's ```styles```, ```cover page```, ```table of contents```, ```content section```, and ```exported data tables```.

The ```MigraDocTablesExportedDataHelper.DefineTablesExportedData``` method is responsible for creating tables in the document and populating them with the provided header column names and data columns. The ```dataSplitChar``` parameter is used to split the data columns into individual values.

##### **ExportController.cs**
```ExportController``` class demonstrates how to export data to a CSV, Excel, Pdf, Word file. This method takes an IQueryable query and an optional fileName parameter. It retrieves the properties of the query's element type and iterates over each item in the query. For each item, it retrieves the values of the properties and appends them to a StringBuilder object.
```
public FileStreamResult ToCSV(IQueryable query, string? fileName = null)
{
	var columns = GetProperties(query.ElementType);

	var sb = new StringBuilder();

	foreach (var item in query)
	{
		var row = new List<string>();

		foreach (var column in columns)
		{
			row.Add($"{GetValue(item, column.Key)}".Trim());
		}

		sb.AppendLine(string.Join(",", row.ToArray()));
	}

	var result = new FileStreamResult(new MemoryStream(UTF8Encoding.Default.GetBytes($"{string.Join(",", columns.Select(c => c.Key))}{System.Environment.NewLine}{sb.ToString()}")), "text/csv");
	result.FileDownloadName = (!string.IsNullOrEmpty(fileName) ? fileName : "Export") + ".csv";

	return result;
}

public FileStreamResult ToExcel(IQueryable query, string? fileName = null)
{
	var columns = GetProperties(query.ElementType);
	var stream = new MemoryStream();

	using (var document = SpreadsheetDocument.Create(stream, SpreadsheetDocumentType.Workbook))
	{
		var workbookPart = document.AddWorkbookPart();
		workbookPart.Workbook = new Workbook();

		var worksheetPart = workbookPart.AddNewPart<WorksheetPart>();
		worksheetPart.Worksheet = new Worksheet();

		var workbookStylesPart = workbookPart.AddNewPart<WorkbookStylesPart>();
		GenerateWorkbookStylesPartContent(workbookStylesPart);

		var sheets = workbookPart.Workbook.AppendChild(new Sheets());
		var sheet = new Sheet() { Id = workbookPart.GetIdOfPart(worksheetPart), SheetId = 1, Name = "Sheet1" };
		sheets.Append(sheet);

		workbookPart.Workbook.Save();

		var sheetData = worksheetPart.Worksheet.AppendChild(new SheetData());

		var headerRow = new Row();

		foreach (var column in columns)
		{
			headerRow.Append(new Cell()
			{
				CellValue = new CellValue(column.Key),
				DataType = new EnumValue<CellValues>(CellValues.String)
			});
		}

		sheetData.AppendChild(headerRow);

		foreach (var item in query)
		{
			var row = new Row();

			foreach (var column in columns)
			{
				var value = GetValue(item, column.Key);
				var stringValue = $"{value}".Trim();

				var cell = new Cell();

				var underlyingType = column.Value.IsGenericType &&
					column.Value.GetGenericTypeDefinition() == typeof(Nullable<>) ?
					Nullable.GetUnderlyingType(column.Value) : column.Value;

				var typeCode = Type.GetTypeCode(underlyingType);

				if (typeCode == TypeCode.DateTime)
				{
					if (!string.IsNullOrWhiteSpace(stringValue))
					{
						cell.CellValue = new CellValue() { Text = ((DateTime)value).ToOADate().ToString(System.Globalization.CultureInfo.InvariantCulture) };
						cell.DataType = new EnumValue<CellValues>(CellValues.Number);
						cell.StyleIndex = (UInt32Value)1U;
					}
				}
				else if (typeCode == TypeCode.Boolean)
				{
					cell.CellValue = new CellValue(stringValue.ToLower());
					cell.DataType = new EnumValue<CellValues>(CellValues.Boolean);
				}
				else if (IsNumeric(typeCode))
				{
					if (value != null)
					{
						stringValue = Convert.ToString(value, CultureInfo.InvariantCulture);
					}
					cell.CellValue = new CellValue(stringValue);
					cell.DataType = new EnumValue<CellValues>(CellValues.Number);
				}
				else
				{
					cell.CellValue = new CellValue(stringValue);
					cell.DataType = new EnumValue<CellValues>(CellValues.String);
				}

				row.Append(cell);
			}

			sheetData.AppendChild(row);
		}

		workbookPart.Workbook.Save();
	}

	if (stream?.Length > 0)
	{
		stream.Seek(0, SeekOrigin.Begin);
	}

	var result = new FileStreamResult(stream, "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet");
	result.FileDownloadName = (!string.IsNullOrEmpty(fileName) ? fileName : "Export") + ".xlsx";

	return result;
}

public FileStreamResult ToPdf(IQueryable query, string? fileName = null)
{
	var columns = GetProperties(query.ElementType);
	List<string> headerColumns = new List<string>();
	foreach (var column in columns)
	{
		headerColumns.Add(column.Key);
	}

	char seperaterChar = '|';
	List<string> dataColumns = new List<string>();
	foreach (var item in query)
	{
		List<string> lineData = new List<string>();
		foreach (var column in columns)
		{
			lineData.Add($"{GetValue(item, column.Key)}".Trim());
		}
		string lineString = string.Join(seperaterChar, lineData);
		dataColumns.Add(lineString);
	}

	var stream = new MemoryStream();

	MigraDoc.DocumentObjectModel.Document doc = MigraDocDocumentHelper.CreateDocument(headerColumns, dataColumns, seperaterChar);
	MigraDoc.Rendering.PdfDocumentRenderer renderer = new();
	renderer.Document = doc;
	renderer.RenderDocument();

	renderer.PdfDocument.Save(stream);

	if (stream?.Length > 0)
	{
		stream.Seek(0, SeekOrigin.Begin);
	}

	FileStreamResult? result = new FileStreamResult(stream, "application/pdf");
	result.FileDownloadName = "Export" + ".pdf";

	return result;
}

public FileStreamResult ToWord(IQueryable query, string? fileName = null)
{
	var columns = GetProperties(query.ElementType);

	var sb = new StringBuilder();

	// titles
	sb.Append("<table border='1' cellpadding='5' cellspacing='0' style='border: 1px solid #ccc;font-family: Arial; font-size: 10pt;'>");
	sb.Append("<tr>");
	foreach (var column in columns)
	{
		sb.Append($"<th style='background-color: #B8DBFD;border: 1px solid #ccc'>{column.Key}</th>");
	}
	sb.Append("</tr>");

	// rows
	foreach (var item in query)
	{
		sb.Append("<tr>");

		//Append data.
		foreach (var column in columns)
		{
			sb.Append("<td style='border: 1px solid #ccc'>");
			sb.Append(GetValue(item, column.Key));
			sb.Append("</td>");
		}
		sb.Append("</tr>");
	}
	sb.Append("</table>");

	MemoryStream stream = new MemoryStream(Encoding.UTF8.GetBytes(sb.ToString()));

	if (stream?.Length > 0)
	{
		stream.Seek(0, SeekOrigin.Begin);
	}

	FileStreamResult? result = new FileStreamResult(stream, "application/vnd.ms-word");

	result.FileDownloadName = (!string.IsNullOrEmpty(fileName) ? fileName : "Export") + ".doc";
	return result;
}
```
A ```FileStreamResult``` is created with the ```stream```. The ```FileDownloadName``` property is set to ```"Export"```. The method returns the ```FileStreamResult```, which will prompt the user to download the exported file.


##### **BlogPostController.cs**
The ```BlogPostController.cs``` file is a partial class that extends the ```ExportController``` class. It contains several action methods that handle the export functionality for the ```BlogPost``` entity.
```
using BlazorAppExport.Services;
using Microsoft.AspNetCore.Mvc;

namespace BlazorAppExport.Controllers;

public partial class BlogPostController : ExportController
{
    private BlogPostService _blogPostService;

    public BlogPostController(BlogPostService blogPostService)
    {
        this._blogPostService = blogPostService;
    }

    [HttpGet("/export/ApplicationDb/BlogPost/csv")]
    public async Task<FileStreamResult> ExportBlogPostToCSV()
    {
        var result = await _blogPostService.GetAllAsync();
        var query = result.AsQueryable();

        return ToCSV(ApplyQuery(query, Request.Query));
    }

    [HttpGet("/export/ApplicationDb/BlogPost/excel")]
    public async Task<FileStreamResult> ExportBlogPostToExcel()
    {
        var result = await _blogPostService.GetAllAsync();
        var query = result.AsQueryable();

        return ToExcel(ApplyQuery(query, Request.Query));
    }

    [HttpGet("/export/ApplicationDb/BlogPost/pdf")]
    public async Task<FileStreamResult> ExportBlogPostToPdf()
    {
        var result = await _blogPostService.GetAllAsync();
        var query = result.AsQueryable();

        return ToPdf(ApplyQuery(query, Request.Query));
    }

    [HttpGet("/export/ApplicationDb/BlogPost/word")]
    public async Task<FileStreamResult> ExportBlogPostToWord()
    {
        var result = await _blogPostService.GetAllAsync();
        var query = result.AsQueryable();

        return ToWord(ApplyQuery(query, Request.Query));
    }
}
```
The constructor of the ```BlogPostController``` class takes an instance of the ```BlogPostService``` class as a parameter. This service is responsible for retrieving the data to be exported.

The ```BlogPostController``` class includes the following action methods:

ExportBlogPostToCSV: This method exports the BlogPost data to a ```CSV file format```. It retrieves the data using the ```GetAllAsync``` method of the ```BlogPostService``` class and applies any ```query parameters``` passed in the request. The ```ToCSV``` method converts the data to a ```CSV file``` and returns it as a ```FileStreamResult```.

ExportBlogPostToExcel: This method exports the ```BlogPost``` data to an ```Excel file``` format. It follows a similar process as the ```ExportBlogPostToCSV``` method but converts the data to an ```Excel file``` using the ```ToExcel``` method.

ExportBlogPostToPdf: This method exports the ```BlogPost``` data to a ```PDF file``` format. It retrieves the data and converts it to a ```PDF file``` using the ```ToPdf``` method.

ExportBlogPostToWord: This method exports the ```BlogPost``` data to a ```Word file``` format. It retrieves the data and converts it to a ```Word file``` using the ```ToWord``` method.



##### **ExportService.cs**
The ```ExportService``` class has a method called ```Export``` that takes three parameters: ```table, type, and query```. The table parameter represents the name of the table from which the data is to be exported. The ```type parameter``` represents the file format in which the data should be exported (e.g., Word, Excel, Pdf, CSV). The query parameter is an optional parameter of type ```IQueryCollection``` that represents any additional ```query parameters``` that should be included in the export URL.

```
using Microsoft.AspNetCore.Components;

namespace BlazorAppExport.Services;

public class ExportService
{
    private readonly NavigationManager navigationManager;

    public ExportService(NavigationManager navigationManager)
    {
        this.navigationManager = navigationManager;
    }

    // Dynamic Query
    public void Export(string table, string type, IQueryCollection? query = null)
    {
        var url = $"/export/ApplicationDb/{table}/{type}";
        if (query is not null)
        {
            string queryString = string.Join("&", query.Select(x => $"{x.Key}={x.Value}"));
            url += $"?{queryString}";
        }

        navigationManager.NavigateTo(url, true);
    }

}
```
In this method, we construct the export URL based on the provided table and type parameters. If the ```query parameter``` is not null, we iterate over its key-value pairs and append them to the URL as query parameters. Finally, we use the ```navigationManager``` to navigate to the generated URL, which triggers the file download.



##### **Index.razor**
Index.razor, ```export buttons``` allow the user to export all ```blog posts``` in different file formats such as ```Excel```, ```CSV```, ```PDF```, and ```Word```.

```
@page "/BlogPost"

<PageTitle>Index</PageTitle>

<h1>Index</h1>

<p>
    <a href="/BlogPost/Create">Create New</a>
</p>

<p>
    <button class="btn btn-primary" @onclick="@(args => Export("excel"))">Export All XLS</button>
    <button class="btn btn-primary" @onclick="@(args => Export("csv"))">Export All CSV</button>
    <button class="btn btn-primary" @onclick="@(args => Export("pdf"))">Export All PDF</button>
    <button class="btn btn-primary" @onclick="@(args => Export("word"))">Export All DOC</button>
</p>

<p>
    <button class="btn btn-primary" @onclick="@(args => ExportCustom("excel"))">Export Custom Items XLS</button>
</p>

@if (blogPosts == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <table class="table">
        <thead>
            <tr>
                <th>@nameof(BlogPostViewModel.Id)</th>
                <th>@nameof(BlogPostViewModel.Title)</th>
                <th>@nameof(BlogPostViewModel.Content)</th>
                <th></th>
            </tr>
        </thead>
        <tbody>
            @foreach (var blogpost in blogPosts)
            {
                <tr>
                    <td>@blogpost.Id</td>
                    <td>@blogpost.Title</td>
                    <td>@blogpost.Content</td>
                    <td>
                        <a href="/BlogPost/Details/@blogpost.Id">Details</a> |
                        <a href="/BlogPost/Edit/@blogpost.Id">Edit</a> |
                        <a href="/BlogPost/Delete/@blogpost.Id">Delete</a>
                    </td>
                </tr>
            }
        </tbody>
    </table>
}

@code {
    private IEnumerable<BlogPostViewModel>? blogPosts;

    protected override async Task OnInitializedAsync()
    {
        if (blogPosts == null)
        {
            var result = await BlogPostService.GetAllAsync();
            blogPosts = Mapper.Map<IEnumerable<BlogPost>, IEnumerable<BlogPostViewModel>>(result);
        }
    }

    public void Export(string type)
    {
        ExportService.Export("BlogPost", type);
    }

    public void ExportCustom(string type)
    {
        // Dynamic Query
        Dictionary<string, Microsoft.Extensions.Primitives.StringValues> queryValues = new();
        //queryValues.Add("$expand", $"{nameof(BlogPost.Title)},{nameof(BlogPost.Content)}");
        queryValues.Add("$filter", $"{nameof(BlogPost.Title)}.Contains(\"Introduction\")");
        queryValues.Add("$orderBy", $"{nameof(BlogPost.Title)} desc");
        queryValues.Add("$skip", "1");
        queryValues.Add("$top", "2");
        queryValues.Add("$select", $"{nameof(BlogPost.Title)},{nameof(BlogPost.Content)}");
        IQueryCollection query = new QueryCollection(queryValues);
        ExportService.Export("BlogPost", type, query);
    }

}
```
Export method, is called when the user clicks on one of the export buttons. It takes a ```parameter```, type, which represents the ```file format``` to export (e.g., "excel", "csv", "pdf", "word"). The ```ExportService``` class is responsible for exporting the data to the specified file format.

The ```ExportCustom``` method is called when the user clicks on the "Export Custom Items XLS" button. It demonstrates how to ```export``` a custom selection of blog posts based on specific criteria. In this example, the ```queryValues``` dictionary is used to define the ```query parameters``` such as filtering, ordering, skipping, and selecting specific fields. The ```ExportService``` class is then called to export the data to the specified file format using the ```query``` parameters.

Exporting data to different file formats can greatly enhance the usability and accessibility of your Blazor application. Whether it's generating reports, sharing data with others, or archiving information, the ability to export data in different formats is a valuable feature to have.

#### **Source**
Full source code is available at this repository in GitHub:
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppExport
