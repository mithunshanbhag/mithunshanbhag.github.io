---
layout: post
title:  MSBuild2020 announcements that I'm excited about
comments: true
---

Tons of amazing products & features were announced at [MSBuild 2020](https://mybuild.microsoft.com/); here are a few that I'm personally excited about (ordered alphabetically):

## Windows PowerToys: New utilities added

In addition to the previously available [FancyZones](https://github.com/microsoft/PowerToys/tree/master/src/modules/fancyzones) and [Keyboard Shortcut Guide](https://github.com/microsoft/PowerToys/tree/master/src/modules/shortcut_guide), Windows PowerToys now has a bunch of new utilities baked-in:

* [PowerRename](https://github.com/microsoft/PowerToys/tree/master/src/modules/powerrename): Windows Shell Extension for advanced bulk renaming using search and replace or regular expressions.
* [File Explorer Preview Panes Add-On](https://github.com/microsoft/PowerToys/tree/master/src/modules/previewpane): For previewing markdown & svg files!
* [Keyboard Manager](https://github.com/microsoft/PowerToys/tree/master/src/modules/keyboardmanager): Utility for remapping keyboard keys & shortcuts.
* [Image Resizer](https://github.com/microsoft/PowerToys/tree/master/src/modules/imageresizer): File Explorer extension for bulk image resizing.
* [Launcher](https://github.com/microsoft/PowerToys/tree/master/src/modules/launcher): Press `ALT + SPACE` to bring up the "Mac spotlight" styled app launcher.

![windows powertoys launcher](../../../images/27-powertoys-launcher.png)

_____

## WinGet

Windows has a new "apt-get" styled package manager: [WinGet](https://devblogs.microsoft.com/commandline/windows-package-manager-preview/) _(currently in preview)_

![winget](../../../images/26-winget1.png)

_____

## VSCode Settings Sync

VSCode Settings Sync will link your VSCode preferences/settings with your Microsoft or Github account. You can now sync your VSCode settings across multiple machines just by signing into VSCode. Works even on VS CodeSpaces' browser IDE.

Currently this feature is in preview and available on VSCode Insider builds. [More details](https://code.visualstudio.com/docs/editor/settings-sync).

_____

## CodeSpaces

I've been using [VS CodeSpaces](https://docs.microsoft.com/en-us/visualstudio/online/overview/what-is-vsonline) for a while now as my dev machine in the cloud. You can access your VS CodeSpace environments via [VSCode (remote extension)](https://code.visualstudio.com/docs/remote/codespaces), Visual Studio 2019 or via the [Cloud-based IDE](https://online.visualstudio.com/).

Using a [devcontainer.json file](https://docs.microsoft.com/en-us/visualstudio/online/reference/configuring#codespaces-configuration-sample) it is possible to configure your CodeSpace (e.g. port forwarding). Using [dotfiles](https://docs.microsoft.com/en-us/visualstudio/online/reference/personalizing), the CodeSpace can be personalized on a per-user basis. And if you really want to, you can even [register your machine](https://docs.microsoft.com/en-us/visualstudio/online/how-to/self-hosting-vscode) as a CodeSpace environment (for remote access).

Initially branded as VS Online, the product was then renamed to VS CodeSpaces. Currently it is in private preview.

[Github CodeSpaces](https://github.com/features/codespaces/) (which uses VS CodeSpaces underneath the covers) allows you to spin up a dev environment on the fly directly from a github project.

_____

## WSL2: GUI apps, GPU workloads

In addition to command line apps, you can now look forward to running GUI apps on WSL2 in the coming days!

The full recap of WSL2 related announcements at MSBuild 2020 can be [seen here](https://devblogs.microsoft.com/commandline/the-windows-subsystem-for-linux-build-2020-summary/).

_____
