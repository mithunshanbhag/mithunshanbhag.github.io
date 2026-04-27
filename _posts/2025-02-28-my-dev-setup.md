---
layout: post
title: My local dev machine setup
comments: true
---

Recently, I had to replace my PC's old SSD, which meant I had to reinstall all my dev tools and utilities. I thought I'd document my setup here for posterity. This is a living document, and I'll keep updating it as my setup evolves.

My primary development environment is WSL (Linux on Windows), and macOS occasionally. Developing with .NET on Azure is where I'm most comfortable at, but I also dabble heavily in Angular / TypeScript, and other frameworks & clouds occasionally.

![my pc](../../../images/my-pc.jpg)

## IDEs, code editors, extensions/plugins

* [LinqPad](https://www.linqpad.net/): Quick scratchpad app to test out your .NET code snippets. Do buy the license for autocompletion. Worth every penny!
* [Visual Studio](https://visualstudio.microsoft.com/) with [ReSharper](https://www.jetbrains.com/resharper/) extension.
  * I also use the [ReSharper CLI tools](https://www.jetbrains.com/help/resharper/ReSharper_Command_Line_Tools.html), which include nifty utilities for code inspection and code cleanup.
* [Visual Studio Code](https://code.visualstudio.com/)
  * I use [Settings Sync](https://code.visualstudio.com/docs/editor/settings-sync) to ensure that my vscode settings & extensions are synced across all my machines.
  * I have some 50+ extensions installed ([see full list here](https://gist.github.com/mithunshanbhag/ad7c4d02856eac62244ccaf37d29ea2c)).

> I've been meaning to play with [Rider](https://www.jetbrains.com/rider/) for a while now. I've heard good things about it, especially for .NET Core development. But I'm so set in my ways with Visual Studio + ReSharper (especially the key bindings) that I haven't made the switch yet.

## AI development tools

* [Github Copilot CLI](https://github.com/features/copilot/cli): My daily driver for AI-assisted coding. Allows me to choose from multiple models.
* [Codex Desktop App](https://developers.openai.com/codex/app): This app has made "chat with your codebase" seamless.
* [Codex CLI](https://developers.openai.com/codex/cli): Command-line interface for Codex.
* [ChatGPT Desktop App](https://chatgpt.com/download/): The OG.
* [NotebookLM](https://notebooklm.google.com/): Chat and converse with your documents & other sources of information. Love the "interactive podcast" feature.

> For now, my go-to model is `GPT 5.4 x-high`. Things are very rapidly evolving and new models, tools, IDEs, etc. are popping up all the time. I'm sure I'll be adding more to this list in the near future.

## Core utilities, terminal, scripting

* [WSL](https://docs.microsoft.com/en-us/windows/wsl/): For running Linux on Windows.
* [Windows Terminal](https://github.com/microsoft/terminal): A new, modern, feature-rich terminal application for command-line users.
* [Winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/): Basically "apt-get" for Windows.
* [PowerShell](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell?view=powershell-7.5): For ad-hoc scripting tasks.
* [Oh My Posh](https://ohmyposh.dev/): For customizing your PowerShell prompt. I use the [agnoster theme](https://ohmyposh.dev/docs/themes#agnoster) as-is.
* [Cascadia Code (font)](https://github.com/microsoft/cascadia-code): An awesome monospaced font; I use the nerd-font version in my terminal and IDEs.
* [Git](https://git-scm.com/): For version control.
  * Also enable [Git Credential Manager](https://github.com/GitCredentialManager/git-credential-manager#readme) for better authentication management.
  * And [Git LFS](https://git-lfs.github.com/) for managing large files.
  * And long path support: `git config --global core.longpaths true` (also do this in the [Windows OS](https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=powershell#enable-long-paths-in-windows-10-version-1607-and-later))
* [GitHub CLI](https://cli.github.com/): For managing GitHub repositories.
* [Clipboard History](https://support.microsoft.com/en-us/windows/using-the-clipboard-30375039-ce71-9fe4-5b30-21b7aab6b13f): For copy/pasting multiple items.
* [RipGrep](https://github.com/burntsushi/ripgrep): A faster grep variant that respects .gitignore files.

## Browser and web stuff

* [Firefox](https://www.mozilla.org/en-US/firefox/new/): My primary browser. I use the following extensions:
  * [Multi-account containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/): For managing multiple identities.
  * [uBlock Origin](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/): For blocking ads.
  * [Block site](https://addons.mozilla.org/en-US/firefox/addon/blocksite/): For blocking distracting websites.
  * [Dark Reader](https://addons.mozilla.org/en-US/firefox/addon/darkreader/): For dark mode.
  * [Grammarly](https://addons.mozilla.org/en-US/firefox/addon/grammarly-1/): For grammar checking.

* [Fiddler Classic](https://www.telerik.com/fiddler/fiddler-classic): For debugging web traffic.

> I've been meaning to try out [Fiddler Everywhere](https://www.telerik.com/fiddler/fiddler-everywhere), but haven't had the chance yet.

## Cloud, containers

> I'm pretty much all-in on Azure. I've dabbled with AWS and GCP, but Azure is where I feel most comfortable.

* [Docker Desktop](https://www.docker.com/products/docker-desktop): For running containers locally (internally it uses WSL partitions).
* [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/): For managing Azure resources.
* [Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/overview): For managing Azure resources.
* [Azurite](https://github.com/Azure/Azurite): For emulating Azure Storage services locally.
* [Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/): For managing Azure storage accounts (including local storage emulator).
* [Azure Cosmos DB Emulator](https://docs.microsoft.com/en-us/azure/cosmos-db/local-emulator): For running Cosmos DB locally.

## Misc tools, utilities

* [DevToys](https://devtoys.app/): Collection of oddball developer utils (e.g. Base64 encoder/decoder, JWT decoder, regex tester, JSON <-> YAML converter, etc).
* [Sysinternals Suite](https://docs.microsoft.com/en-us/sysinternals/): Tons of useful tools, including [ZoomIt](https://learn.microsoft.com/en-us/sysinternals/downloads/zoomit).
* [WinDirStat](https://windirstat.net/): For visualizing disk usage.
* [Microsoft PowerToys](https://github.com/microsoft/PowerToys): A set of Windows utilities to enhance productivity. I mostly use fancy zones and the color picker.
* [Handy](https://handy.computer/): Voice to text transcriber. This is a great alternative to Windows' built-in `WIN+H` dictation tool.
* [WinMerge](https://winmerge.org/): Useful for diffing directories and their nested contents.
* [1Password](https://1password.com/): For managing passwords & keys.
* [CloudFlare WARP](https://one.one.one.one/): DNS resolver and VPN service.
* [TailScale](https://tailscale.com/): Secure VPN between my devices.
* [DB Browser for SQLite](https://sqlitebrowser.org/): A nice GUI for SQLite DB management, queries.

## Misc services

* [Trello](https://trello.com/): For managing tasks.
* [Microsoft Todo](https://todo.microsoft.com/): Also for managing tasks.
* [Microsoft 365](https://www.microsoft.com/en-us/microsoft-365): For office apps, but mostly for OneNote and OneDrive.

## Frameworks, libraries, SDKs

* [.NET](https://docs.microsoft.com/en-us/dotnet/core/tools/): SDK, runtime and CLI for managing .NET core projects.
* [NodeJS](https://nodejs.org/en/): For some server-side development, managing NPM packages.
* [TypeScript](https://www.typescriptlang.org/): Mostly for building Angular apps.
* [Angular](https://angular.dev/installation): For building SPAs / web apps.

That's it folks! Know of any awesome tool that I should be using? Would love to hear from you, [send me a tweet]({{site.author.twitter}}).
