# Wazuh Detections

## Built-in rules used

Wazuh ships rules for several scenarios out of the box. Rule IDs below are from the default
ruleset and must be confirmed in the dashboard (*Server management → Rules*) for the installed version.

| Scenario | Built-in rule | Triggers on |
|---|---|---|
| 1 – Windows brute-force | 60122 | Windows logon failure (4625) |
| 1 – Windows brute-force | 60204 | Multiple Windows logon failures |
| 2 – SSH brute-force | 5710 | SSH attempt with a non-existent user |
| 2 – SSH brute-force | 5712 | SSH brute-force with non-existent users |
| 2 – SSH brute-force | 5720 | Multiple SSH authentication failures |
| 4 – Malware activity | Windows Defender rules | Defender detection events (1116) |

## Custom rules

[local_rules.xml](local_rules.xml) adds detections the default ruleset does not cover:

| Rule ID | Level | Detects | MITRE |
|---|---|---|---|
| 100100 | 10 | PowerShell with an encoded command | T1059.001, T1027 |
| 100101 | 8 | PowerShell hidden or with execution policy bypass | T1059.001 |
| 100200 | 12 | Office application spawning a shell or script host | T1204.002 |
| 100201 | 8 | Process executed from a user-writable folder | T1204.002 |

Custom rule IDs use the `100000`+ range reserved for local rules.

## Testing a rule without a live attack

Paste a sample log line into `wazuh-logtest` on the manager to see which rule fires:

```bash
sudo /var/ossec/bin/wazuh-logtest
```

## Results

_To be filled with alert screenshots after each scenario runs._
