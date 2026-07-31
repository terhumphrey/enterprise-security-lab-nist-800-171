# Enterprise Security Lab Investigation Runbooks

| Field             | Value                    |
|-------------------|--------------------------|
| Document Name     | Investigation Runbooks   |
| Document Version  | v0.1.0                   |
| Author            | Terry Humphrey           |
| Status            | Active                   |
| Last Updated      | 2026-07-30               |

---

## Table of Contents

- [1. Purpose](#1-purpose)
- [2. Scope](#2-scope)
- [3. Investigation Overview](#3-investigation-overview)
- [4. Investigation Workflow](#4-investigation-workflow)
- [5. Alert Triage Runbook](#5-alert-triage-runbook)
- [6. Authentication Investigation Runbook](#6-authentication-investigation-runbook)
- [7. PowerShell Execution Investigation Runbook](#7-powershell-execution-investigation-runbook)
- [8. Office Application Spawning PowerShell Runbook](#8-office-application-spawning-powershell-runbook)
- [9. LSASS Process Access Runbook](#9-lsass-process-access-runbook)
- [10. Persistence Investigation Runbook](#10-persistence-investigation-runbook)
- [11. Suspicious Network Activity Runbook](#11-suspicious-network-activity-runbook)
- [12. Discovery Activity Investigation Runbook](#12-discovery-activity-investigation-runbook)
- [13. Remote Access Investigation Runbook](#13-remote-access-investigation-runbook)
- [14. Security Control Change Investigation Runbook](#14-security-control-change-investigation-runbook)
- [15. Investigation Documentation](#15-investigation-documentation)
- [16. Detection Rule Mapping](#16-detection-rule-mapping)
- [17. Validation and Testing](#17-validation-and-testing)
- [18. Troubleshooting](#18-troubleshooting)
- [19. Planned Enhancements](#19-planned-enhancements)
- [20. Related Documentation](#20-related-documentation)

---

# 1. Purpose

## Overview

This document defines standardized investigation procedures for security alerts and suspicious activity identified within the Enterprise Security Lab.

The runbooks provide repeatable procedures for:

- Alert triage
- Security event investigation
- Endpoint activity analysis
- Authentication investigation
- Process investigation
- Network activity investigation
- Persistence investigation
- Detection validation
- Investigation documentation

The runbooks are designed to support consistent analyst workflows and demonstrate practical security operations capabilities.

---

# 2. Scope

This document covers:

- Investigation workflow
- Alert triage
- Windows endpoint investigations
- Sysmon-based investigations
- PowerShell investigations
- Authentication investigations
- Process access investigations
- Persistence investigations
- Network connection investigations
- Discovery activity investigations
- Remote access investigations
- Investigation documentation
- Validation and testing

This document does not cover:

- Incident response procedures
- Elasticsearch deployment
- Kibana deployment
- Fleet Server deployment
- Elastic Agent deployment
- Sysmon installation and configuration
- Detection rule development

Those topics are documented separately.

---

# 3. Investigation Overview

Security investigations begin when a detection rule generates an alert or when suspicious activity is identified through security telemetry.

Investigation activities should establish:

- What occurred
- When it occurred
- Which system was affected
- Which user account was involved
- Which process or application initiated the activity
- Whether the activity was expected
- Whether additional suspicious activity occurred
- Whether the activity represents a security incident

Investigation results should be documented consistently and linked to the applicable alert or case.

---

# 4. Investigation Workflow

## Standard Investigation Flow

The standard investigation workflow consists of:

1. Alert generation
2. Initial triage
3. Alert validation
4. Evidence collection
5. Scope determination
6. Activity classification
7. Documentation
8. Closure or escalation

## Investigation Phases

### Phase 1: Initial Triage

Determine:

- Alert name
- Detection rule
- Alert severity
- Risk score
- Event timestamp
- Affected host
- User account
- Source IP
- Destination IP
- Process name
- Parent process
- Command line
- Related telemetry source

### Phase 2: Alert Validation

Determine whether:

- The alert was generated from expected telemetry.
- The triggering event exists.
- Required event fields are populated.
- Detection logic matched the intended activity.
- The activity appears expected or suspicious.

### Phase 3: Evidence Collection

Collect relevant evidence including:

- Security events
- Sysmon events
- Process creation events
- Authentication activity
- Network activity
- File activity
- Registry activity
- Persistence indicators
- Related alerts

### Phase 4: Scope Determination

Determine:

- Affected endpoint(s)
- Affected account(s)
- Related processes
- Related files
- Related IP addresses
- Related domains
- Additional impacted systems

### Phase 5: Activity Classification

Classify activity as:

- Benign
- Expected administrative activity
- False positive
- Suspicious activity
- Confirmed malicious activity
- Requires incident response

### Phase 6: Documentation

Record:

- Investigation summary
- Detection rule
- Evidence reviewed
- Findings
- Classification
- Recommended actions
- Related alerts or cases


## Escalation Criteria

Escalation should be considered when investigations identify:

- Confirmed malicious execution
- Credential theft indicators
- Unauthorized privilege escalation
- Unauthorized persistence mechanisms
- Defense evasion activity
- Command and control communication
- Data exfiltration indicators
- Multiple affected systems

---

# 5. Alert Triage Runbook

## Objective

Determine whether a newly generated security alert represents expected activity, a false positive, suspicious behavior, or a potential security incident.

## Applicable Detection Rules

This runbook applies to all detection rules, including:

- Authentication detections
- PowerShell detections
- Process detections
- Persistence detections
- Network detections
- Discovery detections
- Credential access detections

## Procedure

### Step 1: Review Alert

Record:

- Alert name
- Rule ID
- Severity
- Risk score
- Timestamp
- Hostname
- User account
- Source address
- Destination address
- Process information

### Step 2: Review Triggering Event

Identify:

- Event ID
- Event timestamp
- Process name
- Process ID
- Parent process
- Parent process ID
- Command line
- User
- Hostname

### Step 3: Review Process Context

Determine:

- Process origin
- Parent-child relationship
- Execution path
- User context
- Whether execution is expected

### Step 4: Review Related Telemetry

Search for:

- Earlier related activity
- Later related activity
- Authentication events
- File creation
- Registry changes
- Network connections
- Additional alerts

### Step 5: Determine Classification

Classify the alert as:

- Benign
- False positive
- Suspicious
- Malicious
- Escalation required

---

# 6. Authentication Investigation Runbook

## Objective

Investigate suspicious authentication activity including failed logons, successful logons after failures, and abnormal account behavior.

## Applicable Detection Rules

- DET-HIGH-WIN-ACCOUNT-001 - Multiple Failed Logons Followed by Success
- DET-MED-WIN-ACCOUNT-001 - Suspicious Failed Logon Activity

## Investigation Procedure

### Step 1: Identify Authentication Activity

Record:

- Username
- Source host
- Destination host
- Authentication type
- Logon type
- Timestamp
- Success or failure status

### Step 2: Analyze Failed Logons

Review:

- Number of failed attempts
- Time period
- Source system
- Target account
- Geographic or network origin

### Step 3: Review Successful Authentication

Determine:

- Whether success occurred after failed attempts
- Whether the account normally accesses the system
- Whether the source system is expected

### Step 4: Review Related Activity

Search for:

- Privilege changes
- Process execution
- Remote access activity
- Account modifications
- Additional authentication events

### Step 5: Classification

Classify as:

- Expected user activity
- Administrative activity
- User error
- Password spraying indicator
- Brute-force indicator
- Potential account compromise

# 7. PowerShell Execution Investigation Runbook

## Objective

Investigate potentially suspicious PowerShell execution and determine whether the activity is legitimate administrative activity or malicious execution.

## Applicable Detection Rules

- DET-HIGH-WIN-POWERSHELL-001 - Encoded Command Execution
- DET-HIGH-WIN-POWERSHELL-002 - PowerShell Execution Policy Bypass
- DET-MED-WIN-POWERSHELL-001 - PowerShell File Download Activity
- DET-MED-WIN-POWERSHELL-002 - PowerShell Execution Policy Bypass
- DET-MED-WIN-POWERSHELL-003 - PowerShell Web Download Activity
- DET-MED-WIN-SYSMON-002 - Suspicious PowerShell Parent Process
- DET-MED-WIN-SYSMON-004 - Network Connection from PowerShell
- DET-MED-WIN-SYSMON-006 - Suspicious Script Interpreter Execution

## Primary Telemetry

Review:

- Sysmon process creation events
- PowerShell operational logs
- Security events
- Network connection events
- File creation events
- Elastic Security alerts

## Investigation Procedure

### Step 1: Identify PowerShell Execution

Record:

- Hostname
- Username
- Process name
- Process ID
- Parent process
- Parent process ID
- Command line
- Timestamp

### Step 2: Analyze Command Line

Review for:

- Encoded commands
- Obfuscated commands
- Execution policy bypass parameters
- Download activity
- Remote execution
- Script execution
- Suspicious URLs
- IP addresses
- Base64 content

Common suspicious indicators:

- `-enc`
- `-encodedcommand`
- `-nop`
- `-noprofile`
- `-executionpolicy bypass`
- `IEX`
- Web download commands

### Step 3: Analyze Parent Process

Determine whether PowerShell was launched by:

- `explorer.exe`
- `cmd.exe`
- Office applications
- Browser processes
- Scheduled task processes
- Services
- Unknown processes

### Step 4: Review Network Activity

Search for connections occurring near PowerShell execution.

Review:

- Destination IP
- Destination port
- Domain
- Protocol
- Connection timing
- Related DNS activity

### Step 5: Review File Activity

Investigate files created or modified around execution time.

Review:

- File path
- File name
- File extension
- Creating process
- File hash where available

### Step 6: Determine Classification

Classify activity as:

- Legitimate administration
- Expected automation
- User activity
- Suspicious execution
- Malware execution
- Requires escalation


---

# 8. Office Application Spawning PowerShell Runbook

## Objective

Investigate Office applications spawning PowerShell or command interpreters.

This behavior may indicate malicious document execution, macro abuse, or exploitation.

## Applicable Detection Rules

- DET-HIGH-WIN-OFFICE-001 - Office Application Spawning PowerShell

## Investigation Procedure

### Step 1: Identify Parent Application

Determine whether the parent process is:

- `WINWORD.EXE`
- `EXCEL.EXE`
- `POWERPNT.EXE`
- Other Office applications

Record:

- Process ID
- Parent process
- User account
- Hostname
- Timestamp

### Step 2: Identify Child Process

Record:

- Child process name
- Process ID
- Command line
- Execution path

### Step 3: Review Process Chain

Analyze:

- Office application
- PowerShell or command interpreter
- Additional child processes
- Execution sequence

Determine whether the chain is expected.

### Step 4: Review File Activity

Investigate:

- Document location
- Downloads folder
- Temporary files
- Scripts
- Executables
- File hashes

### Step 5: Review Network Activity

Search for:

- Network connections from Office applications
- Network connections from PowerShell
- Suspicious destinations
- External communication

### Step 6: Determine Classification

Classify activity as:

- Expected user activity
- Administrative activity
- Suspicious document execution
- Potential malware execution


---

# 9. LSASS Process Access Runbook

## Objective

Investigate suspicious access to the Local Security Authority Subsystem Service (`lsass.exe`).

Unauthorized LSASS access may indicate credential dumping attempts.

## Applicable Detection Rules

- DET-HIGH-WIN-SYSMON-001 - LSASS Memory Access Attempt
- DET-MED-WIN-CREDENTIAL-001 - Suspicious Credential Access Attempt

## Investigation Procedure

### Step 1: Identify Source Process

Record:

- Source process
- Source process path
- Source process hash
- Source process ID
- User account
- Hostname
- Timestamp

### Step 2: Confirm Target Process

Verify:

- Target process is `lsass.exe`
- Target process ID
- Access timestamp
- Access type

### Step 3: Analyze Source Process

Determine whether the source process is:

- Security software
- Administrative tooling
- Known operating system process
- Unknown executable
- Unsigned executable
- Suspicious process

### Step 4: Review Process Ancestry

Analyze:

- Parent process
- Grandparent process
- Command line
- User context

### Step 5: Review Related Activity

Search for:

- Process creation
- File creation
- Network connections
- Credential access indicators
- Additional alerts

### Step 6: Determine Classification

Classify as:

- Expected security software behavior
- Legitimate administrative activity
- Suspicious credential access
- Potential credential theft


---

# 10. Persistence Investigation Runbook

## Objective

Investigate mechanisms that may establish unauthorized persistence.

## Applicable Detection Rules

- DET-MED-WIN-PERSISTENCE-001 - Scheduled Task Created
- DET-MED-WIN-PERSISTENCE-002 - Registry Run Key Added
- DET-MED-WIN-SYSMON-003 - Suspicious File Creation in User Temp Directory

## Investigation Areas

Persistence investigations should review:

- Scheduled tasks
- Registry Run keys
- Startup locations
- Newly created executables
- Scripts
- Services
- User profile locations

## Scheduled Task Investigation

### Step 1: Identify Task

Record:

- Task name
- Task path
- Creation time
- Modification time
- Creating user

### Step 2: Identify Execution Action

Review:

- Executable
- Script
- PowerShell command
- Arguments
- File location

### Step 3: Review Creator Activity

Identify:

- Creating process
- Parent process
- User account
- Hostname

### Step 4: Review Associated Files

Search for:

- Newly created files
- Modified files
- Hash information
- File locations

## Registry Run Key Investigation

Review:

- Registry path
- Registry value
- Executable path
- Creating process
- User context
- Related file activity
- Review common persistence locations:
  - HKCU\Software\Microsoft\Windows\CurrentVersion\Run
  - HKLM\Software\Microsoft\Windows\CurrentVersion\Run
  - Startup folder locations

## Classification

Classify persistence activity as:

- Legitimate software behavior
- Administrative configuration
- Suspicious persistence
- Confirmed malicious persistence

# 11. Suspicious Network Activity Runbook

## Objective

Investigate potentially suspicious inbound or outbound network activity and determine whether the communication is expected, suspicious, or malicious.

## Applicable Detection Rules

- DET-HIGH-WIN-NETWORK-001 - Suspicious Outbound Connection
- DET-MED-WIN-NETWORK-001 - Suspicious RDP Connection
- DET-MED-WIN-NETWORK-002 - PowerShell Remoting Activity
- DET-MED-WIN-NETWORK-003 - SMB Connection Activity
- DET-MED-WIN-NETWORK-004 - Remote Service Execution
- DET-MED-WIN-NETWORK-005 - Internal Port Scanning Activity
- DET-MED-WIN-SYSMON-004 - Network Connection from PowerShell

## Investigation Procedure

### Step 1: Identify Network Connection

Record:

- Source host
- Source IP address
- Destination host
- Destination IP address
- Destination port
- Protocol
- Timestamp

### Step 2: Identify Associated Process

Determine:

- Process name
- Process path
- Process ID
- Parent process
- User account
- Command line

### Step 3: Investigate Destination

Review:

- Destination IP
- Domain name
- Port usage
- Known service
- Expected business purpose
- Internal versus external destination

### Step 4: Review Process Context

Determine whether the connection originated from:

- Browser
- PowerShell
- Command interpreter
- Office application
- System service
- Administrative tooling
- Unknown executable

### Step 5: Review Related Activity

Search for:

- Process creation events
- File creation events
- DNS queries
- Authentication events
- Additional network activity
- Related alerts

### Step 6: Determine Classification

Classify activity as:

- Expected communication
- Legitimate administrative activity
- Suspicious network activity
- Potential command and control
- Potential data exfiltration


---

# 12. Discovery Activity Investigation Runbook

## Objective

Investigate suspicious discovery commands used to identify systems, users, accounts, or network resources.

## Applicable Detection Rules

- DET-MED-WIN-DISCOVERY-001 - System Discovery Commands
- DET-MED-WIN-DISCOVERY-002 - Account Discovery Commands

## Investigation Procedure

### Step 1: Identify Discovery Command

Record:

- Command executed
- Process name
- Parent process
- User account
- Hostname
- Timestamp

Examples include:

- `whoami`
- `hostname`
- `ipconfig`
- `systeminfo`
- `net user`
- `net group`
- `nltest`
- `netstat`

### Step 2: Analyze Execution Context

Determine:

- How the command was launched
- Whether execution was interactive
- Whether execution came from PowerShell or another interpreter
- Whether the user normally performs this activity

### Step 3: Review Related Activity

Search for:

- Additional discovery commands
- Process execution
- Network connections
- Credential access activity
- Persistence indicators

### Step 4: Determine Scope

Identify:

- User account involved
- Endpoint affected
- Commands executed
- Additional systems accessed

### Step 5: Determine Classification

Classify as:

- Normal administrative activity
- Security testing activity
- User troubleshooting
- Suspicious discovery behavior
- Potential attacker reconnaissance


---

# 13. Remote Access Investigation Runbook

## Objective

Investigate suspicious remote access activity including RDP, SMB, PowerShell remoting, and remote service execution.

## Applicable Detection Rules

- DET-MED-WIN-NETWORK-001 - Suspicious RDP Connection
- DET-MED-WIN-NETWORK-002 - PowerShell Remoting Activity
- DET-MED-WIN-NETWORK-003 - SMB Connection Activity
- DET-MED-WIN-NETWORK-004 - Remote Service Execution

## Investigation Procedure

### Step 1: Identify Remote Access Activity

Record:

- Source system
- Destination system
- User account
- Authentication method
- Timestamp
- Protocol
- Port

### Step 2: Validate User Activity

Determine:

- Whether the user normally accesses the system
- Whether access time is expected
- Whether the source device is authorized

### Step 3: Review Authentication Events

Review:

- Successful logons
- Failed logons
- Privileged account usage
- Logon type
- Authentication failures

### Step 4: Review Remote Execution

Investigate:

- Remote processes
- Services created
- Scheduled tasks
- File transfers
- Administrative commands

### Step 5: Determine Classification

Classify as:

- Expected administration
- Approved remote access
- Suspicious lateral movement
- Potential compromise


---

# 14. Security Control Change Investigation Runbook

## Objective

Investigate changes to security controls that may indicate defense evasion activity.

## Applicable Detection Rules

- DET-HIGH-WIN-DEFENSE-001 - Windows Event Logs Cleared
- DET-HIGH-WIN-DEFENSE-002 - Windows Defender Disabled

## Investigation Procedure

### Step 1: Identify Security Control Change

Record:

- Hostname
- User account
- Timestamp
- Event ID
- Process responsible
- Command line
- Parent process

### Step 2: Analyze Event Log Activity

Review:

- Log cleared
- Logs affected
- User performing action
- Associated process activity

### Step 3: Analyze Defender Changes

Review:

- Defender configuration changes
- Protection settings modified
- User context
- Process responsible

### Step 4: Review Related Activity

Search for:

- PowerShell execution
- Administrative commands
- Process creation
- Persistence activity
- Credential access indicators

### Step 5: Determine Classification

Classify activity as:

- Approved administrative change
- Security testing activity
- Suspicious defense evasion
- Confirmed malicious activity

---

# 15. Investigation Documentation

## Documentation Requirements

Each significant investigation should document:

- Alert name
- Detection rule ID
- Date and time
- Hostname
- User account
- Investigation scope
- Evidence reviewed
- Findings
- Classification
- Recommended actions

## Investigation Summary

Investigations should include a concise summary describing:

- What triggered the investigation
- What activity was observed
- What evidence was reviewed
- Whether the activity was expected
- Whether additional investigation is required

## Evidence Collection

Evidence may include:

- Event IDs
- Timestamps
- Process names
- Process IDs
- Command lines
- File paths
- File hashes
- IP addresses
- Domains
- User accounts
- Related alerts

Sensitive information should be handled according to the lab's security and documentation requirements.


---

# 16. Detection Rule Mapping

The following detection rules are mapped to their applicable MITRE ATT&CK techniques and investigation procedures. This mapping provides traceability between detection engineering, security telemetry, analyst workflows, and investigation response procedures.

| Detection Rule              | MITRE ATT&CK Technique                                                    | Investigation Runbook                                 |
|-----------------------------|---------------------------------------------------------------------------|-------------------------------------------------------|
| DET-HIGH-WIN-ACCOUNT-001    | T1110 - Brute Force; T1078 - Valid Accounts                               | Authentication Investigation                          |
| DET-HIGH-WIN-DEFENSE-001    | T1070.001 - Clear Windows Event Logs                                      | Alert Triage / Security Control Change Investigation  |
| DET-HIGH-WIN-DEFENSE-002    | T1562.001 - Impair Defenses: Disable or Modify Tools                      | Alert Triage / Security Control Change Investigation  |
| DET-HIGH-WIN-LOLBIN-001     | T1218 - System Binary Proxy Execution                                     | Alert Triage / Process Investigation                  |
| DET-HIGH-WIN-NETWORK-001    | T1071 - Application Layer Protocol; T1105 - Ingress Tool Transfer         | Suspicious Network Activity                           |
| DET-HIGH-WIN-OFFICE-001     | T1204.002 - User Execution: Malicious File; T1059.001 - PowerShell        | Office Application Spawning PowerShell                |
| DET-HIGH-WIN-POWERSHELL-001 | T1059.001 - PowerShell                                                    | PowerShell Execution Investigation                    |
| DET-HIGH-WIN-POWERSHELL-002 | T1059.001 - PowerShell; T1562.001 - Impair Defenses                       | PowerShell Execution Investigation                    |
| DET-HIGH-WIN-PROCESS-001    | T1059 - Command and Scripting Interpreter                                 | Alert Triage / Process Analysis                       |
| DET-HIGH-WIN-SYSMON-001     | T1003.001 - OS Credential Dumping: LSASS Memory                           | LSASS Process Access                                  |
| DET-HIGH-WIN-SYSMON-002     | T1204.002 - User Execution: Malicious File; T1105 - Ingress Tool Transfer | Alert Triage / Process Analysis                       |
| DET-MED-WIN-ACCOUNT-001     | T1110 - Brute Force                                                       | Authentication Investigation                          |
| DET-MED-WIN-CREDENTIAL-001  | T1003 - OS Credential Dumping                                             | LSASS / Credential Access Investigation               |
| DET-MED-WIN-DISCOVERY-001   | T1082 - System Information Discovery                                      | Discovery Investigation                               |
| DET-MED-WIN-DISCOVERY-002   | T1087 - Account Discovery                                                 | Discovery Investigation                               |
| DET-MED-WIN-NETWORK-001     | T1021.001 - Remote Services: RDP                                          | Remote Access Investigation                           |
| DET-MED-WIN-NETWORK-002     | T1021.006 - Remote Services: Windows Remote Management                    | Remote Access Investigation                           |
| DET-MED-WIN-NETWORK-003     | T1021.002 - Remote Services: SMB/Windows Admin Shares                     | Remote Access Investigation                           |
| DET-MED-WIN-NETWORK-004     | T1021 - Remote Services                                                   | Remote Access Investigation                           |
| DET-MED-WIN-NETWORK-005     | T1046 - Network Service Scanning                                          | Network Activity Investigation                        |
| DET-MED-WIN-PERSISTENCE-001 | T1053.005 - Scheduled Task/Job: Scheduled Task                            | Persistence Investigation                             |
| DET-MED-WIN-PERSISTENCE-002 | T1547.001 - Registry Run Keys / Startup Folder                            | Persistence Investigation                             |
| DET-MED-WIN-POWERSHELL-001  | T1059.001 - PowerShell; T1105 - Ingress Tool Transfer                     | PowerShell Investigation                              |
| DET-MED-WIN-POWERSHELL-002  | T1059.001 - PowerShell; T1562.001 - Impair Defenses                       | PowerShell Investigation                              |
| DET-MED-WIN-POWERSHELL-003  | T1059.001 - PowerShell; T1105 - Ingress Tool Transfer                     | PowerShell Investigation                              |
| DET-MED-WIN-SYSMON-001      | T1003 - OS Credential Dumping                                             | LSASS Process Access                                  |
| DET-MED-WIN-SYSMON-002      | T1059.001 - PowerShell                                                    | PowerShell Investigation                              |
| DET-MED-WIN-SYSMON-003      | T1547 - Boot or Logon Autostart Execution                                 | Persistence Investigation                             |
| DET-MED-WIN-SYSMON-004      | T1059.001 - PowerShell; T1071 - Application Layer Protocol                | Network Investigation                                 |
| DET-MED-WIN-SYSMON-005      | T1059 - Command and Scripting Interpreter; T1106 - Native API             | Alert Triage / Process Analysis                       |
| DET-MED-WIN-SYSMON-006      | T1059 - Command and Scripting Interpreter                                 | PowerShell Investigation                              |

---


# 17. Validation and Testing

Investigation runbooks should be validated using controlled activity within the Enterprise Security Lab.

Validation should confirm that:

| Validation Item                     | Status   |
|-------------------------------------|----------|
| Alert Triage Workflow               | Complete |
| Authentication Investigation        | Complete |
| PowerShell Investigation            | Complete |
| Office-to-PowerShell Investigation  | Planned  |
| LSASS Access Investigation          | Complete |
| Persistence Investigation           | Complete |
| Network Investigation               | Planned  |
| Discovery Investigation             | Complete |
| Remote Access Investigation         | Complete |
| Investigation Documentation         | Complete |

## Validation Activities

Validation activities should confirm that:

1. The applicable detection rule generates an alert.
2. The alert contains sufficient information for triage.
3. Related telemetry can be located.
4. The applicable investigation runbook can be followed.
5. Findings can be documented consistently.
6. The activity can be classified appropriately.

## Detection Validation Examples

| Detection Category | Validation Activity                                                              |
|--------------------|----------------------------------------------------------------------------------|
| Authentication     | Generate controlled failed logon activity followed by successful authentication. |
| PowerShell         | Execute controlled PowerShell commands matching detection criteria.              |
| Office Execution   | Test Office application process spawning behavior.                               |
| LSASS Access       | Validate credential access detection using approved testing tools.               |
| Persistence        | Create controlled scheduled tasks or registry run key entries.                   |
| Network Activity   | Generate controlled network connections and validate telemetry.                  |
| Discovery          | Execute approved discovery commands and validate detection coverage.             |
| Remote Access      | Validate RDP, SMB, and remote execution telemetry.                               |

---

# 18. Troubleshooting

## Alert Does Not Contain Expected Fields

Verify:

- The underlying event contains the required fields.
- The applicable Elastic integration is enabled.
- The Elastic Agent is healthy.
- The event was indexed correctly.
- The detection rule references the correct fields.
- The ECS field mappings are correct.

---

## Related Events Cannot Be Found

Verify:

- The investigation time range is correct.
- The hostname is correct.
- The username is correct.
- The process ID is correct.
- The relevant telemetry source is enabled.
- The expected event was indexed.
- The correct data view or index pattern is being searched.

---

## Detection Alert Does Not Match Investigation Data

Verify:

- The detection rule query.
- The event timestamp.
- The index pattern.
- The ECS field mappings.
- Available event fields.
- Rule execution interval.
- Agent integration status.

---

## Investigation Cannot Be Completed

Document:

- Missing telemetry.
- Required data sources.
- Additional integrations needed.
- Detection improvements required.

Identify whether additional logging or configuration changes are required to complete future investigations.

---

# 19. Planned Enhancements

Planned improvements include:

- Validate all investigation runbooks using controlled attack simulations.
- Expand runbooks for additional detection scenarios.
- Add screenshots and investigation examples.
- Add KQL queries to applicable runbooks.
- Add investigation decision trees.
- Link detection rules directly to applicable runbooks.
- Add MITRE ATT&CK technique references.
- Add evidence collection procedures.
- Add standardized investigation templates.
- Integrate runbooks with incident response procedures.
- Automate investigation enrichment where practical.

---

# 20. Related Documentation

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
| 18-Backup-Recovery.md             | Backup strategy, VM recovery, file restoration, disaster recovery, and recovery validation.                                                                       |
| 19-Security-Hardening.md          | Windows/Linux hardening, security baselines, auditing, logging, and defensive controls.                                                                           |
| 20-NIST-CSF-Mapping.md            | Maps lab capabilities to the NIST Cybersecurity Framework and demonstrates alignment with enterprise security practices.                                          |
| 99-Lab-Journal.md                 | Chronological implementation record, troubleshooting, design decisions, testing, snapshots, and future improvements.                                              |

---


# Revision History

| Version | Date | Changes |
|---------|------|---------|
| v0.1.0 | 2026-07-30 | Initial investigation runbook documentation published |

---