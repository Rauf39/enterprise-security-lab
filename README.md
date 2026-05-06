# Enterprise Security Lab

![Status](https://img.shields.io/badge/Status-Completed-success)
![Project](https://img.shields.io/badge/Project-Final%20Graduation%20Project-blue)
![Grade](https://img.shields.io/badge/Grade-12%2F12-brightgreen)
![Focus](https://img.shields.io/badge/Focus-Cybersecurity%20%7C%20Infrastructure%20%7C%20SOC-critical)

## Overview


You can view or download the project PDF here:

[Enterprise Security Lab PDF](docs/project.pdf)

This repository contains my final graduation project completed at **STEP IT Academy**.

I worked on this project for a little over a month and received the maximum grade: **12/12**.

The goal of this project was to design and build an **enterprise-style cybersecurity and infrastructure lab** that combines networking, firewall security, Windows Server administration, Linux integration, monitoring, SIEM, backup, automation, privileged access management, and defensive security operations.

This is not just a collection of installed tools. The main idea was to connect different technologies into one realistic environment where infrastructure, security, monitoring, alerting, access control, and incident detection work together.

> Note: This repository contains only part of the full implementation. Some configurations, screenshots, credentials, IP addresses, certificates, API keys, and sensitive details are intentionally removed or sanitized.

---

## Project Goals

The main goals of this lab were:

- Build a realistic enterprise-style network topology
- Implement firewall security and network segmentation
- Configure monitoring and alerting for servers and services
- Deploy SIEM with Windows and Linux telemetry
- Integrate honeypot alerts into the detection pipeline
- Implement Active Directory policies and endpoint management
- Secure internal web services using certificates and reverse proxy
- Test backup, recovery, and Azure restore scenarios
- Practice privileged access management and jump-server access control
- Document the project in a clear and professional way

---

## High-Level Architecture

```text
                          Internet / WAN
                               |
                        Palo Alto Firewall
                         HA Active/Passive
                               |
                    Core Switches / LACP Links
                               |
                         Access Switches
                               |
        -------------------------------------------------
        |              |              |                 |
     VLAN Users     VLAN Servers    VLAN Security     DMZ/Lab
        |              |              |                 |
   Windows Clients   AD DS/DNS/CA   Wazuh/Zabbix     Cowrie
   Linux Clients     NTP Server     Grafana/Nessus   HAProxy
   Endpoint Central  Nextcloud      GitLab/PAM       Mattermost
                     Vaultwarden

Additional integrations:

Active Directory  --->  Entra Connect  --->  Microsoft Entra ID
Veeam Backup      --->  Azure Restore Testing
MARS Agent        --->  Azure Recovery Services Vault
Wazuh             --->  Telegram Alerts / VirusTotal / Sysmon / auditd / FIM
Zabbix            --->  Grafana / Telegram Alerts / HTTPS
HAProxy           --->  HTTPS access to internal services
Key Components
1. Network and Firewall Security

The network was designed with segmentation and controlled access in mind.

Implemented components:

Palo Alto Firewall with High Availability
Active/Passive HA firewall design
Core and Access switch architecture
LACP trunks between switches
Port Security on access switch ports
VLAN-based network segmentation
DHCP services configured through the firewall
Site-to-Site VPN with backup tunnel
Security policies for restricted inbound and inter-zone access
SNAT for outbound internet access
DNS forwarding policies for controlled name resolution
Deny rules for unnecessary internal access
Secure traffic flow between user, server, security, and management zones

The firewall was used not only for internet access, but also as a central security control point between different network segments.

2. Site-to-Site VPN

A Site-to-Site VPN was configured to simulate secure communication between two locations.

Implemented:

Primary VPN tunnel
Backup VPN tunnel
IKE/IPsec configuration
Security policies for tunnel traffic
Route and policy-based traffic control
Failover testing between primary and backup tunnels

The goal was to understand how enterprise VPN connectivity works, including tunnel negotiation, encryption, policy control, and backup tunnel behavior.

3. Wazuh SIEM and Security Monitoring

Wazuh was deployed as the main SIEM and security monitoring platform.

Implemented:

Wazuh Manager
Wazuh agents on Windows and Linux systems
Syslog collection
Sysmon integration for Windows telemetry
auditd integration for Linux monitoring
File Integrity Monitoring
VirusTotal integration
Telegram bot alerts
Custom alerting workflow
Security event collection and analysis

Detection use cases included:

Suspicious file changes
Windows event monitoring
Linux audit events
Honeypot login attempts
Malware/hash checking through VirusTotal
Security alerts forwarded to Telegram
4. Cowrie SSH Honeypot

Cowrie was deployed to simulate an exposed SSH service and collect attacker behavior.

Implemented:

Cowrie SSH honeypot
Honeypot SSH moved to port 22
Real SSH access moved to port 2222
Attack logging
Integration with Wazuh
Telegram alerting for suspicious activity

This allowed me to test how honeypot events can be collected, parsed, and used inside a SIEM environment.

5. Active Directory and Identity Management

A Windows Server Active Directory environment was configured as the identity core of the lab.

Implemented:

Active Directory Domain Services
DNS Server
Local Certificate Authority
Group Policy Objects
LAPS
Password policy
Account lockout policy
Audit policy
Wallpaper policy
Folder Redirection
Mapped network drives
Item-Level Targeting
Domain join restrictions
DNS scavenging
Linux integration with Active Directory using realmd/SSSD
NTP server with domain clients

The goal was to simulate a real corporate identity environment with centralized authentication, policy enforcement, and endpoint configuration.

6. Microsoft Entra Connect

Microsoft Entra Connect was configured to simulate hybrid identity.

Implemented:

On-premises Active Directory integration
Microsoft Entra Connect
Hybrid identity synchronization testing

This helped demonstrate how on-premises identity can be extended toward Microsoft cloud services.

7. Zabbix Monitoring

Zabbix was deployed for infrastructure and service monitoring.

Implemented:

Zabbix Server
Zabbix agents
Server monitoring
Service availability monitoring
Telegram alerting
MFA for web access
HTTPS certificate configuration
Monitoring dashboards

Zabbix was used to monitor servers, agents, availability, and infrastructure health.

8. Grafana Integration

Grafana was integrated with Zabbix to provide better visualization.

Implemented:

Grafana dashboarding
Zabbix data source integration
HTTPS access
Infrastructure visualization

Grafana was used to create cleaner dashboards for monitoring data collected by Zabbix.

9. HAProxy Reverse Proxy and HTTPS

HAProxy was configured as a reverse proxy for internal services.

Implemented:

HAProxy reverse proxy
HTTPS certificate configuration
Local CA-issued certificates
SAN-based certificate access
Internal DNS names such as:
zabbix.step.local
grafana.step.local
nextcloud.step.local

Additional hardening:

Restricted default page
Controlled access on port 80
HTTPS access for internal services
Reverse proxy routing based on hostnames

The goal was to make internal services accessible in a cleaner and more secure way using DNS names and HTTPS.

10. Local Certificate Authority

A local Certificate Authority was deployed on Windows Server.

Implemented:

Internal CA
Certificate generation for internal services
SAN certificates
HTTPS support for internal applications
Trust between domain clients and internal certificates

This was used together with HAProxy, Zabbix, Grafana, and Nextcloud.

11. Nextcloud

Nextcloud was deployed as an internal collaboration and file-sharing platform.

Implemented:

Nextcloud server
HTTPS access through HAProxy
Active Directory integration
MFA configuration
Internal DNS-based access

This simulated an internal enterprise file-sharing service.

12. Mattermost

Mattermost was deployed as an internal collaboration platform.

Implemented:

Mattermost server
Active Directory integration
MFA
Internal service access

This helped simulate secure internal communication inside the lab environment.

13. Vaultwarden

Vaultwarden was deployed for password and secret management.

Implemented:

Password storage
SSH key storage
Secure internal access
MFA

This was used to demonstrate centralized credential and secret management.

14. Endpoint Central

Endpoint Central was configured for endpoint management.

Implemented:

Domain integration
Endpoint monitoring
Patch management
Self-Service Portal
Software deployment for users
Managed endpoint visibility

The goal was to simulate IT endpoint administration and user self-service software access.

15. Nessus Vulnerability Scanning

Nessus was used for vulnerability assessment.

Implemented:

Nessus scanner
Credentialed scans
Vulnerability checks
Email report delivery
Scheduled/report-based workflow

This allowed me to test vulnerability scanning with authenticated checks and automated reporting.

16. Backup and Recovery

Backup and restore scenarios were tested using Veeam and Azure services.

Implemented:

Veeam Backup
Restore testing to Azure
Azure Recovery Services Vault
MARS agent backup configuration

This part of the project focused on business continuity, backup validation, and cloud recovery testing.

17. GoPhish Security Awareness Testing

GoPhish was deployed to simulate phishing awareness testing.

Implemented:

GoPhish server
User awareness simulation
Phishing campaign testing in a lab environment

This was used only for controlled educational testing inside the lab.

18. Privileged Access Management

A PAM solution was tested to control privileged access.

Implemented:

Session recording
Prohibited command control
MFA
Active Directory integration
SSH access restricted through jump servers

The goal was to simulate enterprise privileged access control and reduce direct administrative access to sensitive systems.

19. GitLab and Automation

GitLab was used for storing scripts and project files.

Implemented:

GitLab repository usage
SSH key authentication
Script storage
Git workflow practice
Pushing scripts through SSH

This helped practice secure code/script management and version control.

Technologies Used
Area	Technologies
Firewall	Palo Alto Firewall, HA, NAT, Security Policies
Switching	Core/Access Design, LACP, Port Security, VLANs
SIEM	Wazuh, Sysmon, auditd, FIM, Syslog
Monitoring	Zabbix, Grafana, Telegram Alerts
Identity	Active Directory, Group Policy, LAPS, Entra Connect
Linux Integration	realmd, SSSD, auditd
Reverse Proxy	HAProxy, HTTPS, SAN Certificates
Certificates	Windows Server CA, Internal PKI
Backup	Veeam Backup, Azure Recovery Services Vault, MARS Agent
Vulnerability Management	Nessus
Awareness Testing	GoPhish
Collaboration	Nextcloud, Mattermost
Secrets Management	Vaultwarden
Endpoint Management	Endpoint Central
Version Control	GitLab, SSH Keys
Access Control	PAM, Jump Server, MFA
Security Notes

Sensitive data is not included in this repository.

Removed or sanitized information includes:

Passwords
API keys
Telegram bot tokens
Public IP addresses
Private keys
VPN secrets
Internal credentials
Full certificate private material
Sensitive configuration values

This project was built for educational and portfolio purposes in a controlled lab environment.

What I Learned

During this project, I improved my practical skills in:

Enterprise network design
Firewall policy design
VLAN segmentation
Site-to-Site VPN configuration
Windows Server and Active Directory administration
Group Policy management
Linux integration with Active Directory
SIEM deployment and alerting
Windows and Linux telemetry collection
File integrity monitoring
Honeypot integration
Vulnerability scanning
Backup and restore testing
Reverse proxy and internal HTTPS
Certificate management
Endpoint management
Privileged access control
Documentation and troubleshooting
Future Improvements

Possible future improvements:

Add Microsoft Sentinel integration
Add more detection rules mapped to MITRE ATT&CK
Improve dashboards for SOC use cases
Add automated incident response workflows
Expand cloud security monitoring
Add more detailed network diagrams
Add Terraform or Ansible automation
Add more documentation for each service
Improve log forwarding between tools
Add more simulated attack scenarios
Screenshots

Recommended screenshots to include in this repository:

/screenshots/topology.png
/screenshots/palo-alto-ha.png
/screenshots/site-to-site-vpn.png
/screenshots/wazuh-dashboard.png
/screenshots/wazuh-alerts.png
/screenshots/cowrie-alert.png
/screenshots/zabbix-dashboard.png
/screenshots/grafana-dashboard.png
/screenshots/active-directory-gpo.png
/screenshots/haproxy-https.png
/screenshots/nextcloud-ad-mfa.png
/screenshots/nessus-report.png
/screenshots/endpoint-central.png
/screenshots/veeam-azure-restore.png
Repository Structure

Suggested structure:

enterprise-security-lab/
│
├── README.md
├── diagrams/
│   └── topology.png
│
├── screenshots/
│   ├── wazuh/
│   ├── zabbix/
│   ├── grafana/
│   ├── palo-alto/
│   ├── active-directory/
│   ├── azure/
│   └── backup/
│
├── configs/
│   ├── firewall/
│   ├── switches/
│   ├── haproxy/
│   ├── wazuh/
│   ├── zabbix/
│   ├── sysmon/
│   ├── auditd/
│   └── group-policy/
│
├── scripts/
│   ├── windows/
│   └── linux/
│
└── docs/
    ├── project-overview.md
    ├── network-design.md
    ├── siem-monitoring.md
    ├── active-directory.md
    ├── backup-recovery.md
    └── security-testing.md
Final Result

This project represents practical hands-on work across multiple areas of cybersecurity and IT infrastructure.

It combines:

Network security
Identity management
Monitoring
SIEM
Backup and recovery
Endpoint management
Vulnerability scanning
Secure internal services
Privileged access control
Automation and documentation

I am proud of this project because it helped me connect many technologies into one realistic enterprise-style lab and strengthen my understanding of how modern infrastructure can be monitored, secured, and managed.

Author

Rauf Mammadov
Cybersecurity Student | Blue Team | Infrastructure Security | SOC | Cloud Security

GitHub: Rauf39

Project Repository:
enterprise-security-lab
