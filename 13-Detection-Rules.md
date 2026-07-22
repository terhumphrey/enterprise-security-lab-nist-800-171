# Detection Rules

| Field				| Value 	    			|
|-------------------|---------------------------|
| Document Name     | Detection Rules           |
| Document Version  | v0.1.0 					|
| Author            | Terry Humphrey 			|
| Status 		    | Active					|
| Last Updated 		| 2026-07-22 				|

---

# Executive Summary

This document describes the detection rules deployed inside of Kibana that are used to create alerts for the various common attack vectors.

---

# Table of Contents

1. [Detection Categories](#1-detection-categories)
2. [Detection Rule Severity Standards](#2-detection-rule-severity-standards)
   - [Medium Severity](#medium-severity)
   - [High Severity](#high-severity)
   - [Severity Decision Logic](#severity-decision-logic)
   - [General Classification Model](#general-classification-model)
   - [Severity Escalation Principle](#severity-escalation-principle)
3. [High Severity Detection Rules](#3-high-severity-detection-rules)
4. [Medium Severity Detection Rules](#4-medium-severity-detection-rules)
5. [Related Documentation](#5-related-documentation)




# 1. Detection Categories

## Account

Rules in the Account category detect suspicious authentication and account-related activity, including failed authentication attempts, abnormal logon patterns, and successful authentication following repeated failures. These detections help identify potential brute-force attacks, password guessing, credential misuse, and unauthorized access.

## Credential

Rules in the Credential category detect activity associated with obtaining, accessing, or attempting to access authentication credentials. This includes suspicious credential access techniques such as attempts to access LSASS memory or other locations where credentials or authentication material may be exposed.

## Defense
Rules in the Defense category detect activity that may weaken, disable, or evade security controls. This includes clearing Windows event logs or disabling security services such as Windows Defender. These behaviors are significant because attackers commonly attempt to impair defenses and remove evidence of their activity.

## Discovery

Rules in the Discovery category detect commands and activity used to gather information about the environment. This includes discovering operating system information, users, accounts, network configuration, systems, and other infrastructure details that may assist an attacker in planning subsequent activity.

## LOLBIN
LOLBIN stands for Living Off the Land Binary. Rules in this category detect the abuse of legitimate, trusted Windows binaries to perform actions that may otherwise require dedicated malware or third-party tools. These binaries are legitimate system utilities but can be abused by attackers to download files, execute code, or bypass security controls.

## Network
Rules in the Network category detect suspicious network-related activity, including remote access, lateral movement, remote service execution, internal scanning, and suspicious outbound communications. These detections help identify attempts to move through the environment, establish remote access, or communicate with potentially malicious destinations.

## Office
Rules in the Office category detect suspicious activity involving Microsoft Office applications, particularly when Office applications spawn scripting engines or command interpreters such as PowerShell. This behavior may indicate malicious documents, macros, exploit activity, or user-assisted execution of malicious code.

## Persistence
Rules in the Persistence category detect activity intended to maintain access to a system after initial compromise. Examples include creating scheduled tasks, modifying registry Run keys, and other mechanisms that allow malicious or unauthorized processes to execute automatically or repeatedly.

## PowerShell
Rules in the PowerShell category detect suspicious use of PowerShell for command execution, downloading content, bypassing execution policies, or other activities commonly associated with script-based attacks. These detections focus specifically on PowerShell behavior and command-line activity.

## Process
Rules in the Process category detect suspicious process execution behavior that does not necessarily depend on a specific tool or application. This includes execution from user-writable directories or other locations commonly abused for unauthorized or malicious execution.

## Sysmon
Rules in the Sysmon category are based primarily on telemetry collected by Microsoft Sysmon. These rules focus on process creation, file creation, parent-child process relationships, network connections, and other low-level endpoint behaviors that Sysmon is designed to capture.
The Sysmon category identifies the primary telemetry source or behavioral data model, while categories such as PowerShell, Process, or Network describe the specific behavior being detected.

---

## 2. Detection Rule Severity Standards

Detection rule severity represents the potential security impact and attacker objective associated with the behavior being detected. Severity is determined by the risk represented by the behavior, not simply by how suspicious the event appears.

The severity classification used by this lab is:

- Medium: Suspicious activity that may represent reconnaissance, initial access, execution, persistence, lateral movement, or other malicious behavior, but does not by itself strongly indicate a high-impact compromise or immediate loss of control.
- High: Activity that provides strong evidence of malicious intent, indicates an attempt to compromise or maintain control of a system, targets security controls or credentials, or represents a behavior that could lead directly to significant compromise.

### Medium Severity
A detection is generally classified as Medium when the behavior is suspicious and security-relevant but additional evidence is normally required to determine whether the activity represents a confirmed compromise.

Medium-severity detections commonly include:

- Discovery and reconnaissance activity
- Suspicious authentication failures
- Remote access or lateral movement indicators
- Suspicious PowerShell usage
- Suspicious network activity
- Persistence mechanisms
- Suspicious file or process creation
- Activity that may have legitimate administrative use
- Behaviors that require additional contextual investigation before determining malicious intent

Medium severity does not mean the activity is unimportant. These alerts may represent early stages of an attack and can become more significant when correlated with other detections.

### High Severity

A detection is generally classified as High when the behavior provides stronger evidence of malicious activity or represents an action that could directly enable significant compromise, credential theft, defense evasion, or execution of malicious code.

High-severity detections commonly include:

- Attempts to disable or impair security controls
- Attempts to clear or remove security logs
- Credential access or credential theft activity
- Suspicious execution of trusted system utilities for malicious purposes
- Highly suspicious code execution patterns
- Execution involving sensitive security processes such as LSASS
- Suspicious activity involving security-sensitive applications or administrative controls
- Behaviors strongly associated with post-compromise activity
- Activity that indicates an attacker is attempting to evade detection or maintain control of a compromised system

### Severity Decision Logic

The following factors are considered when assigning severity:
- Potential Impact How much damage could the behavior cause if successful?
- Attacker Objective Does the behavior indicate reconnaissance, execution, persistence, credential access, defense evasion, or another stage of an attack?
- Evidence of Malicious Intent How strongly does the behavior indicate intentional malicious activity rather than legitimate administration?
- Privilege and Sensitivity Does the activity involve administrative privileges, credentials, security controls, or sensitive system processes?
- Attack Progression Does the behavior indicate an attacker is progressing toward or has already achieved system compromise?
- Defense Evasion Does the activity attempt to disable security controls, remove evidence, or evade detection?
- Likelihood of Legitimate Use How commonly could the same behavior occur during legitimate administrative or operational activity?
- Need for Correlation Does the alert require additional events or context to establish malicious intent, or is the event itself strongly indicative of compromise?

### General Classification Model

| Severity  | Classification Logic                                                                                                                                                              |
|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Medium    | Suspicious behavior requiring investigation and potentially indicating malicious activity, but normally requiring additional context or correlation to establish compromise.      |
| High      | Strong indicator of malicious intent, significant attack progression, credential compromise, defense evasion, or activity capable of directly enabling serious system compromise. |

### Severity Escalation Principle

A detection may be classified as Medium when viewed in isolation but become High when correlated with additional activity.
For example:

- A single suspicious PowerShell command may be Medium.
- PowerShell downloading and executing a payload may be High.
- Repeated failed logons may be Medium.
- Failed logons followed by a successful authentication may warrant High severity if the behavior strongly indicates successful account compromise.
- A scheduled task creation may be Medium.
- A scheduled task that executes a suspicious binary from a user-writable directory may warrant High severity.
- A discovery command may be Medium.
- Credential access involving LSASS may be High.

The purpose of this model is to maintain consistent severity classification while allowing individual rules to be evaluated based on their specific behavior, potential impact, and context.

---

## 3. High Severity Detection Rules 

### DET-HIGH-WIN-ACCOUNT-001 - Multiple Failed Logons Followed by Success

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.security-default-*`
- Custom Query: `event.code:4624 AND winlog.event_data.LogonType:(2 OR 10) AND user.name:*`
- Name: `DET-HIGH-WIN-ACCOUNT-001 - Multiple Failed Logons Followed by Success`
- Description: Detects authentication patterns consistent with brute-force attempts followed by successful account access.
- Severity: `SEVERITY`
- Risk Score: `73`
- Tags:     Windows, Account, Authentication, Brute Force, MITRE ATT&CK
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Attempt to login to the client machine: `WIN-PRO-01` several times with the wrong password
2. Login to the client machine: `WIN-PRO-01` with the correct password


**Expected Results:**

- Multiple failed authentication events (4625) are generated.
- A successful authentication event (4624) follows the failed attempts.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-HIGH-WIN-DEFENSE-001 - Windows Event Logs Cleared

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.code:1102`
- Name: `DET-HIGH-WIN-DEFENSE-001 - Windows Event Logs Cleared`
- Description: Detects the clearing of the Windows Security Event Log.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, Defense Evasion, Event Log, MITRE T1070
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
wevtutil cl Security
```

**Expected Results:**

- The Windows Security event log is cleared.
- Windows Security Event ID 1102 is generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-HIGH-WIN-DEFENSE-002 - Windows Defender Disabled

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.command_line:(*Set-MpPreference* AND *DisableRealtimeMonitoring*)`
- Name: `DET-HIGH-WIN-DEFENSE-002 - Windows Defender Disabled`
- Description: Detects attempts to disable Windows Defender real-time monitoring. Attackers commonly disable security controls before executing malicious activity.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, Defense Evasion, PowerShell, MITRE ATT&CK, T1562.001
- Schedule: `Every 5 Minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
Set-MpPreference -DisableRealtimeMonitoring $true

```

**Expected Results:**

- The PowerShell command executes.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-HIGH-WIN-LOLBIN-001 - Certutil Download

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.category:process and process.name:certutil.exe and process.command_line:*urlcache*`
- Name: `DET-HIGH-WIN-LOLBIN-001 - Certutil Download`
- Description: Detects certutil being used to download files from remote locations.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, Certutil, LOLBin, MITRE T1105
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
certutil.exe -urlcache -split -f https://example.com test.txt
```

**Expected Results:**

- certutil.exe executes with the -urlcache parameter.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-HIGH-WIN-NETWORK-001 - Suspicious Outbound Connection

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.category:network AND network.direction:outgoing`
- Name: `DET-HIGH-WIN-NETWORK-001 - Suspicious Outbound Connection`
- Description: Detects outbound network connections that may indicate command and control activity.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, Network, Command and Control, MITRE T1071
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
Test-NetConnection example.com -Port 443
```

**Expected Results:**

- An outbound network connection attempt is generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-HIGH-WIN-OFFICE-001 - Office Application Spawning PowerShell 

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:process AND process.name:powershell.exe AND process.parent.name:(winword.exe OR excel.exe OR outlook.exe OR powerpnt.exe)`
- Name: `DET-HIGH-WIN-OFFICE-001 - Office Application Spawning PowerShell`
- Description: Detects Microsoft Office applications launching PowerShell, a common behavior associated with malicious macros and phishing payload execution.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, Office, PowerShell, Initial Access, Execution
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open Word
3. Enable the Developer Tools
4. Run macro launching PowerShell

**Expected Results:**

- Microsoft Word launches PowerShell as a child process.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-HIGH-WIN-POWERSHELL-001 - Encoded Command Execution

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.name:powershell.exe AND process.command_line:(*EncodedCommand* OR * -enc * OR * -encodedcommand *)`
- Name: `DET-HIGH-WIN-POWERSHELL-001 - Encoded Command Execution`
- Description: Detects PowerShell execution using encoded command parameters. Attackers commonly use PowerShell -EncodedCommand to obfuscate malicious scripts and bypass simple command-line detection.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, PowerShell, MITRE ATT&CK, Detection Engineering, Sysmon
- Schedule: `Every 5 minutes. Additional look-back time: 1 minute`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
powershell.exe -NoProfile -EncodedCommand VwByAGkAdABlAC0ASABvAHMAdAAgACIASQBOAFYATwBLAEUAIABFAEwAQQBTAFQASQBDAF8AVABFAFMAVAAiAA==
```
4. This should invoke `Write-Host "INVOKE ELASTIC_TEST"`


**Expected Results:**

- Sysmon Event ID 1
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-HIGH-WIN-POWERSHELL-002 - PowerShell Execution Policy Bypass

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.name:powershell.exe AND process.command_line:(*ExecutionPolicy* OR *-ExecutionPolicy Bypass* OR *-ep bypass*)`
- Name: `DET-HIGH-WIN-POWERSHELL-002 - PowerShell Execution Policy Bypass`
- Description: Detects PowerShell execution with policy bypass parameters. Attackers frequently use execution policy bypass to execute unauthorized scripts.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, PowerShell, Sysmon, MITRE ATT&CK, T1059.001
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
powershell.exe -ExecutionPolicy Bypass -Command "Write-Host TEST"
```

**Expected Results:**

- PowerShell executes with the ExecutionPolicy Bypass parameter.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated


### DET-HIGH-WIN-PROCESS-001 - Suspicious Process Execution from User Writable Directory

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.executable:(*\\Users\\*\\AppData\\* OR *\\Temp\\* OR *\\Windows\\Temp\\*)`
- Name: `DET-HIGH-WIN-PROCESS-001 - Suspicious Process Execution from User Writable Directory`
- Description: Detects processes executed from user-writable directories such as AppData and Temp locations. Attackers commonly use these locations to execute malware and evade traditional application controls.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, Sysmon, Process Creation, MITRE ATT&CK, T1204, T1059
- Schedule: `Every 5 Minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
Copy-Item "C:\Windows\System32\cmd.exe" "$env:TEMP\test.exe"
Start-Process "$env:TEMP\test.exe"
```

**Expected Results:**

- A copy of cmd.exe is created in the user’s temporary directory.
- The executable is launched from a user-writable directory.
- Sysmon Event ID 1 generated
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-HIGH-WIN-SYSMON-001 - LSASS Memory Access Attempt 

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:process AND process.name:(procdump.exe OR mimikatz.exe OR rundll32.exe) AND process.command_line:(*lsass* OR *MiniDump*)`
- Name: `DET-HIGH-WIN-SYSMON-001 - LSASS Memory Access Attempt`
- Description: Detects processes attempting to access LSASS memory, which may indicate credential dumping activity.
- Severity: `High`
- Risk Score: `73`
- Tags: Windows, Credential Access, LSASS, MITRE ATT&CK
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
# Run an approved credential-dumping simulation tool
# specifically configured for the lab environment.
```

**Expected Results:**

- The approved simulation generates a process event matching the detection criteria.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-HIGH-WIN-SYSMON-002 - Suspicious Executable Created and Executed

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:process AND process.name:(*.exe) AND process.command_line:(*Temp* OR *AppData*)`
- Name: `DET-HIGH-WIN-SYSMON-002 - Suspicious Executable Created and Executed`
- Description: Detects executable execution from user-writable locations, which may indicate malware execution.
- Severity: `SEVERITY`
- Risk Score: `73`
- Tags: Windows, Malware, Execution, Persistence
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
Copy-Item "C:\Windows\System32\cmd.exe" "$env:TEMP\test.exe"
& "$env:TEMP\test.exe"
```

**Expected Results:**

- A copy of cmd.exe is created in the user’s temporary directory.
- The executable is launched from the temporary directory.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated



## 4. Medium Severity Detection Rules

### DET-MED-WIN-ACCOUNT-001 - Suspicious Failed Logon Activity 

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.security-default-*`
- Custom Query: `event.code:4625`
- Name: `DET-MED-WIN-ACCOUNT-001 - Suspicious Failed Logon Activity`
- Description: Detects failed Windows authentication attempts that may indicate unauthorized access attempts.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Authentication, Credential Access
- Schedule: `Every 5 Minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Attempt to login multiple times with the wrong password


**Expected Results:**

- Failed Windows authentication events (4625) are generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-CREDENTIAL-001 - Suspicious Credential Access Attempt

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:process AND process.name:(rundll32.exe OR reg.exe OR powershell.exe) AND process.command_line:(*comsvcs.dll* OR *MiniDump* OR *sekurlsa* OR *sam* OR *security*)`
- Name: `DET-MED-WIN-CREDENTIAL-001 - Suspicious Credential Access Attempt`
- Description: Detects potential credential access activity by identifying suspicious processes and command-line patterns commonly associated with credential dumping, SAM database access, and security credential extraction attempts. This detection helps identify early indicators of credential theft techniques used by attackers.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Credential-Access, Windows, Credential-Dumping, Security-Monitoring, Mitre-Attack
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
rundll32.exe C:\Windows\System32\comsvcs.dll, MiniDump

```

**Expected Results:**

- <TBD>
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-DISCOVERY-001 - System Discovery Commands

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.category:process and process.name:(whoami.exe or systeminfo.exe or hostname.exe or ipconfig.exe)`
- Name: `DET-MED-WIN-DISCOVERY-001 - System Discovery Commands`
- Description: Detects common host discovery commands executed on Windows.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Discovery, MITRE T1082
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
whoami
systeminfo
hostname
ipconfig
```

**Expected Results:**

- The system discovery commands execute successfully.
- Process creation events are generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-DISCOVERY-002 - Account Discovery Commands

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.category:process and process.name:net.exe and process.command_line:(*" user"* or *" localgroup"*)`
- Name: `DET-MED-WIN-DISCOVERY-002 - Account Discovery Commands`
- Description: Detects account and group enumeration using net.exe.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Discovery, Accounts, MITRE T1087,
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
net user
net localgroup administrators
```

**Expected Results:**

- net user enumerates local user accounts.
- net localgroup administrators enumerates members of the local Administrators group.
- Process creation events are generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-NETWORK-001 - Suspicious RDP Connection

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.code:4624 AND winlog.logon.type:"RemoteInteractive"`
- Name: `DET-MED-WIN-NETWORK-001 - Suspicious RDP Connection`
- Description: Detects successful Remote Desktop Protocol logons to Windows systems.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, RDP, Remote-Access, mitre-T1021.001
- Schedule: `Every 5 minutes, Lookback 2 minutes`

#### Validation Steps

1. From a client machine other than : `WIN-PRO-01`
2. Open `run`
3. Execute: 

```
mstsc /v:WIN-PRO-01
```

4. Log in successfully

**Expected Results:**

- A successful RDP logon occurs.
- A Windows Security event corresponding to a Remote Interactive logon is generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-NETWORK-002 - PowerShell Remoting Activity

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.provider:"Microsoft-Windows-PowerShell" AND (message:*Remote* OR message:*WinRM*)`
- Name: `DET-MED-WIN-NETWORK-002 - PowerShell Remoting Activity`
- Description: Detects PowerShell remoting activity that may indicate lateral movement.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, PowerShell, Winrm, Lateral-Movement, mitre-T1021.006
- Schedule: `Every 5 minutes, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell to start the Enable Remoting Process
3. Execute: 

```powershell
Enable-PSRemoting -Force
Invoke-Command -ComputerName WIN-PRO-01 -ScriptBlock {hostname}
```



**Expected Results:**

- PowerShell remoting is enabled.
- A remote PowerShell command is executed.
- PowerShell/WinRM telemetry is generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-NETWORK-003 - SMB Connection Activity

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.code:(5140 OR 5145)`
- Name: `DET-MED-WIN-NETWORK-003 - SMB Connection Activity`
- Description: Detects SMB share access activity that may indicate lateral movement.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, SMB, File Share, Lateral Movement, MITRE T1021.002
- Schedule: `Every 5 minutes, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
net use \\WIN-PRO-01\C$
```

**Expected Results:**

- An SMB connection to the Windows administrative share is attempted.
- SMB share access telemetry is generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-NETWORK-004 - Remote Service Execution

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.code:(7045 OR 4697)`
- Name: `DET-MED-WIN-NETWORK-004 - Remote Service Execution`
- Description: Detects creation of Windows services, which can be abused for remote execution.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Services, Remote Execution, Lateral Movement, MITRE T1569.002
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
sc.exe create ElasticTest binPath= "cmd.exe /c whoami"
```

**Expected Results:**

- A Windows service named ElasticTest is created.
- A Windows service creation event is generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-NETWORK-005 - Internal Port Scanning Activity

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.category:network AND network.direction:internal`
- Name: `DET-MED-WIN-NETWORK-005 - Internal Port Scanning Activity`
- Description: Detects internal network connections that may indicate reconnaissance or port scanning.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Network, Reconnaissance, Port Scan, MITRE T1046
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the Kali Linux machine: `TBD`
2. Open a terminal
3. Execute: 

```Bash
nmap -sT 192.168.1.51
```

**Expected Results:**

- Kali performs a TCP connect scan against the internal target.
- Network connection events are generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-PERSISTENCE-001 - Scheduled Task Created

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.code:4698`
- Name: `DET-MED-WIN-PERSISTENCE-001 - Scheduled Task Created`
- Description: Detects creation of a scheduled task, a common persistence mechanism.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Scheduled Task, Persistence, MITRE T1053,
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
schtasks /create /tn "ElasticTest" /tr "cmd.exe" /sc once /st 23:59 /f
```

**Expected Results:**

- A scheduled task named ElasticTest is created.
- Windows generates event ID 4698.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-PERSISTENCE-002 - Registry Run Key Added

#### Configuration Settings

- Index Pattern: `logs-*`
- Custom Query: `event.code:13 and registry.path:*\\CurrentVersion\\Run\\*`
- Name: `DET-MED-WIN-PERSISTENCE-002 - Registry Run Key Added`
- Description: Detects persistence through the Windows Run registry key.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Registry, Persistence, MITRE T1547
- Schedule: `Every 1 minute, Lookback 2 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\Run" /v ElasticTest /t REG_SZ /d "cmd.exe" /f
```

**Expected Results:**

- A new ElasticTest value is added to the Windows Run registry key.
- Sysmon generates a registry modification event.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-POWERSHELL-001 - PowerShell File Download Activity

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.name:powershell.exe AND process.command_line:(*Invoke-WebRequest* OR *DownloadString* OR *Net.WebClient* OR *wget* OR *curl*)`
- Name: `DET-MED-WIN-POWERSHELL-001 - PowerShell File Download Activity`
- Description: Detects PowerShell commands commonly used to download files from remote sources. These techniques are frequently used during malware delivery and post-exploitation activity.
- Severity: `Medium`
- Risk Score: `50`
- Tags: Windows, PowerShell, Sysmon, MITRE ATT&CK, T1059.001
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
powershell.exe -Command "Invoke-WebRequest https://example.com -OutFile test.txt"
```

**Expected Results:**

- PowerShell executes Invoke-WebRequest.
- The download request is generated.
- Sysmon Event ID 1 is generated.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-POWERSHELL-002 - PowerShell Execution Policy Bypass

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.name:powershell.exe AND process.command_line:(*-ExecutionPolicy*Bypass* OR *-ep*Bypass*)`
- Name: `DET-MED-WIN-POWERSHELL-002 - PowerShell Execution Policy Bypass`
- Description: Detects PowerShell execution using an execution policy bypass, which may allow scripts to execute while circumventing normal PowerShell execution policy controls.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, PowerShell, Execution Policy, Defense Evasion, MITRE ATT&CK, T1059.001
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
powershell.exe -ExecutionPolicy Bypass -Command "Write-Host 'ATTCK_T1059_001_EXECUTION_POLICY_BYPASS_TEST'"
```

**Expected Results:**

- Sysmon Event ID 1 is generated for the PowerShell process creation.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-POWERSHELL-003 – PowerShell Web Download Activity

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.name:powershell.exe AND process.command_line:(*Invoke-WebRequest* OR *DownloadString* OR *Net.WebClient* OR *wget* OR *curl*)`
- Name: `DET-MED-WIN-POWERSHELL-003 – PowerShell Web Download Activity`
- Description: Detects PowerShell commands commonly used to download files from remote sources. These techniques are frequently used during malware delivery and post-exploitation activity.
- Severity: `Medium`
- Risk Score: `50`
- Tags: Windows, PowerShell, MITRE ATT&CK, T1059.001, T1105
- Schedule: `Every 5 Minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
powershell.exe -Command "Invoke-WebRequest https://example.com -OutFile test.txt"
```

**Expected Results:**

- Sysmon Event ID 1
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-SYSMON-001 - Suspicious Process Creation from Temp Directory

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:process AND process.executable:(*\\Temp\\* OR *\\Users\\Public\\*)`
- Name: `DET-MED-WIN-SYSMON-001 - Suspicious Process Creation from Temp Directory`
- Description: Detects processes executing from temporary or publicly accessible directories, which may indicate malware execution or unauthorized activity.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Sysmon, Execution, Defense Evasion
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
New-Item -ItemType Directory -Path "C:\Temp\Test" -Force
Copy-Item "C:\Windows\System32\cmd.exe" "C:\Temp\Test\test.exe"
& "C:\Temp\Test\test.exe"
```

**Expected Results:**

- A copy of cmd.exe is created in C:\Temp\Test.
- The executable is launched from the temporary directory.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-SYSMON-002 - Suspicious PowerShell Parent Process

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:process AND process.name:powershell.exe AND process.parent.name:(cmd.exe OR wscript.exe OR cscript.exe)`
- Name: `DET-MED-WIN-SYSMON-002 - Suspicious PowerShell Parent Process`
- Description: Detects PowerShell launched from scripting engines or command shells, which may indicate malicious execution chains.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, PowerShell, Process Creation, Defense Evasion
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open a command prompt
3. Execute: 

```powershell
powershell.exe -Command "Write-Host 'PARENT_PROCESS_TEST'"
```

**Expected Results:**

- PowerShell is launched as a child process of cmd.exe.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-SYSMON-003 - Suspicious File Creation in User Temp Directory 

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:file AND file.directory:(*\\AppData\\Local\\Temp* OR *\\Users\\Public\\*)`
- Name: `DET-MED-WIN-SYSMON-003 - Suspicious File Creation in User Temp Directory`
- Description: Detects file creation in temporary user locations, which are commonly abused for malware staging and execution.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Sysmon, File Activity, Persistence
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
New-Item "$env:TEMP\TestDetection.txt"
```

**Expected Results:**

- A test file is created in the user’s temporary directory.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-SYSMON-004 - Network Connection from PowerShell 

#### Configuration Settings

- Index Pattern: `.ds-logs-windows.sysmon_operational-default-*`
- Custom Query: `event.category:network AND process.name:powershell.exe`
- Name: `DET-MED-WIN-SYSMON-004 - Network Connection from PowerShell`
- Description: Detects PowerShell processes making network connections, which may indicate payload retrieval or command and control activity.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, PowerShell, Network, Command and Scripting Interpreter
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
Invoke-WebRequest -Uri "https://example.com"
```

**Expected Results:**

- PowerShell makes an outbound network connection.
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated

### DET-MED-WIN-SYSMON-005 - Suspicious Parent Process Relationship

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.parent.name:(winword.exe OR excel.exe OR outlook.exe OR wscript.exe OR cscript.exe) AND process.name:powershell.exe`
- Name: `DET-MED-WIN-SYSMON-005 - Suspicious Parent Process Relationship`
- Description: Detects suspicious process chains where commonly abused applications launch PowerShell. This behavior is frequently associated with phishing payloads and malware execution.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Sysmon, Process Creation, MITRE ATT&CK, T1059.001
- Schedule: `Every 5 Minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
script to be determined after deferral.
```

**Expected Results:**

- Results to be determined after deferral
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Not Validated

### DET-MED-WIN-SYSMON-006 - Suspicious Script Interpreter Execution

#### Configuration Settings

- Index Pattern: `logs-windows.sysmon_operational-*`
- Custom Query: `event.category:process AND process.name:(wscript.exe OR cscript.exe OR mshta.exe OR regsvr32.exe)`
- Name: `DET-MED-WIN-SYSMON-006 - Suspicious Script Interpreter Execution`
- Description: Detects execution of Windows script interpreters and proxy execution binaries commonly abused by attackers.
- Severity: `Medium`
- Risk Score: `47`
- Tags: Windows, Sysmon, LOLBin, MITRE ATT&CK
- Schedule: `Every 5 minutes`

#### Validation Steps

1. Open the client machine: `WIN-PRO-01`
2. Open PowerShell
3. Execute: 

```powershell
cscript.exe //nologo C:\Windows\System32\test.vbs
```

**Expected Results:**

- Sysmon Event ID 1
- The event is ingested into Elasticsearch.
- The detection rule triggers.
- An Elastic Security alert is created.

**Validation Status:**
- Validated


# 5. Related Documentation

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
| 14-Vulnerability-Management.md    | Vulnerability scanning, risk prioritization, remediation workflows, and verification.                                                                             |
| 15-Patch-Management.md            | WSUS deployment, update approvals, client targeting, maintenance windows, and patch compliance.                                                                   |
| 16-Incident-Response.md           | Incident response lifecycle, alert triage, investigation, containment, eradication, recovery, and lessons learned.                                                |
| 17-Investigation-Runbooks.md      | New. Step-by-step analyst procedures for investigating high-value alerts and detection scenarios.                                                                 |
| 18-Backup-Recovery.md             | Backup strategy, VM recovery, file restoration, disaster recovery, and recovery validation.                                                                       |
| 19-Security-Hardening.md          | Windows/Linux hardening, security baselines, auditing, logging, and defensive controls.                                                                           |
| 20-NIST-CSF-Mapping.md            | Maps lab capabilities to the NIST Cybersecurity Framework and demonstrates alignment with enterprise security practices.                                          |
| 99-Lab-Journal.md                 | Chronological implementation record, troubleshooting, design decisions, testing, snapshots, and future improvements.                                              |

---




