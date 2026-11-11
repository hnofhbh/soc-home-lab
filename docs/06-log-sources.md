# 06 – Log Source Reference

The events this lab relies on, and why each one matters for detection.

## Windows Security log

| Event ID | Meaning | Used for |
|---|---|---|
| 4624 | Successful logon | Spotting a success after many failures (brute-force that worked) |
| 4625 | Failed logon | Brute-force and password-spraying detection |
| 4648 | Logon with explicit credentials | Credential use and lateral movement |
| 4672 | Special privileges assigned to new logon | Admin-level logons |
| 4688 | Process creation (with command line once enabled) | Process execution when Sysmon is not available |

Logon types that matter in 4624/4625: `2` interactive, `3` network (SMB), `10` remote interactive (RDP).

## Sysmon (Microsoft-Windows-Sysmon/Operational)

| Event ID | Meaning | Used for |
|---|---|---|
| 1 | Process creation (image, command line, parent, hashes) | Suspicious PowerShell, Office spawning shells, execution from temp folders |
| 3 | Network connection | Process making unexpected outbound connections |
| 7 | Image loaded | DLL loading by unusual processes |
| 8 | CreateRemoteThread | Process injection |
| 11 | File created | Files dropped into user-writable folders |
| 22 | DNS query | Domains contacted by a process |

## PowerShell (Microsoft-Windows-PowerShell/Operational)

| Event ID | Meaning | Used for |
|---|---|---|
| 4103 | Module logging | Cmdlets and parameters executed |
| 4104 | Script Block Logging | The decoded script text, even when the command line was base64-encoded |

## Windows Defender (Microsoft-Windows-Windows Defender/Operational)

| Event ID | Meaning | Used for |
|---|---|---|
| 1116 | Malware detected | EICAR and other test-file detections |
| 1117 | Action taken on malware | Confirming quarantine or removal |

## Linux

| Source | Pattern | Used for |
|---|---|---|
| `/var/log/auth.log` | `Failed password for ... from <ip>` | SSH brute-force |
| `/var/log/auth.log` | `Invalid user ... from <ip>` | Username enumeration |
| `/var/log/auth.log` | `Accepted password for ... from <ip>` | Successful SSH logon after failures |
