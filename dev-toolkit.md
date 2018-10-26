c# and typescript, some c and c++, some powershell. 

no on-premise
no vms, no containers (no machine administration)
Serverless. 

----------


## IDEs and code editors
- **visual studio**
    productivity power tools: 
- **visual studio code**   
- SQL server management studio (SMSS)
- LinqPad
- azure cli
- angular cli
* [powershell ise]()
cloud9

## Configuration, dependency and package management
Web platform installer
Chocolatey
Nuget
NPM
homebrew

## Diffing & merging tools

## Cloud computing 
Azure
    functions
    Azure web apps
    eventgrid, servicebus
    sql
    sql warehouse
    data factory
    cloud shell
    Azure storage explorer:
    table storage
    AppInsights
    Azure monitor
    eventhub
    api management/gateway
    azure search
    storage
    keyvault
    active directory
    redis
    cosmos db
    data lake storage

Auth0
sendgrid

## Testing utilities
XUnit
MOQ
Fiddler
Postman
JSON Formatter
Jasmine
karma

Windows subsystem for linux

Logopit plus

Authenticator apps

ngrok

winmerge

linter

logparser : https://www.microsoft.com/en-us/download/details.aspx?id=24659

git
github
[azure devops](): Formerly known as vsts. 
    boards
    repos
    pipelines

## Other miscellaneous tools
- paint.net
- gifcam

## Sharing code snippets
jsfiddle
github gists

Readthedocs
gitbook

## Team communication
- [onenote](): Pity it doesn't support markdown.
- [trello]()
- [slack]():
- [zoom]():
- [google hangouts]():
- [gsuite]
- [whatsapp]

## Document storage
* **[onedrive](), [google drive](), [dropbox]()**: All three get the job done. I use all of them and don't really have a favorite here.

----------

## Awesome tools that I no longer use
This is a list of really awesome tools & apps that were used very heavily by me in a past life (pretty much all of these are "windows-centric"). Hope you find these useful.  

* **[Debugging tools for windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools)**: This includes a family of windows debuggers and related diagnostic utilities. An "absolute must have" for developers who primarily work on the Windows platform.
  * **[WinDbg, CDB, NTSD](https://en.wikipedia.org/wiki/WinDbg)**: Extremely powerful user-mode debuggers for native windows applications. Great for troubleshooting issues both in live applications and in crash dumps. You can also author custom extensions for them as well as script their operations 
    * Tip: see [Roberto Farah's blog](https://blogs.msdn.microsoft.com/debuggingtoolbox/) for great examples on WinDbg scripts.
    * Apparently there is a [new (preview) version of windbg](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-using-windbg-preview), completely re-written from the ground up.
  * **[KD](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-using-kd-and-ntkd)**: We'd run pre-release OSes with KD (kernel debugger) enabled over null-modem cables (and later usb, tcp/ip). This was our primary mechanism for catching and investigating [BSOD](https://en.wikipedia.org/wiki/Blue_Screen_of_Death)s. Also NTSD piped to KD was the only mechanism for investigating issues with NT system services (that ran before user logon).
  * **[GFlags](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/gflags)**: An oddball utility that could toggle various user-mode windows diagnostic features. Mainly used for detecting attempts to access heap-memory addresses beyond allocation boundary (heap verification done using fill patterns).
  * **[UMDH](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/umdh)**: User mode dump heap, a utility for detecting memory leaks. Later integrated into GFlags itself.
  * **[IDNA/TTD](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-185-Time-Travel-Debugging-Introduction)**: Time travel debugging is like a flight data recorder for your application. Unlike a process dump which only captures "point in time" information, the TTD's generated trace file captures information over the application's entire lifetime. This enables some cool replay/rewind features that aids root cause analysis.
    * This used to be a microsoft internal-only tool, but is now being made publicly available in the [next (preview) version of WinDbg](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-using-windbg-preview).

* **Tools for .net diagnostics**:
  * **[SOS.dll](https://docs.microsoft.com/en-us/dotnet/framework/tools/sos-dll-sos-debugging-extension)**: WinDbg extension to debug managed code. Particularly useful for peeking into CLR structures & diagnosing GC issues.
  * **[PSSCOR](https://www.microsoft.com/en-us/download/details.aspx?id=21255)**: WinDbg extension useful for debugging asp.net issues.
  * **[ILDASM](https://docs.microsoft.com/en-us/dotnet/framework/tools/ildasm-exe-il-disassembler)**: Inspect metadata of a .net executable.
  * **[ILSPY](https://github.com/icsharpcode/ILSpy)** and **[.Net Reflector](https://www.red-gate.com/products/dotnet-development/reflector/)**: Similar to ILDASM above but also allows you to decompile IL to C# or VB.net 
  * **[PInvoke.net](http://pinvoke.net/)**: Easily generate pinvoke signatures for calling native Win32 API or COM components from .net code.  
  * **[CLR Profiler](https://www.microsoft.com/en-in/download/details.aspx?id=16273)**: A memory profiler which displayed the allocation profile & call tree using sankey diagrams.
  * **[PerfView](https://github.com/Microsoft/perfview)**: An [ETW](https://docs.microsoft.com/en-us/windows/desktop/etw/about-event-tracing) based utility for diagnosing performance issues in .net code.
  * **[MDbg](https://docs.microsoft.com/en-us/dotnet/framework/tools/mdbg-exe)**: A simple, yet powerful command-line debugger for .net code.

* **Other miscellaneous diagnostic utilities**  
    * **[Spy++](https://docs.microsoft.com/en-us/visualstudio/debugger/introducing-spy-increment?view=vs-2017)**: Visualize WndProc messages and window handles. Extremely useful for Win32 GUI programmers.
    * **[App Verifier](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/application-verifier)**: A runtime verification tool that monitors an application's interaction with the OS, profiling its use of kernel objects, registry, file system and Win32 apis (heap, handles, locks, and more) and flagging any violations.
    * **[Orca](https://docs.microsoft.com/en-us/windows/desktop/msi/orca-exe)**: Inspect and modify MSI installer packages.
    * **[OleView](https://docs.microsoft.com/en-us/windows/desktop/com/using-oleview)**: A convenience utility for inspecting registration details of COM components.
 
* **[Sysinternals suite](https://docs.microsoft.com/en-us/sysinternals/)**: An awesome, awesome suite of diagnostic & administration tools for Windows. I'll whip these out whenever needed, but I really don't use them on a daily basis anymore.
  * **[Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer)**: 'Task manager on steroids' would be an overly simplistic way to describe it. It does much, much more. You can view a process's handles, loaded DLLs and various perf stats. 
  * **[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)**: A flight data recorder of sorts that tracks an application's file, registry, network, process & thread activities. 
  * **[DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)**: A tool to monitor system-wide or application-specific debug output. Specifically user-mode debug outputs from Kernel32!OutputDebugString() or System.Diagnostics.Debug.Write(). Kernel mode debug outputs are also shown. 
  * **[AutoLogon](https://docs.microsoft.com/en-us/sysinternals/downloads/autologon)**: Automatically logs a users in to Windows. This utility piggybacks on the autologon keys baked in the windows registry.  
  * **[PSExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)**: Enables admins to execute commands and apps on remote windows machines. A quick-n-dirty version of ssh/telnet, minus the setup pain.
  * **[Disk2Vhd](https://docs.microsoft.com/en-us/sysinternals/downloads/disk2vhd)**: Useful tool that allows you to snapshot a disk as a VHD.

* **IDEs, code editors and related stuff**
  * **[Atom](https://atom.io/), [Notepad++](), [SublimeText](https://www.sublimetext.com/), [SlickEdit](https://notepad-plus-plus.org/)**: All great code editors in their own right. But once VSCode arrived on the scene, I was hooked on to it and never looked back.
  * **[FxCop](https://en.wikipedia.org/wiki/FxCop)**: Static analyzer for .net code. Now superseded by [roslyn analyzers](https://docs.microsoft.com/en-us/visualstudio/code-quality/install-roslyn-analyzers?view=vs-2017).

* **Scripting**
  * **[DOS/CMD/Batch scripting](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands)**: Never again!

* **Cloud computing**
  * **[Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/virtual-machines/)**: @todo 
  * **[azure VM Scale-sets](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/overview)**: @todo
  * **[Azure Service Fabric](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-overview)**: @todo
  * **[Azure WebJobs]()**: In the past, I've used WebJobs for my time-triggered & data-triggered tasks (the tasks execute in the context of the associated Azure app service). I've now moved these tasks to Azure functions, which is truly serverless and [no app-service is needed](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale#consumption-plan) (well unless you really [need it](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale#app-service-plan)).  

* **Configuration management**
  * **[PowerShell DSC](https://docs.microsoft.com/en-us/powershell/dsc/overview)**: Allows you to express your system's DSC (desired state configuration) via a psuedo-declarative (PowerShell based) language. Very effective for managing state of Windows machines (both on-prem and Azure VMs).

* **Miscellaneous administration tools**
  * **[Remote desktop connection](https://support.microsoft.com/en-in/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection)**: Allows an admin to log into a remote windows machine. Also known as mstsc (microsoft terminal services client).
  * **[Remote desktop connection manager](https://www.microsoft.com/en-in/download/details.aspx?id=44989)**: The "enterprise" version of mstsc, allows an admin to log into a group/pool of remote windows machines.
  * **[openssh](@todo)**: @todo
  * **[mmc](@todo)**: @todo. Some of the snap-ins that I used very actively were:
    * **[certmgr.msc](https://docs.microsoft.com/en-us/dotnet/framework/tools/certmgr-exe-certificate-manager-tool)**: @todo
    * **[devmgmt.msc](@todo)**: a.k.a. the windows device manager.
    * **[diskmgmt.msc](@todo)**: @todo
    * **[eventvwr.msc](https://en.wikipedia.org/wiki/Event_Viewer)**: @todo
    * **[perfmon.msc](@todo)**: @todo
    * **[services.msc](@todo)**: Manage the set of NT background services running on your machine.
    * **[taskschd.msc](https://docs.microsoft.com/en-us/windows/desktop/taskschd/task-scheduler-start-page)**: @todo 
    * **[compmgmt.msc](@todo)**: @todo
    * **[fsmgmt.msc](@todo)**: @todo
  * **[regedit](https://support.microsoft.com/en-us/help/82821/registration-info-editor-regedit-command-line-switches)**: @todo 
  * **[hyper-v manager](@todo)**: @todo
  * **[server manager](@todo)**: @todo
  * **[resource monitor]()**: Lists top resource-consuming app and services (CPU, memory, disk I/O and network I/O). 
  * **[iis]()**: @todo
  * **[wmic](https://docs.microsoft.com/en-us/windows/desktop/wmisdk/wmic)**: @todo

* **Team communication**
  * **[microsoft office/office-365](https://www.office.com/)**: @todo
  * **[skype](https://www.skype.com/en/)**: My least preferred option for video chat, I try to avoid it as much as possible. But you'd be surprised by how many people out there use this as their primary tool for video conferencing.

----------

That's it folks! Know of any awesome tool that I should be using? Would love to hear from you, [send me a tweet]({{site.author.twitter}}).