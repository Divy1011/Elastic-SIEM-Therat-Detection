# Elastic SIEM Threat Detection Lab

## Project Overview
Advanced SOC threat detection lab built with Elastic SIEM, Sysmon, 
and Kali Linux. Simulated a full attack chain against a Windows target 
machine and built custom detection rules that generated 213 real alerts.
This project demonstrates real-world SOC analyst skills including 
threat detection, alert triage, and incident response.

## Architecture
```
Kali Linux (Attacker)
        ↓
Real attack simulation
        ↓
Windows PC (Victim) + Sysmon
        ↓
Elastic Agent collects telemetry
        ↓
Elastic Cloud SIEM processes logs
        ↓
Custom KQL detection rules fire alerts
        ↓
Kibana dashboard visualizes threats
        ↓
SOC analyst investigates 213 alerts
```

## Tools Used
| Tool | Purpose |
|---|---|
| Elastic Cloud SIEM | Cloud-based SIEM and alert management |
| Elastic Agent | Log collection and endpoint monitoring |
| Elastic Defend | Endpoint detection and response |
| Sysmon | Deep Windows telemetry capture |
| Kibana | Dashboard and data visualization |
| Kali Linux | Attack simulation machine |
| Nmap | Network reconnaissance and port scanning |
| Hydra | Brute force attack simulation |
| VirtualBox | Kali Linux virtualization |

## Attack Chain Simulated
| Phase | Attack | Tool | MITRE Technique |
|---|---|---|---|
| Reconnaissance | Network host discovery | Nmap -sn | T1046 |
| Scanning | Full port + service scan | Nmap -sV -sC -O | T1046 |
| Vulnerability Scan | SMB vulnerability check | Nmap smb-vuln scripts | T1210 |
| Brute Force | SMB authentication attack | Hydra | T1110 |
| Post Exploitation | Privilege enumeration | whoami /priv | T1069 |
| Discovery | User enumeration | net user | T1087 |
| Discovery | Network configuration | ipconfig /all | T1016 |
| Discovery | System information | systeminfo | T1082 |

## Custom Detection Rules Created
| Rule Name | Severity | MITRE Technique |
|---|---|---|
| Nmap Port Scan Detected | High | T1046 |
| Mimikatz Credential Dumping Detected | Critical | T1003 |
| Suspicious Encoded PowerShell Execution | High | T1027 |
| Brute Force Authentication Failure Detected | High | T1110 |
| Suspicious Network Reconnaissance Detected | Medium | T1046 |

## Results
- Total alerts generated: **213**
- Alert severity: Medium
- Primary rule triggered: Suspicious Network Reconnaissance
- Target host: DESKTOP-DDM3ULB
- Attack source: Kali Linux VM
- Detection rate: 100% of simulated attacks captured

## Key Findings
| Finding | Details |
|---|---|
| Open ports discovered | 135, 139, 445, 3001, 5001, 8000, 8089, 9200 |
| Services identified | Windows RPC, NetBIOS, SMB, HTTP |
| SMB signing | Enabled but not required (vulnerable) |
| OS fingerprinted | Windows 10/11 |
| Brute force attempts | 14,344,399 password attempts |

## Kibana Dashboard Panels
- Alerts by Severity — shows distribution of alert severity levels
- Top Source IPs — identifies most active attacking IP addresses
- Security Events Over Time — timeline showing attack spikes
- Event Categories — breakdown of network, process, file, registry events

## Detection Engineering Highlights
All 5 custom rules were written using KQL (Kibana Query Language)
and mapped to the MITRE ATT&CK framework. Rules were configured
to run every 5 minutes with a 1 minute look-back window for
near real-time detection.

## MITRE ATT&CK Coverage
```
Reconnaissance    → T1046 Network Service Discovery
Initial Access    → T1190 Exploit Public-Facing Application  
Execution         → T1059 Command and Scripting Interpreter
Persistence       → T1053 Scheduled Task
Privilege Esc     → T1069 Permission Groups Discovery
Defense Evasion   → T1027 Obfuscated Files or Information
Credential Access → T1003 OS Credential Dumping
Discovery         → T1082 System Information Discovery
Discovery         → T1087 Account Discovery
Lateral Movement  → T1210 Exploitation of Remote Services
```

## How to Reproduce
1. Create free Elastic Cloud account at cloud.elastic.co
2. Create new Security serverless project
3. Install Elastic Agent on Windows target machine
4. Install Sysmon with olafhartong modular config
5. Add Windows integration in Elastic Fleet
6. Set up Kali Linux in VirtualBox
7. Run attack chain from Kali against Windows target
8. Create custom KQL detection rules in Elastic Security
9. Build Kibana dashboard to visualize threats
10. Analyze generated alerts in Elastic SIEM

## Screenshots
| Screenshot | Description |
|---|---|
| kibana_dashboard.png | 4-panel SOC dashboard showing live threat data |
| elastic_alerts.png | 213 alerts triggered by custom detection rules |
| kali_attacks.png | Kali Linux running Nmap and Hydra attacks |
| detection_rules.png | 5 custom KQL rules in Elastic Security |

## Comparison to Standard Projects
Most SIEM labs only collect logs passively.
This project goes further by:
- Using a real attacker machine (Kali Linux)
- Simulating a full 8-phase attack chain
- Writing custom detection rules from scratch
- Generating and triaging real alerts
- Documenting findings like a real SOC analyst

## Related Projects
- [Splunk Firewall Log Analysis]
- [SOC Automation — Shuffle SOAR + TheHive]

## Skills Demonstrated
- Elastic SIEM configuration and management
- KQL detection rule writing
- Threat hunting and alert triage
- Attack simulation with Kali Linux
- Sysmon deployment and configuration
- Kibana dashboard creation
- MITRE ATT&CK framework mapping
- Incident documentation
- Detection engineering
