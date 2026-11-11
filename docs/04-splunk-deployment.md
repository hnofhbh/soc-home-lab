# 04 – Splunk Deployment

> Status: **Planned.** Will be updated with the actual steps, version numbers, and screenshots.

## Server: `SPLUNK-SRV` (Windows 10, 10.10.10.20, 4 GB RAM)

1. Install Splunk Enterprise and switch to the Free license (500 MB/day is enough for this lab).
2. Create the indexes `win` and `linux` (*Settings → Indexes*).
3. Enable receiving on port `9997` (*Settings → Forwarding and receiving*).
4. Install these add-ons from Splunkbase:
   - Splunk Add-on for Microsoft Windows
   - Splunk Add-on for Sysmon
   - Splunk Add-on for Unix and Linux

Web UI: `http://10.10.10.20:8000`

## Universal Forwarder on Windows endpoints

Install the Universal Forwarder pointing at `10.10.10.20:9997`, then create
`C:\Program Files\SplunkUniversalForwarder\etc\system\local\inputs.conf`:

```ini
[WinEventLog://Security]
disabled = 0
index = win

[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = win
renderXml = true

[WinEventLog://Microsoft-Windows-PowerShell/Operational]
disabled = 0
index = win

[WinEventLog://Microsoft-Windows-Windows Defender/Operational]
disabled = 0
index = win
```

Restart: `& "C:\Program Files\SplunkUniversalForwarder\bin\splunk.exe" restart`

## Universal Forwarder on `LNX-01`

`/opt/splunkforwarder/etc/system/local/inputs.conf`:

```ini
[monitor:///var/log/auth.log]
disabled = 0
index = linux
sourcetype = linux_secure
```

## Verification

```spl
index=win | stats count by host, sourcetype
```

```spl
index=linux sourcetype=linux_secure | stats count by host
```

- [ ] Both searches return every endpoint
- [ ] Screenshots saved to `assets/screenshots/04-*`
