# Scenario 2 – SSH Brute-Force

| Field | Value |
|---|---|
| Target | `LNX-01` (10.10.10.110), test account created for this scenario |
| Source | `KALI-ATK` (10.10.10.66) |
| MITRE | T1110.001 Password Guessing |
| Status | Planned |

## Objective

Generate repeated failed SSH logons, including attempts with non-existent usernames, followed by
one successful logon, and confirm both SIEMs alert on the pattern.

## Method

- Create a dedicated test account on `LNX-01` with password authentication enabled for SSH.
- From `KALI-ATK`, run repeated SSH logon attempts with a small user list and wordlist that include
  the test account near the end.
- Record start and end times.

## Expected telemetry

| Event | Where |
|---|---|
| `Failed password` and `Invalid user` lines from 10.10.10.66 | `/var/log/auth.log` |
| `Accepted password` line from 10.10.10.66 | `/var/log/auth.log` |
| Wazuh rules 5710, 5712, 5720 | Wazuh dashboard |
| Results from [ssh-brute-force](../detections/splunk/ssh-brute-force.md) queries 1 and 2 | Splunk |

## Evidence checklist

- [ ] Screenshot of Splunk query results
- [ ] Screenshot of Wazuh alerts
- [ ] Relevant `auth.log` excerpt (IPs and usernames only)
- [ ] Incident report written in `incidents/`

## Run log

_Filled in after the run._
