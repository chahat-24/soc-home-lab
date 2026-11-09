# Finding 02 · Brute-force attack detected in Wazuh

> ✏️ **Template, to complete with your own results** after running the exercise. Keep the structure; replace the placeholders with your real timestamps, alert IDs and screenshots.

| | |
|---|---|
| **Date** | <!-- ✏️ --> |
| **Alert** | Rule `100100` / `100110` (custom) <!-- ✏️ or built-in rule that fired --> |
| **Severity** | <!-- ✏️ --> |
| **ATT&CK** | T1110.001 Password Guessing · T1078 Valid Accounts (if login succeeded) |
| **Status** | <!-- ✏️ True positive (simulated) --> |

## 1. Simulation (red side)
```bash
# from Kali, against a deliberately weak lab account
hydra -l labuser -P small-wordlist.txt rdp://10.0.2.x      # or ssh://
```

## 2. Detection (blue side)
- Alert fired at: <!-- ✏️ time --> · Rule: <!-- ✏️ ID + description -->
- Evidence: <!-- ✏️ number of 4625 events, source IP, target account, logon type -->

![Wazuh alert](../screenshots/02-wazuh-bruteforce-alert.png)

## 3. Triage
| Question | Answer |
|---|---|
| Internal or external source? | <!-- ✏️ --> |
| How many attempts, over what time? | <!-- ✏️ --> |
| Which accounts were targeted? | <!-- ✏️ --> |
| Any successful logon (4624) afterwards? | <!-- ✏️ --> |
| What happened after the logon? | <!-- ✏️ processes, new users, etc. --> |

## 4. Response (NIST SP 800-61)
- **Containment:** <!-- ✏️ e.g. block source IP, disable account -->
- **Eradication:** <!-- ✏️ reset password, remove persistence -->
- **Recovery:** <!-- ✏️ restore from snapshot, monitor -->
- **Lessons learned:** <!-- ✏️ e.g. enable account lockout policy, MFA for RDP -->

## 5. Timeline
| Time | Event |
|---|---|
| <!-- ✏️ --> | First failed logon from `10.0.2.x` |
| <!-- ✏️ --> | Wazuh alert raised |
| <!-- ✏️ --> | Analyst triage started |
| <!-- ✏️ --> | Contained |
