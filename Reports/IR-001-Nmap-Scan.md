# Incident Response Report #001
**Date:** June 26, 2026  
**Analyst:** Himanshu Jha  
**Environment:** Cloud-based SOC Lab (Google Cloud)

---

## Alert Details
| Field | Value |
|---|---|
| **Alert** | sshd: Attempt to login using a non-existent user |
| **Severity** | Medium (Level 5) |
| **Rule ID** | 5710 |
| **Agent** | victim-machine |
| **Time** | 2026-06-26 16:49:26 UTC |

## MITRE ATT&CK Mapping
| Field | Value |
|---|---|
| **Technique** | T1110.001 - Brute Force: Password Guessing |
| **Technique** | T1021.004 - Remote Services: SSH |
| **Tactic** | Credential Access, Lateral Movement |

## Timeline
| Time | Event |
|---|---|
| 16:49:26 UTC | Nmap -sV -sC scan launched from kali-attacker |
| 16:49:26 UTC | SSH probe triggered on victim-machine port 22 |
| 16:49:26 UTC | Wazuh alert fired — non-existent user login attempt |

## Investigation
- **Source IP:** 10.190.0.x (kali-attacker)
- **Target IP:** 10.190.0.x (victim-machine)
- **Tool Used:** Nmap service/script scan
- **Port Targeted:** 22 (SSH)
- **Outcome:** No successful login — probe only

## Conclusion
Attacker performed a service enumeration scan using Nmap which triggered SSH probing against the victim machine. No successful authentication occurred. This is consistent with early-stage reconnaissance activity.

## Remediation
- Disable SSH password authentication, enforce key-based auth only
- Implement fail2ban to block repeated SSH attempts
- Restrict SSH access by IP using firewall rules
- Monitor port 22 activity continuously via SIEM
