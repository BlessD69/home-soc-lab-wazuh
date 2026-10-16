# Home SOC Lab — Wazuh SIEM Deployment

🚧 **Status: In Progress**

## Overview

This project documents my step-by-step build of a home Security Operations Center (SOC) lab. The goal is to gain hands-on, practical experience with the tools and workflows used by real-world SOC analysts — SIEM deployment, endpoint monitoring, threat detection, log analysis, and incident response.

The lab is built entirely on a virtual machine using VMware Workstation, simulating a small enterprise environment with a central monitoring system (Wazuh) and multiple "protected" endpoints.

## Why This Project

I'm developing hands-on skills toward a SOC Analyst role. Rather than relying on theory alone, this lab lets me:

- Practice deploying and configuring a real SIEM platform (Wazuh)
- Simulate attacks and observe how they're detected
- Build log analysis and incident response skills in a safe, controlled environment
- Document real troubleshooting — including failures — the way it actually happens in practice

## Architecture

```
                    ┌──────────────────────┐
                    │   Wazuh Manager      │
                    │  (Indexer + Manager  │
                    │   + Dashboard)       │
                    │  Ubuntu Server       │
                    └──────────┬───────────┘
                               │
              ┌────────────────┼─────────────────┐
              │                │                 │
      ┌───────▼───────┐ ┌───────▼──────┐  ┌───────▼──────┐
      │ Ubuntu Target │ │ Windows      │  │ Kali Linux   │
      │ (Wazuh Agent) │ │ Target       │  │ (Attacker)   │
      │               │ │ (Wazuh Agent)│  │              │
      └───────────────┘ └──────────────┘  └──────────────┘
```

*(Diagram will be replaced later with a proper visual as the lab progresses)*
<!-- take note of the above DAMILOLA and change it later -->

## Tools Used

| Tool | Purpose |
|---|---|
| VMware Workstation Pro | Virtualization platform for all lab machines |
| Wazuh (Manager, Indexer, Dashboard) | SIEM — log collection, alerting, threat detection |
| Ubuntu Server | Wazuh Manager host + monitored Linux endpoint |
| Windows 10/11 | Monitored Windows endpoint |
| Kali Linux | Attack simulation |

## Project Phases

| Phase | Description | Status |
|---|---|---|
| [Phase 1 — Planning](docs/01-phase2-wazuh-manager-install.md#prerequisites) | Environment planning, tool selection | ✅ Complete |
| [Phase 2 — Wazuh Manager Install](docs/01-phase2-wazuh-manager-install.md) | Deployed Wazuh Manager, Indexer, Dashboard on Ubuntu Server | ✅ Complete |
| [Phase 3 — Ubuntu Agent](docs/02-phase3-agent-setup.md) | Connected first monitored Linux endpoint | ✅ Complete |
| [Phase 4 — Windows Agent](docs/03-phase4-windows-agent.md) | Connected monitored Windows endpoint | ✅ Complete |
| [Phase 5 — Attack Simulation](docs/04-phase5-attack-simulation.md) | Run attacks from Kali, observe detection in Wazuh | ✅ Complete |
| Phase 6 — Custom Detection Rules | Tune and write custom Wazuh rules | Next |

## Key Skills Demonstrated So Far

- Linux server administration (Ubuntu Server install, SSH, package management)
- SIEM deployment and architecture (Wazuh Indexer, Manager, Dashboard)
- Troubleshooting failed service installations using systemd, logs, and journalctl
- Network configuration in a virtualized lab environment
- Disk capacity troubleshooting: recovering a SIEM after a full disk, resizing a VM disk and extending an LVM volume live
- Multi-platform endpoint monitoring (Linux and Windows agents reporting to one SIEM)

## Author

Damilola Awoma | SOC Analyst (in training), focused on SOC-aligned threat detection and incident response.

[LinkedIn](https://www.linkedin.com/in/damilola-awoma-544447393/) · [Instagram](https://www.instagram.com/ethicalblessd1/)
