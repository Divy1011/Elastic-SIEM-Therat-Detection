# SOC Incident Report
## Threat Detection Lab — Simulated Attack Analysis

**Report Date:** March 9, 2026
**Analyst:** Divy Patel
**Severity:** Medium
**Status:** Resolved (Simulated)

---

## Executive Summary
A simulated multi-phase attack was conducted against Windows host 
DESKTOP-DDM3ULB using Kali Linux as the attacker machine. The attack 
chain included network reconnaissance, port scanning, SMB vulnerability 
scanning, and brute force authentication attempts. Elastic SIEM 
successfully detected and alerted on all attack phases generating 
213 alerts across 5 custom detection rules.

---

## Timeline of Events

| Time | Event | Source IP | Destination | Tool |
|---|---|---|---|---|
| 11:28 | Network host discovery scan | Kali Linux | 192.168.1.0/24 | Nmap |
| 11:32 | Full port and service scan | Kali Linux | 192.168.1.16 | Nmap -sV -sC -O |
| 11:38 | SMB vulnerability scan | Kali Linux | 192.168.1.16 | Nmap smb-vuln |
| 11:40 | SMB brute force attack | Kali Linux | 192.168.1.16:445 | Hydra |
| 11:42 | HTTP shellshock probe | Kali Linux | 192.168.1.16 | Nmap scripts |
| 16:22 | 213 alerts triggered | DESKTOP-DDM3ULB | Elastic SIEM | Custom rules |

---

## Attack Details

### Phase 1 — Reconnaissance
**Tool:** Nmap -sn 192.168.1.0/24
**Result:** Discovered 20+ hosts including DESKTOP-DDM3ULB at 192.168.1.16
**MITRE:** T1046 — Network Service Discovery

### Phase 2 — Port Scanning
**Tool:** Nmap -sV -sC -O 192.168.1.16
**Result:** Discovered 8 open ports including SMB (445), Splunk (8000), 
Shuffle (3001), and OpenSearch (9200)
**MITRE:** T1046 — Network Service Discovery

### Phase 3 — SMB Vulnerability Scan
**Tool:** Nmap --script smb-vuln* 192.168.1.16
**Result:** SMB signing enabled but not required — potential relay attack vector
**MITRE:** T1210 — Exploitation of Remote Services

### Phase 4 — Brute Force Attack
**Tool:** Hydra -l administrator -P rockyou.txt 192.168.1.16 smb
**Result:** 14,344,399 password attempts against SMB port 445
**MITRE:** T1110 — Brute Force

### Phase 5 — Post Exploitation Discovery
**Commands:** whoami /priv, net user, net localgroup administrators,
ipconfig /all, systeminfo
**Result:** System information and user enumeration completed
**MITRE:** T1082, T1087, T1016, T1069

---

## Detection Summary

| Rule | Alerts | Severity | Status |
|---|---|---|---|
| Suspicious Network Reconnaissance | 213 | Medium | Detected |
| Nmap Port Scan Detected | Active | High | Enabled |
| Brute Force Authentication Failure | Active | High | Enabled |
| Mimikatz Credential Dumping | Active | Critical | Enabled |
| Suspicious Encoded PowerShell | Active | High | Enabled |

---

## Affected Systems
| Host | IP | OS | Status |
|---|---|---|---|
| DESKTOP-DDM3ULB | 192.168.1.16 | Windows 10/11 | Monitored |

---

## Open Ports Exposed
| Port | Service | Risk |
|---|---|---|
| 135 | Windows RPC | Medium |
| 139 | NetBIOS | Medium |
| 445 | SMB | High |
| 3001 | Shuffle SOAR | Medium |
| 8000 | Splunk | Medium |
| 8089 | Splunk SSL | Medium |
| 9200 | OpenSearch | High |

---

## Vulnerabilities Identified
1. **SMB signing not required** — allows potential NTLM relay attacks
2. **Multiple management ports exposed** — Splunk and OpenSearch accessible
3. **No account lockout policy** — allowed 14M+ brute force attempts
4. **NetBIOS enabled** — legacy protocol increases attack surface

---

## Recommendations
1. Enable SMB signing requirement on all Windows hosts
2. Implement account lockout policy after 5 failed attempts
3. Restrict management ports to specific IP ranges
4. Disable NetBIOS if not required
5. Implement network segmentation between attacker and target
6. Deploy IPS/IDS to block port scanning activity

---

## Conclusion
The Elastic SIEM platform successfully detected all simulated attack 
phases. Custom KQL detection rules generated 213 alerts providing 
full visibility into the attack chain. The lab demonstrates effective 
detection engineering and SOC monitoring capabilities aligned with 
MITRE ATT&CK framework techniques T1046, T1110, T1082, T1087, 
T1016, T1069, T1210, and T1027.

---

**Report prepared by:** Divy Patel
**Institution:** Rowan University — M.S. Cybersecurity
**Date:** March 9, 2026