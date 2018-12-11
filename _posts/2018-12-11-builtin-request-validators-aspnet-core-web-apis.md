---
layout: post
title:  Built-in request validators for ASP.NET Core Web APIs.
---
There are various techniques for validating requests in ASP.NET Core Web APIs. Here is a quick recap: 

  * For simple DTO/REST model validation, you can use attributes like `[Required]`, `[Email]` etc from the [System.ComponentModel.DataAnnotation](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations?view=netcore-2.2) namespace ([more details here](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/validation?view=aspnetcore-2.2#validation-attributes)).

  * You can set up [custom model validation](https://docs.microsoft.com/en-us/aspnet/core/mvc/models/validation?view=aspnetcore-2.2#custom-validation).

  * For other miscellaneous validation (e.g. JWT validation), you can set up middleware. Several 1st & 3rd party nuget packages are available for these, I generally use the built-in [Microsoft.AspNetCore.Authentication.JwtBearer](https://github.com/aspnet/AspNetCore/tree/master/src/Security/src/Microsoft.AspNetCore.Authentication.JwtBearer) library.

  * You can also set up [route constraints](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing?view=aspnetcore-2.2#route-constraint-reference) like `{id:int}`, `{name:required}`, `{weight:float}` etc. Strictly speaking, they aren't for validation but for route disambiguation.

You can use 3rd party libraries like [FluentValidation](https://fluentvalidation.net/) to set up validation rules. FluentValidation middleware is executed before any of the above ([More details here](https://fluentvalidation.net/aspnet#asp-net-core)). Maybe I should write a blog post soon on this. 

_PS: This blog post emanated from a discussion on a slack channel. Thought I should capture it in case anyone else finds it useful. Did I get something wrong? Or miss out on some details? Would love to hear from you, [send me a tweet]({{site.author.twitter}})._

That's it for today folks!
