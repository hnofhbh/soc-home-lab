# Attack Simulations

Controlled simulations that generate realistic telemetry for detection and investigation practice.

## Safety rules

1. **Isolated network only.** Targets are lab VMs on `VMnet2` (`10.10.10.0/24`), which has no route to the
   internet or the home network.
2. **Own systems only.** Every target is a virtual machine I own and operate.
3. **Snapshot first.** Take a `pre-<scenario>` snapshot of each target before starting and revert
   to it afterwards.
4. **No real malware.** Malware scenarios use the EICAR test file and Atomic Red Team tests, which are
   designed to be harmless.
5. **Log the timeline.** Record the exact start and end time of each run so SIEM alerts can be
   matched to the activity.

## Scenarios

| # | Plan | Target | MITRE |
|---|---|---|---|
| 1 | [Windows brute-force](01-windows-brute-force.md) | `WIN10-01` | T1110.001 |
| 2 | [SSH brute-force](02-ssh-brute-force.md) | `LNX-01` | T1110.001 |
| 3 | [Suspicious PowerShell](03-suspicious-powershell.md) | `WIN10-01` | T1059.001, T1027 |
| 4 | [Malware-related activity](04-malware-activity.md) | `WIN10-02` | T1204.002, T1105 |

## Test plan format

Each plan lists the objective, the method, the telemetry expected in each SIEM, and an evidence
checklist. The **Run log** section is filled in only after the simulation has actually been run.
