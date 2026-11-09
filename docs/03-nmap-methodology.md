# 03 · Nmap scanning methodology

Scans run **only** from the Kali VM against lab hosts I own.

## 1. Host discovery

```bash
sudo nmap -sn 10.0.2.0/24 -oN scans/01-discovery.txt
```

## 2. Port scan (top 1000, then full range on interesting hosts)

```bash
sudo nmap -sS -T4 <target> -oN scans/02-top1000-<host>.txt
sudo nmap -sS -p- -T4 <target> -oN scans/03-allports-<host>.txt
```

## 3. Service and version enumeration + default scripts

```bash
sudo nmap -sV -sC -p <open-ports> <target> -oA scans/04-services-<host>
```

## 4. Targeted checks (examples)

```bash
nmap --script smb-security-mode,smb2-security-mode -p445 <target>   # SMB signing
nmap --script rdp-enum-encryption -p3389 <target>                   # RDP encryption
nmap --script ssl-enum-ciphers -p443 <target>                       # weak TLS
```

## How I write up a finding

Each finding in [`../findings/`](../findings/) uses the same structure:

| Field | Example |
|---|---|
| Host / port / service | `10.0.2.10 · 445/tcp · Microsoft-DS` |
| Observation | SMB signing *enabled but not required* |
| Risk | Allows SMB relay attacks (T1557.001) |
| Evidence | Nmap output line + screenshot |
| Recommendation | Require SMB signing via GPO: *Microsoft network server: Digitally sign communications (always)* |
| Detection | What the SOC would see in Wazuh if this were exploited |

## The defender's view

Every scan also shows up on the blue side. After scanning, I check Wazuh and the Windows Firewall log to see **what a SOC analyst would actually see**, and note any gaps. That port-scan detection logic is in [log-analysis-toolkit/port_scan.py](https://github.com/chahat-24/log-analysis-toolkit/blob/main/detections/port_scan.py).
