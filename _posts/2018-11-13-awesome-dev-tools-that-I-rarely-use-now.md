---
layout: post
title:  Awesome dev tools that I rarely use now
comments: true
---

This is a list of really awesome tools, apps and utilities that were used very heavily by me in a past life. I still whip these out whenever needed, but I really don't use them on a daily basis anymore.

Pretty much all of these are "Windows OS centric" or  ".Net framework centric".

Thought I should take a small trip down the memory lane and capture these tools here in a blog post. Hopefully someone will find this list useful.

<br>**[Debugging tools for Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-download-tools)**: This includes a family of Windows debuggers and related diagnostic utilities. An "absolute must have" for developers who primarily work on the Windows platform.

  * **[WinDbg, CDB, NTSD](https://en.wikipedia.org/wiki/WinDbg)**: Extremely powerful user-mode debuggers for native Windows applications, based on the [dbgeng.dll](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/introduction) engine. Great for troubleshooting issues both in live applications and in crash dumps. You can also author custom extensions for them as well as script their operations.
    * Tip: see [Roberto Farah's blog](https://blogs.msdn.microsoft.com/debuggingtoolbox/) for great examples on WinDbg scripts.
    * Apparently there is a [new (preview) version of windbg](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-using-windbg-preview), completely re-written from the ground up.

  * **[KD](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-using-kd-and-ntkd)**: We'd run pre-release OSes with KD (kernel debugger) enabled over null-modem cables (and later usb, tcp/ip). This was our primary mechanism for catching and investigating [BSOD](https://en.wikipedia.org/wiki/Blue_Screen_of_Death)s. Also NTSD piped to KD was the only mechanism for investigating issues with NT system services (that ran before user logon).

  * **[IDNA/TTD](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-185-Time-Travel-Debugging-Introduction)**: Time travel debugging is like a flight data recorder for your application. Unlike a process dump which only captures "point in time" information, the TTD's generated trace file captures information over the application's entire lifetime. This enables some cool replay/rewind features that aids root cause analysis.
    * This used to be a microsoft internal-only tool, but is now being made publicly available in the [next (preview) version of WinDbg](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-using-windbg-preview).

  * **[GFlags](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/gflags)**: An oddball utility that could toggle various user-mode Windows diagnostic features. Mainly used for detecting attempts to access heap-memory addresses beyond allocation boundary (heap verification done using fill patterns).

  * **[UMDH](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/umdh)**: User mode dump heap, a utility for detecting memory leaks. Later integrated into GFlags itself.

<br>**Diagnostic tools for .Net framework**: Quite a few of the tools on this list were authored by my awesome co-workers on Microsoft's Common Language Runtime team (they started out as internal-only tools, but grew in usefulness over time and were then publicly shipped).

  * **[SOS.dll](https://docs.microsoft.com/en-us/dotnet/framework/tools/sos-dll-sos-debugging-extension)**: WinDbg extension to debug managed code. Particularly useful for peeking into CLR structures & diagnosing GC issues.

  * **[PSSCOR](https://www.microsoft.com/en-us/download/details.aspx?id=21255)**: WinDbg extension useful for debugging asp.net issues.

  * **[ILDASM](https://docs.microsoft.com/en-us/dotnet/framework/tools/ildasm-exe-il-disassembler)**: Inspect metadata of a .Net executable.

  * **[ILSPY](https://github.com/icsharpcode/ILSpy)** and **[.Net Reflector](https://www.red-gate.com/products/dotnet-development/reflector/)**: Similar to ILDASM above but also allows you to decompile IL to C# or VB.net.

  * **[PInvoke.net](http://pinvoke.net/)**: Easily generate pinvoke signatures for calling native Win32 API or COM components from .Net code.  

  * **[CLR Profiler](https://www.microsoft.com/en-in/download/details.aspx?id=16273)**: A memory profiler which displayed the allocation profile & call tree using sankey diagrams.

  * **[PerfView](https://github.com/Microsoft/perfview)**: An [ETW](https://docs.microsoft.com/en-us/windows/desktop/etw/about-event-tracing) based utility for diagnosing performance issues in .Net code.

  * **[MDbg](https://docs.microsoft.com/en-us/dotnet/framework/tools/mdbg-exe)**: A simple, yet powerful command-line debugger for .Net code.

  * **[CLRMD](https://github.com/Microsoft/clrmd)**: APIs for analyzing the .Net heap and other runtime structures from running managed processes and dumps. Hat tip to @leculver.

  * **[CCI](https://github.com/Microsoft/cci)**: a.k.a Common Compiler Infrastructure. APIs that allow manipulation of .Net executables (metadata, IL) and related PDB symbols.

<br>**[Sysinternals suite](https://docs.microsoft.com/en-us/sysinternals/)**: An awesome, awesome suite of diagnostic & administration tools for Windows. Looks like there is now an [effort underway to port these tools to linux](https://github.com/microsoft/procdump-for-linux).

  * **[Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer)**: 'Task manager on steroids' would be an overly simplistic way to describe it. It does much, much more. You can view a process's handles, loaded DLLs and various perf stats.

  * **[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon)**: A flight data recorder of sorts that tracks an application's file, registry, network, process & thread activities.

  * **[DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview)**: A tool to monitor system-wide or application-specific debug output. Specifically user-mode debug outputs from Kernel32!OutputDebugString() or System.Diagnostics.Debug.Write(). Kernel mode debug outputs are also shown.

  * **[AutoLogon](https://docs.microsoft.com/en-us/sysinternals/downloads/autologon)**: Automatically logs a users in to Windows. This utility piggybacks on the autologon keys baked in the Windows registry.  

  * **[PSExec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)**: Enables admins to execute commands and apps on remote Windows machines. A quick-n-dirty version of ssh/telnet, minus the setup pain.

  * **[Disk2Vhd](https://docs.microsoft.com/en-us/sysinternals/downloads/disk2vhd)**: Useful tool that allows you to snapshot a disk as a VHD.

<br>**Other miscellaneous Windows diagnostic utilities**  

  * **[Spy++](https://docs.microsoft.com/en-us/visualstudio/debugger/introducing-spy-increment?view=vs-2017)**: Visualize WndProc messages and window handles. Extremely useful for Win32 GUI programmers.

  * **[App Verifier](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/application-verifier)**: A runtime verification tool that monitors an application's interaction with the OS, profiling its use of kernel objects, registry, file system and Win32 apis (heap, handles, locks, and more) and flagging any violations.

  * **[DebugDiag v2](https://www.microsoft.com/en-us/download/details.aspx?id=49924)**: Another utility based on the dbgeng.dll engine. Useful for snapping dumps from IIS apps and analyzing them.

  * **[Orca](https://docs.microsoft.com/en-us/windows/desktop/msi/orca-exe)**: Inspect and modify MSI installer packages.

  * **[OleView](https://docs.microsoft.com/en-us/windows/desktop/com/using-oleview)**: A convenience utility for inspecting registration details of COM components.

  * **[Depends](https://en.wikipedia.org/wiki/Dependency_Walker)**: a.k.a. Dependency Walker. A utility that scanned an executable's import & export address tables to figure out its module dependencies.

  * **[PEDump](http://www.wheaty.net/downloads.htm)**: Dump the contents of a Windows [portable executable](https://docs.microsoft.com/en-us/windows/desktop/debug/pe-format) file.

<br>**IDEs, code editors and related stuff**

  * **[Atom](https://atom.io/), [Notepad++](https://notepad-plus-plus.org/), [SublimeText](https://www.sublimetext.com/), [SlickEdit](https://notepad-plus-plus.org/)**: All great code editors in their own right. But once VSCode arrived on the scene, I was hooked on to it and never looked back.

  * **[FxCop](https://en.wikipedia.org/wiki/FxCop)**: Static analyzer for .Net code. Now superseded by [roslyn analyzers](https://docs.microsoft.com/en-us/visualstudio/code-quality/install-roslyn-analyzers?view=vs-2017).

<br>**Windows administration tools**

  * **[Remote desktop connection](https://support.microsoft.com/en-in/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection)**: Allows an admin to log into a remote Windows machine. Also known as mstsc (microsoft terminal services client).

  * **[Remote desktop connection manager](https://www.microsoft.com/en-in/download/details.aspx?id=44989)**: The "enterprise" version of mstsc, allows an admin to log into a group/pool of remote Windows machines.

  * **[MMC](https://en.wikipedia.org/wiki/Microsoft_Management_Console)**: The "go to" toolset for Windows system administration. Some of the [extensions/snap-ins](https://blogs.msdn.microsoft.com/windowsvistanow/2009/03/30/running-microsoft-management-console-mmc-snap-ins-from-the-start-menu/) that I used very actively were:
    * **[certmgr.msc](https://docs.microsoft.com/en-us/dotnet/framework/tools/certmgr-exe-certificate-manager-tool)**: Manage certificate stores on your local Windows machine.
    * **[devmgmt.msc](https://en.wikipedia.org/wiki/Device_Manager)**: a.k.a. the Windows device manager.
    * **[diskmgmt.msc](https://docs.microsoft.com/en-us/windows-server/storage/disk-management/overview-of-disk-management)**: Manage disk partitions and volumes.
    * **[eventvwr.msc](https://en.wikipedia.org/wiki/Event_Viewer)**: See system-wide and application-specific event logs.
    * **fsmgmt.msc**: Lists all file shares on your machine along with any open connections to them, open files etc.  
    * **[perfmon.msc](https://en.wikipedia.org/wiki/Performance_Monitor)**: Choose from hundreds of performance counters to monitor & measure system-wide or application-specific perf stats.
    * **[services.msc](https://en.wikipedia.org/wiki/Service_Control_Manager)**: Manage the set of NT background services running on your machine.
    * **[taskschd.msc](https://docs.microsoft.com/en-us/windows/desktop/taskschd/task-scheduler-start-page)**: Create and schedule tasks to run on your machine. Define their trigger conditions and actions (commands, scripts, executables etc).
    * **compmgmt.msc**: A convenience snap-in that loads most of the various snap-ins mentioned above.
    * **dsa.msc**: Manage active directory users, groups, computer accounts, OUs (organizational units) etc.  

  * **[Regedit](https://support.microsoft.com/en-us/help/82821/registration-info-editor-regedit-command-line-switches)**: Duh. Windows's built-in registry editing tool.

  * **[Hyper-V Manager](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/about/)**: Allows you to configure & host VMs on your Windows Server machine.

  * **[Server Manager](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/server-manager)**: Allows you to enable/disable features + add/remove roles on your Windows Server machine.

  * **[Resource Monitor](https://en.wikipedia.org/wiki/Resource_Monitor)**: Lists top resource-consuming app and services (CPU, memory, disk I/O and network I/O).

  * **[WMIC](https://docs.microsoft.com/en-us/windows/desktop/wmisdk/wmic)**: The CLI for [WMI (Windows Management Instrumentation)](https://docs.microsoft.com/en-us/windows/desktop/wmisdk/about-wmi). Allows you to interact with various [WMI providers](https://docs.microsoft.com/en-us/windows/desktop/wmisdk/wmi-providers).

  * **[WBEMTest](https://msdn.microsoft.com/en-us/library/dn529014.aspx)**: GUI app to explore WMI namespaces and classes.

I'm pretty sure there were a couple more .Net and Windows tools that I've forgotten about. Know of any? Would love to hear from you, please leave a comment below or [send me a tweet]({{site.author.twitter}}).

That's it for today folks!