# NIST Cybersecurity Framework Mapping

| Field						 | Value 						  		    				|
|-------------------|-------------------------------------------------------------------|
| Document Name 	| NIST Cybersecurity Framework Mapping 							|
| Document Version 	| v0.1.0 															|
| Author			| Terry Humphrey 													|
| Status 		 	| Active 															|
| Last Updated 		| 2026-07-30 														|

---

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. NIST Cybersecurity Framework Overview](#3-nist-cybersecurity-framework-overview)
- [4. Lab Cybersecurity Profile](#4-lab-cybersecurity-profile)
- [5. Govern](#5-govern)
- [6. Identify](#6-identify)
- [7. Protect](#7-protect)
- [8. Detect](#8-detect)
- [9. Respond](#9-respond)
- [10. Recover](#10-recover)
- [11. NIST CSF Function Mapping](#11-nist-csf-function-mapping)
- [12. Security Capability Mapping](#12-security-capability-mapping)
- [13. Control Evidence](#13-control-evidence)
- [14. Gaps and Limitations](#14-gaps-and-limitations)
- [15. Future Improvements](#15-future-improvements)
- [16. Mapping Maintenance](#16-mapping-maintenance)
- [17. Related Documentation](#17-related-documentation)
- [18. Revision History](#18-revision-history)

---

# 1. Purpose

## Overview

The Enterprise Security Lab NIST Cybersecurity Framework Mapping document maps the capabilities, technologies, processes, and security activities implemented within the lab environment to the NIST Cybersecurity Framework (NIST CSF).

The purpose of this document is to demonstrate how the Enterprise Security Lab applies cybersecurity concepts across governance, asset management, protection, detection, response, and recovery activities.

The mapping provides a structured view of the lab's cybersecurity capabilities and demonstrates how individual technical implementations support broader enterprise security objectives.

This document is intended to demonstrate practical application of cybersecurity principles rather than represent a formal organizational compliance assessment.

---

# 2. Scope

This document covers the mapping of Enterprise Security Lab capabilities to the NIST Cybersecurity Framework functions:

- Govern
- Identify
- Protect
- Detect
- Respond
- Recover

The mapping includes:

- Asset management
- Identity and access management
- Active Directory
- Certificate Authority and PKI
- Security hardening
- Vulnerability management
- Patch management
- Elastic SIEM
- Elastic Fleet
- Elastic Agent
- Sysmon
- Detection engineering
- Incident response
- Investigation runbooks
- Backup and recovery
- Security documentation
- Security validation

This document does not represent a formal NIST CSF certification or independent assessment.

---

# 3. NIST Cybersecurity Framework Overview

The NIST Cybersecurity Framework provides a structured approach for managing cybersecurity risk.

The Enterprise Security Lab maps its capabilities to the six NIST CSF Functions:

| Function  | Purpose                                                                                   |
|-----------|-------------------------------------------------------------------------------------------|
| Govern    | Establish and monitor cybersecurity strategy, expectations, policy, and risk management   |
| Identify  | Understand assets, risks, vulnerabilities, and organizational context                     |
| Protect   | Implement safeguards to reduce cybersecurity risk                                         |
| Detect    | Identify and analyze potential cybersecurity events                                       |
| Respond   | Take action regarding detected cybersecurity incidents                                    |
| Recover   | Restore capabilities and improve resilience after incidents                               |

The lab uses these Functions as a framework for organizing security capabilities and identifying areas for future development.

---

# 4. Lab Cybersecurity Profile

The Enterprise Security Lab represents a small-scale enterprise security environment designed to demonstrate practical cybersecurity operations.

## Core Security Capabilities

| Capability                | Technology / Process                          |
|---------------------------|-----------------------------------------------|
| Identity Management       | Active Directory                              |
| DNS                       | Active Directory Integrated DNS               |
| PKI                       | Microsoft Certificate Authority               |
| SIEM                      | Elastic Security                              |
| Log Management            | Elasticsearch                                 |
| Security Analytics        | Kibana                                        |
| Agent Management          | Elastic Fleet                                 |
| Endpoint Telemetry        | Elastic Agent                                 |
| Endpoint Monitoring       | Sysmon                                        |
| Detection Engineering     | 30 Custom Detection Rules                     |
| Vulnerability Management  | Vulnerability Scanning / Risk Prioritization  |
| Patch Management          | WSUS                                          |
| Incident Response         | Incident Response Procedures                  |
| Investigation             | Investigation Runbooks                        |
| Backup and Recovery       | VM / System / Configuration Backups           |
| Security Hardening        | Windows / Linux Security Baselines            |
| Asset Inventory           | Documented Asset Inventory                    |
| Security Documentation    | NIST / Markdown Documentation                 |
| Version Control           | Git / GitHub                                  |

---

# 5. Govern

The Govern Function establishes and monitors cybersecurity strategy, policies, expectations, and risk management.

## Lab Mapping

| NIST CSF Area             | Lab Capability                                                | Evidence                          |
|---------------------------|---------------------------------------------------------------|-----------------------------------|
| Cybersecurity Strategy    | Enterprise Security Lab architecture and security objectives  | 01-Architecture.md                |
| Cybersecurity Policy      | Documented security standards and procedures                  | Security documentation            |
| Risk Management           | Vulnerability and security risk prioritization                | 14-Vulnerability-Management.md    |
| Security Roles            | Defined administrative and analyst responsibilities           | Architecture / Runbooks           |
| Security Documentation    | Version-controlled security documentation                     | GitHub Repository                 |
| Security Metrics          | Security and recovery metrics                                 | Security documentation            |
| Exception Management      | Documented hardening exceptions                               | 19-Security-Hardening.md          |

## Govern Objectives

The lab's governance objectives include:

- Define cybersecurity responsibilities.
- Document security expectations.
- Maintain consistent security standards.
- Track cybersecurity risks.
- Document security exceptions.
- Measure security capabilities.
- Maintain security documentation.
- Periodically review the security architecture.

---

# 6. Identify

The Identify Function focuses on understanding assets, risks, vulnerabilities, and the environment.

## Asset Management

The lab maintains an inventory of physical systems, virtual machines, operating systems, hostnames, IP addresses, and system roles.

| Capability                | Implementation                        |
|---------------------------|---------------------------------------|
| Asset Inventory           | Documented system inventory           |
| Hardware Inventory        | Physical device documentation         |
| VM Inventory              | Virtualization documentation          |
| Network Identification    | IP addressing and topology            |
| System Roles              | Server and endpoint role assignments  |
| Ownership                 | Documented system ownership           |
| Criticality               | System priority classification        |

## Risk Assessment

Security risks are identified through:

- Vulnerability scanning
- Security configuration reviews
- Detection engineering
- Attack simulation
- Log analysis
- Incident investigations
- Patch status
- Security hardening reviews

## Vulnerability Management

The lab identifies and prioritizes vulnerabilities based on factors such as:

- Severity
- Exploitability
- System criticality
- Exposure
- Potential impact
- Availability of remediation

Vulnerability findings are tracked through the vulnerability management process.

---

# 7. Protect

The Protect Function focuses on implementing safeguards to reduce cybersecurity risk.

## Identity and Access Management

Active Directory provides centralized identity and access management.

Security controls include:

- User accounts
- Security groups
- Organizational Units
- Group Policy
- Administrative separation
- Least privilege
- Password policies
- Account lockout policies

## Data Security

Security-sensitive data is protected through:

- Access controls
- File permissions
- Repository controls
- Backup procedures
- Credential protection
- Certificate management

## Platform Security

Windows and Linux systems are hardened using documented security baselines.

Hardening activities include:

- Firewall configuration
- Service minimization
- Secure authentication
- Restricted administrative access
- Security logging
- Endpoint protection
- Patch management

## Technology Protection

Security technologies include:

- Microsoft Defender
- Windows Firewall
- Linux firewall controls
- Sysmon
- Elastic Agent
- Elastic Fleet
- Elastic Security
- Certificate Authority

## Training and Awareness

The lab's training objective is to demonstrate practical cybersecurity knowledge through hands-on implementation.

Activities include:

- Detection engineering
- Security testing
- Attack simulation
- Incident investigation
- Vulnerability remediation
- Security hardening
- Recovery testing

---

# 8. Detect

The Detect Function focuses on identifying and analyzing potential cybersecurity events.

## Continuous Monitoring

Elastic Security provides centralized security monitoring.

The environment collects telemetry from:

- Windows endpoints
- Windows Servers
- Sysmon
- Linux systems
- Elastic Agents

## Detection Engineering

The lab currently includes 30 custom detection rules mapped against MITRE ATT&CK techniques and enterprise attack scenarios.

Each detection includes:

- Severity
- Risk score
- MITRE ATT&CK mapping
- KQL logic
- Validation procedures
- False-positive considerations
- Investigation guidance

Detection rules are designed to identify suspicious activity such as:

- PowerShell abuse
- Office application abuse
- Suspicious process execution
- Credential access
- LSASS memory access
- Persistence mechanisms
- Scheduled task creation
- Suspicious network activity
- Defense evasion
- Privilege escalation

## Detection Validation

Detection rules are validated through controlled testing and attack simulation.

Validation activities confirm:

- Expected logging is generated.
- Events are ingested by Elastic.
- KQL queries identify relevant events.
- Detection rules trigger as expected.
- Alerts are generated.
- Analysts can investigate resulting alerts.

## Security Monitoring

The lab monitors:

- Process creation
- Process termination
- Authentication
- Network connections
- Privilege changes
- Persistence
- Configuration changes
- Security control changes

---

# 9. Respond

The Respond Function focuses on taking action after a cybersecurity event is detected.

## Incident Response

The lab maintains documented incident response procedures covering:

- Preparation
- Detection
- Analysis
- Containment
- Eradication
- Recovery
- Lessons learned

## Alert Triage

Security alerts are evaluated based on:

- Severity
- Risk score
- Detection confidence
- Affected system
- Affected user
- Observed behavior
- Potential impact

## Investigation

Investigation runbooks provide repeatable procedures for analyzing high-value security alerts.

Investigations may include:

- Reviewing process execution
- Reviewing parent-child process relationships
- Examining command lines
- Reviewing user activity
- Reviewing network activity
- Reviewing persistence mechanisms
- Identifying affected systems
- Determining scope

## Containment

Potential containment actions include:

- Isolating affected systems.
- Disabling compromised accounts.
- Blocking malicious network communication.
- Removing persistence.
- Stopping malicious processes.

Because this is a home laboratory environment, containment actions are performed only against designated lab systems.

---

# 10. Recover

The Recover Function focuses on restoring systems and improving resilience after cybersecurity incidents.

## Recovery Capabilities

Recovery capabilities include:

- VM backups
- System State backups
- Configuration backups
- Git repositories
- Documentation backups
- Elastic recovery procedures
- Active Directory recovery procedures
- Certificate Authority recovery procedures

## Recovery Prioritization

Systems are recovered according to their dependency relationships.

The general recovery priority is:

1. Network connectivity
2. DNS
3. Active Directory
4. Certificate Authority
5. Elastic Stack
6. Fleet Server
7. Elastic Agents
8. Security telemetry
9. WSUS
10. Workstations
11. Attack simulation systems

## Recovery Validation

Recovery is validated by confirming:

- Services are operational.
- DNS resolution works.
- Authentication works.
- Certificates are trusted.
- Elastic telemetry is ingested.
- Detection rules function.
- Security monitoring is restored.

## Lessons Learned

Recovery activities should be reviewed to identify:

- What failed.
- Why it failed.
- How long recovery required.
- Whether backups were sufficient.
- Whether recovery procedures were accurate.
- What improvements are required.

---

# 11. NIST CSF Function Mapping

The following table provides a high-level mapping of major lab capabilities to the NIST Cybersecurity Framework Functions.

| NIST CSF Function | Category                          | Lab Capability                            | Evidence                          |
|-------------------|-----------------------------------|-------------------------------------------|-----------------------------------|
| Govern            | Organizational Context            | Defined lab architecture and objectives   | 01-Architecture.md                |
| Govern            | Risk Management Strategy          | Vulnerability and risk prioritization     | 14-Vulnerability-Management.md    |
| Govern            | Policy                            | Documented security standards             | Security Documentation            |
| Govern            | Oversight                         | Security metrics and reviews              | Security Documentation            |
| Identify          | Asset Management                  | Asset inventory                           | 03-Asset-Inventory.md             |
| Identify          | Risk Assessment                   | Vulnerability scanning and risk review    | 14-Vulnerability-Management.md    |
| Identify          | Improvement                       | Lessons learned and security reviews      | 16-Incident-Response.md           |
| Protect           | Identity Management               | Active Directory                          | 04-Active-Directory.md            |
| Protect           | Access Control                    | Users, groups, GPOs, least privilege      | 04-Active-Directory.md            |
| Protect           | Data Security                     | Access controls and backups               | 18-Backup-Recovery.md             |
| Protect           | Platform Security                 | Windows/Linux hardening                   | 19-Security-Hardening.md          |
| Protect           | Technology Infrastructure         | Firewalls and secure services             | 06-Server-Build-Standards.md      |
| Detect            | Continuous Monitoring             | Elastic Security                          | 12-Elastic-Security.md            |
| Detect            | Adverse Event Analysis            | Detection rules and alert analysis        | 13-Detection-Rules.md             |
| Detect            | Detection Processes               | Detection engineering                     | 13-Detection-Rules.md             |
| Respond           | Incident Management               | Incident response lifecycle               | 16-Incident-Response.md           |
| Respond           | Incident Analysis                 | Investigation procedures                  | 17-Investigation-Runbooks.md      |
| Respond           | Incident Reporting                | Security documentation and cases          | 12-Elastic-Security.md            |
| Respond           | Incident Mitigation               | Containment and eradication               | 16-Incident-Response.md           |
| Recover           | Incident Recovery Plan Execution  | Recovery procedures                       | 18-Backup-Recovery.md             |
| Recover           | Incident Recovery Communication   | Recovery documentation                    | 18-Backup-Recovery.md             |
| Recover           | Incident Recovery Improvements    | Lessons learned                           | 16-Incident-Response.md           |

This mapping is a high-level capability mapping and does not constitute a formal assessment against every NIST CSF Subcategory.

> **Note:** This mapping demonstrates capability alignment within the Enterprise Security Lab and should not be interpreted as formal NIST CSF compliance.

---

# 12. Security Capability Mapping

The following table maps major Enterprise Security Lab capabilities to the NIST CSF Functions they support.

| Capability                    | Govern | Identify | Protect | Detect | Respond | Recover |
| ------------------------------|--------|----------|---------|--------|---------|---------|
| Active Directory              |   X    |    X     |    X    |   X    |    X    |    X    |
| Certificate Authority / PKI   |   X    |    X     |    X    |   X    |    X    |    X    |
| Asset Inventory               |   X    |    X     |         |        |         |    X    |
| Vulnerability Management      |   X    |    X     |    X    |   X    |    X    |    X    |
| Patch Management              |   X    |    X     |    X    |        |         |    X    |
| Security Hardening            |   X    |    X     |    X    |   X    |         |    X    |
| Elastic SIEM                  |        |          |         |   X    |    X    |    X    |
| Elastic Fleet                 |        |    X     |    X    |   X    |         |    X    |
| Elastic Agent                 |        |    X     |    X    |   X    |         |    X    |
| Sysmon                        |        |          |    X    |   X    |         |         |
| Detection Engineering         |        |    X     |    X    |   X    |    X    |         |
| Incident Response             |   X    |          |         |   X    |    X    |    X    |
| Investigation Runbooks        |        |          |         |   X    |    X    |         |
| Backup and Recovery           |   X    |          |    X    |        |         |    X    |
| Security Documentation        |   X    |    X     |    X    |   X    |    X    |    X    |
| Git / Version Control         |   X    |    X     |    X    |        |         |    X    |

**Legend**

| Function | Description                                        |
|----------|----------------------------------------------------|
| Govern   | Governance, policy, risk management, and oversight |
| Identify | Asset management and understanding the environment |
| Protect  | Safeguards and preventive controls                 |
| Detect   | Monitoring, logging, and threat detection          |
| Respond  | Investigation and incident handling                |
| Recover  | Restoration and resilience capabilities            |

---

# 13. Control Evidence

The Enterprise Security Lab maintains documentation and technical evidence supporting the implementation of cybersecurity capabilities.

## Evidence Sources

Evidence may include:

- Configuration files
- Git commits
- Detection rules
- Elastic alerts
- Kibana dashboards
- Sysmon events
- Windows Event Logs
- Linux logs
- Vulnerability scan results
- Patch compliance reports
- Backup records
- Recovery test results
- Security hardening reviews
- Incident investigations
- Lab journal entries

## Evidence Management

Security evidence should be:

- Documented.
- Version controlled where practical.
- Traceable to the relevant security capability.
- Protected from unauthorized modification.
- Retained long enough to demonstrate implementation history.

---

# 14. Gaps and Limitations

The Enterprise Security Lab is a continuously developing environment and does not currently implement every capability associated with a mature enterprise cybersecurity program.

Known limitations may include:

- Limited network segmentation.
- Limited physical infrastructure redundancy.
- Limited backup infrastructure.
- No dedicated security operations team.
- No formal security governance organization.
- Limited automated compliance monitoring.
- Limited automated vulnerability scanning.
- Limited identity lifecycle automation.
- No enterprise-scale privileged access management solution.
- Limited centralized configuration management.
- Limited disaster recovery infrastructure.
- Limited high-availability architecture.

These limitations are recognized as areas for future improvement.

The lab should not be represented as a fully compliant implementation of the NIST Cybersecurity Framework.

## Current Maturity Assessment

The Enterprise Security Lab demonstrates practical implementation of enterprise security concepts across all six NIST Cybersecurity Framework Functions; however, several controls remain partially implemented or intentionally out of scope due to the laboratory nature of the environment.

The lab should be viewed as a cybersecurity engineering and detection platform rather than a production-ready enterprise deployment.

---

# 15. Future Improvements

Future improvements may include:

- Implement dedicated network segmentation.
- Expand vulnerability scanning.
- Implement automated vulnerability remediation tracking.
- Expand patch compliance reporting.
- Implement automated configuration compliance.
- Expand security hardening baselines.
- Implement additional detection coverage.
- Expand incident response automation.
- Add additional investigation runbooks.
- Implement automated backup validation.
- Conduct regular recovery exercises.
- Expand security metrics.
- Map capabilities to NIST CSF Subcategories.
- Map applicable controls to NIST SP 800-171.
- Implement formal risk registers.
- Implement security control ownership.
- Develop a formal Cybersecurity Profile.
- Add additional endpoints and servers.
- Expand attack simulation scenarios.
- Improve network isolation.

---

# 16. Mapping Maintenance

This document should be updated as the Enterprise Security Lab evolves.

The NIST CSF mapping should be reviewed when:

- New systems are deployed.
- New security technologies are implemented.
- Detection capabilities are added.
- Incident response procedures change.
- New security controls are implemented.
- Vulnerability management processes change.
- Patch management processes change.
- Backup and recovery capabilities change.
- Security architecture changes.

## Mapping Review

| Review ID  | Review Date | Reviewer       | Changes    | Status   |
|------------|-------------|----------------|------------|----------|
| NISTRW-001 | 2026-07-30  | Terry Humphrey | First Pass | Complete |

---

# 17. Related Documentation

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
| 19-Security-Hardening.md          | Windows/Linux hardening, security baselines, auditing, logging, and defensive controls.                                                                           |
| 99-Lab-Journal.md                 | Chronological implementation record, troubleshooting, design decisions, testing, snapshots, and future improvements.                                              |

---

# 18. Revision History

| Version 	| Date 		 | Changes 									    	    |
|-----------|------------|------------------------------------------------------|
| v0.1.0    | 2026-07-30 | Initial NIST CSF Mapping document created           |

---