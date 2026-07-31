# Security Hardening

| Field						 | Value 						  		    				|
|-------------------|-------------------------------------------------------------------|
| Document Name 	| Security Hardening 										    	|
| Document Version 	| v0.1.0 															|
| Author			| Terry Humphrey 													|
| Status 		 	| Active 															|
| Last Updated 		| 2026-07-30 														|

---

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Security Hardening Overview](#3-security-hardening-overview)
- [4. Hardening Objectives](#4-hardening-objectives)
- [5. Security Baseline](#5-security-baseline)
- [6. Windows Server Hardening](#6-windows-server-hardening)
- [7. Windows Workstation Hardening](#7-windows-workstation-hardening)
- [8. Active Directory Hardening](#8-active-directory-hardening)
- [9. Group Policy Hardening](#9-group-policy-hardening)
- [10. Linux Server Hardening](#10-linux-server-hardening)
- [11. Elastic Stack Hardening](#11-elastic-stack-hardening)
- [12. Network Security Hardening](#12-network-security-hardening)
- [13. Endpoint Security](#13-endpoint-security)
- [14. Logging and Auditing](#14-logging-and-auditing)
- [15. Certificate and PKI Security](#15-certificate-and-pki-security)
- [16. Vulnerability and Patch Management](#16-vulnerability-and-patch-management)
- [17. Security Validation](#17-security-validation)
- [18. Hardening Exceptions](#18-hardening-exceptions)
- [19. Security Hardening Metrics](#19-security-hardening-metrics)
- [20. Future Hardening Enhancements](#20-future-hardening-enhancements)
- [21. Related Documentation](#21-related-documentation)

---

# 1. Purpose

## Overview

The Enterprise Security Lab Security Hardening document defines the security configuration standards and defensive controls used to reduce the attack surface of systems within the lab environment.

The purpose of hardening is to establish secure baseline configurations, reduce unnecessary services and exposure, improve endpoint security, strengthen identity controls, and ensure that security-relevant activity is logged and monitored.

The hardening strategy is designed to reflect enterprise security practices while remaining appropriate for a controlled home laboratory environment.

---

# 2. Scope

This document covers:

- Windows Server hardening
- Windows workstation hardening
- Active Directory hardening
- Group Policy security
- Linux server hardening
- Elastic Stack hardening
- Network security
- Endpoint security
- Logging and auditing
- Certificate and PKI security
- Vulnerability management
- Patch management
- Security validation
- Hardening exceptions

This document applies to systems that are part of the Enterprise Security Lab.

This document does not define detailed deployment procedures for individual systems. Those procedures are documented separately in the related documentation.

---

# 3. Security Hardening Overview

Security hardening is implemented as a layered defense strategy.

The lab uses a combination of:

- Secure operating system configuration
- Identity and access controls
- Group Policy
- Firewall restrictions
- Service minimization
- Endpoint security
- Centralized logging
- Sysmon telemetry
- Elastic Security monitoring
- Vulnerability management
- Patch management
- Certificate-based trust
- Configuration management
- Security validation

Hardening activities should be documented and validated to ensure that security controls do not negatively affect required lab functionality.

---

# 4. Hardening Objectives

The primary hardening objectives are:

- Reduce attack surface.
- Minimize unnecessary services.
- Restrict administrative access.
- Strengthen authentication.
- Protect privileged accounts.
- Enforce secure password policies.
- Improve endpoint security.
- Enable security auditing.
- Centralize security telemetry.
- Detect unauthorized changes.
- Protect sensitive credentials.
- Maintain secure network communication.
- Reduce known vulnerabilities.
- Maintain consistent security baselines.
- Validate security controls through testing.

---

# 5. Security Baseline

The lab should maintain a documented security baseline for each major operating system and infrastructure role.

## Baseline Categories

| Category                  | Security Objective                                |
|---------------------------|---------------------------------------------------|
| Identity                  | Restrict account and privilege usage              |
| Authentication            | Enforce secure authentication controls            |
| Authorization             | Apply least privilege                             |
| Network                   | Restrict unnecessary network access               |
| Services                  | Disable unnecessary services                      |
| Firewall                  | Restrict inbound and outbound traffic             |
| Logging                   | Enable security-relevant auditing                 |
| Monitoring                | Forward telemetry to Elastic Security             |
| Patching                  | Maintain current security updates                 |
| Vulnerability Management  | Identify and remediate security weaknesses        |
| Encryption                | Protect sensitive communication and data          |
| Configuration             | Maintain controlled and documented configurations |
| Recovery                  | Maintain recoverable system configurations        |

## Baseline Management

Security baselines should be reviewed when:

- New systems are deployed.
- New services are enabled.
- Major operating system updates are applied.
- Security requirements change.
- Vulnerabilities are identified.
- Configuration changes introduce new risks.

## Baseline References

Security hardening decisions are informed by established security frameworks and industry guidance, including:

- Microsoft Security Baselines
- CIS Benchmarks
- NIST Cybersecurity Framework
- NIST SP 800-171 security requirements
- Vendor security recommendations

---

# 6. Windows Server Hardening

Windows Server systems should be hardened according to their specific roles.

Applicable systems include:

- WIN-DC-01
- WIN-WSUS-01
- Other Windows Server systems

## Account Security

Windows Server systems should:

- Use unique administrative credentials.
- Restrict local administrator access.
- Avoid routine use of privileged accounts.
- Use standard user accounts for normal administration where practical.
- Disable unnecessary built-in accounts where appropriate.
- Protect service account credentials.
- Apply least privilege.
- Use managed service accounts where practical.

## Password Security

Password policies should enforce:

- Minimum password length.
- Password history.
- Password complexity.
- Account lockout controls.
- Password expiration where appropriate.
- Protection against weak passwords.

## Windows Firewall

Windows Firewall should be enabled.

Inbound access should be restricted to required services.

Examples include:

- Active Directory
- DNS
- Kerberos
- LDAP
- SMB
- Certificate Services
- WSUS
- Elastic Agent
- Administrative services

Unnecessary inbound ports should be blocked.

## Services

Unnecessary services should be disabled where practical.

Before disabling a service, verify that it is not required for:

- Operating system functionality
- Active Directory
- DNS
- Certificate Services
- WSUS
- Elastic Agent
- Security monitoring

## Remote Administration

Remote administration should be restricted to authorized systems and administrators.

Administrative protocols should use secure communication where supported.

---

# 7. Windows Workstation Hardening

Windows workstations should be configured to reduce common attack vectors.

Hardening should include:

- Windows Defender enabled.
- Windows Firewall enabled.
- Security updates installed.
- Strong password policy.
- Screen lock enabled.
- User Account Control enabled.
- Local administrator access restricted.
- Unnecessary software removed.
- Unnecessary services disabled.
- PowerShell logging enabled where practical.
- Sysmon installed.
- Elastic Agent installed.
- Security telemetry forwarded to Elastic Security.

## Application Security

Applications should be kept current.

Unnecessary applications should be removed.

Software downloaded from untrusted sources should not be installed on systems outside designated attack simulation activities.

---

# 8. Active Directory Hardening

Active Directory is a critical security boundary within the Enterprise Security Lab.

## Privileged Accounts

Administrative privileges should be restricted.

The following principles should be applied:

- Use separate administrative and standard user accounts.
- Minimize Domain Admin membership.
- Avoid using Domain Admin accounts for routine activities.
- Review privileged group membership.
- Remove unnecessary privileged accounts.
- Protect service accounts.

## Organizational Units

OUs should be structured to support:

- Administrative separation
- Group Policy application
- System role separation
- Security policy enforcement

## Security Groups

Security groups should follow least-privilege principles.

Group membership should be reviewed periodically.

## Domain Controller Security

The Domain Controller should:

- Run only required services.
- Restrict administrative access.
- Use Windows Firewall.
- Enable security auditing.
- Forward security telemetry.
- Maintain current security updates.
- Protect Active Directory backups.
- Protect the Certificate Authority if hosted on the Domain Controller.

## Active Directory Security Monitoring

The Enterprise Security Lab monitors Active Directory security events through Elastic Security.

Monitoring focuses on:

- Privileged group membership changes
- User account creation
- User account deletion
- Password changes
- Authentication failures
- Kerberos-related events
- Group Policy changes
- Domain Controller security events

---

# 9. Group Policy Hardening

Group Policy provides centralized enforcement of security controls.

## Recommended Security Controls

Group Policy should be used where practical to enforce:

- Password policies
- Account lockout policies
- Windows Firewall
- Security auditing
- User rights assignments
- Screen lock policies
- PowerShell logging
- Windows Defender configuration
- Microsoft Defender Firewall
- Security event logging
- Restriction of unnecessary services
- Removable media controls where appropriate

## Group Policy Management

Group Policies should:

- Use descriptive names.
- Be documented.
- Be scoped appropriately.
- Avoid unnecessary overlapping policies.
- Be tested before broad deployment.
- Be reviewed after major changes.

---

# 10. Linux Server Hardening

Linux servers should be hardened according to their assigned roles.

Applicable systems include:

- LNX-ELK-01
- LNX-WEB-01

## Account Security

Linux systems should:

- Use unique user accounts.
- Restrict root access.
- Use `sudo` for administrative privileges.
- Disable direct root SSH access where practical.
- Use strong authentication.
- Remove unnecessary accounts.

## SSH Security

SSH should be restricted to authorized administrators.

Where practical:

- Use key-based authentication.
- Disable direct root login.
- Restrict SSH access by firewall rules.
- Disable unnecessary authentication methods.
- Monitor authentication logs.

## Firewall

Linux firewall controls should restrict access to required services.

Examples include:

- SSH
- Elasticsearch
- Kibana
- Fleet Server
- DNS
- Other required infrastructure services

Unnecessary ports should remain closed.

## Services

Only required services should be enabled.

Unnecessary services and packages should be removed or disabled where practical.

## File Permissions

Sensitive configuration files should use appropriate ownership and permissions.

Examples include:

- Elastic configuration
- TLS certificates
- Private keys
- Docker configuration
- SSH keys
- Service credentials

---

# 11. Elastic Stack Hardening

The Elastic Stack is a critical security monitoring platform and must be protected from unauthorized access.

## Access Control

Elastic Security should enforce:

- Authentication
- Role-based access control
- Least privilege
- Administrative separation
- Restricted access to sensitive data

## Elasticsearch

Elasticsearch should:

- Use authenticated access.
- Use encrypted communication where practical.
- Restrict network exposure.
- Avoid unnecessary public exposure.
- Protect credentials.
- Restrict administrative APIs.

## Kibana

Kibana should:

- Require authentication.
- Use TLS encryption for administrative access and service communication where supported.
- Restrict access to trusted users.
- Use role-based access control.
- Avoid unnecessary Internet exposure.

## Fleet Server

Fleet Server should:

- Use TLS.
- Require authenticated agents.
- Restrict network access.
- Protect enrollment tokens.
- Protect service credentials.

## Elastic Agent

Elastic Agents should:

- Use secure communication.
- Remain enrolled with Fleet Server.
- Use appropriate Agent Policies.
- Receive only required integrations.
- Be monitored for connectivity failures.

---

# 12. Network Security Hardening

The Enterprise Security Lab currently shares the home network with production systems.

Because of this architecture, network security controls are particularly important.

## Current Network Security Position

The current architecture does not provide full network segmentation between lab and production systems.

Compensating controls include:

- Host-based firewalls.
- Restricted service exposure.
- Limited administrative access.
- No Internet exposure of lab services.
- Monitoring through Elastic Security.

## Network Controls

Recommended controls include:

- Restrict unnecessary inbound traffic.
- Restrict administrative services.
- Use host-based firewalls.
- Avoid exposing lab services to the Internet.
- Restrict management interfaces.
- Monitor unexpected network connections.
- Use secure protocols where practical.

## Future Segmentation

Future improvements may include:

- Dedicated lab VLAN
- Dedicated lab subnet
- Firewall-based segmentation
- Restricted inter-VLAN routing
- Isolated attack simulation network

---

# 13. Endpoint Security

Endpoints should implement multiple layers of defensive controls.

## Windows Endpoint Controls

Windows endpoints should use:

- Microsoft Defender
- Windows Firewall
- Sysmon
- Elastic Agent
- Security event logging
- PowerShell logging
- Application control where practical
- Current security updates

## Linux Endpoint Controls

Linux systems should use:

- Host-based firewall
- Secure SSH configuration
- Centralized logging
- Elastic Agent
- Current security updates
- Restricted administrative access

## Endpoint Monitoring

Security telemetry should be forwarded to Elastic Security.

Monitoring should include, where available:

- Process creation
- Process termination
- Network connections
- Authentication events
- Privilege changes
- File activity
- Service changes
- Scheduled tasks
- PowerShell activity
- Security configuration changes

---

# 14. Logging and Auditing

Logging is a critical component of the lab's defensive architecture.

## Windows Logging

Windows systems should collect relevant:

- Security events
- System events
- Application events
- PowerShell events
- Sysmon events
- Defender events

## Linux Logging

Linux systems should collect relevant:

- Authentication events
- SSH activity
- System events
- Service activity
- Privilege escalation activity
- Firewall events

## Centralized Monitoring

Logs should be forwarded to Elastic Security where practical.

The objective is to provide centralized visibility into:

- Authentication
- Process execution
- Network activity
- Privilege changes
- Persistence mechanisms
- Security control changes

---

# 15. Certificate and PKI Security

The Enterprise Security Lab uses an internal Certificate Authority to establish trust for secure communications.

## PKI Security Controls

The CA should be protected through:

- Restricted administrative access
- Protected CA private keys
- Secure certificate issuance
- Controlled certificate templates
- Appropriate certificate lifetimes
- Certificate revocation procedures
- Secure backup procedures

## Certificate Trust

Systems that communicate securely with Elastic services should trust the appropriate CA certificate.

CA trust should be validated on:

- Windows systems
- Linux systems
- Elastic Stack systems
- Elastic Agents

Private keys should never be stored in publicly accessible repositories.

---

# 16. Vulnerability and Patch Management

Security hardening must be supported by continuous vulnerability and patch management.

The lab should:

- Identify known vulnerabilities.
- Prioritize vulnerabilities based on risk.
- Apply security updates.
- Track remediation status.
- Validate successful remediation.
- Monitor for newly disclosed vulnerabilities.

Patch management should be coordinated with the lab's WSUS infrastructure where applicable.

Vulnerability findings should be used to identify weaknesses that may require configuration changes in addition to software updates.

---

# 17. Security Validation

Security hardening controls should be validated through testing.

## Validation Activities

Validation may include:

- Configuration reviews
- Vulnerability scans
- Port scans
- Authentication testing
- Firewall testing
- Privilege reviews
- Security event validation
- Detection rule validation
- Attack simulation
- Log ingestion testing

## Validation Checklist

- Windows Firewall enabled
- Linux firewall enabled
- Unnecessary services disabled
- Administrative access restricted
- Strong password policies enforced
- Privileged accounts reviewed
- Security auditing enabled
- Sysmon operational
- Elastic Agent operational
- Security telemetry ingested
- Detection rules operational
- Critical vulnerabilities addressed
- Security updates applied
- Certificates trusted
- Backup procedures validated

---

# 18. Hardening Exceptions

Exceptions may be required when a security control conflicts with a legitimate lab requirement.

Examples may include:

- Temporarily enabling a service for testing.
- Allowing a firewall port for security testing.
- Disabling a security control to simulate an attack scenario.
- Running intentionally vulnerable software.
- Allowing administrative access for troubleshooting.

## Exception Requirements

Hardening exceptions should:

- Be documented.
- Identify the affected system.
- Identify the reason for the exception.
- Define the security risk.
- Define the duration.
- Identify compensating controls.
- Be removed when no longer required.

## Exception Tracking

| Exception ID | System      | Control              | Reason                                                                          | Risk                                                                  | Compensating Control                                                                       | Expiration              |
|--------------|-------------|----------------------|---------------------------------------------------------------------------------|-----------------------------------------------------------------------|--------------------------------------------------------------------------------------------|-------------------------|
| EXC-001      | Lab Network | Network Segmentation | Lab environment currently shares network infrastructure with production systems | Increased risk of lateral movement between lab and production devices | Host-based firewalls, restricted service exposure, no Internet exposure, Elastic Security monitoring | Future VLAN implementation |

---

# 19. Security Hardening Metrics

Security hardening metrics should be used to measure the effectiveness of the lab's security baseline.

> **Note:** Metrics shown in this section are representative examples used to demonstrate security measurement methodology. They do not represent production compliance measurements.

| Metric                                | Sample Value |
|---------------------------------------|--------------|
| Systems in Scope                      | 6            |
| Systems Meeting Baseline              | 5            |
| Baseline Compliance Rate              | 83%          |
| Critical Hardening Findings           | 0            |
| High Hardening Findings               | 2            |
| Open Hardening Exceptions             | 1            |
| Critical Vulnerabilities              | 0            |
| High Vulnerabilities                  | 3            |
| Systems With Active Monitoring        | 6            |
| Systems With Current Security Updates | 6            |
| Last Hardening Review                 | 2026-07-30   |

## Hardening Review Tracking

| Review ID | System        | Review Date | Result    | Findings                                                             | Remediation Status               |
|-----------|---------------|-------------|-----------|----------------------------------------------------------------------|----------------------------------|
| HR-001    | WIN-DC-01     | 2026-07-30  | Completed | 1 high-priority finding related to network segmentation              | Documented compensating controls |
| HR-002    | LNX-ELK-01    | 2026-07-30  | Completed | 1 high-priority finding related to future TLS hardening improvements | Planned enhancement              |
| HR-003    | WIN-WSUS-01   | 2026-07-30  | Completed | No significant findings                                              | Baseline maintained              |

---

# 20. Future Hardening Enhancements

Planned or potential future improvements include:

- Implement formal Windows security baselines.
- Implement Microsoft security baseline policies.
- Expand Group Policy security controls.
- Implement additional Linux hardening.
- Implement centralized configuration management.
- Implement automated configuration compliance checks.
- Add vulnerability scanning.
- Expand network segmentation.
- Implement dedicated lab VLANs.
- Implement additional firewall restrictions.
- Expand endpoint detection coverage.
- Implement application control.
- Evaluate credential protection technologies.
- Expand privileged account management.
- Implement automated certificate lifecycle management.
- Conduct periodic security configuration reviews.
- Automate hardening validation.
- Map hardening controls to NIST Cybersecurity Framework categories.
- Map security controls to NIST SP 800-171 requirements.

---

# 21. Related Documentation

| Document                          | Purpose                                                                                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| README.md                         | High-level overview of the Enterprise Security Lab, objectives, architecture, technologies, hardware inventory, capabilities, and documentation index.            |
| 01-Architecture.md                | Overall lab architecture, physical hardware, virtualization layout, server roles, infrastructure components, and system relationships.                            |
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
| 20-NIST-CSF-Mapping.md            | Maps lab capabilities to the NIST Cybersecurity Framework and demonstrates alignment with enterprise security practices.                                          |
| 99-Lab-Journal.md                 | Chronological implementation record, troubleshooting, design decisions, testing, snapshots, and future improvements.                                              |

---

# Revision History

| Version 	| Date 		 | Changes 									    	    |
|-----------|------------|------------------------------------------------------|
| v0.1.0    | 2026-07-30 | Initial Security Hardening document created         |

---