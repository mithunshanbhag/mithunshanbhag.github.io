---
layout: post
title: "High Availability in Azure (Part 9): App Service, Function Apps"
comments: true
---
_Note: This blog post is part 9 of a series centered around the topic of high availability in Azure:_

* _[Part 1: The basics]({{ site.baseurl }}{% post_url 2019-02-28-high-availability-azure-1-basics %})_
* _Part 2: SLAs and the 9s (coming soon)_
* _[Part 3: Availability Sets]({{ site.baseurl }}{% post_url 2019-03-29-high-availability-azure-3-availability-sets %})_
* _[Part 4: Availability Zones]({{ site.baseurl }}{% post_url 2019-03-31-high-availability-azure-4-availability-zones %})_
* _[Part 5: Storage redundancies]({{ site.baseurl }}{% post_url 2019-03-02-high-availability-azure-5-storage %})_
* _Part 6: Load balancing (coming soon)_
* _Part 7: Application gateways (coming soon)_
* _[Part 8: Traffic management]({{ site.baseurl }}{% post_url 2019-03-16-high-availability-azure-8-traffic %})_
* _**Part 9: App Service, Function Apps (this post)**_
* _Part 10: SQL (coming soon)_
* _Part 11: CosmosDB (coming soon)_
* _Part 12: Wrapping up (coming soon)_

_I'll not be addressing scaling (horizontal or vertical), backups/restores and resiliency/healing in these posts. Each of those topics deserve their own series, perhaps I'll write about them in the future if time permits._

---

## Azure App Service Apps (web apps)

![azure storage account](../../../images/13-azure-app-service.png)

An [Azure App Service Plan](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans) is pinned to a specific [Azure Region](../../../2019/02/28/high-availability-azure-1-basics.html#region). Any [App Service Apps](https://docs.microsoft.com/en-us/azure/app-service/overview) created in the App Service Plan will be provisioned in that same region. If your app needs additional redundancies in other regions or [geographies](../../../2019/02/28/high-availability-azure-1-basics.html#geography), you'll have to:

1. Provision them yourself (you'll need to create new App Service Plans in those regions, if they don't already exist).
2. Use [Azure Traffic Manager]({{ site.baseurl }}{% post_url 2019-03-16-high-availability-azure-8-traffic %}) to route traffic to all available redundancies (you can only specify one App Service endpoint per region in a Traffic Manager profile). [More details here](https://docs.microsoft.com/en-us/azure/app-service/web-sites-traffic-manager#app-service-and-traffic-manager-profiles).

_[image attribution: [Azure documentation](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/basic-web-app)]_
![azure app service redundancy](../../../images/14-azure-app-service-redundancy.jpg)

The [SLA for Azure App Services](https://azure.microsoft.com/en-in/support/legal/sla/app-service/v1_4/) guarantee a 99.95% uptime for each regional deployment.

## Azure Function Apps

![azure functions](../../../images/15-azure-functions.png)

[Azure Function Apps](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) too have regional deployments. If you're using the [consumption plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale#consumption-plan), then you explicitly specify the region. If on the [App Service Plan](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale#app-service-plan), then the region is the same as that of the App Service Plan.

Similar to App Services above, any additional redundancies will have to be explicitly created and traffic to these will have to be routed via Azure Traffic Manager.

_[image attribution: [Azure documentation](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/multi-region)]_
[![azure functions redundancy](../../../images/16-azure-functions-redundancy.jpg)](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/app-service-web-app/multi-region)

The [SLA for Azure Functions](https://azure.microsoft.com/en-us/support/legal/sla/functions/v1_1/) guarantee a 99.95% uptime for each regional deployment (for both app service plan and consumption plan).

## Miscellaneous

### Horizontally scaled instances

As [I've previously mentioned](../../../2019/02/28/high-availability-azure-1-basics.html#what-about-vm-scale-sets), horizontal auto-scaling exists to address performance concerns rather than high-availability concerns.

_**App Service Apps**_: When horizontal auto-scaling is enabled on a parent App Service Plan, additional instances are created, and each instance hosts all App Service Apps contained in the parent App Service Plan. All instances are created in the same [WebSpace](#webspaces). The App Service's integrated load-balancer (non-accessible) manages the traffic. Note that all scaled out instances of an app will still have the same endpoint URL.

_**Function Apps**_: Based on a combination of factors (trigger types, rate of incoming requests, language/runtime and perhaps the [host health-monitor stats](https://github.com/Azure/azure-functions-host/wiki/Host-Health-Monitor)), the [scale controller](https://docs.microsoft.com/en-in/azure/azure-functions/functions-scale#runtime-scaling) will create additional instances of an Azure Function App (max limit of 200 instances). Note that the scaling unit is the Function App (host) itself and not individual functions.  

Bonus reading:

* Read more about the [scaling limits imposed on App Service Apps](https://docs.microsoft.com/en-in/azure/azure-subscription-service-limits#app-service-limits) based on [pricing tiers](https://azure.microsoft.com/en-us/pricing/details/app-service/windows/).
* Read more about [ARR affinity](https://stackoverflow.com/a/49651618) and [ARRAffinity cookies](https://azure.microsoft.com/en-in/blog/disabling-arrs-instance-affinity-in-windows-azure-web-sites/) for scaled out instances.
* You can now enable [per-app horizontal scaling](https://docs.microsoft.com/en-us/azure/app-service/manage-scale-per-app). More details [in this blog post](https://markheath.net/post/per-app-scaling-app-service).
* Read more about the [scaling behavior](https://docs.microsoft.com/en-in/azure/azure-functions/functions-scale#understanding-scaling-behaviors) of Function Apps.

### The "Always On" setting

If you have an App Service App or a Function App associated with an App Service Plan in the production or isolated tier, then you should consider enabling the "always on" setting. This ensures that your app is always running and never unloaded (default behavior is to deactivate/unload idle apps to conserve resources).

Notes:

* This setting is not available for App Service Apps in dev/test tier.
* Idle Function Apps in the consumption plan will be subject to [cold start latency](https://blogs.msdn.microsoft.com/appserviceteam/2018/02/07/understanding-serverless-cold-start/).

![azure app service always on](../../../images/17-azure-app-service-always-on.jpg)

### Cloning and Moving App Service Apps

Using Azure Powershell, it is possible to [create clones of existing App Service App](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-app-cloning) within the same region or in a new region. Please note that there are some [caveats/restrictions](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-app-cloning#current-restrictions) though.

You can also [move an App Service App to another App Service plan](https://docs.microsoft.com/en-us/azure/app-service/app-service-plan-manage#move-an-app-to-another-app-service-plan) as long as both the source plan and the destination plan are within the same [WebSpace](#webspaces).

FWIW, I've never tried this out myself.

And yes, like any other Azure Resource, App Service Plans and App Service Apps can be moved between resource groups.

### WebSpaces

WebSpaces are units of deployment for Azure App Service Plans. An [App Service Plan's WebSpace](https://docs.microsoft.com/en-us/azure/app-service/app-service-plan-manage#move-an-app-to-another-app-service-plan) is identified by the combination of its resource group and the region in its deployed. Any additional App Service Plan deployments to the same resource group + region combination gets assigned to the same WebSpace. See [more details here](https://github.com/projectkudu/kudu/wiki/ResourceGroup-VS.-WebSpace).

To see the WebSpace associated with an App Service App or App Service Plan, navigate to that resource in the Azure Resource Explorer (via the [Azure Portal](https://portal.azure.com/#blade/HubsExtension/ArmExplorerBlade) or via the [website](https://resources.azure.com/)) and see the `WebSpace` and `SelfLink` properties.

<br>
_That's all for today folks! Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}})._
