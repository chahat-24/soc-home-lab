# Finding 01 · Nmap discovery and service enumeration

> ✏️ **Template, to complete with your own results.** Replace every `<!-- ✏️ -->` with what you actually saw in your lab and add screenshots. Interviewers will ask about this, so only include what you ran.

| | |
|---|---|
| **Date** | <!-- ✏️ YYYY-MM-DD --> |
| **Source** | Kali Linux (`10.0.2.x`) |
| **Targets** | Windows Server, Windows 10, Ubuntu (Wazuh) |
| **ATT&CK** | T1046 – Network Service Discovery |

## Objective
Identify live hosts and exposed services in the lab, assess the risk of each, and check what the scan looks like from the SOC side.

## Commands
```bash
sudo nmap -sn 10.0.2.0/24
sudo nmap -sS -sV -sC -p- -T4 <target> -oA scans/<host>
```

## Results

| Host | Open ports / services | Notable observations |
|---|---|---|
| Windows Server `10.0.2.x` | <!-- ✏️ e.g. 53, 88, 135, 139, 389, 445, 3389 --> | <!-- ✏️ e.g. SMB signing not required --> |
| Windows 10 `10.0.2.x` | <!-- ✏️ --> | <!-- ✏️ --> |
| Ubuntu `10.0.2.x` | <!-- ✏️ e.g. 22, 443, 1514, 1515 --> | <!-- ✏️ --> |

![Nmap output](../screenshots/01-nmap-output.png)

## Findings and recommendations

| # | Finding | Risk | Recommendation |
|---|---|---|---|
| 1 | <!-- ✏️ --> | <!-- ✏️ High/Med/Low + why --> | <!-- ✏️ --> |
| 2 | <!-- ✏️ --> | | |

## Defender's view
- Did Wazuh alert on the scan? <!-- ✏️ yes/no, which rule, screenshot -->
- What did the Windows Firewall log show? <!-- ✏️ -->
- Detection gap and fix: <!-- ✏️ e.g. no alert for a slow scan → enable firewall logging + port-scan rule -->

## What I learned
<!-- ✏️ 2–3 sentences in your own words -->
