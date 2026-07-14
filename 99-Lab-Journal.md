# SIEM Lab Journal

**Current Version:** v0.3.0
**Last Updated:** 2026-07-07

## Version History

| Version       | Date          | Highlights            |
|---------------|---------------|-----------------------|
| v0.1.0		| 2026-07-02    | Intitial Setup        |
| v0.2.0		| 2026-07-06    | Elastic Setup         |
| v0.3.0		| 2026-07-07    | Windows Deployment    |
| v0.4.0        | 2026-07-13    | AD Structure          |


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
| Linux Log Collection | Finished and Validated |
| Linux Metrics | Finished and Validated |
| Custom Dashboards | Currently Being Built |
| Windows Server 2025 Deployment | Currently Being Built |
| Windows Server 2025 DC Promo | Currently Being Built |
| Windows 11 Workstation Deployment | Finished and Validated |
| Windows 11 Workstation Configuration | Waiting on Dependency |
| Sysmon | Waiting on Dependency |
| Detection Rules | Waiting on Dependency |
| Attack Simulations | Waiting on Dependency |
---

# Journal Entries

## 2026-07-13

### Completed
- OU Creation Powershell Script Created
- Security Group Creation Powershell Script Created
- User Creation and Group Assignment Powershell Script Created
- OUs Created
- Security Groups created
- Users Assigned to Groups

### Issues Encountered

“During initial Active Directory automation, default AD containers (CN=Users and CN=Computers) were identified as containers rather than organizational units. Custom management OUs were created using distinct names to avoid namespace conflicts.”


### Lessons Learned
- OUs and Containers function differently and attempting to create new OUs inside of a container will result in an error

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
- Verify Linux syslog injection
- Create first Kibana Dashboard