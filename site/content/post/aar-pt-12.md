---
title: 'AAR pt 12 (C# and Azure)'
date: '2019-04-01T09:04:38-07:00'
---
![Azure banner](/img/blog/azure.png)

If you have not had a chance to read the first entry in the series for context, <a href="/post/after-action-review-aar/">you can do so here</a> 

Last weekend I went through my notebook and transcribed all of my hand-written lessons learned from the past year or so into my digital tracker.  A big chunk of those entries were leftovers from my time at Steelcase.  Since Steelcase is a ASP.NET core shop there were plenty of C# and Azure notes. Since Nautilus uses Kotlin and AWS, I figured this was as good a time as any to wrap up my experiences with the Microsoft stack.  <a href="/post/aar-pt-3-xamarin-c-azure/">You can read my last Azure AAR entry here</a>

* In Azure Active Directory, if your app registration contains spaces, you cannot use the name as part of the resource url.  Instead you must use the resource URL auto-generated for the registration in the application's settings tab.
* In C# you cannot extend dynamic objects (objects created at runtime)
.
* You can declare nullable properties in C# by appending `?` to the type.  The type itself must be non-nullable already.
 C# allows you to do pseudo-optional chaining as well.  A single null object in the chain will render the whole expression null, allowing you to do a single null check rather than nested 'if (object = null)" checks.
* C# has a "public bool ShouldSerialize()" method that you can implement in classes to create Boolean expression for when you want to include the property as a JSON value.
* You can create anonymous asyncs in C# with


```
 async () => { ... }
```

* C# allows you to use it's async/await paradigm to await the completion of multiple tasks executed in parallel.  This is done with the command 


```
 await Task.WhenAll(yourArrayOfTasksHere)
```

* The Azure Notification Hub for iOS does not allow you to access the registration Id of devices in any way.
* The Nuget package manager will default to the latest version of a library, even if it is in preview.  You should avoid this where possible, just as you would with any other dependency manager. Unlike other dependency managers such as cocoapods, maven, and gradle, Nuget has packages that extend other packages.  That means they won't work unless BOTH packages are included in the project.
* ASP is a C# framework that encapsulates all the Controllers & Routes necessary for building an MVC server
. In an ASP .NET core application you should avoid calling routes from other routes.
 You can run multiple micro-services simultaneously on the same MVC by editing their individual run configurations as .NET projects and ensuring they're operating on seperate ports.
* The IntelliJ Rider IDE is significantly better than Visual Studio Code for C# and ASP.NET Core development.  Use it where possible.
* In MVC the model binding for a route's parameters does NOT throw an error.  Instead it generates a default constructor object for Reference Types, a null for Nullables, an empty collection for Collections, and a default value (i.e. 0) for value types.  This can create subtle errors if a client fails to provide expected arguments.
* When instantiating services and http clients in the startup.cs file you can add various elements to the IServiceCollection services.  This includes Singletons, HttpClients, COR (Cross Origin Resource sharing), Authentication, Swagger, api versioning, MVC, MVC Core, and Automapper.
 Startup.cs is also where you define the request handling pipeline.  This is done with two functions, "ConfigureServices()" which declares services available throughout the app, and "Configure()" which declares actively running middle-ware such as swagger.
* In ASP .NET you can use the "\[Authorize]" attribute with a controller class' routes to ensure that the endpoint is only available to authorized users.  
To validate a token in ASP .Net you have to add token validation parameters and a JWTBearerAuthentication function in the "configure" function of your "startup.cs" class.
* Microsoft is partnered with digisign and Global Cert.  This allows them to refresh certificates in the Azure secure storage service automatically.
* The IOptions library in C# allows you to serialize and deserialize config .json as POCOs (Plain Old C Objects)
. When referring to a nested key in .NET core without IOptions you should use ':' to separate JSON keys.
* Unlike OkHttp & NSUrlSession you need a new client for every url/set of headers in .NET development.
* Razor pages are the ASP.NET solution to front-end development.  Pages use the ".cshtml" file extension.
