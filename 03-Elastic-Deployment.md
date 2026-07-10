# Elastic Deployment

| Field				      | Value    	   				|
|-------------------|---------------------|
| Document Name 	  | Elastic Deployment  |
| Document Version 	| v0.1.0 							|
| Author			      | Terry Humphrey 			|
| Status 		      	| Active 							|
| Last Updated 		  | 2026-07-09 					|

---

## Table of Contents

- [Server Overview](#server-overview)
- [Configuration Steps](#configuration-steps)
  - [1. VM Installation](#1-vm-installation)
  - [2. System Configuration](#2-system-configuration)
  - [3. Install the Docker Engine](#3-install-the-docker-engine)
  - [4. Create the Elastic Stack Directory](#4-create-the-elastic-stack-directory)
  - [5. Create the docker-compose.yml](#5-create-the-docker-composeyml)
  - [6. Validate the Deployment](#6-validate-the-deployment)
- [Related Documentation](#related-documentation)


---

## Server Overview

This is a Rocky Linux 9.8 virtual machine named `elastic-node-01.serenity.lab`. It is hosted on a Mac Mini Late 2014 using VirtualBox. This virtual machine hosts the core Elastic Stack services used throughout the lab:

- Docker
  - Elasticsearch
  - Kibana

### Server Specifications

#### Physical Host

| Host                 | Operating System        | CPU                             | Memory | Storage    | IP            |
|----------------------|-------------------------|---------------------------------|--------|------------|---------------|
| Mac Mini (Late 2014) | macOS Monterey (12.7.6) | 2.6 GHz Dual-Core Intel Core i5 | 16 GB  | 1.2 TB     | 192.168.1.220 |

---

#### Virtual Machine

| Host Name                     | Operating System | CPU       | Memory | Storage   | IP             | Network Mode | SSH Access | 
|-------------------------------|------------------|-----------|--------|-----------|----------------|--------------|------------|
| elastic-node-01.serenity.lab  | Rocky Linux      | 2 vCPU    | 8 GB   | 100 GB    | 192.168.1.220  |  NAT         | 2222       |


![Virtual Machines](/screenshots/03-Elastic-Deployment/01-virtualbox-vm-configuration.png)

---
## Configuration Steps

This section goes over each specific configuration step of the server, line by line, for ease of recreation.

### 1. VM Installation

1. Partitioning: Automatic
2. Software Installation: Minimal Server Install

![Rocky Linux Login](/screenshots/03-Elastic-Deployment/02-rocky-linux-login.png)

### 2. System Configuration

**1. Set the hostname**
```bash
sudo hostnamectl set-hostname elastic-node-01.serenity.lab
```
*Reason:* Sets a descriptive hostname so the node is identifiable on the network and in logs. `elastic-node-01` describes the server; `serenity.lab` is the unique identifier used across all homelab items.

![hostnamectl output](/screenshots/03-Elastic-Deployment/03-hostnamectl-output.png)

**2. Update the system**
```bash
sudo dnf update -y
```
*Reason:* Installs the latest bug fixes and security patches. `dnf` is Rocky's package manager; `update` specifies that you want to update installed packages, and `-y` tells it to automatically answer yes to prompts.

**3. Install core utilities**
```bash
sudo dnf install -y curl wget vim git nano unzip tar
```
*Reason:* Installs `curl`, `wget`, `vim`, `git`, `nano`, `unzip`, and `tar`. Since this was a minimal server install, none of these are present by default.

| Tool | Purpose |
|---|---|
| `curl` | Downloads and talks to web APIs |
| `wget` | Downloads files |
| `vim` | Text editor |
| `git` | Version control |
| `nano` | Text editor |
| `unzip` | Extracts archive files |
| `tar` | Creates archive files |

**4. Disable swap**
```bash
sudo swapoff -a
```
*Reason:* Elasticsearch requires swap to be disabled, otherwise there is degraded performance. The `swapoff` command disables swap; `-a` tells it to disable all swap spaces.

**5. Remove the swap entry from fstab**
```bash
sudo sed -i '/swap/d' /etc/fstab
```
*Reason:* Removes the swap entry so that it stays disabled after reboot.
- `sed -i` invokes the stream editor to edit the file in place
- `/swap/` is the search pattern — it finds every line containing the word "swap"
- `d` stands for delete
- `/etc/fstab` is the file being searched

So this command searches `/etc/fstab` for lines containing "swap," deletes them, then saves the modified file.

**6. Increase max memory map count**
```bash
sudo sysctl -w vm.max_map_count=262144
```
*Reason:* The default number of memory mappings on Linux (65530) is not enough for Elasticsearch, which requires 262144.
- `sysctl` is the System Control command
- `-w` is the parameter to write a value
- `vm.max_map_count` is the maximum number of memory mappings a single process can have (`vm` here means *virtual memory*, not *virtual machine*)
- `262144` is the value being set

**7. Make the change permanent**
```bash
echo "vm.max_map_count=262144" | sudo tee /etc/sysctl.d/99-elastic.conf
```
*Reason:* Makes the memory mapping change persist across reboots.
- `echo` prints the given text
- `|` pipes the output of `echo` into the `tee` command
- `tee` writes the input to both a file and the screen
- `/etc/sysctl.d/99-elastic.conf` is the destination file

The resulting file contains exactly one line: `vm.max_map_count=262144`. `tee` is used instead of `>` because `tee` runs as root (via `sudo`), while `>` redirection happens in the unprivileged shell and would fail to write to a root-owned path.

### 3. Install the Docker Engine

Docker is used to containerize the Elastic Stack components, allowing each service to run in an isolated, reproducible environment with minimal dependency conflicts.

**1. Remove any existing Docker packages**
```bash
sudo dnf remove -y docker docker-client docker-client-latest docker-common \
  docker-latest docker-latest-logrotate docker-logrotate docker-engine
```
*Reason:* Removes any broken or partial Docker installs. This is safe to run even if nothing is currently installed. `-y` auto-confirms the removal; the rest of the arguments are the various Docker package names being targeted.

**2. Install DNF plugin support**
```bash
sudo dnf -y install dnf-plugins-core
```
*Reason:* Installs additional DNF features — specifically, the ability to add repositories, which is required to install Docker.

**3. Add Docker's official repository**
```bash
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
*Reason:* Adds Docker's official repo. `config-manager` was added as part of the `dnf-plugins-core` install and is used to manage repositories; `--add-repo` specifies the repo URL to add.

**4. Install Docker Engine and Compose**
```bash
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
*Reason:* Installs the Docker Engine and the Compose plugin. Each listed package is a required component for Docker Engine and the Compose plugin.

**5. Enable and start Docker**
```bash
sudo systemctl enable --now docker
```
*Reason:* Enables Docker to start on boot and starts it immediately.

**6. Verify Docker**
```bash
docker --version
```
*Reason:* Confirms Docker is installed by checking its version.

**7. Verify Docker Compose**
```bash
docker compose version
```
*Reason:* Confirms Docker Compose is installed by checking its version.

![docker installed](/screenshots/03-Elastic-Deployment/04-docker-installed.png)

### 4. Create the Elastic Stack Directory

**1. Create the directory**
```bash
mkdir -p ~/elastic
```
*Reason:* Creates the `~/elastic` directory. `-p` creates any needed parent directories.

**2. Move into the directory**
```bash
cd ~/elastic
```
*Reason:* Changes into the `~/elastic` directory.

### 5. Create the docker-compose.yml

This file is a complete, reproducible definition of the Elastic SIEM environment, where Docker builds and connects Elasticsearch and Kibana as a single system from a single configuration file.

**1. Open the file for editing**
```bash
nano docker-compose.yml
```
*Reason:* Opens `docker-compose.yml` in the nano text editor (creating it if it doesn't exist).

**2. Paste the following configuration:**

```yaml
version: "3.8"

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.13.4
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
      - ES_JAVA_OPTS=-Xms2g -Xmx2g
    ports:
      - "9200:9200"
    volumes:
      - esdata:/usr/share/elasticsearch/data

  kibana:
    image: docker.elastic.co/kibana/kibana:8.13.4
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch

volumes:
  esdata:
```

#### YAML File Breakdown

- `version` — the Docker Compose file format version being used
- `services` — the applications being defined
  - **elasticsearch** — installs Elasticsearch
    - `image`: the official Elasticsearch image
    - `container_name`: name assigned to the container
    - `environment`: environment variables passed into the container
      - `discovery.type=single-node` — run as a standalone cluster rather than multi-node
      - `xpack.security.enabled=false` — disables built-in authentication/security for easier use in a lab environment. Security is temporarily disabled because this is the initial deployment phase. Authentication and TLS are configured later during the Fleet and Elastic Security setup.
      - `ES_JAVA_OPTS=-Xms2g -Xmx2g` — configures JVM memory usage
    - `ports`: ports exposed by the container
      - `"9200:9200"` — maps host port 9200 to container port 9200
    - `volumes`: persists data outside the container
      - `esdata:/usr/share/elasticsearch/data` — named volume mounted to Elasticsearch's data directory
  - **kibana** — installs Kibana
    - `image`: the official Kibana image, version 8.13.4
    - `container_name`: name assigned to the container
    - `environment`: environment variables passed into the container
      - `ELASTICSEARCH_HOSTS=http://elasticsearch:9200` — tells Kibana to connect to the Elasticsearch container using its internal Docker network name
    - `ports`: ports exposed by the container
      - `"5601:5601"` — maps host port 5601 to container port 5601
    - `depends_on`: startup dependencies
      - `elasticsearch` — starts Elasticsearch before Kibana (note: this controls startup *order* only, it does not guarantee Elasticsearch is fully *ready* before Kibana starts)
- `volumes` — top-level declaration of named volumes used by the services
  - `esdata` — the persistent volume used by Elasticsearch

![Docker Compose File](/screenshots/03-Elastic-Deployment/05-docker-compose-file.png)

**3. Start the stack**
```bash
sudo docker compose up -d
```
*Reason:* Creates and starts everything defined in `docker-compose.yml`. The `-d` flag stands for detached mode, which runs the containers in the background.

**4. Verify the containers are running**
```bash
sudo docker ps
```
*Reason:* Lists running Docker containers, used to confirm Elasticsearch and Kibana started successfully.

*Expected Result:*

Two running containers should be displayed:

- elasticsearch
- kibana

Both containers should report a status of Up.

![docker ps](/screenshots/03-Elastic-Deployment/06-docker-containers-running.png)

**5. Verify Elasticsearch**
```bash
sudo curl http://localhost:9200
```
*Reason:* Verifies Elasticsearch is responding.

### 6. Validate the deployment

From a separate machine, open a web browser and navigate to the following URLs:

- **Kibana:** `http://192.168.1.220:5601`
- **Elasticsearch:** `http://192.168.1.220:9200`

![Kibanaa Validation](/screenshots/03-Elastic-Deployment/07-kibana-homepage.png)

![Elasticsearch Validation](/screenshots/03-Elastic-Deployment/08-elasticsearch-browser-response.png)



# Related Documentation

| Document 					        | Purpose 																																	                                                                |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|
| README.md					        | Provides a high-level overview of the ELK Stack SIEM Home Lab, including objectives, architecture, technologies, and documentation index.	|
| 01-Architecture.md		    | Defines the lab architecture, infrastructure, networking, identity services, Elastic components, and system relationships.				        |
| 02-Initial-Design.md		  | Documents the original objectives, requirements, constraints, technology selections, and architectural decisions.							            |
| 04-Kibana-Fleet-Setup.md  | Documents Kibana configuration, Fleet Server setup, Elastic Agent enrollment, integrations, policies, and dashboards.						          |
| 05-Windows-AD.md 			    | Documents Active Directory, DNS, organizational structure, and identity management configuration.											                    |
| 06-Windows-Agent.md 		  | Documents the deployment, enrollment, and configuration of Elastic Agents on Windows endpoints.											                      |
| 07-Sysmon.md 				      | Documents Sysmon installation, configuration, and Windows endpoint visibility improvements.												                        |
| 08-Elastic-Security.md 	  | Documents Elastic Security configuration, including detections, alerts, cases, and analyst workflows.										                  |
| 09-Detection-Rules.md 	  | Documents custom detection rules, testing procedures, and MITRE ATT&CK mappings.															                            |
| 10-Incident-Response.md   | Documents incident response workflows, investigations, evidence collection, and lessons learned.											                    |
| 99-Lab-Journal.md			    | Documents lab progress, implementation activities, troubleshooting, decisions, and lessons learned.										                    |

---

# Revision History

| Version | Date	  		| Changes 									                           	|
|---------|-------------|-------------------------------------------------------|
| v0.1.0  | 2026-07-09  | Initial Elastic Deployment documentation published    |


---	
