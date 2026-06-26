# Incident Response Report #002
**Date:** June 26, 2026  
**Analyst:** Himanshu Jha  
**Environment:** Cloud-based SOC Lab (Google Cloud)

---

## Alert Details
| Field | Value |
|---|---|
| **Alert** | Multiple authentication failures followed by a success |
| **Severity** | Critical (Level 12) |
| **Rule ID** | 40112 |
| **Agent** | victim-machine |
| **Time** | 2026-06-26 17:06:33 UTC |

## MITRE ATT&CK Mapping
| Field | Value |
|---|---|
| **Technique** | T1110 - Brute Force |
| **Technique** | T1078 - Valid Accounts |
| **Tactic** | Credential Access, Privilege Escalation, Initial Access, Defense Evasion |

## Timeline
| Time | Event |
|---|---|
| 17:06:25 UTC | Hydra brute force launched from kali-attacker |
| 17:06:31 UTC | Multiple failed SSH login attempts detected |
| 17:06:31 UTC | Wazuh alert — brute force trying to get access (Level 10) |
| 17:06:33 UTC | Successful login with cracked password "ROOT" |
| 17:06:33 UTC | Wazuh critical alert — auth failures followed by success (Level 12) |

## Investigation
- **Source IP:** 10.190.0.4 (kali-attacker)
- **Target IP:** 10.190.0.7 (victim-machine)
- **Tool Used:** Hydra v9.1
- **Attack Type:** SSH brute force — dictionary attack using rockyou.txt
- **Credential Compromised:** root / ROOT
- **Outcome:** Root account fully compromised ⚠️

## Conclusion
Attacker successfully brute forced the root SSH account using a dictionary attack. The weak password "ROOT" was found within 8 attempts. This represents a critical security incident — the attacker has full root access to the victim machine.

## Remediation
- **Immediately** disable password authentication on SSH
- Reset root password to strong randomly generated value
- Disable root SSH login entirely
- Implement account lockout after 3 failed attempts
- Deploy fail2ban immediately
- Audit all actions taken by root after 17:06:33 UTC
- Consider machine as fully compromised — rebuild recommended
