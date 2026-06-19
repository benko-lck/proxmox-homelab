# Lab Overview

## Purpose

This Proxmox home lab is built for practical IT, Linux administration, networking, and Blue Team/SOC learning.

The lab is designed to simulate small enterprise-style scenarios in a safe local environment.

Main goals:

* practice Proxmox VE basics
* manage Linux LXC containers
* configure SSH securely
* configure a host-based firewall
* run and monitor a web server
* generate controlled security events
* analyze logs
* document findings in incident-style reports

---

## Current Architecture

The lab currently contains:

| Component         | Purpose                                                    |
| ----------------- | ---------------------------------------------------------- |
| Proxmox VE Host   | Local virtualization platform                              |
| Debian Server LXC | Linux server running SSH, UFW, and nginx                   |
| Linux Client LXC  | Test client for curl, SSH testing, and controlled scanning |

Simplified topology:

```text
Main PC
  |
Local Network
  |
Proxmox VE Host
  |
  |-- Debian Server LXC
  |     |-- SSH
  |     |-- UFW
  |     |-- nginx
  |     |-- system logs
  |
  |-- Linux Client LXC
        |-- curl
        |-- SSH client
        |-- nmap
```

All private IP addresses, usernames, and hostnames are intentionally removed from the public documentation.

---

## Current Services

| Service | System            | Purpose                                              |
| ------- | ----------------- | ---------------------------------------------------- |
| SSH     | Debian Server LXC | Remote administration and authentication log testing |
| UFW     | Debian Server LXC | Basic firewall control                               |
| nginx   | Debian Server LXC | Web server and HTTP log generation                   |
| curl    | Linux Client LXC  | HTTP request testing                                 |
| nmap    | Linux Client LXC  | Controlled internal service enumeration              |

---

## Security Scenarios Practiced

### SSH Authentication Testing

The lab was used to generate and review:

* invalid username attempts
* failed password attempts
* successful login events
* SSH service logs

### Web Path Probing

The lab was used to generate HTTP requests toward common test paths such as:

```text
/admin
/login
/test
```

These requests were reviewed through nginx access logs.

### Internal Port Scan

The lab client was used to run a controlled scan against the lab server to identify intentionally exposed services.

---

## SOC Learning Value

This lab helps practice the basic investigation workflow:

```text
Event happens
    ↓
Log is generated
    ↓
Analyst reviews evidence
    ↓
Activity is classified
    ↓
Findings are documented
```

This is the same thinking process used in SOC environments, just on a smaller and safer lab setup.

---

## Current Status

Completed:

* Proxmox VE installed
* Debian server LXC created
* Linux client LXC created
* SSH configured
* root SSH login disabled
* UFW firewall enabled
* nginx installed and tested
* nginx logs reviewed
* SSH logs reviewed
* controlled failed login attempts generated
* controlled web path probing generated
* controlled nmap scan performed
* mini incident report created

---

## Planned Expansion

Future phases:

* SSH failed-login parser
* nginx suspicious path parser
* centralized logging
* Windows Server VM
* Active Directory lab
* Windows client VM
* Windows Event ID analysis
* Sysmon
* Wazuh
* incident timeline building
