#Incident Detection & Response Case Study: Unauthorized Access Investigation

**Author:** Khanyisile Natashie Nkosi  
**Role:** Cybersecurity Analyst (Simulation)  
**Framework Followed:** NIST SP 800-61 r2 (Incident Handling Guide)  

---

##Executive Summary
In this simulated incident, an organization’s network monitoring system flagged multiple unauthorized login attempts originating from an external IP address, followed by successful access to a sensitive database server. 

As the Security Analyst, I investigated the alerts, analyzed the authentication logs, identified the compromised account, executed containment procedures, and provided recommendations to prevent future incidents.

---

##phase 1: Detection & Analysis

### 1. The Alert
The Security Operations Center (SOC) triggered a high-severity alert:
* **Alert Trigger:** `Brute-Force Authentication Attempt Detected` followed by `Successful Login from Anomalous IP`.
* **Target System:** Production Database Server (`DB-SRV-01`)
* **Timestamp:** 2026-05-11 02:15:30 UTC

### 2. Log Analysis & Evidence Gathering
To investigate, I analyzed the authentication log files (`/var/log/auth.log`) using Linux command-line utilities.

**Evidence Log Snippet Analyzed:**
```text
May 11 02:10:15 DB-SRV-01 sshd[1402]: Failed password for invalid user admin from 198.51.100.42 port 49210 ssh2
May 11 02:11:02 DB-SRV-01 sshd[1405]: Failed password for invalid user root from 198.51.100.42 port 49212 ssh2
May 11 02:12:45 DB-SRV-01 sshd[1410]: Failed password for user jsmith from 198.51.100.42 port 49215 ssh2
May 11 02:13:01 DB-SRV-01 sshd[1412]: Accepted password for user jsmith from 198.51.100.42 port 49218 ssh2
