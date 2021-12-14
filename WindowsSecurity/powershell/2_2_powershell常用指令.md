# powershell常用指令 
```
Get-Command
Get-Help
Get-Member
Get-Process
Get-Service
Get-Eventlog

Get-Item
Get-Module

Get-Wmiobject
Get-Ciminstance
```
- Get-Help Get-Module
- Get-Module -ListAvailable

## 參考資料

pcgeek86/cheatsheet.ps1
https://gist.github.com/pcgeek86/336e08d1a09e3dd1a8f0a30a9fe61c8a


## Get-Command ==> 用來判斷 PowerShell 中有哪些 WMI Cmdlet

Get-Command                                               # Retrieves a list of all the commands available to PowerShell
                                                          # (native binaries in $env:PATH + cmdlets / functions from PowerShell modules)
Get-Command -Module Microsoft*                            # Retrieves a list of all the PowerShell commands exported from modules named Microsoft*
Get-Command -Name *item                                   # Retrieves a list of all commands (native binaries + PowerShell commands) ending in "item"

Get-Help                                                  # Get all help topics
Get-Help -Name about_Variables                            # Get help for a specific about_* topic (aka. man page)
Get-Help -Name Get-Command                                # Get help for a specific PowerShell function
Get-Help -Name Get-Command -Parameter Module              # Get help for a specific parameter on a specific command

###################################################
# process
###################################################
Get-Command -Name *process

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-AppvVirtualProcess                             1.0.0.0    AppvClient
Function        Start-AppvVirtualProcess                           1.0.0.0    AppvClient
Cmdlet          Debug-Process                                      3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Enter-PSHostProcess                                3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Exit-PSHostProcess                                 3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Get-Process                                        3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Start-Process                                      3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Stop-Process                                       3.1.0.0    Microsoft.PowerShell.Management
Cmdlet          Wait-Process                                       3.1.0.0    Microsoft.PowerShell.Management


###################################################
# job
###################################################
 Get-Command -Name *job

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Get-PrintJob                                       1.1        PrintManagement
Function        Get-StorageJob                                     2.0.0.0    Storage
Function        Remove-PrintJob                                    1.1        PrintManagement
Function        Restart-PrintJob                                   1.1        PrintManagement
Function        Resume-PrintJob                                    1.1        PrintManagement
Function        Stop-StorageJob                                    2.0.0.0    Storage
Function        Suspend-PrintJob                                   1.1        PrintManagement
Cmdlet          Debug-Job                                          3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Disable-ScheduledJob                               1.1.0.0    PSScheduledJob
Cmdlet          Enable-ScheduledJob                                1.1.0.0    PSScheduledJob
Cmdlet          Get-Job                                            3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Get-ScheduledJob                                   1.1.0.0    PSScheduledJob
Cmdlet          Receive-Job                                        3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Register-ScheduledJob                              1.1.0.0    PSScheduledJob
Cmdlet          Remove-Job                                         3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Resume-Job                                         3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Set-ScheduledJob                                   1.1.0.0    PSScheduledJob
Cmdlet          Start-Job                                          3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Stop-Job                                           3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Suspend-Job                                        3.0.0.0    Microsoft.PowerShell.Core
Cmdlet          Unregister-ScheduledJob                            1.1.0.0    PSScheduledJob
Cmdlet          Wait-Job                                           3.0.0.0    Microsoft.PowerShell.Core

###################################################
# memory
###################################################
Get-Command -Name *memory

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-VMMemory                                       2.0.0.0    Hyper-V
Cmdlet          Set-VMMemory                                       2.0.0.0    Hyper-V

###################################################
# Filesystem
###################################################

New-Item -Path c:\test -ItemType Directory                  # Create a directory
mkdir c:\test2                                              # Create a directory (short-hand)

New-Item -Path c:\test\myrecipes.txt                        # Create an empty file
Set-Content -Path c:\test.txt -Value ''                     # Create an empty file
[System.IO.File]::WriteAllText('testing.txt', '')           # Create an empty file using .NET Base Class Library

Remove-Item -Path testing.txt                               # Delete a file
[System.IO.File]::Delete('testing.txt')                     # Delete a file using .NET Base Class Library



## invoke-item

- invoke-item "c:\windows\system32\calc.exe"

###################################################
# PowerShell Drives (PSDrives)
###################################################

Get-PSDrive                                                 # List all the PSDrives on the system
New-PSDrive -Name videos -PSProvider Filesystem -Root x:\data\content\videos  # Create a new PSDrive that points to a filesystem location
New-PSDrive -Name h -PSProvider FileSystem -Root '\\storage\h$\data' -Persist # Create a persistent mount on a drive letter, visible in Windows Explorer
Set-Location -Path videos:                                  # Switch into PSDrive context
Remove-PSDrive -Name xyz                                    # Delete a PSDrive

###################################################
# Data Management
###################################################

Get-Process | Group-Object -Property Name                   # Group objects by property name
Get-Process | Sort-Object -Property Id                      # Sort objects by a given property name
Get-Process | Where-Object -FilterScript { $PSItem.Name -match '^c' } # Filter objects based on a property matching a value
gps | where Name -match '^c'                                # Abbreviated form of the previous statement


###################################################
# Windows Management Instrumentation (WMI) (Windows only)
###################################################

Get-CimInstance -ClassName Win32_BIOS                       # Retrieve BIOS information
Get-CimInstance -ClassName Win32_DiskDrive                  # Retrieve information about locally connected physical disk devices
Get-CimInstance -ClassName Win32_PhysicalMemory             # Retrieve information about install physical memory (RAM)
Get-CimInstance -ClassName Win32_NetworkAdapter             # Retrieve information about installed network adapters (physical + virtual)
Get-CimInstance -ClassName Win32_VideoController            # Retrieve information about installed graphics / video card (GPU)

Get-CimClass -Namespace root\cimv2                          # Explore the various WMI classes available in the root\cimv2 namespace
Get-CimInstance -Namespace root -ClassName __NAMESPACE      # Explore the child WMI namespaces underneath the root\cimv2 namespace

## Get-WmiObject

- Get-WmiObject Win32_Processor
- gwmi win32_Processor
```
Caption           : Intel64 Family 6 Model 158 Stepping 10
DeviceID          : CPU0
Manufacturer      : GenuineIntel
MaxClockSpeed     : 3000
Name              : Intel(R) Core(TM) i5-8500 CPU @ 3.00GHz
SocketDesignation : LGA1151
```



###################################################
# Working with Modules
###################################################

Get-Command -Name *module* -Module mic*core                 # Which commands can I use to work with modules?

Get-Module -ListAvailable                                   # Show me all of the modules installed on my system (controlled by $env:PSModulePath)
Get-Module                                                  # Show me all of the modules imported into the current session

$PSModuleAutoLoadingPreference = 0                          # Disable auto-loading of installed PowerShell modules, when a command is invoked

Import-Module -Name NameIT                                  # Explicitly import a module, from the specified filesystem path or name (must be present in $env:PSModulePath)
Remove-Module -Name NameIT                                  # Remove a module from the scope of the current PowerShell session

New-ModuleManifest                                          # Helper function to create a new module manifest. You can create it by hand instead.

New-Module -Name trevor -ScriptBlock {                      # Create an in-memory PowerShell module (advanced users)
  function Add($a,$b) { $a + $b } }

New-Module -Name trevor -ScriptBlock {                      # Create an in-memory PowerShell module, and make it visible to Get-Module (advanced users)
  function Add($a,$b) { $a + $b } } | Import-Module

###################################################
# Module Management
###################################################

Get-Command -Module PowerShellGet                           # Explore commands to manage PowerShell modules

Find-Module -Tag cloud                                      # Find modules in the PowerShell Gallery with a "cloud" tag
Find-Module -Name ps*                                       # Find modules in the PowerShell Gallery whose name starts with "PS"

Install-Module -Name NameIT -Scope CurrentUser -Force       # Install a module to your personal directory (non-admin)
Install-Module -Name NameIT -Force                          # Install a module to your personal directory (admin / root)
Install-Module -Name NameIT -RequiredVersion 1.9.0          # Install a specific version of a module

Uninstall-Module -Name NameIT                               # Uninstall module called "NameIT", only if it was installed via Install-Module

Register-PSRepository -Name <repo> -SourceLocation <uri>    # Configure a private PowerShell module registry
Unregister-PSRepository -Name <repo>                        # Deregister a PowerShell Repository



