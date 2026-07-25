# Enterprise Security Lab Server Build Standards

| Field             | Value                    |
|-------------------|--------------------------|
| Document Name     | Server Build Standards   |
| Document Version  | v0.1.0                   |
| Author            | Terry Humphrey           |
| Status            | Active                   |
| Last Updated      | 2026-07-24               |



---

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Server Build Standards Overview](#3-server-build-standards-overview)
- [4. Server Naming Standards](#4-server-naming-standards)
- [5. Windows Server Standards](#5-windows-server-standards)
- [6. Linux Server Standards](#6-linux-server-standards)
- [7. Operating System Configuration](#7-operating-system-configuration)
- [8. Network Configuration](#8-network-configuration)
- [9. Security Hardening](#9-security-hardening)
- [10. Logging and Monitoring](#10-logging-and-monitoring)
- [11. Certificate and Trust Configuration](#11-certificate-and-trust-configuration)
- [12. Backup and Recovery](#12-backup-and-recovery)
- [13. Validation and Testing](#13-validation-and-testing)
- [14. Troubleshooting](#14-troubleshooting)
- [15. Planned Enhancements](#15-planned-enhancements)
- [16. Related Documentation](#16-related-documentation)

---

# 1. Purpose

## Overview

This document defines baseline server build standards for Windows and Linux servers deployed within the Enterprise Security Lab.

The purpose of these standards is to establish consistent:

- Server naming
- Operating system configuration
- Network configuration
- Security hardening
- Logging and monitoring
- Certificate trust
- Backup and recovery
- Validation procedures

These standards are intended to support repeatable server deployments and provide a consistent baseline for infrastructure, security monitoring, detection engineering, and compliance documentation.

---

# 2. Scope

This document applies to Windows and Linux servers deployed within the Enterprise Security Lab.

This document covers:

- Server naming conventions
- Windows server standards
- Linux server standards
- Operating system configuration
- Network configuration
- Security hardening
- Logging and monitoring
- Certificate and trust configuration
- Backup and recovery
- Validation and testing
- Troubleshooting

This document does not define detailed configuration procedures for individual applications or services.

Application-specific configuration is documented separately.

---

# 3. Server Build Standards Overview

All servers deployed within the Enterprise Security Lab should follow a consistent baseline configuration appropriate to their operating system and assigned role.

The standard server build process consists of:

1. Provision server hardware or virtual machine.
2. Install the approved operating system.
3. Assign the appropriate hostname.
4. Configure network connectivity.
5. Configure DNS.
6. Configure time synchronization.
7. Apply operating system updates.
8. Apply security hardening requirements.
9. Configure required certificates and trust relationships.
10. Configure logging and monitoring.
11. Install required server roles and applications.
12. Enroll the server with applicable management systems.
13. Validate functionality.
14. Document the completed build.

The specific configuration applied to each server depends on its role within the lab.

---

# 4. Server Naming Standards

## Windows Servers

Windows server hostnames should follow the format:

`WIN-<ROLE>-<NUMBER>`

Examples:

- `WIN-DC-01`
- `WIN-CA-01`
- `WIN-WSUS-01`
- `WIN-FS-01`

## Windows Clients

Windows client systems should follow the format:

`WIN-PRO-<NUMBER>`

Examples:

- `WIN-PRO-01`
- `WIN-PRO-02`

## Linux Servers

Linux server hostnames should follow the format:

`LNX-<ROLE>-<NUMBER>`

Examples:

- `LNX-ELK-01`
- `LNX-WEB-01`
- `LNX-DB-01`

## Naming Requirements

Hostnames should:

- Be unique within the environment
- Clearly identify the system role
- Follow the approved naming convention
- Be documented in the asset inventory
- Be resolvable through DNS where applicable

---

# 5. Windows Server Standards

## Operating System

Windows servers should use an approved and supported Windows Server release appropriate for the assigned role.

The Enterprise Security Lab currently uses:

- Windows Server 2025

## Baseline Configuration

Windows servers should be configured with:

- Approved hostname
- Static or reserved IP address where appropriate
- Correct DNS configuration
- Correct time synchronization
- Current security updates
- Windows Firewall enabled
- Microsoft Defender enabled where appropriate
- Appropriate audit policies
- Required server roles only
- Required security certificates
- Elastic Agent where applicable

## Active Directory Servers

Domain controllers should be configured according to the Active Directory design documented in:

`04-Active-Directory.md`

Domain controllers should provide only the services required for their assigned role.

## Certificate Authority Servers

Certificate Authority systems should be configured according to the PKI design documented in:

`05-Certificate-Authority-PKI.md`

## Windows Server Roles

Server roles should be assigned according to documented infrastructure requirements.

Examples include:

- Active Directory Domain Services
- DNS
- Certificate Authority
- Group Policy
- WSUS
- File Services

Additional roles should be documented before deployment.

---

# 6. Linux Server Standards

## Operating System

Linux servers should use an approved and supported distribution appropriate for the assigned role.

The Enterprise Security Lab currently uses:

- Rocky Linux 9.8

## Baseline Configuration

Linux servers should be configured with:

- Approved hostname
- Static or reserved IP address where appropriate
- Correct DNS configuration
- Correct time synchronization
- Current security updates
- Firewall enabled
- SELinux enabled where supported and practical
- SSH access restricted appropriately
- Required services only
- Required certificate trust configuration
- Elastic Agent where applicable

## Linux Server Roles

Linux server roles may include:

- Elastic Stack
- Web Server
- Database Server
- Other infrastructure services

Each server should be assigned only the roles required for its intended purpose.

---

# 7. Operating System Configuration

## System Updates

Operating systems should be maintained with current security updates appropriate to the lab environment.

Update procedures should include:

- Reviewing available updates
- Applying approved security updates
- Rebooting when required
- Validating system functionality
- Documenting significant update-related issues

Windows patch management is documented separately in:

`15-Patch-Management.md`

## Time Synchronization

Servers should maintain accurate system time.

Time synchronization is required to support:

- Authentication
- Certificate validation
- Event correlation
- SIEM investigations
- Detection rule accuracy

Windows systems should use the domain time hierarchy where applicable.

Linux systems should use the approved NTP or chrony configuration.

## DNS

Servers should use the designated DNS infrastructure for name resolution.

DNS configuration should support:

- Forward resolution
- Reverse resolution where applicable
- Internal domain name resolution
- Server-to-server communication
- Active Directory functionality where applicable

DNS configuration should be validated after server deployment.

---

# 8. Network Configuration

## IP Addressing

Servers should use static IP addresses or DHCP reservations where appropriate.

IP assignments should be documented in:

`03-Asset-Inventory.md`

## Network Configuration Requirements

Server network configuration should include:

- IP address
- Subnet mask or prefix
- Default gateway
- DNS servers
- Hostname
- Fully qualified domain name where applicable

## Connectivity Validation

Server builds should validate connectivity to required infrastructure services.

Validation may include:

- DNS resolution
- Gateway connectivity
- Internet connectivity where required by server role
- Active Directory connectivity
- Elasticsearch connectivity
- Fleet Server connectivity
- Required application ports

Only required network communication should be permitted.

---

# 9. Security Hardening

## Baseline Security

All servers should be configured according to the security requirements appropriate to their operating system and role.

Security controls should include, where applicable:

- Host firewall
- Endpoint protection
- Security updates
- Least privilege
- Restricted administrative access
- Secure remote administration
- Account management
- Audit logging
- Secure configuration
- Removal of unnecessary services

## Administrative Access

Administrative access should be limited to authorized users.

Administrative accounts should:

- Use strong authentication
- Follow least-privilege principles
- Be uniquely identifiable
- Be reviewed periodically

Shared administrative accounts should be avoided where practical.

## Firewall Configuration

Host-based firewalls should remain enabled unless a documented requirement exists to disable them.

Firewall rules should:

- Permit required services
- Restrict unnecessary inbound access
- Restrict unnecessary outbound access where appropriate
- Be documented when significant

## Service Configuration

Servers should run only required services.

Unnecessary services should be:

- Disabled
- Removed
- Documented if retained for testing

---

# 10. Logging and Monitoring

## Logging Requirements

Servers should generate security and operational telemetry appropriate to their operating system and role.

Windows systems should collect applicable:

- Windows Event Logs
- Security events
- System events
- Application events
- Sysmon events where deployed

Linux systems should collect applicable:

- System logs
- Authentication logs
- Service logs
- Audit logs where deployed

## Elastic Agent

Elastic Agent should be deployed to systems that require centralized telemetry collection.

Agent deployment and management are documented in:

- `08-Elastic-Fleet-Deployment.md`
- `09-Windows-Agent.md`
- `10-Linux-Agent.md`

## Sysmon

Sysmon should be deployed to Windows endpoints where detailed endpoint logging and monitoring is required.

Sysmon deployment is documented in:

`11-Sysmon.md`

## Monitoring Validation

Logging and monitoring validation should confirm that:

- Expected events are generated
- Elastic Agent is healthy
- Events are collected
- Events are indexed
- Events are visible in Kibana
- Detection rules can access required telemetry

---

# 11. Certificate and Trust Configuration

## Certificate Trust

Servers requiring trusted internal certificates should be configured to trust the Enterprise Security Lab Certificate Authority where applicable.

The Enterprise CA and PKI architecture are documented in:

`05-Certificate-Authority-PKI.md`

## Linux Certificate Trust

Linux systems that require trust of the internal CA should have the appropriate CA certificate installed in the operating system trust store.

Certificate trust should be validated after installation.

## Windows Certificate Trust

Windows domain members should receive appropriate internal CA trust through Active Directory and Group Policy where applicable.

## Certificate Validation

Certificate validation should confirm:

- The certificate is trusted
- The certificate chain is valid
- The certificate is not expired
- The certificate subject and SAN values are correct
- The hostname matches the certificate
- Required services can establish trusted connections

---

# 12. Backup and Recovery

## Backup Requirements

Critical server configurations and data should be backed up according to their importance to the lab.

Backup considerations include:

- VM backup/export or snapshot-based recovery where appropriate.
- Configuration files
- Application data
- Detection rules
- Elastic configuration
- Sysmon configuration
- Infrastructure documentation

## Recovery Validation

Backups should be periodically tested to confirm that required systems and data can be restored.

Backup and recovery procedures are documented in:

`18-Backup-Recovery.md`

---

# 13. Validation and Testing

Each server build should be validated before being considered operational.

## Validation Status

| Validation Item                   | Status                     |
|-----------------------------------|----------------------------|
| Operating System Installed        | Validated                  |
| Hostname Configured               | Validated                  |
| IP Configuration Applied          | Validated                  |
| DNS Resolution                    | Validated                  |
| Time Synchronization              | Validated                  |
| Security Updates Applied          | Validated                  |
| Host Firewall Configured          | Validated                  |
| Certificate Trust Configured      | Validated where applicable |
| Required Server Roles Installed   | Validated                  |
| Elastic Agent Deployed            | Validated where applicable |
| Logging and Monitoring            | Validated where applicable |
| Server Functionality              | Validated                  |
| Asset Inventory Updated           | Validated                  |

## Validation Activities

Validation should confirm that:

1. The operating system is installed and supported.
2. The hostname follows the approved naming standard.
3. Network configuration is correct.
4. DNS resolution functions correctly.
5. Time synchronization is operational.
6. Required security updates are installed.
7. Host firewall configuration is appropriate.
8. Required certificates and trust relationships are functional.
9. Required server roles and applications are operational.
10. Logging and monitoring function as expected.
11. The system is documented in the asset inventory.

---

# 14. Troubleshooting

## DNS Resolution Failure

Verify:

- DNS server configuration
- Hostname configuration
- Forward DNS records
- Reverse DNS records where applicable
- Network connectivity to DNS servers

## Time Synchronization Failure

Verify:

- NTP or domain time configuration
- Network connectivity
- Firewall rules
- System time
- Time source availability

## Certificate Trust Failure

Verify:

- CA certificate is installed
- Certificate chain is complete
- Certificate is not expired
- Hostname matches certificate SAN
- System time is correct
- Required trust store has been updated

## Elastic Agent Connectivity Failure

Verify:

- Elastic Agent service is running
- Agent policy is assigned
- Fleet Server is reachable
- Required ports are accessible
- DNS resolution is working
- Certificates are trusted
- Agent enrollment is valid

## Server Build Validation Failure

If a server fails validation:

1. Identify the failed validation item.
2. Review the applicable configuration.
3. Correct the configuration.
4. Re-run validation.
5. Document significant issues in the lab journal.

---

# 15. Planned Enhancements

Planned improvements include:

- Formalize Windows Server build checklist
- Formalize Linux Server build checklist
- Automate baseline configuration
- Automate server naming and asset inventory updates
- Expand Group Policy security baselines
- Implement centralized configuration management
- Expand server hardening standards
- Implement configuration drift monitoring
- Automate certificate deployment
- Expand backup validation procedures
- Map server build standards to NIST Cybersecurity Framework controls
- Document configuration exceptions and risk acceptance

---

# 16. Related Documentation


| Document                          | Purpose                                                                                                                                                           |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| README.md                         | High-level overview of the Enterprise Security Lab, objectives, architecture, technologies, hardware inventory, capabilities, and documentation index.            |
| 01-Architecture.md                | Overall lab architecture, physical hardware, virtualization layout, server roles, infrastructure components, and system relationships.                            |
| 02-Network-Design.md              | Network architecture, IP addressing, DNS, communication flows, firewall requirements, segmentation, and network security considerations.                          |
| 03-Asset-Inventory.md             | Inventory of physical devices, VMs, operating systems, hostnames, IP addresses, and system roles/ownership.                                                       |
| 04-Active-Directory.md            | Active Directory architecture, OUs, users, groups, naming conventions, GPOs, authentication, and identity management.                                             |
| 05-Certificate-Authority-PKI.md   | Enterprise CA, certificate templates, trust relationships, certificate lifecycle, and PKI implementation.                                                         |
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
| 17-Investigation-Runbooks.md      | Step-by-step analyst procedures for investigating high-value alerts and detection scenarios.                                                                 |
| 18-Backup-Recovery.md             | Backup strategy, VM recovery, file restoration, disaster recovery, and recovery validation.                                                                       |
| 19-Security-Hardening.md          | Windows/Linux hardening, security baselines, auditing, logging, and defensive controls.                                                                           |
| 20-NIST-CSF-Mapping.md            | Maps lab capabilities to the NIST Cybersecurity Framework and demonstrates alignment with enterprise security practices.                                          |
| 99-Lab-Journal.md                 | Chronological implementation record, troubleshooting, design decisions, testing, snapshots, and future improvements.                                              |


---

# Revision History

| Version   | Date       | Changes                                                |
|---        |------------|--------------------------------------------------------|
| v0.1.0    | 2026-07-24 | Initial Server Build Standards documentation published |