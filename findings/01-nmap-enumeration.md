# Finding 01 · Nmap discovery and service enumeration

> 🚧 **Status: in progress.** This page sets out the exercise plan and how the results will be written up. Results, evidence and lessons learned will be added once the exercise has been run end to end.

| | |
|---|---|
| **Source** | Kali Linux (lab attacker VM) |
| **Targets** | Windows Server, Windows 10, Ubuntu (Wazuh manager) |
| **ATT&CK** | T1046 Network Service Discovery |

## Objective
Identify live hosts and exposed services in the lab, assess the risk of each, and check what the scan looks like from the SOC side.

## Commands
```bash
# 1. Host discovery
sudo nmap -sn <lab-subnet>/24

# 2. Full TCP scan with service versions and default scripts
sudo nmap -sS -sV -sC -p- -T4 <target> -oA scans/<host>
```

## What I'll record for each host
| Field | Why it matters |
|---|---|
| Open ports and services | The attack surface |
| Service versions | Outdated versions map to known CVEs |
| Notable script output | e.g. SMB signing not required, weak TLS, RDP exposed |
| Risk rating (High / Medium / Low) | Prioritises what to fix first |
| Recommendation | The concrete fix (GPO setting, firewall rule, patch) |

## Defender's view (what I'll check)
- Did Wazuh raise an alert for the scan, and which rule?
- What does the Windows Firewall log show for the scan?
- Detection gap: would a slow scan (`-T1`) be missed? If so, what logging or rule closes the gap?

The port-scan detection logic is already implemented and tested in [log-analysis-toolkit/port_scan.py](https://github.com/chahat-24/log-analysis-toolkit/blob/main/detections/port_scan.py).

## Results
*To be added after the exercise.*
