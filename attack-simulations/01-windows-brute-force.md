# Scenario 1 – Windows Brute-Force Logon

| Field | Value |
|---|---|
| Target | `WIN10-01` (10.10.10.101), local test account created for this scenario |
| Source | `KALI-ATK` (10.10.10.66) |
| MITRE | T1110.001 Password Guessing |
| Status | Planned |

## Objective

Generate a burst of failed network logons (RDP/SMB) against a test account, followed by one
successful logon, and confirm both SIEMs alert on the pattern.

## Method

- Create a dedicated local test account on `WIN10-01` used only for this scenario.
- From `KALI-ATK`, run repeated logon attempts with a small wordlist that contains the test
  account's password near the end.
- Record start and end times.

## Expected telemetry

| Event | Where |
|---|---|
| Many 4625 events, logon type 3 or 10, source 10.10.10.66 | Windows Security log |
| One 4624 from 10.10.10.66 after the failures | Windows Security log |
| Wazuh rules 60122 and 60204 | Wazuh dashboard |
| Results from [windows-brute-force](../detections/splunk/windows-brute-force.md) queries 1 and 2 | Splunk |

## Evidence checklist

- [ ] Screenshot of Splunk query 1 results
- [ ] Screenshot of Splunk query 2 results
- [ ] Screenshot of Wazuh alerts
- [ ] Incident report written in `incidents/`

## Run log

_Filled in after the run: date, start/end time, attempt count, observations._
