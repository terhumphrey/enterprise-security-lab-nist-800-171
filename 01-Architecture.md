# ELK Stack SIEM Home Lab Architecture

| Field						 | Value 						  		    				|
|---------------------------|-----------------------------------------------------------|
| Document Name 	| ELK Stack SIEM Home Lab Architecture	 							|
| Document Version 	| v0.2.0 															|
| Author			| Terry Humphrey 													|
| Status 		 	| Active 															|
| Last Updated 		| 2026-07-22 														|

---

## Table of Contents
- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Design Philosophy](#3-design-philosophy)
- [4. Physical Architecture](#4-physical-architecture)
- [5. Deployment Architecture](#5-deployment-architecture)
- [6. Network Architecture](#6-network-architecture)
- [7. Host Inventory](#7-host-inventory)
- [8. Identity Architecture](#8-identity-architecture)
- [9. Elastic Architecture](#9-elastic-architecture)
- [10. Monitoring Architecture](#10-monitoring-architecture)
- [11. Naming Standards](#11-naming-standards)
- [12. Security Architecture](#12-security-architecture)
- [13. Backup Strategy](#13-backup-strategy)
- [14. Planned Enhancements](#14-planned-enhancements)
- [15. Related Documentation](#15-related-documentation)

---


# 1. Purpose

## Overview

The ELK Stack SIEM Home Lab is a self-hosted cybersecurity training environment designed to simulate a small enterprise network. This environment is designed to provide hands-on experience with Security Operations Center (SOC) operations, Elastic SIEM administration, Active Directory management, endpoint monitoring, threat hunting, detection engineering, and incident response.

## Goals

The primary goals of this environment are:

- Learn enterprise security monitoring
- Build practical SIEM experience
- Develop detection engineering skills
- Practice incident response workflows
- Simulate adversary activity
- Build cybersecurity portfolio experience

---

# 2. Scope

## Included Systems

The following systems are a part of this lab:

| System 					| Purpose							|
|---------------------------|-----------------------------------|
| Elastic Stack 			| Centralized logging and analysis	|
| Active Directory 			| Enterprise Identity Management 	|
| Windows Endpoints 		| User workstation simulation		|
| Linux Systems 			| Server Monitoring					|
| Attack Systems 			| Security Testing					|


## Future Systems

The following systems are planned to be included in future iterations of the lab:
- Additional Windows clients
- Additional Linux servers

---

# 3. Design Philosophy

## Principles

This lab follows these design principles:

### Centralized Visibility

All endpoints forward logs to a central Elastic Stack deployment.

### Realistic Enterprise Architecture

Systems are designed to resemble a small business environment.

### Repeatability

All configurations should be documented and reproducible.

### Security Monitoring First

Systems are deployed with logging and detection capabilities as a priority.

# 4. Physical Architecture 

```mermaid
graph TD

    subgraph MacMini2014["Mac Mini Late 2014 - macOS Monterey"]
        subgraph HypervisorMac["VirtualBox"]
            subgraph Win25DC["Windows Server 2025 Domain Controller VM"]
            end
        end
    end

    subgraph WindowsMacMini["Mac Mini Late 2014 - Windows 10 Home"]
        subgraph HypervisorWindows["VirtualBox"]
            subgraph ElasticStackVM["Rocky Linux 9.8 VM"]
            end
        end
    end

    subgraph MBA["MacBook Air 2025 - macOS 26"]
        subgraph MBAHypervisor["VirtualBox"]
            subgraph Win11Pro1["Windows 11 Pro Workstation 1"]
            end
        end
    end

    subgraph MBP["MacBook Pro 2015 - macOS Monterey"]
        subgraph MBPHypervisor["VirtualBox"]
            subgraph Future["Reserved for Future Expansion"]
            end
        end
    end
```

---

# 5. Deployment Architecture

## Deployment Diagram

```mermaid
graph TD

    subgraph WindowsMacMini["Mac Mini Late 2014 - Windows 10 Home"]
        subgraph VBOX_ELK["VirtualBox"]
            subgraph ElasticStackVM["Rocky Linux 9.8 VM"]
                subgraph DockerEngine["Docker"]
                    ES["Elasticsearch Container"]
                    Kibana["Kibana Container"]
                end
                Fleet["Elastic Agent / Fleet Server"]
            end
        end
    end

    subgraph MacMini2014["Mac Mini Late 2014 - macOS Monterey"]
        subgraph VBOX_DC["VirtualBox"]
            subgraph Win25DC["Windows Server 2025 Domain Controller VM"]
            end
        end
    end

    subgraph MBA["MacBook Air 2025 - macOS 26"]
        subgraph VBOX_MBA["VirtualBox"]
            subgraph Win11Pro1["Windows 11 Pro Workstation 1"]
                W11P1AG["Elastic Agent"]
            end
        end
    end

    Win11Pro1 -->|Domain Services / DNS| Win25DC
    Win11Pro1 -->|Telemetry| Fleet
    Win25DC -->|Telemetry| Fleet
    Fleet -->|Data Ingestion| ES
    ES -->|Data Query| Kibana
 ```

---

# 6. Network Architecture

## Network Overview

Due to SSH connectivity requirements and existing network addressing within my home environment, the lab shares the same subnet as the production home network. Re-addressing the network was determined to be outside the scope of this project because it would require recreating numerous DHCP reservations and client configurations.

## Network Segmentation

| Network		| Address Space 	|
|---------------|-------------------|
| Home Network	| 192.168.1.0/24	|
| Lab Network 	| 192.168.1.0/24 	|

	
## IP Addresses

| Host 				| IP 							| Purpose 			|
| ------------------|-------------------------------|-------------------|
| LNX-ELK-01     	| 192.168.1.44                  | Elastic Stack 	| 
| WIN-DC-01		    | 192.168.1.10 					| Domain Controller | 
| WIN-PRO-01 		| DHCP						 	| Workstation 		|
| kali-01 			| TBD 							| Attacker			|

---

## DNS

| Setting 		| Value 							|
|---------------|-----------------------------------|
| DNS Provider	| Active Directory Integrated DNS	|
| Domain		| serenity.lab 						|

## Important Ports

| Port 	| Service 			|
|-------|-------------------|
| 9200 	| Elasticsearch	 	|
| 5601 	| Kibana			|
| 8220 	| Fleet Server	 	|

----

# 7. Host Inventory

| Hostname 			| Operating System 		| Role 							| Status 		|
|-------------------|-----------------------|-------------------------------|---------------|
| LNX-ELK-01    	| Rocky Linux 9.8 		| Elastic Stack/Fleet Server	| Active 		|
| WIN-DC-01 		| Windows Server 2025	| Domain Controller 			| Active 		|
| WIN-PRO-01 		| Windows 11 Pro 		| User Workstation 				| Active     	|

---

# 8. Identity Architecture 

## Overview

The ELK Stack SIEM Home Lab uses Microsoft Active Directory Domain Services (AD DS) as the centralized identity provider for the Windows environment.

The Active Directory domain provides centralized authentication, authorization, DNS integration, and Group Policy management for domain-joined Windows systems.

## Domain Information

| Setting 						| Value 							|
|---------------------------------------|---------------------------|
| Domain Name 					| serenity.lab 						|
| Forest Functional Level 		| Windows Server 2025 				|
| Domain Functional Level 		| Windows Server 2025 				|
| Primary Domain Controller 	| WIN-DC-01 						|
| DNS Role						| Active Directory Integrated DNS	|


## Domain Components

| System 				| Role 								|
|-----------------------|-----------------------------------|
| Windows Server 2025 	| Domain Controller 				|
| Windows 11 Pro 		| Domain-joined Workstation 		|
| Rocky Linux 			| Standalone Linux Host 			| 
| Elastic Stack 		| Standalone Application Services 	|


## Authentication Flow

```mermaid
flowchart TD
	WU[Windows User]
	WW[Windows 11 Workstation]
	AD[Active Directory]
	KA[Kerberos Authentication]
	AG[Access Granted]
	
	WU --> WW --> AD --> KA --> AG
 ```

## Organizational Units

Planned Structure

```
serenity.lab

├── Users
├── Computers
├── Servers
├── Workstations
├── Administrators
└── Service Accounts
```

---

# 9. Elastic Architecture

## Components

| Component 		| Purpose 						| Version 	| Port 	|
|-------------------|-------------------------------|-----------|-------|
| Elasticsearch 	| Data storage and search		| 8.13.4	| 9200	|
| Kibana 			| Visualization and analysis	| 8.13.4	| 5601	|
| Fleet Server 		| Agent management 				| 8.13.4	| 8220	|
| Elastic Agent 	| Endpoint collection 			| 8.13.4	|		|


## Deployment Method

| Component 	| Method 			|
|---------------|-------------------|
| Elasticsearch | Docker Container 	|
| Kibana 		| Docker Container 	|
| Fleet Server 	| Elastic Agent 	|


## Data Flow

```mermaid
flowchart LR
    Endpoint["Windows & Linux Endpoints"]
    Agent["Elastic Agent"]
    Fleet["Fleet Server"]
    ES["Elasticsearch"]
    Kibana["Kibana"]
    SOC["SOC Analyst"]

    Endpoint --> Agent
    Fleet -.->|Manages| Agent
    Agent -->|Telemetry| ES
    ES --> Kibana
    Kibana --> SOC
```

---

# 10. Monitoring Architecture

## Linux Monitoring

Primary Data Sources:
- CPU
- Memory
- Network
- Filesystem
- Processes
- Syslog
- Audit logs


## Windows Monitoring

Primary Data Sources:
- Security Events
- System Events
- Application Events
- PowerShell Logs
- Sysmon Events
- Defender Events

---

# 11. Naming Standards

## Hostnames

| Resource 				| Standard 			| Example                                   			|
|-----------------------|-------------------|-------------------------------------------------------|
| Windows Servers 		| WIN-FUNCTION-XX   | WIN-DC-01 		                                    |  
| Windows Workstations	| WIN-PRO-XX        | WIN-PRO-01		                                    |
| Linux Servers			| LNX-FUNCTION-XX   | LNX-ELK-01                                            |
| Agent Policies		| POL-FUNCTION      | POL-Windows-Servers                                   |
| Dashboards			| DB-XXX-Function   | DB-001 - Windows Security & Authentication Overview   |

## Agent Policies
- Windows Servers
- Windows Workstations
- Linux Servers

## Dashboards
- SOC - Overview
- Linux - Infrastructure
- Windows - Endpoints
- Fleet - Agent Health

---

# 12 . Security Architecture

## Security Controls

Current Controls:
- Centralized logging
- Endpoint monitoring
- Identity management
- Role-based access
- Security alerting
- Centralized policy management

---

# 13. Backup Strategy

- Documentation stored in GitHub
- VM Snapshots used for recovery points
- Configuration files maintained for rebuild capabilities

---

# 14. Planned Enhancements

- Kali Linux attack simulation workstation
- Additional Windows workstations
- Additional Linux servers
- Threat hunting scenarios

---

# 15. Related Documentation


| Document                          | Purpose                                                                                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| README.md                         | High-level overview of the Enterprise Security Lab, objectives, architecture, technologies, hardware inventory, capabilities, and documentation index.            |
| 02-Network-Design.md              | Network architecture, IP addressing, DNS, communication flows, firewall requirements, segmentation, and network security considerations.                          |
| 03-Asset-Inventory.md             | Inventory of physical devices, VMs, operating systems, hostnames, IP addresses, and system roles/ownership.                                                       |
| 04-Active-Directory.md            | Active Directory architecture, OUs, users, groups, naming conventions, GPOs, authentication, and identity management.                                             |
| 05-Certificate-Authority-PKI.md   | Enterprise CA, certificate templates, trust relationships, certificate lifecycle, and PKI implementation.                                                         |
| 06-Server-Build-Standards.md      | Baseline configuration standards for Windows and Linux servers, including naming, security settings, and required services.                                       |
| 07-Elastic-Deployment.md          | Elasticsearch and Kibana installation, configuration, cluster architecture, and core Elastic Stack infrastructure.                                                |
| 08-Elastic-Fleet-Deployment.md    | Fleet Server, agent policies, integrations, enrollment, and centralized agent management.                                                                         |
| 09-Windows-Agent.md               | Elastic Agent deployment, configuration, integrations, validation, and troubleshooting for Windows endpoints.                                                     |
| 10-Linux-Agent.md                 | Elastic Agent deployment, configuration, integrations, validation, and troubleshooting for Linux systems.                                                         |
| 11-Sysmon.md                      | Sysmon installation, configuration, event collection, telemetry, and Elastic integration.                                                                         |
| 12-Elastic-Security.md            | Elastic Security configuration, detection alerting, dashboards, cases, investigations, and analyst workflows.                                                     |
| 13-Detection-Rules.md             | The 30 custom detection rules, KQL, index patterns, severity, risk scores, MITRE ATT&CK mappings, validation status, tuning, and false-positive considerations.   |
| 14-Vulnerability-Management.md    | Vulnerability scanning, risk prioritization, remediation workflows, and verification.                                                                             |
| 15-Patch-Management.md            | WSUS deployment, update approvals, client targeting, maintenance windows, and patch compliance.                                                                   |
| 16-Incident-Response.md           | Incident response lifecycle, alert triage, investigation, containment, eradication, recovery, and lessons learned.                                                |
| 17-Investigation-Runbooks.md      | New. Step-by-step analyst procedures for investigating high-value alerts and detection scenarios.                                                                 |
| 18-Backup-Recovery.md             | Backup strategy, VM recovery, file restoration, disaster recovery, and recovery validation.                                                                       |
| 19-Security-Hardening.md          | Windows/Linux hardening, security baselines, auditing, logging, and defensive controls.                                                                           |
| 20-NIST-CSF-Mapping.md            | Maps lab capabilities to the NIST Cybersecurity Framework and demonstrates alignment with enterprise security practices.                                          |
| 99-Lab-Journal.md                 | Chronological implementation record, troubleshooting, design decisions, testing, snapshots, and future improvements.                                              |

---

# Revision History

| Version 	| Date			| Changes 										|
|-----------|---------------|-----------------------------------------------|
| v0.1.0	| 2026-07-09	| Initial architecture documentation published	|
| v0.1.1	| 2026-07-10	| Updated Related documentation section.		|
| v0.2.0    | 2026-07-22    | Updated to reflect new architecture design    |
---	



