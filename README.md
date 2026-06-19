# Proxmox Blue Team Home Lab

This repository documents my beginner-friendly Proxmox home lab built for IT, System Administration, Networking, and Blue Team/SOC learning.

The goal of this lab is to practice real operational workflows in a safe local environment, including Linux administration, SSH hardening, firewall configuration, web server logging, basic reconnaissance simulation, and incident documentation.

> This is a personal learning lab. It does not contain production data, real company data, credentials, or exposed services.

---

## Lab Goals

* Build a small local virtualization lab using Proxmox VE
* Practice Linux server administration
* Configure SSH, UFW firewall, and nginx
* Generate and analyze basic security logs
* Simulate controlled SSH failed login attempts
* Simulate basic web path probing against a test server
* Practice writing mini incident reports
* Later expand into Active Directory, Windows Event Logs, Sysmon, Wazuh, and SOC-style detections

---

## Current Lab Design

The lab currently contains:

| Component         | Role                                                          |
| ----------------- | ------------------------------------------------------------- |
| Proxmox VE Host   | Local virtualization host                                     |
| Debian Server LXC | Linux server running SSH, UFW, and nginx                      |
| Linux Client LXC  | Test client used for curl, SSH tests, and controlled scanning |

All IP addresses, usernames, and hostnames in this repository are sanitized placeholders.

Example topology:

```text
Main PC
  |
  | Browser / SSH
  |
Local Router / Lab Network
  |
  | Ethernet
  |
Proxmox VE Host
  |
  |-- Debian Server LXC    SSH + nginx + logs
  |
  |-- Linux Client LXC     curl + nmap + SSH client
```

---

## Completed Work

### Proxmox Host

* Installed Proxmox VE on an old laptop
* Configured the host for local lab use
* Disabled enterprise repositories and enabled the no-subscription repository
* Fixed DNS resolution for package updates
* Configured the laptop so closing the lid does not suspend the host
* Added basic low-power tuning
* Confirmed that containers and virtual machines are started manually only when needed

### Debian Server LXC

* Created a Debian LXC container as a lab server
* Assigned a static lab IP address
* Installed basic administration tools
* Created a non-root administrative user
* Configured SSH access
* Tested SSH key-based login
* Disabled direct root SSH login
* Installed and configured UFW firewall
* Allowed only SSH and HTTP traffic
* Installed nginx
* Created a simple static test web page
* Reviewed nginx access and error logs

### Linux Client LXC

* Created a second Debian LXC container as a test client
* Installed curl, SSH client, and nmap
* Tested connectivity to the Debian server
* Generated HTTP requests toward the nginx server
* Generated controlled SSH failed login attempts
* Ran a controlled nmap scan inside the lab network

---

## Security Testing Performed

### SSH Failed Login Testing

Controlled SSH login attempts were generated from the lab client toward the lab server.

Observed log patterns included:

```text
Invalid user
Failed password
Accepted publickey
Accepted password
Connection closed
```

Detection questions practiced:

* Which username was attempted?
* What was the source system?
* How many failed attempts occurred?
* Was there a successful login?
* Does the activity look like normal administration or brute-force behavior?

---

### Web Path Probing

Controlled HTTP requests were sent toward paths such as:

```text
/
 /test
/admin
/login
```

The nginx access log was reviewed to identify:

* source system
* requested path
* HTTP method
* HTTP status code
* user-agent
* repeated suspicious paths

SOC-style interpretation:

Attackers often probe web servers for common admin or exposed application paths such as:

```text
/admin
/login
/wp-admin
/phpmyadmin
/backup
```

In a real environment, this type of behavior could be investigated through web server logs, firewall logs, IDS/IPS alerts, reverse proxy logs, or SIEM detections.

---

## Mini Incident Report

A mini incident report was created for a controlled lab scenario involving:

* invalid SSH username attempts
* failed password attempts
* HTTP requests to suspicious paths
* controlled nmap scanning
* confirmation that no unauthorized login occurred

The report is stored in:

```text
incidents/mini-incident-ssh-web-probing.md
```

---

## Current Services

| Service          | Purpose                                                |
| ---------------- | ------------------------------------------------------ |
| SSH              | Remote administration of the Debian server             |
| UFW              | Host-based firewall                                    |
| nginx            | Web server used for HTTP logging practice              |
| journalctl       | SSH service log review                                 |
| nginx access.log | Web request log review                                 |
| nmap             | Controlled internal lab scanning from the Linux client |

---

## Roadmap

Planned next phases:

### Phase 1 — SSH Failed Login Parser

Create a Bash script that extracts SSH failed login activity from system logs and summarizes:

* failed password count
* invalid user count
* source IP addresses or lab hosts
* last relevant log entries

### Phase 2 — nginx Suspicious Path Report

Create a simple log analysis script for nginx access logs that identifies suspicious paths such as:

```text
/admin
/login
/wp-admin
/phpmyadmin
/backup
```

### Phase 3 — Centralized Logging

Add a central logging component using rsyslog or another lightweight logging approach.

### Phase 4 — Windows / Active Directory Lab

Add Windows Server and Windows client machines when resources allow.

Planned topics:

* Active Directory
* DNS
* users and groups
* Organizational Units
* Group Policy
* domain join
* Windows Event ID 4624 and 4625
* Sysmon

### Phase 5 — SOC / Blue Team Expansion

Future SOC-focused expansion:

* Wazuh agent
* Wazuh server if hardware resources allow
* Windows Event Log analysis
* Sysmon event review
* brute-force detection
* incident timeline building

---

## Safety Notes

* This lab is local only.
* No Proxmox panel or internal service is exposed to the internet.
* No port forwarding is used.
* No real company data is used.
* No credentials, secrets, SSH keys, or production information are stored in this repository.
* All scanning is performed only inside my own lab environment.
* Snapshots or backups are created before major changes.
