# Automated System Security Auditing with Python

**Author:** Khanyisile Natashie Nkosi  
**Language:** Python 3  
**Focus:** Security Automation, Log Parsing, Threat Detection  

---

## Project Overview
Manual log analysis is time-consuming and prone to human error. To solve this, I developed a lightweight Python utility that automates the audit of system authorization logs (`auth.log`). 

The script scans for failed login attempts, counts the failures per IP address, and automatically flags any IP exceeding a specific threshold (e.g., more than 5 failed attempts) as a potential **brute-force threat**.

---

##  The Python Script (`security_audit.py`)

Here is the source code for the automated auditing tool. It uses Python's built-in `re` (Regular Expressions) module to parse log lines and extract IP addresses.

```python
import re
from collections import Counter

# Configuration
LOG_FILE_PATH = "auth.log"
FAILED_THRESHOLD = 5

def analyze_security_logs(log_path):
    print("[-] Starting automated security log audit...")
    failed_attempts = []
    
    # Regex pattern to match failed SSH logins and extract the IP address
    # Example log: "Failed password for invalid user admin from 192.168.1.50 port 22"
    failed_regex = r"Failed password for .* from (\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3})"

    try:
        with open(log_path, "r") as file:
            for line in file:
                match = re.search(failed_regex, line)
                if match:
                    # Extract the IP address from the regex group
                    ip_address = match.group(1)
                    failed_attempts.append(ip_address)
                    
    except FileNotFoundError:
        print(f"[!] Error: The log file '{log_path}' was not found.")
        return

    # Aggregate and count failed attempts per IP
    ip_counts = Counter(failed_attempts)
    
    # Display the results
    print("\n[📊] SECURITY AUDIT SUMMARY:")
    print("-" * 50)
    print(f"{'IP Address':<20} | {'Failed Attempts':<15} | {'Status':<15}")
    print("-" * 50)
    
    threats_detected = 0
    for ip, count in ip_counts.items():
        if count >= FAILED_THRESHOLD:
            status = "🚨 CRITICAL (Flagged)"
            threats_detected += 1
        else:
            status = "🟢 Low Risk"
            
        print(f"{ip:<20} | {count:<15} | {status:<15}")
        
    print("-" * 50)
    print(f"[✓] Audit complete. Threats flagged: {threats_detected}\n")

if __name__ == "__main__":
    analyze_security_logs(LOG_FILE_PATH)
