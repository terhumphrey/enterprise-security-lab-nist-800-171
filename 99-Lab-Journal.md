# SIEM Lab Journal

**Current Version:** v0.5.0
**Last Updated:** 2026-07-17

## Version History

| Version       | Date          | Highlights                |
|---------------|---------------|---------------------------|
| v0.1.0		| 2026-07-02    | Intitial Setup            |
| v0.2.0		| 2026-07-06    | Elastic Setup             |
| v0.3.0		| 2026-07-07    | Windows Deployment        |
| v0.4.0        | 2026-07-13    | AD Structure              |
| v0.5.0        | 2026-07-17    | Lab Architecture Redesign |

## Overall Status

| Component | Status |
| ----------------|-----------|
| Rocky Linux Install | Finished and Validated |
| Docker | Finished and Validated |
| Elasticsearch | Finished and Validated |
| Kibana | Finished and Validated |
| Fleet Server | Finished and Validated |
| Elastic Agent | Finished and Validated |
| System Integration | Finished and Validated |
| Linux Log Collection | Not Started |
| Linux Metrics | Not Started |
| Custom Dashboards | Currently Being Built |
| Windows Server 2025 Deployment | Finished and Validated |
| Windows Server 2025 DC Promo | Finished and Validated |
| Windows 11 Workstation Deployment | Finished and Validated |
| Windows 11 Workstation Configuration | Finished and Validated |
| Sysmon | Waiting on Dependency |
| Detection Rules | Waiting on Dependency |
| Attack Simulations | Waiting on Dependency |
---

# Current Architecture

## Infrastructure

| System        | Role                                                              |
|---------------|-------------------------------------------------------------------|
| WIN-DC-01     | Windows Server 2025 Domain Controller, DNS, Certificate Authority |
| WIN-CLIENT-01 | Windows 11 Domain Joined Workstation                              |
| LNX-ELK-01    | Rocky Linux 9 ELK SIEM Platform                                   |

## Security Stack

| Component         | Purpose                       |
|-------------------|-------------------------------|
| Elasticsearch     | Log storage and search        |
| Kibana            | Visualization and analysis    |
| Fleet Server      | Elastic Agent management      |
| Elastic Agent     | Endpoint telemetry collection |
| Microsoft AD CS   | Internal PKI                  |

# Journal Entries

## 2026-07-17

### Note
On 2026-07-15, I encountered a major issue completing Windows Elastic Agent enrollment due to certificate and trust configuration challenges. After several hours of troubleshooting the original deployment, I determined that the existing lab architecture would create additional limitations as the scope expanded.

Rather than continue modifying an environment that was not designed for the long-term goals of the project, I used the opportunity to redesign the lab architecture from the ground up.

During this process, I discovered two additional pieces of hardware available for the lab environment:
- Mac Mini running Windows 10
- 2015 MacBook Pro

These additional systems allowed me to redistribute workloads, establish clearer server roles, eliminate unnecessary NAT port forwarding dependencies, standardize naming conventions, and design the environment around future requirements including Active Directory, PKI, SIEM operations, and NIST SP 800-171 aligned security documentation.

The original lab was intentionally built with a smaller scope while I was learning the Elastic stack and SIEM concepts. As the project evolved, additional requirements became clear, including domain services, certificate management, endpoint telemetry, attack simulation, and enterprise-style documentation. The original architecture was not optimized for these expanded objectives.

The evening of 2026-07-15 and all day on 2026-07-16 were dedicated to rebuilding and validating the redesigned environment. These activities are consolidated into this journal entry.

Note: The Overall Status section above has not yet been fully updated to reflect all components of the redesigned architecture.


### Completed
- Windows DC Server Built
- Windows DC Promoted as new Domain Controller
- Windows DC Configured as Certificate Authority
- Windows DC Configured as DNS
- Windows 11 Client Built
- Windows 11 Client joined to domain
- New ELK Server Built
- Kibana, Elasticsearch, and Fleet installed and configured
- Everything enrolled in Fleet
- New documentation structure established


### Issues Encountered
- Certificate trust configuration required additional troubleshooting during endpoint enrollment.
- Original architecture required redesign due to expanded project scope. Various issues are going to be included in the updated documentation


### Lessons Learned
- Lab environments should evolve with project requirements. When architecture no longer supports future objectives, redesigning early can be more efficient than continuing to modify an unsuitable foundation.
- Enterprise concepts such as PKI, identity management, endpoint telemetry, and documentation requirements should be considered early when designing security labs.

### Next Steps
- Configure additional Elastic Integrations
- Deploy Sysmon
- Configure security dashboards
- Create detection rules
- Deploy the Kali Linux attack simulation environment



## 2026-07-14

### Completed
- Windows 11 Client joined to domain


### Issues Encountered
- Display settings caused issues with being able to join the domain


### Lessons Learned
- VirtualBox requires additional add-ons to be installed for the display driver to function properly.

### Next Steps
- 


## 2026-07-13

### Completed
- OU Creation Powershell Script Created
- Security Group Creation Powershell Script Created
- User Creation and Group Assignment Powershell Script Created
- OUs Created
- Security Groups created
- Users Assigned to Groups

### Issues Encountered

During initial Active Directory automation, default AD containers (CN=Users and CN=Computers) were identified as containers rather than organizational units. Custom management OUs were created using distinct names to avoid namespace conflicts.


### Lessons Learned
- OUs and Containers function differently and attempting to create new OUs inside of a container will result in an error

### Next Steps
- Join the Workstation to the domain



## 2026-07-07

### Completed
- Deployed Windows 2025 Server
- Promoted Windows 2025 Server as a Domain Controller
- Deployed Windows 11 Pro workstation
- 
- 
- 
- 
- 
- 

### Issues Encountered
- Server manager hangs on Windows 2025 while attempting to install AD roles
- 

### Lessons Learned
- Powershell can bypass the Server Manager to install and promote Windows 2025 as a DC
-  

### Next Steps
 - AD OU Creation
 - AD Group Creation
 - AD User Creation
 - Assigned Users to Groups



## 2026-07-06

### Completed
- Deployed Elasticsearch in Docker
- Deployed Kibana in Docker
- Configured Docker volumes
- Verified Elasticsearch cluster health
- Installed Fleet Server
- Enrolled Elastic Agent
- Added System Integration
- Verified Linux Syslog ingestion
- Created first Kibana Dashboard

### Issues Encountered
- Kibana authentication failed due to an outdated 'kibana_system' password.
- System clock drift caused Fleet metrics to fail until NTP synchronization.

### Lessons Learned
- Recreating a Docker container is required after changing environment variables.
- Fleet metrics are sensitive to large time drift. 

### Next Steps
 - Deploy Windows 2025 Server
 - Promote Windows 2025 Server as AD machine
 - Deploy Windows 11


## 2026-07-02

### Completed
- Installed Rocky Linux 9.8
- Installed Docker Engine and Docker Compose
- Created '.env'

### Issues Encountered
- SSH Failed on server due to a compatibility issue between the Mac Mini Network adapter and Rocky Linux.

### Lessons Learned
- Port forwarding is required to allow SSH into Rocky Linux machines when using VirtualBox and a Mac Mini

### Next Steps
- Deploy Elasticsearch in Docker
- Deploy Kibana in Docker
- Configure Fleet Server
- Enroll Elastic Agent
- Add System Integration
- Verify Linux syslog Ingestion
- Create first Kibana Dashboard