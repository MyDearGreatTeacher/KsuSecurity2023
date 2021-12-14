## 資料來源:教科書 ==> PowerShell 流程自動化攻略

- [PowerShell 流程自動化攻略 (Powershell for Sysadmins) Adam Bertram(2020-12-09)](https://www.tenlong.com.tw/products/9789865026677)  [github](https://github.com/adbertram/PowerShellForSysadmins)
- [Powershell 學習筆記](https://blog.darkthread.net/blog/powershell-learning-notes/)
- [PowerShell® Notes for Professionals book](https://books.goalkicker.com/PowerShellBook/)
- [Powershell Special Characters And Tokens](https://www.neolisk.blog/posts/2009-07-23-powershell-special-characters/)
```
Part I: Fundamentals
Chapter 1: Getting Started
Chapter 2: Basic PowerShell Concepts
Chapter 3: Combining Commands
Chapter 4: Control Flow
Chapter 5: Error Handling
Chapter 6: Writing Functions
Chapter 7: Exploring Modules

Part II: Automating Day-to-Day Tasks
Chapter 10: Parsing Structured Data
```
## Chapter 2: Basic PowerShell Concepts

###  Variable

- All variables in PowerShell start with a dollar sign ($), which indicates to PowerShell that you are calling a variable and not a cmdlet, function, script file, or executable file

- $MaximumHistoryCount 
  - `(內建變數built-in variable)` 
  - determines the maximum number of commands PowerShell saves in its command history;
  - default 4096 commands

- Variables in PowerShell come in two broad classes: 
  - user-defined variables
    - A variable needs to exist before you can use it
    - $color
    - $color = 'blue'
    - $color
    - Set-Variable -Name color -Value blue
    - Get-Variable -Name color
    - use Get-Variable to return all available variables  ==> Get-Variable
  - automatic variables ==> already exist in PowerShell.
    - the $null variable
    - $LASTEXITCODE
    - the preference variables.


### Data type
- Boolean Values
- Integers and Floating Points
- string ==> 
- 單引號與雙引號的差異
```PowerShell
PS> $language = 'PowerShell'
PS> $color = 'blue'
PS> $sentence = "Today, you learned that $language loves the color $color"
PS> $sentence
PS> 'Today, $name learned that $language loves the color $color'
```
- Objects ==> 物件屬性(properties)與物件方法(methods)
  - In PowerShell, everything is an object
```
PS> $color = 'red'
PS> $color
PS> Select-Object -InputObject $color -Property *
PS> $color.Length
PS> Get-Member -InputObject $color

PS> Get-Member -InputObject $color –Name Remove
```
```
PS> $color.Remove(1,1)
Rd
PS> $color
red
```
```
PS> $newColor = $color.Remove(1,1)
PS> $newColor
```

### Data Structures: Arrays |  ArrayLists | hashtables | 自訂物件

- Arrays
  - arrays in PowerShell have a fixed size.
  - When you change them, you can’t modify the size, so you have to create a new array.
```powershell
PS> $colorPicker = @('blue','white','yellow','black')
PS> $colorPicker
PS> $colorPicker[1]
PS> $colorPicker[1..3]
PS> $colorPicker = $colorPicker + 'orange'
PS> $colorPicker
PS> $colorPicker += @('pink','cyan')
PS> $colorPicker
```

- ArrayLists
  - ArrayLists behave nearly identically to the typical PowerShell array, but with one crucial difference: they don’t have a fixed size.
  - They can dynamically adjust to added or removed elements, giving a much higher performance when working with large amounts of data.
```powershell
PS> $colorPicker = [System.Collections.ArrayList]@('blue','white','yellow', 'black')
PS> $colorPicker
PS> $colorPicker.Add('gray')
```

- hashtable (or dictionary)
  - a PowerShell data structure that contains a list of key-value pairs.

```powershell
PS> $users = @{
abertram = 'Adam Bertram'
raquelcer = 'Raquel Cerillo'
zheng21 = 'Justin Zheng'
}

PS> $users
PS> $users['abertram']
PS> $users.abertram

PS> $users.Keys
PS> $users.Values

PS> Select-Object -InputObject $yourobject -Property *
```

- 自訂物件
```powershell
PS> $myFirstCustomObject = New-Object -TypeName PSCustomObject

PS> $myFirstCustomObject = [PSCustomObject]@{OSBuild = 'x'; OSVersion = 'y'}

PS> Get-Member -InputObject $myFirstCustomObject

PS> $myFirstCustomObject.OSBuild

PS> $myFirstCustomObject.OSVersion
```

## Chapter 3: Combining Commands
```powershell
PS> $serviceName = 'wuauserv'
PS> Get-Service -Name $serviceName
PS> Start-Service -Name $serviceName

PS> Get-Service -Name 'wuauserv' | Start-Service
PS> 'wuauserv' | Get-Service | Start-Service
```


```powershell

PS> Get-Content -Path C:\Services.txt
PS> Get-Content -Path C:\Services.txt | Get-Service
```

#### Parameter Binding
```
PS> 'string' | Get-Process

PS> Get-Help -Name Get-Service –Full


PS C:\> 'wuauserv' | Get-Service
```

```
PS> $serviceObject = [PSCustomObject]@{Name = 'wuauserv'; ComputerName = 'SERV1'}
PS> $serviceObject | Get-Service
```

## Chapter 4: Control Flow
```
Conditional Statements 
The if Statement
The else Statement 
The elseif Statement
The switch Statement

Using Loops 
The foreach Loop 
The for Loop 
The while Loop
The do/while and do/until Loops
```


```powershell
# HOW TO the contents of a file on multiple servers

$servers = @('SRV1','SRV2','SRV3','SRV4','SRV5')
```

-  get-help about_Comparison_Operators
-  [about_Comparison_Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.2)
`

```powershell
$servers = @('SRV1','SRV2','SRV3','SRV4','SRV5')

if (Test-Connection -ComputerName $servers[0] -Quiet -Count 1) {
  Get-Content -Path "\\$($servers[0])\c$\App_configuration.txt"
}
```

```powershell
if (Test-Connection -ComputerName $servers[0] -Quiet -Count 1) {
  if ($servers[0] –eq $problemServer) {
    Write-Error -Message "The server $servers[0] does not have the right file!"
  } else {
    Get-Content -Path "\\$servers[0]\c$\App_configuration.txt"
  }
} else {
  Write-Error -Message "The server $servers[0] is not responding!"
}
```

```powershell
if (-not (Test-Connection -ComputerName $servers[0] -Quiet -Count 1)){
   Write-Error -Message "The server $servers[0] is not responding!"
} elseif ($servers[0] –eq $problemServer) 
   Write-Error -Message "The server $servers[0] does not have the right file!"
} else {
   Get-Content -Path "\\$servers[0]\c$\App_configuration.txt" 
}
```


```powershell
$currentServer = $servers[0]
switch ($currentServer) {
   $servers[0] {
   # Check if server is online and get content at SRV1 path.
   break
}
  $servers[1] {
  ## Check if server is online and get content at SRV2 path.
  break
}
  $servers[2] {
  ## Check if server is online and get content at SRV3 path.
  break
}
```

### Using Loops 
- The foreach Loop 
- PowerShell will copy the object it’s looking at into the variable. 
- Note that because the variable is just a copy, you cannot directly change the item in the original list.

```powershell
foreach ($server in $servers) {
  Get-Content -Path "\\$server\c$\App_configuration.txt"
}
```


```powershell
$servers = @('SRV1','SRV2','SRV3','SRV4','SRV5')
  foreach ($server in $servers) {
    $server = "new $server"
}
$servers
```


```powershell
$servers = @('SRV1','SRV2','SRV3','SRV4','SRV5')
  ForEach-Object -InputObject $servers -Process {
    Get-Content -Path "\\$_\c$\App_configuration.txt"
}
```

```powershell
$servers | ForEach-Object -Process {
  Get-Content -Path "\\$_\c$\App_configuration.txt"
}
```


```powershell
$servers.foreach({Get-Content -Path "\\$_\c$\App_configuration.txt"})
```

- The for Loop 


```powershell
$servers = @('SERVER1','SERVER2','SERVER3','SERVER4','SERVER5')

for ($i = 0; $i –lt $servers.Length; $i++) {
  $servers[$i] = "new $server"
}

$servers
```

```powershell
for ($i = 1; $i –lt $servers.Length; $i++) {
  Write-Host $servers[$i] "comes after" $servers[$i-1]
}
```

- The while Loop

```powershell
while (Test-Connection -ComputerName $problemServer -Quiet -Count 1) {
  Get-Content -Path "\\$problemServer\c$\App_configuration.txt"
  break
}
```


- The do/while and do/until Loops

```powershell
do {
  $choice = Read-Host -Prompt 'What is the best programming language?'
} until ($choice -eq 'PowerShell')

Write-Host -Object 'Correct!'
```


## Chapter 5: Error Handling
```
Exceptions and Errors 
Handling Nonterminating Errors 
Handling Terminating Errors 
Exploring the $Error Automatic Variable 
```
```powershell

```


```powershell

```


## Chapter 6: Writing Functions
```
Adding Parameters to Functions
Creating a Simple Parameter 
The Mandatory Parameter Attribute
Default Parameter Values 
Adding Parameter Validation Attributes 

Accepting Pipeline Input
Adding Another Parameter 
Making the Function Pipeline Compatible
Adding a process Block 
```
- Functions vs. Cmdlets
  - The difference between a function and a cmdlet is how each of these constructs is made. 
  - A cmdlet isn’t written with PowerShell. It’s written in another language, typically something like C#, and then it’s compiled
and made available inside PowerShell. 
  - Functions, on the other hand, are written in PowerShell’s simple scripting language.
  - 練習指令== > Get-Command –CommandType Function
- Defining a Function
```
PS> function Install-Software { Write-Host 'I installed some software, Yippee!' }
PS> Install-Software
```
- Adding Parameters to Functions
  - PowerShell functions can have any number of parameters.
  - Creating a Simple Parameter==>  requires a param block, which will hold all the parameters for the function.

- advanced function vs basic functions


## Creating a Simple Parameter
```
function Install-Software {
  [CmdletBinding()]
  param(
    [Parameter()]
    [string] $Version
)
  Write-Host "I installed software version $Version. Yippee!"
}
```
## The Mandatory Parameter Attribute
```
function Install-Software {
  [CmdletBinding()]
  param(
    [Parameter(Mandatory)]
    [string] $Version
)
  Write-Host "I installed software version $Version. Yippee!"
}
```
### Default Parameter Values
```
function Install-Software {
  [CmdletBinding()]
  param(
    [Parameter()]
    [string] $Version =2 
)
  Write-Host "I installed software version $Version. Yippee!"
}
```
## Adding Parameter Validation Attributes
```powershell
function Install-Software {
  param(
  [Parameter(Mandatory)]
  #[ValidateSet('1','2')]
  [string]$Version
  )
  Get-ChildItem -Path \\SRV1\Installers\SoftwareV$Version
}

Install-Software -Version 3
```

## Accepting Pipeline Input
```
function Install-Software {
  param(
    [Parameter(Mandatory)]
    [ValidateSet('1','2')]
    [string]$Version,

# [Parameter(Mandatory, ValueFromPipeline)]
    [Parameter(Mandatory)]
    [string]$ComputerName
    )

  Write-Host "I installed software version $Version on $ComputerName. Yippee!"
}

#Install-Software -Version 2 -ComputerName "SRV1"  
$computers = @("SRV1", "SRV2", "SRV3")

foreach ($pc in $computers) {
  Install-Software -Version 2 -ComputerName $pc
}

$computers | Install-Software -Version 2
#$computers | Install-Software -Version 2
```
### process block
```
function Install-Software {
  param(
    [Parameter(Mandatory)]
    [ValidateSet('1','2')]
    [string]$Version,

    [Parameter(Mandatory, ValueFromPipeline)]
#    [Parameter(Mandatory)]
    [string]$ComputerName
    )

  process {
    Write-Host "I installed software version $Version on $ComputerName. Yippee!"
  }
  # Write-Host "I installed software version $Version on $ComputerName. Yippee!"
}
 
$computers = @("SRV1", "SRV2", "SRV3")
$computers | Install-Software -Version 2
```

### [CmdletBinding()](https://sabin.io/blog/when-to-use-cmdletbinding-in-powershell/)
- Recommendation
  - Use CmdletBinding in all your public and internal functions.
  - Write clean code.
  - Automate everything.

- 根據底下兩隻程式執行下列指令 find the differences
- Get-Message 
- Get-Message  –Verbose
- Get-Message –UseInternal –Verbose

```
function Get-MessageFromInternalFunction {
    Write-Output "Output from internal function"
    Write-Verbose "Verbose Message from internal function"    
}

function Get-Message {
    [cmdletbinding()]
    param([switch]$UseInternal)
    Write-Output "`nOutput message"
    Write-Verbose "This is a verbose message"
    if ($UseInternal) {
        Get-MessageFromInternalFunction
    }
}
```

```
function Get-MessageFromInternalFunction {
    [cmdletbinding()]
    param()
    Write-Output "Output from internal function"
    Write-Verbose "Verbose Message from internal function"    
}

function Get-Message {
    [cmdletbinding()]
    param([switch]$UseInternal)
    Write-Output "`nOutput message"
    Write-Verbose "This is a verbose message"
    if ($UseInternal) {
        Get-MessageFromInternalFunction -Verbose:$False
    }
}
```
## Chapter 7: EXPLORING MODULES

- Exploring Default Modules
  - Finding Modules in Your Session ==> Get-Module
  - Get-Command -Module Microsoft.PowerShell.Management
- powershell modules:
  - a PowerShell module is just a text file with a .psm1 file extension and some optional, extra metadata 
- binary modules 
- dynamic modules

## 預設模組

- Get-Module
```powershell

ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Computer, Add-Content, Checkpoint-Computer, Clear-Con...
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Add-Member, Add-Type, Clear-Variable, Compare-Object...}
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Get-PSReadLineOption, Remove-PS...
```

- Get-Command -Module Microsoft.PowerShell.Management


#### Using Get-Module to view all available modules

- Get-Module –ListAvailable

```powershell
get-module -Listavailable


    目錄: C:\Program Files\WindowsPowerShell\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Script     1.1.0      ClassExplorer                       {Find-Member, Find-Type, Find-Namespace, Get-Assembly...}
Script     1.0.1      Microsoft.PowerShell.Operation.V... {Get-OperationValidation, Invoke-OperationValidation}
Binary     1.0.0.1    PackageManagement                   {Find-Package, Get-Package, Get-PackageProvider, Get-Packa...
Script     3.4.0      Pester                              {Describe, Context, It, Should...}
Script     1.0.0.1    PowerShellGet                       {Install-Module, Find-Module, Save-Module, Update-Module...}
Script     2.0.0      PSReadline                          {Get-PSReadLineKeyHandler, Set-PSReadLineKeyHandler, Remov...


    目錄: C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules


ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Manifest   1.0.0.0    AppBackgroundTask                   {Disable-AppBackgroundTaskDiagnosticLog, Enable-AppBackgro...
Manifest   2.0.0.0    AppLocker                           {Get-AppLockerFileInformation, Get-AppLockerPolicy, New-Ap...
Manifest   1.0.0.0    AppvClient                          {Add-AppvClientConnectionGroup, Add-AppvClientPackage, Add...
Manifest   2.0.1.0    Appx                                {Add-AppxPackage, Get-AppxPackage, Get-AppxPackageManifest...
Script     1.0.0.0    AssignedAccess                      {Clear-AssignedAccess, Get-AssignedAccess, Set-AssignedAcc...
Manifest   1.0.0.0    BitLocker                           {Unlock-BitLocker, Suspend-BitLocker, Resume-BitLocker, Re...
Manifest   2.0.0.0    BitsTransfer                        {Add-BitsFile, Complete-BitsTransfer, Get-BitsTransfer, Re...
Manifest   1.0.0.0    BranchCache                         {Add-BCDataCacheExtension, Clear-BCCache, Disable-BC, Disa...
Manifest   1.0.0.0    CimCmdlets                          {Get-CimAssociatedInstance, Get-CimClass, Get-CimInstance,...
Manifest   1.0        ConfigCI                            {Get-SystemDriver, New-CIPolicyRule, New-CIPolicy, Get-CIP...
Manifest   1.0        ConfigDefender                      {Get-MpPreference, Set-MpPreference, Add-MpPreference, Rem...
Manifest   1.0        Defender                            {Get-MpPreference, Set-MpPreference, Add-MpPreference, Rem...
Manifest   1.0.2.0    DeliveryOptimization                {Delete-DeliveryOptimizationCache, Set-DeliveryOptimizatio...
Manifest   1.0.0.0    DirectAccessClientComponents        {Disable-DAManualEntryPointSelection, Enable-DAManualEntry...
Script     3.0        Dism                                {Add-AppxProvisionedPackage, Add-WindowsDriver, Add-Window...
Manifest   1.0.0.0    DnsClient                           {Resolve-DnsName, Clear-DnsClientCache, Get-DnsClient, Get...
Manifest   1.0.0.0    EventTracingManagement              {Start-EtwTraceSession, New-EtwTraceSession, Get-EtwTraceS...
Binary     1.0.0.0    HostComputeService                  {Get-ComputeProcess, Stop-ComputeProcess}
Manifest   1.0.0.1    HostNetworkingService               {Remove-HnsNamespace, Remove-HnsEndpoint, Get-HnsEndpoint,...
Script     1.1.0.0    IISAdministration                   {Get-IISAppPool, Start-IISCommitDelay, Stop-IISCommitDelay...
Manifest   2.0.0.0    International                       {Get-WinDefaultInputMethodOverride, Set-WinDefaultInputMet...
Manifest   1.0.0.0    iSCSI                               {Get-IscsiTargetPortal, New-IscsiTargetPortal, Remove-Iscs...
Script     1.0.0.0    ISE                                 {New-IseSnippet, Import-IseSnippet, Get-IseSnippet}
Manifest   1.0.0.0    Kds                                 {Add-KdsRootKey, Get-KdsRootKey, Test-KdsRootKey, Set-KdsC...
Manifest   1.0.1.0    Microsoft.PowerShell.Archive        {Compress-Archive, Expand-Archive}
Manifest   3.0.0.0    Microsoft.PowerShell.Diagnostics    {Get-WinEvent, Get-Counter, Import-Counter, Export-Counter...
Manifest   3.0.0.0    Microsoft.PowerShell.Host           {Start-Transcript, Stop-Transcript}
Manifest   1.0.0.0    Microsoft.PowerShell.LocalAccounts  {Add-LocalGroupMember, Disable-LocalUser, Enable-LocalUser...
Manifest   3.1.0.0    Microsoft.PowerShell.Management     {Add-Content, Clear-Content, Clear-ItemProperty, Join-Path...
Script     1.0        Microsoft.PowerShell.ODataUtils     Export-ODataEndpointProxy
Manifest   3.0.0.0    Microsoft.PowerShell.Security       {Get-Acl, Set-Acl, Get-PfxCertificate, Get-Credential...}
Manifest   3.1.0.0    Microsoft.PowerShell.Utility        {Format-List, Format-Custom, Format-Table, Format-Wide...}
Manifest   3.0.0.0    Microsoft.WSMan.Management          {Disable-WSManCredSSP, Enable-WSManCredSSP, Get-WSManCredS...
Manifest   1.0        MMAgent                             {Disable-MMAgent, Enable-MMAgent, Set-MMAgent, Get-MMAgent...
Manifest   1.0.0.0    MsDtc                               {New-DtcDiagnosticTransaction, Complete-DtcDiagnosticTrans...
Manifest   2.0.0.0    NetAdapter                          {Disable-NetAdapter, Disable-NetAdapterBinding, Disable-Ne...
Manifest   1.0.0.0    NetConnection                       {Get-NetConnectionProfile, Set-NetConnectionProfile}
Manifest   1.0.0.0    NetDiagnostics                      Get-NetView
Manifest   1.0.0.0    NetEventPacketCapture               {New-NetEventSession, Remove-NetEventSession, Get-NetEvent...
Manifest   2.0.0.0    NetLbfo                             {Add-NetLbfoTeamMember, Add-NetLbfoTeamNic, Get-NetLbfoTea...
Manifest   1.0.0.0    NetNat                              {Get-NetNat, Get-NetNatExternalAddress, Get-NetNatStaticMa...
Manifest   2.0.0.0    NetQos                              {Get-NetQosPolicy, Set-NetQosPolicy, Remove-NetQosPolicy, ...
Manifest   2.0.0.0    NetSecurity                         {Get-DAPolicyChange, New-NetIPsecAuthProposal, New-NetIPse...
Manifest   1.0.0.0    NetSwitchTeam                       {New-NetSwitchTeam, Remove-NetSwitchTeam, Get-NetSwitchTea...
Manifest   1.0.0.0    NetTCPIP                            {Get-NetIPAddress, Get-NetIPInterface, Get-NetIPv4Protocol...
Manifest   1.0.0.0    NetworkConnectivityStatus           {Get-DAConnectionStatus, Get-NCSIPolicyConfiguration, Rese...
Manifest   1.0.0.0    NetworkSwitchManager                {Disable-NetworkSwitchEthernetPort, Enable-NetworkSwitchEt...
Manifest   1.0.0.0    NetworkTransition                   {Add-NetIPHttpsCertBinding, Disable-NetDnsTransitionConfig...
Manifest   1.0.0.0    PcsvDevice                          {Get-PcsvDevice, Start-PcsvDevice, Stop-PcsvDevice, Restar...
Binary     1.0.0.0    PersistentMemory                    {Get-PmemDisk, Get-PmemPhysicalDevice, Get-PmemUnusedRegio...
Manifest   1.0.0.0    PKI                                 {Add-CertificateEnrollmentPolicyServer, Export-Certificate...
Manifest   1.0.0.0    PnpDevice                           {Get-PnpDevice, Get-PnpDeviceProperty, Enable-PnpDevice, D...
Manifest   1.1        PrintManagement                     {Add-Printer, Add-PrinterDriver, Add-PrinterPort, Get-Prin...
Binary     1.0.12     ProcessMitigations                  {Get-ProcessMitigation, Set-ProcessMitigation, ConvertTo-P...
Script     3.0        Provisioning                        {Install-ProvisioningPackage, Export-ProvisioningPackage, ...
Manifest   1.1        PSDesiredStateConfiguration         {Set-DscLocalConfigurationManager, Start-DscConfiguration,...
Script     1.0.0.0    PSDiagnostics                       {Disable-PSTrace, Disable-PSWSManCombinedTrace, Disable-WS...
Binary     1.1.0.0    PSScheduledJob                      {New-JobTrigger, Add-JobTrigger, Remove-JobTrigger, Get-Jo...
Manifest   2.0.0.0    PSWorkflow                          {New-PSWorkflowExecutionOption, New-PSWorkflowSession, nwsn}
Manifest   1.0.0.0    PSWorkflowUtility                   Invoke-AsWorkflow
Manifest   1.0.0.0    ScheduledTasks                      {Get-ScheduledTask, Set-ScheduledTask, Register-ScheduledT...
Manifest   2.0.0.0    SecureBoot                          {Confirm-SecureBootUEFI, Set-SecureBootUEFI, Get-SecureBoo...
Manifest   2.0.0.0    SmbShare                            {Get-SmbShare, Remove-SmbShare, Set-SmbShare, Block-SmbSha...
Manifest   2.0.0.0    SmbWitness                          {Get-SmbWitnessClient, Move-SmbWitnessClient, gsmbw, msmbw...
Manifest   1.0.0.1    StartLayout                         {Export-StartLayout, Import-StartLayout, Export-StartLayou...
Manifest   2.0.0.0    Storage                             {Add-InitiatorIdToMaskingSet, Add-PartitionAccessPath, Add...
Manifest   1.0.0.0    StorageBusCache                     {Clear-StorageBusDisk, Disable-StorageBusCache, Disable-St...
Manifest   2.0.0.0    TLS                                 {New-TlsSessionTicketKey, Enable-TlsSessionTicketKey, Disa...
Manifest   1.0.0.0    TroubleshootingPack                 {Get-TroubleshootingPack, Invoke-TroubleshootingPack}
Manifest   2.0.0.0    TrustedPlatformModule               {Get-Tpm, Initialize-Tpm, Clear-Tpm, Unblock-Tpm...}
Binary     2.1.639.0  UEV                                 {Clear-UevConfiguration, Clear-UevAppxPackage, Restore-Uev...
Manifest   2.0.0.0    VpnClient                           {Add-VpnConnection, Set-VpnConnection, Remove-VpnConnectio...
Manifest   1.0.0.0    Wdac                                {Get-OdbcDriver, Set-OdbcDriver, Get-OdbcDsn, Add-OdbcDsn...}
Manifest   1.0.0.0    WebAdministration                   {Start-WebCommitDelay, Stop-WebCommitDelay, Get-WebConfigu...
Manifest   2.0.0.0    Whea                                {Get-WheaMemoryPolicy, Set-WheaMemoryPolicy}
Manifest   1.0.0.0    WindowsDeveloperLicense             {Get-WindowsDeveloperLicense, Unregister-WindowsDeveloperL...
Script     1.0        WindowsErrorReporting               {Enable-WindowsErrorReporting, Disable-WindowsErrorReporti...
Manifest   1.0.0.0    WindowsSearch                       {Get-WindowsSearchSetting, Set-WindowsSearchSetting}
Manifest   1.0.0.0    WindowsUpdate                       Get-WindowsUpdateLog
Manifest   1.0.0.2    WindowsUpdateProvider               {Get-WUAVersion, Get-WULastInstallationDate, Get-WULastSca..
```

#### $PSModulePath environment variable

to add a new module path by using the $PSModulePath environment variable, which defines each module folder separated by a semicolon

```powershell
PS C:\> $env:PSModulePath
C:\Users\KSU\Documents\WindowsPowerShell\Modules;C:\Program Files\WindowsPowerShell\Modules;C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules
```

- 臨時加入環境變數
```powershell
$env:PSModulePath + ';C;\MyNewModulePath'.
```

- 永久加入環境變數 ==> use the SetEnvironment Variable() method on the Environment .NET class
```powershell
PS> $CurrentValue = [Environment]::GetEnvironmentVariable("PSModulePath", "Machine")
PS> [Environment]::SetEnvironmentVariable("PSModulePath", $CurrentValue + ";C:\MyNewModulePath", "Machine")
```

## 手動匯入模組

```powershell
PS> Import-Module -Name Microsoft.PowerShell.Management
PS> Import-Module -Name Microsoft.PowerShell.Management -Force
PS> Remove-Module -Name Microsoft.PowerShell.Management
```

## 模組架構與組件(The Components of a PowerShell Module)

## PowerShellGet與模組
```powershell
Get-Command -Module PowerShellGet
```
```
PS C:\> Get-Command -Module PowerShellGet

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Find-Command                                       1.0.0.1    PowerShellGet
Function        Find-DscResource                                   1.0.0.1    PowerShellGet
Function        Find-Module                                        1.0.0.1    PowerShellGet
Function        Find-RoleCapability                                1.0.0.1    PowerShellGet
Function        Find-Script                                        1.0.0.1    PowerShellGet
Function        Get-InstalledModule                                1.0.0.1    PowerShellGet
Function        Get-InstalledScript                                1.0.0.1    PowerShellGet
Function        Get-PSRepository                                   1.0.0.1    PowerShellGet
Function        Install-Module                                     1.0.0.1    PowerShellGet
Function        Install-Script                                     1.0.0.1    PowerShellGet
Function        New-ScriptFileInfo                                 1.0.0.1    PowerShellGet
Function        Publish-Module                                     1.0.0.1    PowerShellGet
Function        Publish-Script                                     1.0.0.1    PowerShellGet
Function        Register-PSRepository                              1.0.0.1    PowerShellGet
Function        Save-Module                                        1.0.0.1    PowerShellGet
Function        Save-Script                                        1.0.0.1    PowerShellGet
Function        Set-PSRepository                                   1.0.0.1    PowerShellGet
Function        Test-ScriptFileInfo                                1.0.0.1    PowerShellGet
Function        Uninstall-Module                                   1.0.0.1    PowerShellGet
Function        Uninstall-Script                                   1.0.0.1    PowerShellGet
Function        Unregister-PSRepository                            1.0.0.1    PowerShellGet
Function        Update-Module                                      1.0.0.1    PowerShellGet
Function        Update-ModuleManifest                              1.0.0.1    PowerShellGet
Function        Update-Script                                      1.0.0.1    PowerShellGet
Function        Update-ScriptFileInfo                              1.0.0.1    PowerShellGet

```

- 尋找模組 ==> Find-Module
```powershell
Find-Module -Name *VMware*
```

## 安裝模組  Install-Module
```powershell
Find-Module -Name VMware.PowerCLI | Install-Module
```

```powershell
Get-Module -Name VMware.PowerCLI -ListAvailable | Select-Object –Property ModuleBase
```

## remove a module from the PowerShell session ==> Remove-Module
##  Uninstalling Modules ==> Uninstall-Module
```powershell
Uninstall-Module -Name VMware.PowerCLI
```

## 自建模組

- a typical PowerShell module consists of a folder (the module container), .psm1 file (the module), and a .psd1 file (the module manifest).
- If the module folder is in one of the three locations (System, All Users, or Current User), PowerShell will automatically see this and import it.

```powershell
mkdir 'C:\Program Files\WindowsPowerShell\Modules\Software'
```


```powershell
Add-Content 'C:\Program Files\WindowsPowerShell\Modules\Software\Software.psm1'
```


```powershell
New-ModuleManifest -Path 'C:\Program Files\WindowsPowerShell\Modules\Software\Software.psd1'
-Author 'Hello ' -RootModule Software.psm1
-Description 'This module helps in deploying software.'
```

## 
```
PS C:\WINDOWS\system32> cd c:\
PS C:\> Get-content -Path .\IPAddresses.csv
```



