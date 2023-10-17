---
title: "Blazor Infinite Scrolling"
date: 2023-10-16T00:00:00+00:00
hero: blazor_dotnet.jpg
description: Blazor Infinite Scrolling
menu:
  dotnet:
    name: Blazor Infinite Scrolling
    identifier: blazor-infinite-scrolling
    weight: -20231016
tags: [Dotnet, Blazor Infinite Scrolling]
categories: [Dotnet, Blazor Infinite Scrolling]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blazor_dotnet.jpg" alt="blazor_dotnet" title="blazor_dotnet" style="border-radius: 20px;"><br>
<p>

#### **Blazor Infinite Scrolling**
Blazor: Blazor is a web framework developed by Microsoft that allows developers to build interactive web UIs using C# instead of JavaScript. It enables full-stack web development with .NET.

Infinite Scrolling: Infinite scrolling is a technique where new data is loaded and displayed as the user scrolls down the page. It provides a smooth and continuous browsing experience without the need for manual pagination.

Razor Components: Razor Components is a part of Blazor that allows developers to build reusable UI components using a combination of HTML and C#. These components can be used to create dynamic and interactive web applications.

##### **InfiniteScrollingItemsProviderRequest.cs**
The ```InfiniteScrollingItemsProviderRequest``` class is defined within the ```BlazorAppInfiniteScrolling.Components``` namespace. It is a sealed class, meaning it cannot be inherited from. The class has two properties: ```StartIndex``` and ```CancellationToken```.

```
namespace BlazorAppInfiniteScrolling.Components;

public sealed class InfiniteScrollingItemsProviderRequest
{
    public InfiniteScrollingItemsProviderRequest(int startIndex, CancellationToken cancellationToken)
    {
        StartIndex = startIndex;
        CancellationToken = cancellationToken;
    }

    public int StartIndex { get; }
    public CancellationToken CancellationToken { get; }
}

public delegate Task<IEnumerable<int>> ItemsProviderRequestDelegate(InfiniteScrollingItemsProviderRequest request);

public delegate Task<IEnumerable<T>> ItemsProviderRequestDelegate<T>(InfiniteScrollingItemsProviderRequest request);
```

The ```InfiniteScrollingItemsProviderRequest``` class has a constructor that takes two parameters: ```startIndex``` and ```cancellationToken```. The ```startIndex``` parameter represents the index from which the data should be loaded, and the ```cancellationToken``` parameter is used to cancel the data loading operation if needed.

The class also has two properties: ```StartIndex``` and ```CancellationToken```. The ```StartIndex``` property is a read-only property that returns the value passed in the constructor. The ```CancellationToken``` property is also read-only and returns the ```cancellation token``` passed in the constructor.

##### **InfiniteScrolling.razor**
Several ```classes``` and ```delegates``` that are essential for implementing infinite scrolling in Blazor.
```
@namespace BlazorAppInfiniteScrolling.Components

@typeparam T

@foreach (var item in _items)
{
    @ItemTemplate(item)
}
@if (_loading)
{
    @LoadingTemplate
}

@if (!_enumerationCompleted)
{
    <div @ref="_lastItemIndicator" style="height:1px;flex-shrink:0"></div>
}
```
The ```InfiniteScrollingItemsProviderRequest``` class represents a request for fetching items for infinite scrolling. It takes the ```start index``` and a ```cancellation token``` as parameters.

The ```ItemsProviderRequestDelegate``` delegate defines a method signature for fetching items based on the ```InfiniteScrollingItemsProviderRequest```. It can be used to provide a custom implementation for fetching data.

##### **InfiniteScrolling.razor.cs**
The ```InfiniteScrolling``` component is a generic component that takes a type parameter ```T```, which represents the type of the items to be displayed. It inherits from the ```ComponentBase``` class and implements the ```IAsyncDisposable``` interface.
```
using Microsoft.AspNetCore.Components;
using Microsoft.JSInterop;

namespace BlazorAppInfiniteScrolling.Components;

public partial class InfiniteScrolling<T> : ComponentBase, IAsyncDisposable
{
    private List<T> _items = new();
    private ElementReference _lastItemIndicator;
    private DotNetObjectReference<InfiniteScrolling<T>>? _currentComponentReference;
    private IJSObjectReference? _module;
    private IJSObjectReference? _instance;
    private bool _loading = false;
    private bool _enumerationCompleted = false;
    private CancellationTokenSource? _loadItemsCts;

    [Inject]
    private IJSRuntime? JSRuntime { get; set; }

    [Parameter]
    public ItemsProviderRequestDelegate<T>? ItemsProvider { get; set; }

    [Parameter]
    public RenderFragment<T>? ItemTemplate { get; set; }

    [Parameter]
    public RenderFragment? LoadingTemplate { get; set; }

    [JSInvokable]
    public async Task LoadMoreItems()
    {
        if (_loading)
            return;

        _loading = true;
        try
        {
            _loadItemsCts ??= new CancellationTokenSource();

            StateHasChanged();
            try
            {
                var newItems = await ItemsProvider(new InfiniteScrollingItemsProviderRequest(_items.Count, _loadItemsCts.Token));

                var previousCount = _items.Count;
                _items.AddRange(newItems);

                if (_items.Count == previousCount)
                {
                    _enumerationCompleted = true;
                }
                else
                {
                    await _instance.InvokeVoidAsync("onNewItems");
                }
            }
            catch (OperationCanceledException oce) when (oce.CancellationToken == _loadItemsCts.Token)
            {
            }
        }
        finally
        {
            _loading = false;
        }

        StateHasChanged();
    }

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            _module = await JSRuntime.InvokeAsync<IJSObjectReference>("import", "./infinite-scrolling.js");
            _currentComponentReference = DotNetObjectReference.Create(this);
            _instance = await _module.InvokeAsync<IJSObjectReference>("initialize", _lastItemIndicator, _currentComponentReference);
        }
    }

    public async ValueTask DisposeAsync()
    {
        if (_loadItemsCts != null)
        {
            _loadItemsCts.Dispose();
            _loadItemsCts = null;
        }

        if (_instance != null)
        {
            await _instance.InvokeVoidAsync("dispose");
            await _instance.DisposeAsync();
            _instance = null;
        }

        if (_module != null)
        {
            await _module.DisposeAsync();
        }

        _currentComponentReference?.Dispose();
    }
}
```
The ```component``` has several private fields, including a list of items ```_items```, a reference to the last item indicator ```_lastItemIndicator```, a reference to the current component ```_currentComponentReference```, and references to the JavaScript module ```_module``` and instance ```_instance```. It also has boolean flags _loading and ```_enumerationCompleted``` to track the loading state and whether the enumeration of items is completed.

The component has several properties, including an injected ```IJSRuntime``` for JavaScript interop, a ```ItemsProvider``` delegate for providing the items, an ItemTemplate for rendering the items, and a ```LoadingTemplate``` for rendering the loading indicator.

The component also defines the ```LoadMoreItems``` method, which is invoked when the user scrolls to the end of the list or grid. It handles the loading of more items by calling the ```ItemsProvider``` delegate and updating the UI.

The ```OnAfterRenderAsync``` method is overridden to initialize the ```JavaScript``` module and instance when the component is first rendered. It imports the JavaScript module, creates a reference to the current component, and initializes the JavaScript instance with the last item indicator and the component reference.

The ```DisposeAsync``` method is implemented to dispose of any resources used by the component, such as ```cancellation tokens```, JavaScript instances, and references.


##### **infinite-scrolling.js**
The ```initialize``` function takes two parameters: ```lastItemIndicator``` and ```componentInstance```. The ```lastItemIndicator``` is the element that indicates the last item in the list, and the ```componentInstance``` is the instance of the Blazor component.
```
export function initialize(lastItemIndicator, componentInstance) {
    const options = {
        root: findClosestScrollContainer(lastItemIndicator),
        rootMargin: '0px',
        threshold: 0,
    };

    const observer = new IntersectionObserver(async (entries) => {
        for (const entry of entries) {
            if (entry.isIntersecting) {
                observer.unobserve(lastItemIndicator);
                await componentInstance.invokeMethodAsync("LoadMoreItems");
            }
        }
    }, options);

    observer.observe(lastItemIndicator);

    return {
        dispose: () => dispose(observer),
        onNewItems: () => {
            observer.unobserve(lastItemIndicator);
            observer.observe(lastItemIndicator);
        },
    };
}

function dispose(observer) {
    observer.disconnect();
}

function findClosestScrollContainer(element) {
    while (element) {
        const style = getComputedStyle(element);
        if (style.overflowY !== 'visible') {
            return element;
        }
        element = element.parentElement;
    }
    return null;
}
```
The ```initialize``` function creates an ```Intersection Observer``` with the specified options and attaches a ```callback function``` to it. When the last item indicator element intersects with the ```viewport```, the callback function is ```triggered```. It then unobserves the ```last item indicator```, and asynchronously invokes the ```LoadMoreItems``` method on the Blazor component instance.

The ```dispose function``` is used to disconnect the ```Intersection Observer``` when it is no longer needed. The ```findClosestScrollContainer``` function is a helper function that finds the closest ```scrollable container``` element for the given element.


#### **Source**
Full source code is available at this repository in GitHub:
https://github.com/akifmt/DotNetCoding/tree/main/src/BlazorAppInfiniteScrolling
