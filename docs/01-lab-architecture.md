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
| `WAZUH-SRV` | New VM `WAZUH-SRV` (Ubuntu Server 24.04 LTS, 2 vCPU, 50 GB disk) | 10.10.10.10 | Wazuh manager, indexer, dashboard |
| `SPLUNK-SRV` | `Windows 10 x64 (3)` | 10.10.10.20 | Splunk Enterprise (Free license, 500 MB/day) |
| `WIN10-01` | `Windows 10 x64` | 10.10.10.101 | Windows endpoint |
| `WIN10-02` | `Windows 10 x64 (2)` | 10.10.10.102 | Windows endpoint |
| `LNX-01` | `Ubuntu 64-bit` (Ubuntu Desktop) | 10.10.10.110 | Linux endpoint, SSH target |
| `KALI-ATK` | `kali-linux-2025.2` | 10.10.10.66 | Attack simulation |

The four lab endpoints are `WIN10-01`, `WIN10-02`, `LNX-01`, and `KALI-ATK`.

`WAZUH-SRV` is a dedicated server VM rather than the existing Ubuntu Desktop VM because the
Wazuh indexer needs far more disk than that VM's 20 GB, and a server install without a desktop
leaves more of its 4 GB of RAM for Wazuh.

## Resource plan: two phases

The host has 16 GB of RAM, and Windows plus everyday apps already use a large share of it.
Running Splunk, the full Wazuh stack, two Windows endpoints, and Kali at once is not possible.
The SIEMs therefore run in separate phases against the same endpoints and the same scenarios,
and only the hosts a scenario needs are powered on.

| Phase | Always on | Added only for the scenario being run |
|---|---|---|
| A – Wazuh | `WAZUH-SRV` 4 GB, `WIN10-01` 2 GB | `KALI-ATK` 2 GB, `LNX-01` 2 GB |
| B – Splunk | `SPLUNK-SRV` 4 GB, `WIN10-01` 2 GB | `KALI-ATK` 2 GB, `LNX-01` 2 GB |

Heavy host applications (browsers with many tabs, chat apps) are closed while the lab is running.
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

Before any lab changes, the existing VMs were snapshotted as `original-before-lab`.
After configuration, every VM gets a clean baseline snapshot named `baseline-clean`. Each simulation starts by taking a
`pre-<scenario>` snapshot of the target and ends by reverting to it.
