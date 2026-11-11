# 03 – Wazuh Deployment

> Status: **Planned.** Will be updated with the actual steps, version numbers, and screenshots.

## Server: `WAZUH-SRV` (Ubuntu Server 24.04 LTS, 10.10.10.10, 4 GB RAM)

### VM specification

| Setting | Value |
|---|---|
| OS | Ubuntu Server 24.04 LTS (minimized install not used) |
| vCPU / RAM | 2 / 4 GB |
| Disk | 50 GB, single file |
| NIC 1 | `VMnet2` host-only, static `10.10.10.10/24`, no gateway |
| NIC 2 | NAT, DHCP, used only during installation, then disconnected |
| Installer options | OpenSSH server enabled; no extra snaps |

### Wazuh installation

Install the all-in-one stack (manager, indexer, dashboard) with the official installer
while the VM has temporary internet access:

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

The installer prints the dashboard admin password at the end. Store it outside this repository.

Dashboard: `https://10.10.10.10`

## Windows agents (`WIN10-01`, `WIN10-02`)

1. Install the Wazuh agent MSI with the manager address set to `10.10.10.10`.
2. Add these channels to `C:\Program Files (x86)\ossec-agent\ossec.conf`:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>

<localfile>
  <location>Microsoft-Windows-PowerShell/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>

<localfile>
  <location>Microsoft-Windows-Windows Defender/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```

3. Restart the agent: `Restart-Service WazuhSvc`

## Linux agent (`LNX-01`)

Install the Wazuh agent package for Ubuntu with `WAZUH_MANAGER=10.10.10.10`.
`/var/log/auth.log` is monitored by default.

## Custom rules

Copy [../detections/wazuh/local_rules.xml](../detections/wazuh/local_rules.xml) to
`/var/ossec/etc/rules/local_rules.xml` on the manager, then:

```bash
sudo /var/ossec/bin/wazuh-analysisd -t   # syntax check
sudo systemctl restart wazuh-manager
```

## Verification checklist

- [ ] All agents show as **Active** in the dashboard
- [ ] Sysmon events from `WIN10-01` are searchable (`data.win.system.channel: Microsoft-Windows-Sysmon/Operational`)
- [ ] SSH logon events from `LNX-01` are visible
- [ ] Screenshots saved to `assets/screenshots/03-*`
