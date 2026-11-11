# 02 – Windows Endpoints: Audit Policy, PowerShell Logging, Sysmon

> Status: **Planned.** This is the build procedure. It will be updated with the steps actually
> taken and screenshots once `WIN10-01` and `WIN10-02` are configured.

Applies to `WIN10-01` and `WIN10-02`. Run everything from an elevated PowerShell prompt.

## 1. Static IP and hostname

```powershell
Rename-Computer -NewName "WIN10-01" -Restart
# after reboot
New-NetIPAddress -InterfaceAlias "Ethernet0" -IPAddress 10.10.10.101 -PrefixLength 24
```

## 2. Advanced audit policy

```powershell
auditpol /set /subcategory:"Logon" /success:enable /failure:enable
auditpol /set /subcategory:"Credential Validation" /success:enable /failure:enable
auditpol /set /subcategory:"Special Logon" /success:enable
auditpol /set /subcategory:"Process Creation" /success:enable
```

Include the command line in event 4688:

```powershell
reg add "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\Audit" /v ProcessCreationIncludeCmdLine_Enabled /t REG_DWORD /d 1 /f
```

Verify:

```powershell
auditpol /get /category:"Logon/Logoff"
```

## 3. PowerShell Script Block Logging (event 4104)

```powershell
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ScriptBlockLogging" /v EnableScriptBlockLogging /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\PowerShell\ModuleLogging" /v EnableModuleLogging /t REG_DWORD /d 1 /f
```

## 4. Sysmon

1. Download Sysmon from Microsoft Sysinternals.
2. Use the configuration in [../detections/sysmon/](../detections/sysmon/).
3. Install:

```powershell
.\Sysmon64.exe -accepteula -i sysmonconfig.xml
```

4. Verify that events appear under
   *Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational*.

## Verification checklist

- [ ] A failed logon creates event 4625
- [ ] Starting `notepad.exe` creates Sysmon event 1 with the full command line
- [ ] Running a PowerShell command creates event 4104
- [ ] Screenshots saved to `assets/screenshots/02-*`
