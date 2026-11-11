# SSH Brute-Force

| Field | Value |
|---|---|
| MITRE ATT&CK | [T1110.001 – Password Guessing](https://attack.mitre.org/techniques/T1110/001/) |
| Data source | Linux `/var/log/auth.log` |
| Index / sourcetype | `linux` / `linux_secure` |
| Status | **Draft:** to be validated against lab data |

## Logic

Repeated `Failed password` lines from one IP in a short window indicate password guessing.
`Invalid user` lines show the attacker is trying usernames that do not exist.
An `Accepted password` from the same IP afterwards means the attempt may have succeeded.

## Query 1: failed SSH logons by source

```spl
index=linux sourcetype=linux_secure "Failed password"
| rex "Failed password for (invalid user )?(?<ssh_user>\S+) from (?<src_ip>\d{1,3}(\.\d{1,3}){3})"
| bin _time span=5m
| stats count AS failures dc(ssh_user) AS distinct_users values(ssh_user) AS users BY _time, src_ip, host
| where failures >= 10
| sort - failures
```

## Query 2: success after failures

```spl
index=linux sourcetype=linux_secure ("Failed password" OR "Accepted password")
| rex "(Failed|Accepted) password for (invalid user )?(?<ssh_user>\S+) from (?<src_ip>\d{1,3}(\.\d{1,3}){3})"
| eval outcome=if(searchmatch("Accepted password"), "success", "failure")
| stats count(eval(outcome="failure")) AS failures
        count(eval(outcome="success")) AS successes
        BY src_ip, ssh_user, host
| where failures >= 10 AND successes >= 1
```

## Thresholds and tuning

| Setting | Value | Reason |
|---|---|---|
| Window | 5 minutes | Automated tools produce dense bursts |
| Failures | ≥ 10 | Well above normal typing mistakes |

## Expected false positives

- Automation or monitoring scripts using an outdated key or password
- Vulnerability scanners run by the IT team

## Results

_To be filled with screenshots and real values after the scenario runs._
