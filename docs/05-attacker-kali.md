# 05 – Attack Simulation Host (`KALI-ATK`)

> Status: **Planned.**

## Network isolation

| Setting | Value |
|---|---|
| Network adapter | `VMnet2` (host-only) only |
| IP | `10.10.10.66/24`, no gateway |
| Internet | Disconnected during simulations |

Before every simulation, confirm isolation from `KALI-ATK`:

```bash
ip addr show
ping -c 2 8.8.8.8     # must fail
ping -c 2 10.10.10.101  # must succeed
```

## Rules

- Targets are limited to lab hosts in `10.10.10.0/24`.
- Each simulation follows the test plan in [../attack-simulations/](../attack-simulations/).
- Attack activity is logged with start and end times so it can be matched to SIEM alerts.
