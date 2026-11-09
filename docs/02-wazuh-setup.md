# 02 · Wazuh SIEM setup

## 1. Install the all-in-one manager (Ubuntu)

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
# credentials for the dashboard are printed at the end – store them safely
```

Dashboard: `https://<manager-ip>` (self-signed certificate).

## 2. Enrol the Windows agents

From the dashboard: **Agents → Deploy new agent → Windows**, set the manager IP and agent name, then run the generated PowerShell command as Administrator on each Windows VM:

```powershell
NET START WazuhSvc
```

Check that the agent shows **Active** in the dashboard.

## 3. Turn on the Windows logs that matter

Default Windows auditing misses a lot. On both Windows VMs (`gpedit.msc` → Advanced Audit Policy, or `auditpol`):

```powershell
auditpol /set /subcategory:"Logon" /success:enable /failure:enable
auditpol /set /subcategory:"Account Lockout" /failure:enable
auditpol /set /subcategory:"User Account Management" /success:enable /failure:enable
auditpol /set /subcategory:"Process Creation" /success:enable
# include command line in 4688 events
reg add "HKLM\Software\Microsoft\Windows\CurrentVersion\Policies\System\Audit" /v ProcessCreationIncludeCmdLine_Enabled /t REG_DWORD /d 1 /f
```

| Event ID | Meaning | Why it matters |
|---|---|---|
| 4624 / 4625 | Logon success / failure | Brute force, spraying, unusual logons |
| 4720 | User account created | Persistence (T1136) |
| 4732 | Member added to security-enabled local group | Privilege escalation |
| 4688 | Process created | Suspicious execution (T1059) |
| 4740 | Account locked out | Brute-force side effect |

**Optional:** install Sysmon with a community config (e.g. SwiftOnSecurity) for much richer process and network telemetry.

## 4. Linux SSH logs

On a Linux target, the agent reads `/var/log/auth.log` by default. Built-in rules already decode sshd events (rule 5760 = authentication failed, 5715 = authentication success).

## 5. Custom rules

Copy [`../configs/local_rules.xml`](../configs/local_rules.xml) into `/var/ossec/etc/rules/local_rules.xml` on the manager, then validate and restart:

```bash
sudo /var/ossec/bin/wazuh-logtest     # paste a sample log line to see which rule fires
sudo systemctl restart wazuh-manager
```

## Troubleshooting notes

<!-- ✏️ Add problems you actually hit and how you fixed them. Interviewers love these. -->
- 
