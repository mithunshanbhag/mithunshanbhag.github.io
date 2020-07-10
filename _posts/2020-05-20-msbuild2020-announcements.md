---
layout: post
title:  My favorite announcements from MSBuild2020
comments: true
---

I had so much fun viewing the [MSBuild 2020 Virtual Conference](https://mybuild.microsoft.com/). Tons of amazing new products & features were announced over three days; here are a few that I'm personally excited about (ordered alphabetically):

## Azure CosmosDB

#### Jupyter notebook support

Jupyter notebooks can now be [enabled on CosmosDB](https://docs.microsoft.com/en-us/azure/cosmos-db/enable-notebooks) for facilitating [analysis & visualization of the stored data](https://docs.microsoft.com/en-us/azure/cosmos-db/use-python-notebook-features-and-commands).

![cosmosdb jupyter notebook support](https://assets.cloudskew.com/assets/blog/images/32-cosmosdb-jupyter-notebooks.png)

#### Customer-managed encryption keys

CosmosDB now supports encrypting its data at rest with [customer-managed keys](https://docs.microsoft.com/en-us/azure/cosmos-db/how-to-setup-cmk).

![customer managed keys](https://assets.cloudskew.com/assets/blog/images/33-cosmosdb-cmk.png)

#### Autoscaled provisioned throughput

CosmosDB has introduced [autoscaled provisioned throughput](https://docs.microsoft.com/en-us/azure/cosmos-db/provision-throughput-autoscale) for workloads that have variable or unpredictable traffic. To enable AutoScale, you specify the max allowable throughput `Tmax`. Then, depending on the load, CosmosDB will then scale the throughput between `0.1 x Tmax` and `Tmax`.

#### Serverless SKU

CosmosDB will also introduce a new Serverless SKU in the coming months. This will be ideal for apps dealing with intermittent traffic spikes/bursts.

#### Free tier

And in case you still haven't heard, CosmosDB recently introduced a free tier!

_____

## Azure DevOps

#### Multi-stage YAML CD pipelines

In addition to CI, the multi-stage YAML pipelines now also support CD. You can now use [deployment jobs](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/deployment-jobs?view=azure-devops) to target your deployments at specific [environments](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/environments?view=azure-devops) (dev, test, staging, production etc).

![multi-stage yaml cd pipeline](https://assets.cloudskew.com/assets/blog/images/28-multistage-cd-pipeline.png)

By using environments it is also possible to define the [checks & approvals](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/approvals?view=azure-devops&tabs=check-pass) that need to be fulfilled for the deployment to start.

_[Read the full details](https://devblogs.microsoft.com/devops/azure-devops-roadmap-update-for-2020-q2/)_.

_____

## Azure Event Grid

#### Partner topics

If you're using Auth0 as your identity provider, you can hook up its log stream to Azure event grid as a [partner (3rd party) topic](https://azure.microsoft.com/en-in/updates/azure-event-grid-partner-topics-are-now-in-preview/). More partners will be unveiled in the coming months.

![event grid partner topic](https://assets.cloudskew.com/assets/blog/images/30-event-grid-partner-topics.png)

#### App Service events

Also you can now subscribe to multiple [App Service events via an event grid subscription](https://azure.microsoft.com/en-in/updates/app-service-is-now-an-events-publisher-on-azure-event-grid-in-preview/). This is great for customized health checks.

![app service events](https://assets.cloudskew.com/assets/blog/images/31-app-service-events.png)

#### Managed Identity support

Finally, Event Grid topics & domains [now support system assigned managed identity](https://azure.microsoft.com/en-in/updates/azure-event-grid-support-for-system-assigned-managed-identities-is-now-in-preview/).

_____

## Azure Monitor

The Azure Monitor team had way too many awesome announcements! Below are some of the ones that caught my eye.

#### App Insights now integrated with Log Workspaces

You can now [direct your application insights logs to a Log Analytics Workspace](https://docs.microsoft.com/en-us/azure/azure-monitor/app/create-workspace-resource). This helps unify all the APM and infra logs in one centralized location.

![application insights log workspaces](https://assets.cloudskew.com/assets/blog/images/34-appinsights-logworkspace.png)

#### Insights for multiple Azure resources

Insights for [Storage](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/storage-insights-overview) and [CosmosDB](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/cosmosdb-insights-overview) is now GA. Support for [Key Vault](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/key-vaults-insights-overview) and [Azure cache for Redis](https://docs.microsoft.com/en-us/azure/azure-monitor/insights/redis-cache-insights-overview) is now in preview.

#### Customer-managed encryption keys

You can now [encrypt your Log Analysis Workspace data with your own keys](https://docs.microsoft.com/en-us/azure/azure-monitor/platform/customer-managed-keys). Includes support for key revocation & rotation.

#### Change analysis support

This feature helps you [diff configuration changes to Azure resources](https://docs.microsoft.com/en-us/azure/azure-monitor/app/change-analysis) over a selected time range. Great for detecting drifts.

_____

## Azure Functions

#### Official PowerShell module

There's now an official PowerShell module `AZ.Functions` to manage Azure Functions. You can [download it from the PowerShell gallery](https://www.powershellgallery.com/packages/Az.Functions/1.0.0) or just run `Install-Module -Name Az.Functions` in a PS console.

_____

## Azure Private Link

#### Support for multiple services

Azure Private Link is [now available for multiple Azure services](https://azure.microsoft.com/en-us/updates/azure-private-link-is-now-available-for-multiple-new-azure-services/) like Event Hubs, Service Bus, Container Registry, Event Grid and Cognitive Search.

_____

## Azure Service Bus

#### Service Bus Explorer in portal

A Service Bus Explorer is now built into the Azure Portal itself. You can use it to send, receive and peek messages on your service bus. You no longer have to download and install the [standalone tool](https://github.com/paolosalvatori/ServiceBusExplorer).

![service bus explorer](https://assets.cloudskew.com/assets/blog/images/29-service-bus-explorer.png)

#### Large message support

Additionally, support for large messages (up to 100 MB) has been announced.

_____

## CodeSpaces

#### VS CodeSpaces

I've been using [VS CodeSpaces](https://docs.microsoft.com/en-us/visualstudio/online/overview/what-is-vsonline) for a while now as my dev machine in the cloud. You can access your VS CodeSpace environments via [VSCode (remote extension)](https://code.visualstudio.com/docs/remote/codespaces), Visual Studio 2019 or via the [Cloud-based IDE](https://online.visualstudio.com/).

Using a [devcontainer.json file](https://docs.microsoft.com/en-us/visualstudio/online/reference/configuring#codespaces-configuration-sample) it is possible to configure your CodeSpace (e.g. port forwarding). Using [dotfiles](https://docs.microsoft.com/en-us/visualstudio/online/reference/personalizing), the CodeSpace can be personalized on a per-user basis. And if you really want to, you can even [register your machine](https://docs.microsoft.com/en-us/visualstudio/online/how-to/self-hosting-vscode) as a CodeSpace environment (for remote access).

Initially branded as VS Online, the product was then renamed to VS CodeSpaces. Currently it is in private preview.

#### Github CodeSpaces

[Github CodeSpaces](https://github.com/features/codespaces/) (which uses VS CodeSpaces underneath the covers) allows you to spin up a dev environment on the fly directly from a github project.

_Read the full details [here](https://devblogs.microsoft.com/visualstudio/introducing-visual-studio-codespaces/) and [here](https://devblogs.microsoft.com/visualstudio/expanding-visual-studio-2019-support-for-visual-studio-codespaces/)_.

_____

## Windows PowerToys

#### New utilities added

In addition to the previously available [FancyZones](https://github.com/microsoft/PowerToys/tree/master/src/modules/fancyzones) and [Keyboard Shortcut Guide](https://github.com/microsoft/PowerToys/tree/master/src/modules/shortcut_guide), Windows PowerToys now has a bunch of new utilities baked-in:

* [PowerRename](https://github.com/microsoft/PowerToys/tree/master/src/modules/powerrename): Windows Shell Extension for advanced bulk renaming using search and replace or regular expressions.
* [File Explorer Preview Panes Add-On](https://github.com/microsoft/PowerToys/tree/master/src/modules/previewpane): For previewing markdown & svg files!
* [Keyboard Manager](https://github.com/microsoft/PowerToys/tree/master/src/modules/keyboardmanager): Utility for remapping keyboard keys & shortcuts.
* [Image Resizer](https://github.com/microsoft/PowerToys/tree/master/src/modules/imageresizer): File Explorer extension for bulk image resizing.
* [Launcher](https://github.com/microsoft/PowerToys/tree/master/src/modules/launcher): Press `ALT + SPACE` to bring up the "Mac spotlight" styled app launcher.

![windows powertoys launcher](https://assets.cloudskew.com/assets/blog/images/27-powertoys-launcher.png)

_____

## Windows Terminal

#### 1.0 release

Windows Terminal steps out of preview mode and into it's official (v1.0) release.

_[Read the full details](https://devblogs.microsoft.com/commandline/windows-terminal-1-0/)_.

_____

## WinGet

#### New package manager for Windows

Windows has a new "apt-get" styled package manager: [WinGet](https://devblogs.microsoft.com/commandline/windows-package-manager-preview/) _(currently in preview)_

![winget](https://assets.cloudskew.com/assets/blog/images/26-winget1.png)

_[Read the full details](https://devblogs.microsoft.com/commandline/windows-package-manager-preview/)_.

_____

## VSCode

#### Settings sync

VSCode Settings Sync will link your VSCode preferences/settings with your Microsoft or Github account. You can now sync your VSCode settings across multiple machines just by signing into VSCode. Works even on VS CodeSpaces' browser IDE.

Currently this feature is in preview and available on VSCode Insider builds.

_[Read the full details](https://code.visualstudio.com/docs/editor/settings-sync)_.

_____

## WSL2

#### GUI apps

In addition to command line apps, you can now look forward to running GUI apps on WSL2 in the coming months! Underneath the covers there is a built-in Wayland display server on Linux talking to a RDP client on Windows.

_[Read the full details](https://devblogs.microsoft.com/commandline/the-windows-subsystem-for-linux-build-2020-summary/)._

_____

I still haven't caught up with all the updates from the C# and .Net teams (I should perhaps do another separate post for that when I'm done).

_That's all for today folks! Comments? Suggestions? Thoughts? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}})._
