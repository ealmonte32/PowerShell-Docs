---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 11/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-service?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Service
---
# New-Service

## SYNOPSIS
Creates a new Windows service.

## SYNTAX

```
New-Service [-Name] <String> [-BinaryPathName] <String> [-DisplayName <String>] [-Description <String>]
 [-SecurityDescriptorSddl <String>] [-StartupType <ServiceStartupType>] [-Credential <PSCredential>]
 [-DependsOn <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## DESCRIPTION

The `New-Service` cmdlet creates a new entry for a Windows service in the registry and in the
service database. A new service requires an executable file that runs during the service.

The parameters of this cmdlet let you set the display name, description, startup type, and
dependencies of the service.

## EXAMPLES

### Example 1: Create a service

```powershell
New-Service -Name "TestService" -BinaryPathName '"C:\WINDOWS\System32\svchost.exe -k netsvcs"'
```

This command creates a service named TestService.

### Example 2: Create a service that includes description, startup type, and display name

```powershell
$params = @{
  Name = "TestService"
  BinaryPathName = '"C:\WINDOWS\System32\svchost.exe -k netsvcs"'
  DependsOn = "NetLogon"
  DisplayName = "Test Service"
  StartupType = "Manual"
  Description = "This is a test service."
}
New-Service @params
```

This command creates a service named TestService. It uses the parameters of `New-Service` to specify
a description, startup type, and display name for the new service.

### Example 3: View the new service

```powershell
Get-CimInstance -ClassName Win32_Service -Filter "Name='testservice'"
```

```Output
ExitCode  : 0
Name      : testservice
ProcessId : 0
StartMode : Auto
State     : Stopped
Status    : OK
```

This command uses `Get-CimInstance` to get the **Win32_Service** object for the new service. This
object includes the start mode and the service description.

### Example 4: Set the SecurityDescriptor of a service when creating.

This example adds the **SecurityDescriptor** of the service being created.

```powershell
$SDDL = "D:(A;;CCLCSWRPWPDTLOCRRC;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCLCSWLOCRRC;;;SU)"
$params = @{
  BinaryPathName = '"C:\WINDOWS\System32\svchost.exe -k netsvcs"'
  DependsOn = "NetLogon"
  DisplayName "Test Service"
  StartupType = "Manual"
  Description = "This is a test service."
  SecurityDescriptorSddl = $SDDL
}
New-Service @params
```

The **SecurityDescriptor** is stored in the `$SDDLToSet` variable. The **SecurityDescriptorSddl**
parameter uses `$SDDL` to set the **SecurityDescriptor** of the new service.

## PARAMETERS

### -BinaryPathName

Specifies the path of the executable file for the service. This parameter is required.

The fully qualified path to the service binary file. If the path contains a space, it must be quoted
so that it is correctly interpreted. For example, `d:\my share\myservice.exe` should be specified as
`'"d:\my share\myservice.exe"'`.

The path can also include arguments for an auto-start service. For example,
`'"d:\myshare\myservice.exe arg1 arg2"'`. These arguments are passed to the service entry point.

For more information, see the **lpBinaryPathName** parameter of
[CreateServiceW](/windows/win32/api/winsvc/nf-winsvc-createservicew) API.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: Path

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

Specifies the account used by the service as the
[Service Logon Account](/windows/desktop/ad/about-service-logon-accounts).

Type a user name, such as **User01** or **Domain01\User01**, or enter a **PSCredential** object,
such as one generated by the `Get-Credential` cmdlet. If you type a user name, this cmdlet prompts
you for a password.

Credentials are stored in a [PSCredential](/dotnet/api/system.management.automation.pscredential)
object and the password is stored as a [SecureString](/dotnet/api/system.security.securestring).

> [!NOTE]
> For more information about **SecureString** data protection, see
> [How secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring).

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DependsOn

Specifies the names of other services upon which the new service depends. To enter multiple service
names, use a comma to separate the names.

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Description

Specifies a description of the service.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisplayName

Specifies a display name for the service.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

Specifies the name of the service. This parameter is required.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: ServiceName

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -StartupType

Sets the startup type of the service. The acceptable values for this parameter are:

- **Automatic** - The service is started or was started by the operating system, at system start-up.
  If an automatically started service depends on a manually started service, the manually started
  service is also started automatically at system startup.
- **AutomaticDelayedStart** - Starts shortly after the system boots.
- **Disabled** - The service is disabled and cannot be started by a user or application.
- **InvalidValue** - This value is not supported. Using this value results in an error.
- **Manual** - The service is started only manually, by a user, using the Service Control Manager,
  or by an application.

 The default value is **Automatic**.

```yaml
Type: Microsoft.PowerShell.Commands.ServiceStartupType
Parameter Sets: (All)
Aliases:
Accepted values: Automatic, Manual, Disabled, AutomaticDelayedStart, InvalidValue

Required: False
Position: Named
Default value: Automatic
Accept pipeline input: False
Accept wildcard characters: False
```

### -SecurityDescriptorSddl

Specifies the **SecurityDescriptor** for the service in **Sddl** format.

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: sd

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

Prompts you for confirmation before running the cmdlet.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

Shows what would happen if the cmdlet runs. The cmdlet is not run.

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### CommonParameters

This cmdlet supports the common parameters: -Debug, -ErrorAction, -ErrorVariable,
-InformationAction, -InformationVariable, -OutVariable, -OutBuffer, -PipelineVariable, -Verbose,
-WarningAction, and -WarningVariable. For more information, see
[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

## INPUTS

### None

You cannot pipe input to this cmdlet.

## OUTPUTS

### System.ServiceProcess.ServiceController

This cmdlet returns an object that represents the new service.

## NOTES

This cmdlet is only available on Windows platforms.

To run this cmdlet, start PowerShell by using the **Run as administrator** option.

## RELATED LINKS

[Get-Service](Get-Service.md)

[Restart-Service](Restart-Service.md)

[Resume-Service](Resume-Service.md)

[Set-Service](Set-Service.md)

[Start-Service](Start-Service.md)

[Stop-Service](Stop-Service.md)

[Suspend-Service](Suspend-Service.md)

[Remove-Service](Remove-Service.md)