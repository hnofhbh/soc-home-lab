# Windows Brute-Force Logon

| Field | Value |
|---|---|
| MITRE ATT&CK | [T1110.001 – Password Guessing](https://attack.mitre.org/techniques/T1110/001/), [T1110.003 – Password Spraying](https://attack.mitre.org/techniques/T1110/003/) |
| Data source | Windows Security log, events 4625 and 4624 |
| Index | `win` |
| Status | **Draft:** to be validated against lab data |

## Logic

Many failed logons (4625) from the same source in a short window suggest password guessing.
Failures spread across many accounts from one source suggest password spraying.
A successful logon (4624) from the same source after those failures is the highest-priority case.

## Query 1: many failures from one source

```spl
index=win sourcetype="WinEventLog:Security" EventCode=4625
| bin _time span=5m
| stats count AS failures dc(user) AS distinct_users values(user) AS users values(Logon_Type) AS logon_types BY _time, src, host
| where failures >= 10
| sort - failures
```

## Query 2: success after failures (likely compromise)

```spl
index=win sourcetype="WinEventLog:Security" (EventCode=4625 OR EventCode=4624) Logon_Type IN (3, 10)
| stats count(eval(EventCode=4625)) AS failures
        count(eval(EventCode=4624)) AS successes
        min(_time) AS first_seen max(_time) AS last_seen
        BY src, user, host
| where failures >= 10 AND successes >= 1
| convert ctime(first_seen) ctime(last_seen)
```

## Thresholds and tuning

| Setting | Value | Reason |
|---|---|---|
| Window | 5 minutes | Automated guessing produces bursts; people mistyping do not |
| Failures | ≥ 10 | A user mistyping a password rarely exceeds 3–5 attempts |
| Logon types | 3 (network/SMB), 10 (RDP) | Remote logon attempts |

## Expected false positives

- A service or scheduled task with an expired password retrying in a loop
- A user with a saved old password on a mapped drive

Triage step: check whether `src` is a known internal host and whether the account is a service account.

## Results

_To be filled with screenshots and real values after the scenario runs._
