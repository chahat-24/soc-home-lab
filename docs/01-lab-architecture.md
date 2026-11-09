# 01 · Lab architecture

## Virtual machines

| VM | OS | Role | Suggested resources |
|---|---|---|---|
| Wazuh manager | Ubuntu Server 22.04 LTS | Wazuh manager, indexer and dashboard (all-in-one) | 2 vCPU · 4–8 GB RAM · 50 GB |
| Attacker | Kali Linux | Nmap scanning, brute-force simulation (Hydra) | 2 vCPU · 2–4 GB RAM |
| Domain/server | Windows Server | Monitored server (Wazuh agent) | 2 vCPU · 4 GB RAM |
| Workstation | Windows 10 | Monitored endpoint (Wazuh agent) | 2 vCPU · 4 GB RAM |

<!-- ✏️ Update the table with your exact versions and resources -->

## Networking

- All VMs sit on a VirtualBox **NAT Network** (or Host-only network) so they can reach each other but **not** the home LAN.
- Lab subnet: `10.0.2.0/24` <!-- ✏️ change to your actual subnet -->
- The Wazuh manager has a static IP so agents can always reach it on **TCP 1514** (events) and **1515** (enrolment); the dashboard is on **443**.

| Host | IP |
|---|---|
| Wazuh manager | `10.0.2.x` |
| Kali | `10.0.2.x` |
| Windows Server | `10.0.2.x` |
| Windows 10 | `10.0.2.x` |

<!-- ✏️ fill in your IPs -->

## Design decisions

1. **Isolation first.** Attack tools never leave the lab network, and snapshots are taken before every exercise so VMs can be rolled back.
2. **All-in-one Wazuh.** A single node is enough for four endpoints and keeps the lab light on RAM.
3. **Log what a real SOC would see.** Windows advanced audit policy is enabled (logon events, process creation with command line, account management) so alerts carry enough context to triage.
4. **Attacker on the same subnet.** This simulates an insider or an already-compromised host, which is where most SOC detections fire.

## Snapshots

| Snapshot | Purpose |
|---|---|
| `clean-install` | Fresh OS + updates |
| `agents-enrolled` | Wazuh agents reporting, baseline taken |
| `pre-exercise-N` | Before each attack simulation |
