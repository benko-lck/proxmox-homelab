# Mini Incident Report: SSH Failed Login and Web Path Probing

## Summary

This mini incident report documents a controlled lab scenario involving SSH failed login attempts, basic web path probing, and internal reconnaissance activity.

The activity was generated inside a personal Proxmox home lab for Blue Team/SOC learning purposes.

No real production systems, company data, credentials, or public-facing services were involved.

---

## Environment

| System            | Role                                         |
| ----------------- | -------------------------------------------- |
| Linux Client LXC  | Source system used to generate test activity |
| Debian Server LXC | Target system running SSH and nginx          |
| Proxmox VE Host   | Local virtualization platform                |

Services involved:

| Service |   Port | Purpose                               |
| ------- | -----: | ------------------------------------- |
| SSH     | 22/tcp | Remote administration                 |
| nginx   | 80/tcp | Web server used for HTTP log analysis |

All hostnames, usernames, and IP addresses are intentionally sanitized.

---

## Observed Activity

The following controlled activity was performed from the lab client toward the lab server:

* SSH login attempt with an invalid username
* SSH failed password attempts for a valid user
* HTTP requests to test paths such as `/admin`, `/login`, and `/test`
* Internal nmap scan against the lab server
* Review of SSH and nginx logs on the target system

---

## Evidence Sources

SSH-related evidence:

```bash
journalctl -u ssh
```

nginx web access logs:

```bash
/var/log/nginx/access.log
```

nginx error logs:

```bash
/var/log/nginx/error.log
```

Network/service enumeration:

```bash
nmap -sV <TARGET_LAB_SERVER>
```

---

## Key Findings

### SSH Activity

Observed SSH log patterns included:

```text
Invalid user
Failed password
Connection closed
Accepted publickey
Accepted password
```

These events are useful for practicing detection of:

* brute-force attempts
* invalid username guessing
* failed password authentication
* successful administrative login
* source tracking

---

### Web Path Probing

HTTP requests were sent to paths such as:

```text
/
 /test
/admin
/login
```

In a real environment, repeated requests to sensitive or common administrative paths may indicate reconnaissance or automated scanning.

Examples of suspicious paths often seen during web probing:

```text
/admin
/login
/wp-admin
/phpmyadmin
/backup
```

---

### Port Scan

An internal nmap scan identified the expected exposed services on the lab server:

```text
22/tcp open ssh
80/tcp open http
```

This was expected because SSH and nginx were intentionally enabled for the lab.

---

## Assessment

This activity appears to be controlled reconnaissance and authentication testing performed from a known lab client.

No unauthorized access was observed.

The activity is useful for practicing:

* Linux log review
* SSH authentication analysis
* web access log review
* basic incident documentation
* SOC investigation thinking

---

## Response Actions

The following response steps were performed:

* Verified the source system
* Confirmed failed SSH login attempts
* Confirmed suspicious HTTP path requests in nginx logs
* Confirmed exposed services with nmap
* Verified that the activity was generated intentionally inside the lab
* Confirmed that no unauthorized login occurred

---

## Lessons Learned

* SSH failed login attempts can be reviewed through `journalctl`
* nginx access logs show source, method, path, status code, and user-agent
* Internal scans may not always appear clearly in application logs
* Separating lab client and lab server makes event generation easier
* Mini incident reports help connect raw logs with SOC-style investigation notes

---

## Next Steps

Planned improvements:

* Build a Bash script to summarize SSH failed login attempts
* Build an nginx suspicious path parser
* Add centralized logging with rsyslog
* Expand the lab with Windows Server and Active Directory
* Add Windows Event ID and Sysmon analysis later
