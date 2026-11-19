# Finding 02 · Brute-force attack detected in Wazuh

> 🚧 **Status: in progress.** This page sets out the exercise plan, the detection rules that should fire and the response steps. The alert evidence, timeline and lessons learned will be added once the exercise has been run end to end.

| | |
|---|---|
| **Detection** | Custom rules `100100` (SSH) and `100110` (Windows/RDP) in [`configs/local_rules.xml`](../configs/local_rules.xml) |
| **ATT&CK** | T1110.001 Password Guessing · T1078 Valid Accounts (if a login succeeds) |

## 1. Simulation (red side)
```bash
# from Kali, against a deliberately weak account on a lab VM only
hydra -l labuser -P small-wordlist.txt ssh://<target>     # or rdp://<target>
```

## 2. Expected detection (blue side)
| Rule | Fires when | Level |
|---|---|---|
| `100100` | 5+ failed SSH logins from the same IP within 2 minutes | 10 |
| `100101` | A successful SSH login from that same IP after the brute force | 14 (possible compromise) |
| `100110` | 8+ failed Windows logons (event 4625) from the same IP within 2 minutes | 10 |

## 3. Triage questions I'll answer
| Question | Where to look |
|---|---|
| Internal or external source? | Source IP in the alert |
| How many attempts, over what time? | Count of 4625 / sshd failures |
| Which accounts were targeted? | `TargetUserName` in 4625 events |
| Any successful logon afterwards? | Event 4624 or rule `100101` |
| What happened after the logon? | Process creation (4688), new users (4720) |

## 4. Response plan (NIST SP 800-61)
- **Containment:** block the source IP, disable the targeted account
- **Eradication:** reset the password, check for persistence (new users, scheduled tasks)
- **Recovery:** roll back to the pre-exercise snapshot and keep monitoring
- **Lessons learned:** account lockout policy, MFA for remote access, alert tuning

## Results and timeline
*To be added after the exercise.*
