# MITRE ATT&CK Coverage

Each row maps a technique simulated in the lab to the data source that sees it and the detection
that alerts on it. Status changes to **Validated** only after the detection fires on real lab data.

| Tactic | Technique | Data source | Splunk detection | Wazuh detection | Status |
|---|---|---|---|---|---|
| Credential Access | [T1110.001](https://attack.mitre.org/techniques/T1110/001/) Password Guessing (Windows) | Security 4625, 4624 | [windows-brute-force](splunk/windows-brute-force.md) | 60122, 60204 | Planned |
| Credential Access | [T1110.003](https://attack.mitre.org/techniques/T1110/003/) Password Spraying | Security 4625 | [windows-brute-force](splunk/windows-brute-force.md) (`distinct_users`) | 60204 | Planned |
| Credential Access | [T1110.001](https://attack.mitre.org/techniques/T1110/001/) Password Guessing (SSH) | `auth.log` | [ssh-brute-force](splunk/ssh-brute-force.md) | 5710, 5712, 5720 | Planned |
| Execution | [T1059.001](https://attack.mitre.org/techniques/T1059/001/) PowerShell | Sysmon 1, PowerShell 4104 | [suspicious-powershell](splunk/suspicious-powershell.md) | 100100, 100101 | Planned |
| Defense Evasion | [T1027](https://attack.mitre.org/techniques/T1027/) Obfuscated Files or Information | Sysmon 1, PowerShell 4104 | [suspicious-powershell](splunk/suspicious-powershell.md) | 100100 | Planned |
| Execution | [T1204.002](https://attack.mitre.org/techniques/T1204/002/) User Execution: Malicious File | Sysmon 1, 11; Defender 1116 | [malware-execution](splunk/malware-execution.md) | 100200, 100201 | Planned |
| Command and Control | [T1105](https://attack.mitre.org/techniques/T1105/) Ingress Tool Transfer | Sysmon 3, 11; PowerShell 4104 | [malware-execution](splunk/malware-execution.md), [suspicious-powershell](splunk/suspicious-powershell.md) | — | Planned |

## Gaps

- No dedicated Wazuh rule for T1105 yet; it relies on the Splunk queries.
- Lateral movement and persistence are out of scope for the first four scenarios.
