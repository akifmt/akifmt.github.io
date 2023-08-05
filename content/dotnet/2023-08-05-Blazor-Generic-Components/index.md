---
title: "Blazor Generic Components"
date: 2023-08-05T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Generic Components
menu:
  dotnet:
    name: Blazor Generic Components
    identifier: blazor-generic-components
    weight: -20230805
tags: [ Dotnet, Blazor Generic Components]
categories: [ Dotnet, Blazor Generic Components]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet"><br>
<p>

##### **Blazor Generic Components**
We will explore a Blazor generic component that can be used for performing CRUD (Create, Read, Update, Delete) operations on a list of items. This component is designed to provide a reusable and efficient solution for displaying and managing data in a Blazor application.

Blazor: Blazor is a web framework that allows developers to build interactive web UIs using C# instead of JavaScript. It enables full-stack development with .NET, providing a seamless development experience.

Generic Components: Generic components in Blazor allow us to create reusable UI components that can work with different types of data. By using generics, we can create components that are flexible and adaptable to various scenarios.

TModel: The ```TModel``` type parameter represents the model class that the component will work with. It must inherit from the ```BaseModel``` class.

TViewModel: The ```TViewModel``` type parameter represents the view model class that the component will use to display the data. It must inherit from the ```BaseViewModel``` class and have a parameterless constructor.

IService: The ```IService<TModel>``` interface represents a service that provides CRUD (Create, Read, Update, Delete) operations for the ```TModel``` class. It is injected into the component to retrieve the data.

IMapper: The ```IMapper``` interface is used for object-to-object mapping. It is injected into the component to map the ```TModel``` objects to ```TViewModel``` objects.

NavigationManager: The ```NavigationManager``` class provides methods for navigating within a Blazor application. It is injected into the component to handle navigation to different pages.

Let's break down the code structure:

Namespace and Using Statements: The code starts with the declaration of the namespace and the necessary using statements.

Type Parameters: The component uses two type parameters, ```TModel and TViewModel```, which represent the model and view model types, respectively. These parameters allow the component to work with different types of data.

Conditional Rendering: The code uses conditional rendering to display a loading message if the ```listViewModel``` is null. Otherwise, it renders a table to display the list of items.

Table Structure: The table structure is defined using HTML markup. The table headers are dynamically generated based on the properties of the ```TViewModel``` type. The ```nameof``` function is used to get the name of the ```Id``` property from the ```BaseViewModel``` class, and the ```GetProperties``` method is used to iterate over the properties of the ```TViewModel``` type.

Data Binding: The table rows are generated using a foreach loop that iterates over the ```listViewModel``` collection. The ```GetProperty``` and ```GetValue``` methods are used to retrieve the values of the properties dynamically. The ```ToString``` method is called to convert the values to strings.

CRUD Links: Each table row includes links for performing CRUD operations on the corresponding item. The ```NavigateLinkItemDetails, NavigateLinkItemEdit, and NavigateLinkItemDelete``` properties are used to generate the URLs for the respective operations. The ```Replace``` method is used to replace the placeholder ```{{{id}}}``` with the actual item ID.


###### **List Posts (List.razor, List.razor.cs)**

Let's take a closer look at the code provided in the List<TModel, TViewModel> component:
```
public partial class List<TModel, TViewModel> : ComponentBase where TModel : BaseModel where TViewModel : BaseViewModel, new()
{
    [Parameter, EditorRequired]
    public string? NavigateLinkItemDetails { get; set; }

    [Parameter, EditorRequired]
    public string? NavigateLinkItemEdit { get; set; }

    [Parameter, EditorRequired]
    public string? NavigateLinkItemDelete { get; set; }

    [Inject]
    private IService<TModel>? _service { get; set; }

    [Inject]
    private IMapper? Mapper { get; set; }

    [Inject]
    private NavigationManager? NavigationManager { get; set; }

    private IEnumerable<TViewModel>? listViewModel;

    protected override async Task OnInitializedAsync()
    {
        if (_service == null || Mapper == null) return;
        var model = await _service.GetAllAsync();
        if (model == null) return;
        listViewModel = Mapper.Map<IEnumerable<TModel>, IEnumerable<TViewModel>>(model);
    }
}
```

In this code, we define the ```List<TModel, TViewModel>``` component as a partial class that inherits from ```ComponentBase```. The component has the following members:

Parameters: The component has three parameters: ```NavigateLinkItemDetails, NavigateLinkItemEdit, and NavigateLinkItemDelete```. These parameters are used to specify the navigation links for item details, item edit, and item delete actions.

Injected Services: The component injects three services: ```IService<TModel>, IMapper, and NavigationManager```. These services are used to retrieve data, map objects, and handle navigation, respectively.

listViewModel: This private property holds the list of view models that will be displayed in the component.

OnInitializedAsync: This method is an overridden method from the ```ComponentBase``` class. It is called when the component is initialized. In this method, we check if the injected services are available. If they are, we retrieve the data using the ```_service``` and map it to the view models using the ```Mapper```. The resulting view models are assigned to the ```listViewModel``` property.

Using the Component:
```
<BlazorAppGenericComponents.Components.List TModel="BlogPost"
                                            TViewModel="BlogPostViewModel"
                                            NavigateLinkItemDetails="/BlogPost/Details/{{{id}}}"
                                            NavigateLinkItemEdit="/BlogPost/Edit/{{{id}}}"
                                            NavigateLinkItemDelete="/BlogPost/Delete/{{{id}}}" />
```


###### **Create Post (Create.razor, Create.razor.cs)**

```
@namespace BlazorAppGenericComponents.Components

@typeparam TModel
@typeparam TViewModel

@if (viewModel == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <EditForm Context="editFormComponent" OnValidSubmit="@HandleValidSubmit" Model="@viewModel">

        @foreach (var property in typeof(TViewModel).GetProperties())
        {
            @if (property.Name != "Id")
            {
                <div class="form-group">
                    <label class="control-label">@property.Name</label>
                    <input @onchange='((e) => HandleValueChanged(e, property.Name))'
                           type="text"
                           value="@property.GetValue(viewModel)?.ToString()"
                           class="form-control" />
                </div>
            }
        }

        <div class="form-group">
            <input type="submit" value="Create" class="btn btn-primary" />
        </div>

    </EditForm>
}
```

In this code snippet, the component checks if the ```viewModel``` is null. If it is null, it displays a loading message. Otherwise, it renders an ```EditForm``` component with the ```viewModel``` as the model.

Inside the ```EditForm``` component, a loop iterates over the properties of the ```TViewModel``` type. For each property, a form group is created with a label and an input field. The label displays the name of the property, and the input field is bound to the corresponding property value in the ```viewModel```.

The ```HandleValueChanged``` method is called whenever the value of an input field changes. It updates the corresponding property value in the ```viewModel```.

Finally, a submit button is displayed to create the entity.

The component has two generic type parameters: ```TModel and TViewModel```. TModel represents the model class for the database entity, while ```TViewModel``` represents the view model class for the form.

The component has the following properties and dependencies:

NavigateLinkAfterSubmit: A string parameter that specifies the URL to navigate to after the form is submitted.
_service: An instance of the ```IService<TModel>``` interface, which is responsible for interacting with the database.
Mapper: An instance of the IMapper interface, which is used to map the view model to the model.
NavigationManager: An instance of the ```NavigationManager``` class, which is used to navigate to the specified URL.
The component also has a private field called "```viewModel```" of type ```TViewModel```, which represents the current state of the form.

OnInitialized: This method is called when the component is initialized. It creates a new instance of the view model and assigns it to the "```viewModel```" field.

HandleValidSubmit: This method is called when the form is submitted and passes validation. It maps the view model to the model using the ```IMapper``` interface. Then, it calls the AddAsync method of the ```_service``` instance to add the model to the database. If the operation is successful, it navigates to the specified URL using the ```NavigationManager```.

HandleValueChanged: This method is called when the value of an input field in the form changes. It updates the corresponding property of the view model with the new value.

Using the Component:
```
<BlazorAppGenericComponents.Components.Create TModel="BlogPost" 
                                              TViewModel="BlogPostViewModel" 
                                              NavigateLinkAfterSubmit="/BlogPost" />
```


###### **Details Post (Details.razor, Details.razor.cs)**
```
<dl class="row">
        <dt class="col-sm-2">
            @nameof(BaseViewModel.Id)
        </dt>
        <dd class="col-sm-10">
            @typeof(TViewModel).GetProperty(nameof(BaseViewModel.Id))?.GetValue(viewModel)?.ToString()
        </dd>
        @foreach (var property in typeof(TViewModel).GetProperties())
        {
            if (property.Name != nameof(BaseViewModel.Id))
            {
                <dt class="col-sm-2">
                    @property.Name
                </dt>
                <dd class="col-sm-10">
                    @property.GetValue(viewModel)?.ToString()
                </dd>
            }
        }
    </dl>
```

The component has the following properties and dependencies:

Id: An optional parameter that represents the ID of the model for which the details are to be displayed.

_service: An injected dependency of type ```IService<TModel>```, which is responsible for retrieving the model data.

Mapper: An injected dependency of type ```IMapper```, which is used to map the model to the view model.

NavigationManager: An injected dependency of type ```NavigationManager```, which is used for navigation within the application.

viewModel: A private property of type ```TViewModel```, which holds the view model instance.

The ```OnInitializedAsync``` method is overridden to fetch the model data and map it to the view model. If the ```Id``` is null or any of the dependencies are null, the method returns early. Otherwise, it calls the ```_service.GetbyId``` method to retrieve the model with the specified ID. If the model is found, it uses the ```Mapper``` to map the model to the view model and assigns it to the ```viewModel``` property.

Using the Component:
```
<BlazorAppGenericComponents.Components.Details TModel="BlogPost"
                                               TViewModel="BlogPostViewModel"
                                               Id="@id" />
```


###### **Edit Post (Edit.razor, Edit.razor.cs)**
Let's break down the code structure:

Namespace and Using Statements: The code starts with the declaration of the namespace and the necessary using statements.

Type Parameters: The component is defined with two type parameters, ```TModel and TViewModel```. These parameters allow the component to work with different models and view models.

Conditional Rendering: The code uses a conditional rendering statement to check if the ```viewModel``` is null. If it is null, it displays a loading message. Otherwise, it renders the form for editing the model.

EditForm Component: The ```EditForm``` component is a built-in Blazor component that provides form validation and submission functionality. It is used to wrap the form elements.

Property Loop: Inside the ```EditForm``` component, a loop is used to iterate over the properties of the ```TViewModel``` type. The loop excludes the ```Id``` property, as it is usually an identifier and not editable.

Form Group: For each property, a form group is created with a label and an input field. The ```onchange``` event is wired to a method called ```HandleValueChanged```, which handles the value change of the input field.

Submit Button: Finally, a submit button is added to the form for saving the changes.

Blazor generic component named ```Edit<TModel, TViewModel>```. Let's break down its structure:

Namespace and Dependencies: The component is defined within the ```BlazorAppGenericComponents.Components``` namespace. It has dependencies on ```AutoMapper, BlazorAppGenericComponents.Models, BlazorAppGenericComponents.Services, BlazorAppGenericComponents.ViewModels, and Microsoft.AspNetCore.Components```.

Component Declaration: The component is declared as a partial class that inherits from ```ComponentBase```. It has two generic type parameters, ```TModel and TViewModel```, which represent the model and view model types, respectively.

Component Parameters: The component has two parameters defined using the ```[Parameter]``` attribute. The ```Id``` parameter is of type ```int?``` and represents the identifier of the model to be edited. The ```NavigateLinkAfterSubmit``` parameter is of type ```string?``` and represents the URL to navigate to after a successful submit.

Dependency Injection: The component injects dependencies for ```IService<TModel>, IMapper, and NavigationManager``` using the ```[Inject]``` attribute.

ViewModel Initialization: The ```OnInitializedAsync``` method is overridden to initialize the view model based on the provided ```Id``` parameter. It retrieves the model from the service using the ```GetbyId``` method and maps it to the view model using AutoMapper.

Form Submission: The ```HandleValidSubmit``` method is called when the form is submitted. It maps the view model back to the model using AutoMapper and calls the ```UpdateAsync``` method of the service to update the model. If the update is successful, it navigates to the specified URL using the ```NavigationManager```.

Value Change Handling: The ```HandleValueChanged``` method is called when the value of an input field changes. It updates the corresponding property of the view model based on the changed value.


Using the Component:
```
<BlazorAppGenericComponents.Components.Edit TModel="BlogPost"
                                            TViewModel="BlogPostViewModel"
                                            Id="@id"
                                            NavigateLinkAfterSubmit="/BlogPost" />
```

###### **Delete Post (Delete.razor, Delete.razor.cs)**

The code starts with the ```@namespace``` directive, which specifies the namespace of the component. It is followed by the ```@using``` directive, which imports the necessary namespaces for the component.

Next, the component declares two type parameters, ```TModel and TViewModel```, using the ```@typeparam``` directive. These type parameters represent the model and view model types that will be used with the component.

The code then checks if the ```viewModel``` variable is null. If it is null, it displays a loading message. Otherwise, it generates the details view for the view model.

Inside the ```dl``` element, the code displays the value of the ```Id``` property of the view model using the ```@nameof``` directive and the ```GetValue``` method. It then iterates over the properties of the view model using a ```foreach``` loop.

For each property, it checks if the property name is not equal to "```Id```". If it is not equal, it displays the property name and its value using the ```@property.Name and @property.GetValue``` directives.

Finally, the code displays a "Delete" button and attaches an ```onclick``` event handler to it.

The component has the following properties:

Id: An optional parameter that represents the ID of the record to be deleted.

NavigateLinkAfterDelete: An optional parameter that specifies the URL to navigate to after the record is deleted.

The component also injects the following dependencies:

IMapper: An object mapper that maps the model to the view model.

NavigationManager: A service that provides navigation functionality.

The component has an ```OnInitializedAsync``` method that is called when the component is initialized. It retrieves the record with the specified ID from the service and maps it to the view model using the mapper.

The component also has a ```DeleteButtonClick``` method that is called when the delete button is clicked. It calls the service's ```DeletebyIdAsync``` method to delete the record with the specified ID. If the deletion is successful, it navigates to the specified URL using the ```NavigationManager```.

Using the Component:
```
<BlazorAppGenericComponents.Components.Delete TModel="BlogPost"
                                              TViewModel="BlogPostViewModel"
                                              Id="@id"
                                              NavigateLinkAfterDelete="/BlogPost" />
```



###### **Source**
Full source code is available at this repository in GitHub: 
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppGenericComponents
