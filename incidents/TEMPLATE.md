# IR-YYYY-NNN – <Short incident title>

Structured on the NIST SP 800-61 incident response lifecycle.

## 1. Summary

| Field | Value |
|---|---|
| Incident ID | IR-YYYY-NNN |
| Date detected | YYYY-MM-DD HH:MM (UTC) |
| Detected by | Splunk alert / Wazuh rule `<id>` |
| Severity | Low / Medium / High / Critical |
| Classification | True positive / False positive / Benign true positive |
| Affected hosts | |
| Affected accounts | |
| MITRE ATT&CK | Txxxx.xxx – Name |
| Status | Open / Contained / Closed |

**One-paragraph summary:** what happened, how it was found, and the outcome.

## 2. Detection

- Alert name and rule or query that fired
- Time of first alert
- Screenshot: `assets/screenshots/...`

## 3. Triage

- Is the activity real or a false positive? Why?
- Initial scope: which hosts, accounts, and time window?
- Severity decision and reasoning

## 4. Analysis

### Timeline

| Time (UTC) | Host | Event | Source |
|---|---|---|---|
| | | | |

### Indicators of Compromise (IOCs)

| Type | Value | Context |
|---|---|---|
| IP address | | |
| Account | | |
| File hash (SHA256) | | |
| File path | | |
| Command line | | |

### Queries used

```spl
<SPL query used during the investigation>
```

### MITRE ATT&CK mapping

| Tactic | Technique | Evidence |
|---|---|---|
| | | |

## 5. Containment

Actions taken to stop the activity (for example: block source IP, disable account, isolate host).

## 6. Eradication

Actions taken to remove the cause (for example: reset password, remove file, close exposed service).

## 7. Recovery

How the system was returned to normal operation and how that was verified
(in the lab: revert to snapshot and confirm the baseline).

## 8. Lessons learned

- What worked in detection
- What was missed or slow
- Detection or tuning changes made as a result (link the commit)
