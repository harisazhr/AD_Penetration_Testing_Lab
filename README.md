# AD_Penetration_Testing_Lab

## Objective

Build and compromise a complete Active Directory environment to demonstrate real-world attack techniques and detection capabilities. This lab simulates an authenticated attacker's progression from standard user access to persistent domain compromise through a chain of five attacks, all monitored via Splunk SIEM.

**Domain:** LAB.LOCAL  
**Attack Perspective:** Authenticated User (suser)  
**Final Status:** Complete Domain Compromise ✅  
**Testing Period:** January 14, 2026

## Lab Architecture

```
Network: 192.168.10.0/24
├── Domain Controller (192.168.10.10)
│   └── Windows Server 2019 - LAB.LOCAL
├── Workstation (192.168.10.20)
│   └── Windows 10 Enterprise (Domain-joined)
├── SIEM Server (192.168.10.30)
│   └── Ubuntu 24.04 LTS + Splunk Enterprise
└── Attack Platform (192.168.10.40)
    └── Kali Linux 2024
```

## Skills Learned

### Offensive Security
- **Kerberoasting** - Extracting and cracking service account TGS tickets
- **BloodHound Enumeration** - Mapping AD relationships and attack paths
- **Pass-the-Hash** - Lateral movement using NTLM hashes
- **DCSync Attack** - Impersonating DC to dump all credentials
- **Golden Ticket** - Forging Kerberos TGTs for persistent access

### Defensive Security
- **SIEM Detection** - Creating Splunk queries for AD attacks
- **Event Analysis** - Correlating Windows Event IDs (4768, 4769, 4662, 4624)
- **Threat Hunting** - Identifying anomalous authentication patterns
- **Log Management** - Configuring Universal Forwarders for DC monitoring

### Active Directory Security
- Service Principal Name (SPN) enumeration
- Kerberos authentication protocol exploitation
- Directory Replication Service (DRS) abuse
- Credential theft and hash extraction
- Understanding AD attack paths and privilege escalation

## Tools Used

**Offensive Tools:**
- **Impacket Suite** - GetUserSPNs, secretsdump, ticketer, smbclient
- **BloodHound** - AD relationship mapping and attack path visualization
- **CrackMapExec** - Network authentication and credential validation
- **Hashcat** - Password hash cracking

**Defensive Tools:**
- **Splunk Enterprise** - SIEM and log analysis platform
- **Splunk Universal Forwarder** - Log collection from Domain Controller
- **Windows Event Logs** - Security audit logging (EventIDs 4768, 4769, 4662, 4624)

**Lab Infrastructure:**
- **VirtualBox** - Virtualization platform
- **Windows Server 2019** - Domain Controller
- **Windows 10 Enterprise** - Domain workstation
- **Ubuntu 24.04** - SIEM server
- **Kali Linux 2024** - Attack platform

## Attack Summary

| Attack Vector | Severity | Impact | Detection |
|--------------|----------|--------|-----------|
| Kerberoasting | 🔴 HIGH | Service account credential compromise | Event ID 4769 |
| BloodHound Enumeration | 🟡 MEDIUM | Complete AD attack path mapping | LDAP queries |
| Pass-the-Hash | 🔴 HIGH | Lateral movement without password | Event ID 4624 (Type 3) |
| DCSync Attack | 🔴 **CRITICAL** | Complete domain credential theft | Event ID 4662 |
| Golden Ticket | 🔴 **CRITICAL** | Persistent domain admin access (10 years) | Event ID 4768 |

## Key Findings

### Critical Vulnerabilities
- ⚠️ **Weak Service Account Passwords** - sqlservice crackable with rockyou.txt
- ⚠️ **Password Reuse** - Multiple accounts shared same NTLM hash
- ⚠️ **Unrestricted Replication Rights** - jadmin had DCSync permissions
- ⚠️ **No Protected Users Group** - Privileged accounts vulnerable to hash theft
- ⚠️ **Limited Monitoring** - No baseline for normal authentication patterns

### Detection Highlights
All attacks were successfully detected via Splunk SIEM with custom SPL queries monitoring specific Event IDs and authentication patterns.

## Full Report

For detailed technical analysis, step-by-step attack execution, Splunk detection queries, and comprehensive remediation recommendations, please refer to the complete report:

📄 **[![View Report](https://img.shields.io/badge/View-Report-blue?style=for-the-badge)](./AD_Penetration_Testing_Lab_Report.pdf)**

The report includes:
- Complete lab architecture and network diagrams
- Detailed attack execution with screenshots
- Splunk detection queries and analysis
- Impact assessment (business & technical)
- Comprehensive mitigation strategies
- Lessons learned and future enhancements

## Technical Insights

### Why This Matters
**Real-World Relevance:** Every attack demonstrated in this lab has been used in actual breaches:
- Kerberoasting: Common initial access technique
- Golden Tickets: Used by APT groups for long-term persistence
- DCSync: Core technique in ransomware attacks

**Defense in Depth:** Single controls are insufficient - layered security with detection, prevention, and response capabilities is essential.

**Visibility is Security:** Organizations without SIEM monitoring remain blind to ongoing compromises. All attacks were stealthy but detectable with proper logging.

---

**Disclaimer:** This lab was conducted in a controlled environment for educational purposes. All techniques demonstrated should only be used in authorized security testing scenarios.

🔐 **Stay Secure!**
