# Sysmon Configuration

> Status: **Planned.** The configuration file will be added here when Sysmon is installed on the endpoints.

## Source

The lab uses the community configuration **sysmon-modular** by Olaf Hartong
(<https://github.com/olafhartong/sysmon-modular>), default `sysmonconfig.xml` build.

It is chosen because it:

- is maintained and widely used in SOC environments,
- tags many rules with MITRE ATT&CK technique IDs,
- filters common noise so the 500 MB/day Splunk Free license is not exceeded.

## Plan

1. Download `sysmonconfig.xml` from the sysmon-modular repository.
2. Commit it here unchanged as `sysmonconfig.xml`, recording the upstream commit hash below.
3. Record any lab-specific changes in a separate commit, with the reason for each change.

| Item | Value |
|---|---|
| Upstream commit | _to be recorded_ |
| Sysmon version | _to be recorded_ |
| Local changes | _none yet_ |

## Event IDs this lab relies on

1 (process creation), 3 (network connection), 7 (image load), 8 (CreateRemoteThread),
11 (file create), 22 (DNS query). See [../../docs/06-log-sources.md](../../docs/06-log-sources.md).
