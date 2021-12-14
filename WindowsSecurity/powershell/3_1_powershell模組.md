#
###

## ch7.MODULES

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

## 模組架構與組件

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


## 分析簡易模組

- [darkoperator/Posh-SecMod](https://github.com/darkoperator/Posh-SecMod/blob/master/Registry/Registry.ps1)

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


```powershell

```


## 
```
PS C:\WINDOWS\system32> cd c:\
PS C:\> Get-content -Path .\IPAddresses.csv
```
