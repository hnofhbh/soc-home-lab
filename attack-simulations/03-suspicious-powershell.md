# Scenario 3 – Suspicious PowerShell Execution

| Field | Value |
|---|---|
| Target | `WIN10-01` (10.10.10.101) |
| MITRE | T1059.001 PowerShell, T1027 Obfuscated Files or Information |
| Status | Planned |

## Objective

Run harmless PowerShell commands using the same patterns attackers use (encoded command line,
hidden window, download from a web server) and confirm the activity is visible in Sysmon and
Script Block Logging, and that both SIEMs alert.

## Method

- Use the Atomic Red Team tests for T1059.001 on `WIN10-01`. Record the exact test numbers used.
- Any "download" step fetches a harmless text file from a web server hosted on `KALI-ATK`
  inside the lab network.
- Record start and end times.

## Expected telemetry

| Event | Where |
|---|---|
| Sysmon 1 with `powershell.exe` and encoded or hidden flags in the command line | Sysmon |
| 4104 containing the decoded script text | PowerShell Operational |
| Sysmon 3 connection to 10.10.10.66 | Sysmon |
| Wazuh custom rules 100100, 100101 | Wazuh dashboard |
| Results from [suspicious-powershell](../detections/splunk/suspicious-powershell.md) queries 1 and 2 | Splunk |

## Evidence checklist

- [ ] Screenshot showing the encoded command line (Sysmon 1)
- [ ] Screenshot showing the decoded script (4104)
- [ ] Screenshot of Wazuh alerts
- [ ] Incident report written in `incidents/`

## Run log

_Filled in after the run._
