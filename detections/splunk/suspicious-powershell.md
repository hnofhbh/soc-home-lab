# Suspicious PowerShell Execution

| Field | Value |
|---|---|
| MITRE ATT&CK | [T1059.001 – PowerShell](https://attack.mitre.org/techniques/T1059/001/), [T1027 – Obfuscated Files or Information](https://attack.mitre.org/techniques/T1027/) |
| Data sources | Sysmon event 1, PowerShell event 4104 |
| Index | `win` |
| Status | **Draft:** to be validated against lab data |

## Logic

Two complementary views:

1. **How PowerShell was started** (Sysmon 1): encoded commands, hidden windows, and policy bypass
   flags are common in malicious use and rare in normal administration.
2. **What PowerShell actually ran** (4104): Script Block Logging records the decoded script text,
   so base64 encoding on the command line does not hide the content.

## Query 1: suspicious PowerShell command-line flags (Sysmon)

```spl
index=win sourcetype="XmlWinEventLog:Microsoft-Windows-Sysmon/Operational" EventCode=1
    (Image="*\\powershell.exe" OR Image="*\\pwsh.exe")
    (CommandLine="* -enc*" OR CommandLine="* -EncodedCommand*"
     OR CommandLine="* -w hidden*" OR CommandLine="* -WindowStyle Hidden*"
     OR CommandLine="* -nop*" OR CommandLine="* -ExecutionPolicy Bypass*")
| table _time host User ParentImage Image CommandLine
| sort - _time
```

## Query 2: suspicious script content (Script Block Logging)

```spl
index=win sourcetype="WinEventLog:Microsoft-Windows-PowerShell/Operational" EventCode=4104
    (ScriptBlockText="*FromBase64String*" OR ScriptBlockText="*DownloadString*"
     OR ScriptBlockText="*DownloadFile*" OR ScriptBlockText="*Net.WebClient*"
     OR ScriptBlockText="*Invoke-Expression*" OR ScriptBlockText="*IEX*")
| table _time host ScriptBlockText
| sort - _time
```

## Expected false positives

- Software deployment and management tools that launch PowerShell with `-ExecutionPolicy Bypass`
- Administrator scripts that download files from internal servers

Triage step: check the parent process. PowerShell started by `explorer.exe` or a management agent is
usually expected; PowerShell started by an Office application or a browser is not.

## Results

_To be filled with screenshots and real values after the scenario runs._
