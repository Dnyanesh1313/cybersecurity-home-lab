# Cybersecurity Home Lab

A virtualized penetration testing and SIEM detection lab built from scratch for hands-on cybersecurity learning. This project covers the full attack-to-detection lifecycle — from reconnaissance and exploitation to log monitoring and alerting.

---

## Lab Architecture

| Machine | Role | OS | IP Address |
|---|---|---|---|
| Kali Linux | Attacker | Linux | 192.168.56.10 |
| Windows 10 | Target + Splunk SIEM | Windows | 192.168.56.20 |
| Metasploitable 2 | Vulnerable Target | Linux | 192.168.56.30 |

All machines are connected via a **VirtualBox Host-Only Network** for isolated testing.

![Network Topology](screenshots/network-topology.png)

---

## Tools Used

| Tool | Purpose |
|---|---|
| VirtualBox 7.0 | Virtualization platform |
| Kali Linux | Attack machine with pre-installed security tools |
| Nmap | Network reconnaissance and port scanning |
| Metasploit Framework | Exploitation |
| Windows 10 Enterprise | Target machine with weak security configurations |
| Splunk Enterprise (Free) | SIEM — log ingestion, dashboards, and alerting |
| Sysmon | Advanced Windows endpoint logging |
| Metasploitable 2 | Intentionally vulnerable Linux target |

---

## Methodology

### Phase 1: Reconnaissance

Performed network scanning to identify live hosts and open ports.

- Discovered all 3 VMs on the 192.168.56.0/24 subnet
- Scanned Metasploitable 2 with Nmap for service enumeration:
- Identified vsftpd 2.3.4 as a high-risk vulnerability (known backdoor)

![Nmap Scan Results](screenshots/nmap-scan.png)

### Phase 2: Exploitation

Used Metasploit to exploit the vsftpd 2.3.4 backdoor vulnerability (CVE-2011-2523).

- Loaded the `exploit/unix/ftp/vsftpd_234_backdoor` module
- Set target to Metasploitable 2 IP
- Executed the exploit and gained a **root shell** without authentication

![Metasploit Exploit](screenshots/metasploit-exploit.png)

### Phase 3: Privilege Escalation & Post-Exploitation

- Confirmed root access via `whoami`
- Extracted `/etc/shadow` file
- Established persistence using a reverse shell (optional demonstration)

### Phase 4: SIEM Monitoring & Detection

- Installed **Splunk Enterprise** on Windows 10 as the central SIEM
- Installed **Sysmon** with SwiftOnSecurity configuration for detailed process and network logging
- Forwarded Windows and Sysmon logs to Splunk
- Created custom **Splunk dashboards** to visualize:
  - Failed logins (Event ID 4625)
  - Successful logins (Event ID 4624)
  - Process creation events (Sysmon Event ID 1)
  - Network connections (Sysmon Event ID 3)

![Splunk Dashboard](screenshots/splunk-dashboard.png)

### Phase 5: Alerting

Configured a Splunk alert that triggers when more than 5 failed login attempts occur within 5 minutes — simulating brute-force detection.

### Phase 6: Penetration Test Report

Compiled all findings into a professional penetration test report including:
- Executive summary
- Scope and methodology
- Detailed findings with CVSS 3.1 scoring
- Screenshots and proof-of-concept
- Remediation recommendations

---

## Key Findings

| Vulnerability | CVSS Score | Description | Remediation |
|---|---|---|---|
| vsftpd 2.3.4 Backdoor | 9.8 (Critical) | Unauthenticated remote root access via port 21 | Upgrade vsftpd or disable FTP service |
| Weak Credentials | 7.5 (High) | Default credentials on Metasploitable (msfadmin:msfadmin) | Change all default passwords |
| Open Telnet (Port 23) | 7.0 (High) | Unencrypted remote access protocol | Disable telnet, use SSH instead |

---

## MITRE ATT&CK Mapping

| Technique | Tactic | ID |
|---|---|---|
| Network Service Scanning | Reconnaissance | T1046 |
| Exploit Public-Facing Application | Initial Access | T1190 |
| Command and Scripting Interpreter | Execution | T1059 |
| Account Discovery | Discovery | T1087 |
| Brute Force | Credential Access | T1110 |

---

## Skills Demonstrated

- Virtual machine setup and network configuration
- Network reconnaissance and service enumeration (Nmap)
- Vulnerability assessment and exploitation (Metasploit)
- SIEM deployment and configuration (Splunk)
- Endpoint logging (Sysmon)
- Security alerting and detection engineering
- Technical report writing
- MITRE ATT&CK framework mapping

---

## What I Learned

- How to build and configure a multi-VM lab environment
- The full penetration testing workflow: recon → exploit → post-exploit → report
- How to detect attacks using a SIEM and create meaningful alerts
- The importance of documentation and report writing in cybersecurity
- How to map technical findings to business risk

---

## Future Improvements

- Add a web application vulnerability assessment using Burp Suite and OWASP Juice Shop
- Integrate Wazuh as an open-source SIEM alternative
- Deploy Caldera for automated adversary emulation
- Add a phishing simulation component using GoPhish
- Implement Atomic Red Team for detection validation

---

## Disclaimer

This project was built and tested in an isolated virtual environment. All techniques demonstrated are for educational purposes only. No real systems or networks were harmed.

---

## Connect With Me

[LinkedIn](https://linkedin.com/in/yourprofile) | [Email](mailto:your.email@example.com)
