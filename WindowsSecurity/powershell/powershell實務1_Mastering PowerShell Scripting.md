## 測試資料來源
- [Mastering Windows PowerShell Scripting(April 2015)第一版](https://www.packtpub.com/product/mastering-windows-powershell-scripting/9781782173557)
- [GITHUB第三版(2019)](https://github.com/PacktPublishing/Mastering-Windows-PowerShell-Scripting-Third-Edition)
- [GITHUB第二版(2017)](https://github.com/PacktPublishing/Mastering-Windows-PowerShell-Scripting-Third-Edition)
  - Chapter 14. Working with REST and SOAP
- [Mastering PowerShell Scripting - Fourth Edition(Chris Dent,2021)](https://www.packtpub.com/product/mastering-powershell-scripting-fourth-edition/9781800206540)
  - Chapter 14. Web Requests and Web Services
  
## content
```
1.	Variables, Arrays, and Hashes
2.	Data Parsing and Manipulation
3.	Comparison Operators
4.	Functions, Switches, and Loops Structures
5.	Regular Expressions
6.	Error and Exception Handling and Testing Code
7.	Session-based Remote Management

8.	Managing Files, Folders, and Registry Items
9.	File, Folder, and Registry Attributes, ACLs, and Properties

10.	Windows Management Instrumentation

11.	XML Manipulation

12.	Managing Microsoft Systems with PowerShell
13.	Automation of the Environment

14.	Script Creation Best Practices and Conclusion
```

## Files, Folders, and the Registry
```
 interact with files, folders, and registry items. These techniques and items include:
• Registry provider
• Creating files, folders, registry keys, and registry named values
• Adding named values to registry keys
• Verifying the existence of item files, folders, and registry keys
• Renaming files, folders, registry keys, and named values
• Copying and moving files and folders
• Deleting files, folders, registry keys, and named values
```

#### To create a new folder and registry item, do the following action:
```powershell
New-item –path "c:\Program Files\" -name MyCustomSoftware –ItemType  Directory

New-item –path HKCU:\Software\MyCustomSoftware\ -force
```
#### To create a log file with a date included in the filename
```powershell
$logpath = "c:\Program Files\MyCustomSoftware\Logs\"
New-item –path $logpath –ItemType Directory | out-null
$itemname = (get-date –format "yyyyMMddmmss") + "MyLogFile.txt"
$itemvalue = "Starting Logging at: " + " " + (get-date)
New-item –path $logpath -name $itemname –ItemType File –value $itemvalue
$logfile = $logpath + $itemname
$logfile
```


#### To create a named value in the registry
```powershell
$regpath = "HKCU:\Software\MyCustomSoftware\"
$regname = "BuildTime"
$regvalue = "Build Started At: " + " " + (get-date)
New-itemproperty –path $regpath –name $regname –PropertyType String –value $regvalue
$verifyValue = Get-itemproperty –path $regpath –name $regname
Write-Host "The $regName named value is set to: " $verifyValue.$regname
```

#### To verify if files, folders, and registry entries exist
```powershell
$testfolder = test-path "c:\Program Files\MyCustomSoftware\Logs"
#Update The Following Line with the Date/Timestamp of your file
$testfile = test-path "c:\Program Files\MyCustomSoftware\
Logs\201503163824MyLogFile.txt"
$testreg = test-path "HKCU:\Software\MyCustomSoftware\"
If ($testfolder) { write-host "Folder Found!" }
If ($testfile) { write-host "File Found!" }
If ($testreg) { write-host "Registry Key Found!" }
```
# Windows Management Instrumentation
```
• WMI structure
• Using WMI objects
• Searching WMI classes
• Creating, modifying, and removing WMI object property instances
• Invoking WMI class methods
```

####
- get-wmiobject –namespace root\cimv2 –class win32_computersystem
- get-ciminstance –namespace root\cimv2 –class win32_computersystem

#### To retrieve all the attributes of the win32_computersystem class
- get-wmiobject –class win32_computersystem | get-member

#### Searching for WMI classes
- get-wmiobject –list | where {$_.Name –like "*Time*"}
- get-cimclass | where {$_.CimClassName -like "*Time*"}

```
 get-wmiobject -list | where {$_.Name -like "*Time*"}

   NameSpace: ROOT\cimv2
Name                                Methods              Properties
----                                -------              ----------
__TimerNextFiring                   {}                   {NextEvent64BitTime, TimerId}
MSFT_NetConnectionTimeout           {}                   {Milliseconds, SECURITY_DESCRIPTOR, Service, TIME_CREATED}
MSFT_NetTransactTimeout             {}                   {Milliseconds, SECURITY_DESCRIPTOR, Service, TIME_CREATED}
MSFT_NetReadfileTimeout             {}                   {Milliseconds, SECURITY_DESCRIPTOR, TIME_CREATED}
__TimerEvent                        {}                   {NumFirings, SECURITY_DESCRIPTOR, TIME_CREATED, TimerId}
__TimerInstruction                  {}                   {SkipIfPassed, TimerId}
__AbsoluteTimerInstruction          {}                   {EventDateTime, SkipIfPassed, TimerId}
__IntervalTimerInstruction          {}                   {IntervalBetweenEvents, SkipIfPassed, TimerId}
Win32_SystemTimeZone                {}                   {Element, Setting}
Win32_CurrentTime                   {}                   {Day, DayOfWeek, Hour, Milliseconds...}
Win32_LocalTime                     {}                   {Day, DayOfWeek, Hour, Milliseconds...}
Win32_UTCTime                       {}                   {Day, DayOfWeek, Hour, Milliseconds...}
Win32_TimeZone                      {}                   {Bias, Caption, DaylightBias, DaylightDay...}
Win32_PnPDevicePropertyDateTime     {}                   {Data, DeviceID, key, KeyName...}
Win32_PerfFormattedData_Counters... {}                   {Caption, Description, Frequency_Object, Frequency_PerfTime...
Win32_PerfRawData_Counters_Stora... {}                   {Caption, Description, Frequency_Object, Frequency_PerfTime...
Win32_PerfFormattedData_Microsof... {}                   {Caption, ClockFrequencyAdjustment, ClockFrequencyAdjustmen...
Win32_PerfRawData_MicrosoftWindo... {}                   {Caption, ClockFrequencyAdjustment, ClockFrequencyAdjustmen...
```

#### To leverage the get-cimclass cmdlet to view the class properties
```
$classProperties = (get-cimclass –class win32_Printer).CimClassProperties
$classProperties.count
```
#### To leverage the get-cimclass cmdlet to view the class methods
- (get-cimclass –class win32_Printer).CimClassMethods

#### To determine the writeable properties for the win32_environment class
```powershell
Get-cimclass win32_Environment | select –ExpandProperty 
CimClassProperties | where {$_.Qualifiers –match "write"}
```
#### Invoking WMI class methods

- get-cimclass win32_process | select –ExpandProperty CimClassMethods

- Invoke-CimMethod兩種運用方式
  - Invoke-CimMethod Win32_Process -MethodName "Create" -Arguments @{ CommandLine = 'mspaint.exe'}
  - Invoke-CimMethod –Query 'select * from Win32_Process where name like "mspaint.exe"' –MethodName "Terminate"

# Managing Microsoft Systems with PowerShell
### Managing Windows services

- Get-service –DisplayName "Windows Audio"
- Get-service –DisplayName "Windows Audio" –RequiredServices
- (Get-service –DisplayName "Windows Audio").Status 

#### To stop a service, you can call the stop-service cmdlet

- stop-service –DisplayName "Windows Audio"
- (Get-service –DisplayName "Windows Audio").Status
- start-service –DisplayName "Windows Audio"
- (Get-service –DisplayName "Windows Audio").Status

### Managing Windows processes
- Use get-process cmdlet to search for  available processes on a system. 
- By running the get-process cmdlet alone, you will get a report of all the running services on the system. 

```powershell
$process = get-process powersh*
$process
get-process -id $process.id
```
```
get-process powersh*

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
    774      38    88432     114280       6.73  22328   4 powershell
```

- The default record set that is  returned about the running services include:
  - Handles: The number of thread handles that are being used by a particular process
  - NPM (K): Non Paged Memory is the memory that is solely in physical memory, and not allocated to the page file that is being used by a process
  - PM (K): Pageable Memory is the memory that is being allocated to the page file that is used by a process
  - WS(K): Working Set is the memory recently referenced by a process
  - VM(M): Virtual Memory is the amount of virtual memory that is being used by a process
  - CPU(s): Processor time, or the time the CPU is utilizing a process
  - ID: An assigned Unique ID to a Process
  - Process name: The name of the process in memory
```powershell
## start-process
start-process -FilePath notepad.exe
$process = get-process notepad*


## stop-process
start-process -FilePath notepad.exe
$process = get-process notepad*
stop-process -ID $process.id
```

## ch13.Automation of the Environment
#### To launch an administrator PowerShell window
```powershell
$filepath = "powershell.exe"
$arguments = "-ExecutionPolicy RemoteSigned"
start-process –filepath $filepath –Verb RunAs -ArgumentList $arguments
```
#### To leverage invoke-item to launch calculator
- invoke-item "c:\windows\system32\calc.exe"

```powershell
$string = "ping 127.0.0.1"
invoke-expression $string
```

- invoke-expression -command c:\temp\scripts\masterscript.ps1 


# Ch.13.Web Requests and Web Services(第四版)
  - Technical requirements
  - Web requests
  - Working with REST
  - Working with SOAP

- Invoke-WebRequest https://blogs.technet.microsoft.com/heyscriptingguy/

```powershell
Invoke-WebRequest www.indented.co.uk  |
     Select-Object -ExpandProperty Headers

Key                         Value
---                         -----
Connection                  keep-alive
Access-Control-Allow-Origin *
x-proxy-cache               MISS
X-GitHub-Request-Id         0941:79C6:21A2B:3FC1E:61AF2F6E
Age                         35
X-Served-By                 cache-hkg17922-HKG
X-Cache                     HIT
X-Cache-Hits                10
X-Timer                     S1638983298.882694,VS0,VE0
Vary                        Accept-Encoding
X-Fastly-Request-ID         6d030316202e1f7ea85e59dc1228dc38530488ad
Accept-Ranges               bytes
Content-Length              1276
Cache-Control               max-age=600
Content-Type                text/html; charset=utf-8
Date                        Wed, 08 Dec 2021 17:08:17 GMT
Expires                     Tue, 07 Dec 2021 10:04:54 GMT
ETag                        "600f1d21-4fc"
Last-Modified               Mon, 25 Jan 2021 19:33:53 GMT
Server                      GitHub.com
Via                         1.1 varnish
```
