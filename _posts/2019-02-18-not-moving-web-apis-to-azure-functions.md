---
layout: post
title:  Why we're not moving our web APIs to azure functions yet
comments: true
---
I'm a big proponent of azure functions, having used it on a daily basis for over a year now (for both work and personal projects). The cost savings have been enormous, especially with the [consumption plan](https://docs.microsoft.com/en-in/azure/azure-functions/functions-scale#consumption-plan).

As you probably know, azure functions can be classified into three groups: _timer-triggered_, _data-triggered_ and _http-triggered_.

* We've converted a bunch of old CRON tasks to time-triggered azure functions.
* Also a bunch of our old azure webjobs have been converted to data-triggered azure functions (the transition has been seamless, very little code changes required to the input bindings & triggers).

However, today I want to talk about why we haven't converted our asp.net core web APIs to http-triggered azure functions. What's stopping us?

### 1. No SLA/guarantees around cold start latency

In our anecdotal observations, for the consumption plan, we've seen cold start latencies exceeding 45 seconds at times.

Since we're talking about the consumption plan, there is no _"always on"_ option for these function apps (and rightfully so). Yes, it is possible to resort to workarounds to keep the function app warm (by calling it every 'x' minutes from an external app). But this is unnecessarily tedious.

_Note: This [blog post](https://blogs.msdn.microsoft.com/appserviceteam/2018/02/07/understanding-serverless-cold-start/) is the only "official" documentation that we could find around cold start latencies in azure functions (it's a well written, detailed blog post)._

### 2. Azure functions v2.x don't yet support OpenAPI / swagger

We use azure [API management](https://docs.microsoft.com/en-in/azure/api-management/) (API gateway) as a "front-door" to our Web APIs. Since [v2.x azure functions don't yet support OpenAPI / swagger](https://docs.microsoft.com/en-us/azure/azure-functions/functions-openapi-definition#set-the-functions-runtime-version), it is not possible to import them into the azure API management ([related twitter thread](https://twitter.com/MithunShanbhag/status/1025052593221820417)).

You'll have to generate the openAPI definitions either manually or using external tools (swagger inspector, postman etc) which again is a tedious process. Currently, it is not possible to use [swashbuckle](https://github.com/domaindrivendev/Swashbuckle) with azure functions either.

FWIW - The azure functions team has been [aware of this issue](https://github.com/Azure/azure-functions-host/issues/2874) for a while. Hopefully, they'll add this support in their coming sprints.

### 3. Lack of dependency injection services (IoC container)

Yes, a whole slew of input bindings exist for azure functions. However it is not possible to inject custom dependencies (a la asp.net). A DI service support would have been really useful for instantiating entity framework dbContexts, auto-mapper profiles etc ([related twitter thread](https://twitter.com/MithunShanbhag/status/1014808563196166144)).

Currently, the "recommended" approach is to [use statics to cache these dependency objects](https://docs.microsoft.com/en-us/azure/azure-functions/manage-connections), but what we really want is a mechanism for creating & passing around ephemeral/transient dependencies.

### 4. Lack of a middleware mechanism

Would have been really nice to have a lightweight middleware (say) to [process JWT tokens](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authentication.jwtbearer?view=aspnetcore-2.2) across all http-triggered functions in a function app. Instead we now have to explicitly invoke helper methods from each http-triggered function.

### 5. No POCO model binding support

_Edit: I stand corrected. Looks like binding to POCOs is indeed supported [as shown in this example](https://github.com/Azure-Samples/functions-dotnet-codercards/blob/master/CoderCards/CardGenerator.cs#L47). More details [here](https://github.com/Azure/Azure-Functions/issues/397) and [here](https://github.com/Azure/Azure-Functions/issues/401)._

~~Azure functions do not support POCOs during request/response model binding. All types must be serialized to one of the following: `string`, `binary` or `stream`. You can choose to author a [custom binding](https://github.com/Azure/azure-webjobs-sdk/wiki/Creating-custom-input-and-output-bindings) as a workaround for this issue (which again is tedious).~~

Technically speaking, there is nothing preventing us from transitioning our web APIs to azure functions. We simply have to work around the above-mentioned issues (which are nitpicks really). However since our web APIs are still nascent & evolving, this transition will require us to sacrifice some developer productivity (e.g. the OpenAPI support issue).

So for the foreseeable future, we'll be sticking with asp.net core + azure web apps to host our web APIs. But really hoping the azure functions team addresses the above-mentioned issues, so we can go 'truly' serverless.

Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}}).
