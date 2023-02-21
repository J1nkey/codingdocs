Native Cross-Platform applications
## Native Application: Là các application được làm ra dành cho 1 platform hay environment nhất định
### ví dụ: Applications chỉ làm cho Android/iOS/Web/Desktop...
### Cross-platform Application: là các applications làm ra để có thể chạy trên mọi platform...

Để build 1 application cho native cross-platform thì ta có .NET MAUI với C#

# What is .NET MAUI?
# Build cross-platform native applications
	* Desktop
	* Mobile

# What is .NET MAUI
# What is  the multi-platform app UI?

.NET MAUI is a brand new framework from Microsoft for building beautiful native and performant
cross-platform desktop and mobile apps for iOS, Android, Mac and Windows

We can do all from a shared single code base (C#) -> tất cả các app được build ra đều sử dụng hung
trên một nền code base là C#

### .NET MAUI
* What is in .NET MAUI project?
* Create UI 
* Doing advanced MVVM architecture and data binding
* Platform integrations
* Navigation
* ...


in .NET MAUI, we can access native APIs completely in C#
We can build native cross-platform user interfaces directly in:
* XAML (which is XML-based markup, which has nice features like data binding)
* Or, completely in C#

.NET MAUI is flexible, enabling we to build these user interfaces in many ways

When the .NET MAUI application startup, whatever code we create,
Example: let's say we have a button, .NET MAUI will generate and render the native control, this mean on:
* iOS, we're getting Ulkit
* Android, we're getting Widgets
* MAC, we're leveraging MAC Catalyst, which enabling we to run own iOS applications directly on MAC devices but also light up native MAC features
* Windows, leveraging the lastest Windows App SDK and WinUI 3

But we as a developer get to write everything in one shared code base (C#).

Additionally, besides creating user interfaces, we'll have all of own business logic that shared too.
### Shared Business Logic
* Models
* View Models
* RESTfull Service Calls
* Databases

Notice: We can access those native APIs directly in C# for the different platforms.
Importantly: .NET MAUI introduces a whole bunch of platform APIs that have been abstracted into common surface for us to code against. This means common things such as connectivity, geolocation, sensors and so much more (Secure settings, Flashlight, Sms, Email, Screen Lock, Text to Speech,...) are available in one common API just like the user interface is as well.



# .NET MAUI Architecture
Let's go to walk through all of important aspects of what is inside of that single project, including a bunch of different cross-platform APIs, framework and a bunch of great things to help we be super productive. 

(Khi tạo .NET Maui project, sẽ có 3 mục chính
* .NET MAUI App
* .NET MAUI Blazor App (Hybrid application)
* .NET MAUI Class Library

chú ý: Class Library cực kỳ hữu dụng khi ta muốn share các classes và các elements khác giữa các .NET MAUI app khác nhau)


.NET MAUI Architecture
## Dependencies
> This folder gives we-all of own dependencies and frameworks in a single project
The default has .netx.x-platfrom dependencies, that means from Maui project, we're deploying to Android, iOS, Mac Catalyst, Windows...

## Platform folder
> This is really great thing because this enables us as developers to access platform specific native APIs.

In that have a little bit of scaffolding code in each of them, such as in Android manifest (AndroidManifest.xml) that defines different permissions and app resources.

There's also a littble of startup code like the MainActivity.cs. The .NET MAUI team has done a fantastic job of minimizing this boilerplate code as much as humanly possible (giảm thiểu các code có sẵn càng ít càng tốt).

All you know that if we need to tweak something on the platform, we can access it right here.


## Resources folder
> Inside of this are shared cross-platform resources such as fonts, images, raw assets.

Advantage: we not only get to put all of our fonts and images into a single project. Here .NET MAUI will automatically put those into the correct places when it compiles it out for each platform.
Notice that the .NET MAUI bot (Images/dotnet_bot.svg), look at it all those paths, but when we compile up the application, it will automatic convert them into PNGs and scale them so they look great on all of own devices. 



## .csproj file
> In this file contains some amazing cross-platform capabilities built directly into the project system itself.

### <PropertyGroup>
1. TargetFrameworks: First and foremost (quan trọng nhất) thing, this indicate what target will be specified to build (android, ios, maccatalyst, windows,...). Also have samsung tizen platform, which is also supported from the Samsung team.

2. Other properties: 
* Cross-platform properties:
	* <ApplicationTitle>
	* <ApplicationIdGuid>: the identifiers
	* <ApplicationDisplayVersion> + <ApplicationVersion>: the version codes
> Really great about this is that we can set these in one place and for each platform.
They will automatically cascade down, so they will automatically set when we compile and deploy our application.

### <ItemGroup>
> This is where those resources come in.
Those are used for our app icon, splash screen,... Those are all cross-platform and generated for us automatically.

* <MauiImage>: Where our images are coming in. Automatically, it will bring in just any image that we put in that folder, but we can also include a single SVG file or PNG or JPEG and also update it as well with a base size. (Its really great for as SVGs)

* <MauiFont>: This is also telling it exactly where the fonts are located
* <MauiAsset>: Same.

> We can put fonts in multiple folders, we could specify different things and automatically this will pick up all the fonts and all the assets for us.


## MauiProgram.cs
> This is the start of any Maui application. This is the scaffolding of the application.

The startup code calls and returns a MauiApp (MauiApp is a type).
1. First, it will create a builder (MauiApp.CreateBuider()) -> very simillar pattern to ASP.NET Code.
2. Second, it's going to go ahead and create that builder, tell the builder which app we will use (.UseMauiApp<TApp> - which TApp is a Class that implement from IApplication, the TApp will have an .xaml file (UI) and its handler). And it's going to configure fonts later (.ConfigureFonts()).

> There're a lot of other things that we can configure as well, such as activity, lifecycles, services and the dependency services.

### What is anything in the App.xaml?
We can see that the app has some app wide resources in the both colors and the styles. Those resources are located in the Resources folder. (Colors.xaml, Styles.xaml)

This's great because these are going to be used in our styles, which is the full style set that automatically will style every single control that's built into .NET MAUI (in Styles.xaml).
So this means all we need to do is modify a few colors and all of the different controls will update based on what our application needs to look like.


## Code behind (.xaml.cs)
> Every xaml.cs is associated with the xaml.
We can see that the MainPage of the application is being set to App Shell.

### What's App Shell?
> An App Shell specifically is an opinionated shell of our application.

What's great here is that it enables content templates which are lazily loaded when our application loads up. 

Here, it's using a single shell piece (**<Shell>**) of content which is a "page", but we can easily add **flyout navigation** or **top and bottom tabs** by just adding more items. We also can even add menu files as well.

It's really flexible and it's also enables our eye-based navigation.

#### The Route property in **<ShellContent>**
Following the default setting, the Route of the app is set to MainPage. That's going to be our main Route.

There's MainPage and this main page will get inflated with home.

If we tap on main page, it's also is XAML, it's an XML-based markup.

### MainPage.xaml
What we have in MainPage is a scroll view (**<ScrollView>**).
Scroll view is one of those layouts with a vertical stack layout (**<VerticalStackLayout>**) inside of it.

And this is going to ahead and stack up some images, labels, and some buttons.

The button contains a Clicked property, which have a code behind for processing when the button is clicked (we can see the process in MainPage.xaml.cs)


## Run the application
Let's see the top bar, we have a drop-down debug menu, inside it, we can select the target framework that we want to debug (On item **Framework(netx.x-platform)**)


### There're have two things, we must notice in VS are the XAML Live Preview and Live Visual Tree
> XAML Live Preview allow we see the debug window when the window is navigated to another position where we cann't see it.
> Live Visual Tree is showing the structure of application as a tree


# Lesson 4: Build .NET MAUI UI with XAML
> Creating user interface and taking advantage of all the different layouts and controls built into .NET MAUI

We will build a Todo list, that enables us to keep track of all the things that we want to do.

### Build .NET MAUI app with XAML
*Create a Todo list
*UI Updates Responsively: How to update our user interface when the users interact with it.

**User interface example image is stored in Images folder**
**Some keywords we must search for:** ScrollView, CollectionView (CollectionView.ItemsSource, CollectionView.ItemPlate(DataTemplate)), SwipeView(SwipeView.RightItems|LeftItems, SwipeItem), Grid, Image, Entry, Button


##The Layout
This is maked by a Grid layout system.
In Grid layout, we have 2 attributes are RowDefinitions and ColumnDefinitions (search for this attributes for learning).

With grid system, everything inside must have specify the attributes Grid.Column, Grid.Row, Grid.ColumnSpan,...
The default value of Grid.Column and Grid.Row will be 0 (that means the element is at position row 0 and column 0.


**In this lesson, we notice to the change of MainPage.xaml**


# Lesson 5: .NET MAUI Data Binding with MVVM & XAML
In this lesson, we're going to do is extend that user interface and make it responsive and reactive using MVVM (Model-View-ViewModel).

## MVVM
This architecture pattern is very popular when we're developing applications with XAML, because it enables a **data binding**

Data binding is a way for user interface to 'respond' to our code behind and vice versa.
It's a way of managing our control and flow of data.

The view just knows how to display data, like there's a button, there's a label, there's an entry,... 

The ViewModel, we can think that is code behind but comepletely decoupled.
**And what is represeting or what to display?**
It may have a list of objects or strings and may know what to when a button is clicked and it may know what to  display in a label.

> The binding system inside .NET MAUI is what brings it all together.
Enabling our UI to automatically update our code behind and vice versa.

The binding system handles not only properties back and forth, but also events like those button clicks or swipe to deletes.











	