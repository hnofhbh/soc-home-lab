# 01 – Lab Architecture

## Goals

- Collect Windows and Linux telemetry into two different SIEMs and compare how each detects the same activity.
- Keep attack simulations fully contained inside an isolated virtual network.
- Fit the whole lab on a single laptop (16 GB RAM, 4-core i7, VMware Workstation).

## Network

| Setting | Value |
|---|---|
| Hypervisor | VMware Workstation |
| Lab network | `VMnet2`, host-only, no gateway, no internet |
| Subnet | `10.10.10.0/24` |
| DHCP | Disabled; all hosts use static IPs |
| Internet access | Temporary second NIC on VMware NAT, used only while installing software, then disconnected |

Victim machines never have internet access while a simulation is running.

## Hosts and IP plan

| Hostname | VMware VM | IP | Role |
|---|---|---|---|
| `WAZUH-SRV` | `Ubuntu 64-bit` | 10.10.10.10 | Wazuh manager, indexer, dashboard |
| `SPLUNK-SRV` | `Windows 10 x64 (3)` | 10.10.10.20 | Splunk Enterprise (Free license, 500 MB/day) |
| `WIN10-01` | `Windows 10 x64` | 10.10.10.101 | Windows endpoint |
| `WIN10-02` | `Windows 10 x64 (2)` | 10.10.10.102 | Windows endpoint |
| `LNX-01` | New Ubuntu Server VM | 10.10.10.110 | Linux endpoint, SSH target |
| `KALI-ATK` | `kali-linux-2025.2` | 10.10.10.66 | Attack simulation |

The four lab endpoints are `WIN10-01`, `WIN10-02`, `LNX-01`, and `KALI-ATK`.

## Resource plan: two phases

Running Splunk, the full Wazuh stack, two Windows endpoints, and Kali at once needs more than 16 GB.
The SIEMs therefore run in separate phases against the same endpoints and the same scenarios.

| Phase | Powered on | Approx. RAM |
|---|---|---|
| A – Wazuh | `WAZUH-SRV` 4 GB, `WIN10-01` 2.5 GB, `LNX-01` 1 GB, `KALI-ATK` 2 GB | ~9.5 GB |
| B – Splunk | `SPLUNK-SRV` 4 GB, `WIN10-01` 2.5 GB, `LNX-01` 1 GB, `KALI-ATK` 2 GB | ~9.5 GB |

`WIN10-02` is powered on instead of `WIN10-01` when a scenario needs a second Windows host.
Both endpoints keep both agents (Wazuh agent and Splunk Universal Forwarder) installed, so the
same activity can be replayed and compared across SIEMs.

## Data flow

| Source | Collected by | Wazuh | Splunk index |
|---|---|---|---|
| Windows Security log | Wazuh agent / Universal Forwarder | yes | `win` |
| Sysmon Operational | Wazuh agent / Universal Forwarder | yes | `win` |
| PowerShell Operational (4104) | Wazuh agent / Universal Forwarder | yes | `win` |
| Windows Defender Operational | Wazuh agent / Universal Forwarder | yes | `win` |
| Linux `/var/log/auth.log` | Wazuh agent / Universal Forwarder | yes | `linux` |

## Snapshots

Every VM has a clean baseline snapshot named `baseline-clean`. Each simulation starts by taking a
`pre-<scenario>` snapshot of the target and ends by reverting to it.
