# Custom KQL Detection Rules

**Platform:** Elastic SIEM
**Total Rules:** 5
**Date Created:** March 9, 2026

---

## Rule 1 — Nmap Port Scan Detection
**Severity:** High
**MITRE:** T1046 — Network Service Discovery
**Runs every:** 5 minutes
**Look back:** 1 minute

**KQL Query:**
```
event.code: "1" and process.name: "nmap"
```

**Description:**
Detects execution of Nmap port scanning tool on the network.
Nmap is commonly used by attackers during reconnaissance phase
to discover open ports and running services on target systems.

---

## Rule 2 — Mimikatz Credential Dumping
**Severity:** Critical
**MITRE:** T1003 — OS Credential Dumping
**Runs every:** 5 minutes
**Look back:** 1 minute

**KQL Query:**
```
process.name: "mimikatz.exe" or process.args: "sekurlsa"
```

**Description:**
Detects execution of Mimikatz credential dumping tool or related
commands. Mimikatz is used by attackers to extract plaintext 
passwords, hashes and Kerberos tickets from memory.

---

## Rule 3 — Suspicious Encoded PowerShell
**Severity:** High
**MITRE:** T1027 — Obfuscated Files or Information
**Runs every:** 5 minutes
**Look back:** 1 minute

**KQL Query:**
```
process.name: "powershell.exe" and 
process.args: (*-enc* or *-encoded* or *-encodedcommand*)
```

**Description:**
Detects encoded PowerShell commands commonly used by attackers
to evade detection and execute malicious code. Encoded commands
are a common defense evasion technique used in malware and 
post-exploitation frameworks.

---

## Rule 4 — Brute Force Authentication Failure
**Severity:** High
**MITRE:** T1110 — Brute Force
**Runs every:** 5 minutes
**Look back:** 1 minute

**KQL Query:**
```
event.category: "authentication" and event.outcome: "failure"
```

**Description:**
Detects multiple authentication failures indicating a brute force
attack. This rule fires when authentication failures are detected
across any service including SMB, RDP, SSH and web applications.

---

## Rule 5 — Suspicious Network Reconnaissance
**Severity:** Medium
**MITRE:** T1046 — Network Service Discovery
**Runs every:** 5 minutes
**Look back:** 1 minute

**KQL Query:**
```
event.category: "network" and event.action: 
"lookup_result" and dns.question.name: *
```

**Description:**
Detects unusual network reconnaissance activity including DNS
lookups and network scanning. This rule generated 213 alerts
during the simulated attack demonstrating its effectiveness
at catching reconnaissance activity in real time.

---

## Rule Performance Summary
| Rule | Alerts Generated | Effectiveness |
|---|---|---|
| Nmap Port Scan | Active | High |
| Mimikatz Detection | Active | Critical |
| Encoded PowerShell | Active | High |
| Brute Force | Active | High |
| Network Recon | 213 alerts | Proven |
```

---

        incident_report.md
        attack_commands.md

        detection_rules.md
