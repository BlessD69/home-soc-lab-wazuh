# Phase 3 — Ubuntu Agent Setup & Connection

## What I Built

A second Ubuntu Server VM (`ubuntu-target`) configured as a monitored endpoint, with the Wazuh Agent installed and connected back to the Wazuh Manager deployed in Phase 2. This is the first "protected machine" in the lab, the point where the SIEM stops being an empty dashboard and starts monitoring something.

## Why This Matters

A SIEM with no agents reporting to it is just an empty database. Connecting an agent is what turns Wazuh into a monitoring system: logs, file integrity checks, and process activity from `ubuntu-target` flow into the Manager for analysis. This is the same relationship a SOC analyst works with daily, with endpoints reporting up to a central console for triage.

## Environment

- **Host platform:** VMware Workstation
- **Guest OS:** Ubuntu Server (`ubuntu-target`)
- **Network mode:** NAT (same network as the Wazuh Manager)
- **Agent version:** Wazuh Agent 4.9.0

## Steps Taken

1. Provisioned a second Ubuntu Server VM (`ubuntu-target`): 2GB RAM, 20GB disk, OpenSSH enabled
2. Confirmed connectivity to the Wazuh Manager with a `ping` test
3. Downloaded the Wazuh Agent `.deb` package and installed it, pointing it at the Manager's IP via `WAZUH_MANAGER`
4. Enabled and started the `wazuh-agent` service with `systemctl`
5. Checked **Endpoints Summary** on the dashboard to confirm the agent appeared as **Active**
6. Re-checked `systemctl status wazuh-agent` on the endpoint after 1h+ of uptime to rule out a flickering connection

## Challenges & How I Solved Them

This phase was far harder than the install steps suggest.

1. **Corrupted `.deb` download.** The first package download was corrupted. I verified the file with `file` and its size before retrying instead of assuming the install process was broken.
2. **Wazuh Manager disk filled up.** The Manager VM ran out of disk space, which took down both the Wazuh Manager and Indexer services. The dashboard became unreachable. I traced the outage to disk exhaustion rather than a config fault. *(Add: the `df -h` output / what was consuming space.)*
3. **Stale lock file.** After space was freed, a leftover lock file stopped services from starting cleanly. I identified and cleared it, then restarted the services.
4. **Resized the VM disk and extended the LVM volume live.** To fix the root cause rather than just free a few MB, I grew the virtual disk in VMware and extended the LVM logical volume and filesystem on the running system, without rebuilding the VM. *(Add: the commands you used, e.g. `lvextend` / `resize2fs`.)*
5. **Trusting the dashboard.** The agent unexpectedly showed Active from an earlier attempt, so I verified it directly on the endpoint:
   ```bash
   sudo systemctl status wazuh-agent
   ```
   `active (running)` with `wazuh-execd`, `wazuh-agentd`, `wazuh-syscheckd`, `wazuh-logcollector`, and `wazuh-modulesd` all healthy after 1h 27min.

**Takeaway:** monitoring infrastructure needs monitoring too. A SIEM that silently runs out of disk is a blind SOC, so disk capacity on the Manager is now something I check as more agents report in.

## Result

| Field | Value |
|---|---|
| Agent ID | 001 |
| Name | ubuntu-target |
| IP | 192.168.214.136 |
| OS | Ubuntu 26.04 LTS |
| Status | ● active |

## Screenshots

*(See `screenshots/phase3/`, credentials redacted)*

- Endpoints Summary showing `ubuntu-target` Active
- `systemctl status wazuh-agent` output
- *(Optional but strong: disk-full error and post-resize `df -h`)*

## Next Phase

[Phase 4 — Windows Agent Setup](03-phase4-windows-agent.md)
