# [Machine Name] – Penetration Test Report

**Target IP**: [e.g., 10.10.11.123]  
**Operating System**: [Linux / Windows]  
**Difficulty**: [Easy / Medium / Hard]  
**Date of Assessment**: [YYYY-MM-DD]  
**Assessor**: James Howard  
**Assessment Type**: External Black Box / Internal Authenticated / CTF Lab

---

## 1. Executive Summary

This report documents the penetration test performed against the target system identified as **[Machine Name]**. The objective of this engagement was to identify vulnerabilities that could be exploited to compromise the system and escalate privileges to administrative or root-level access. 

The system was successfully compromised through [insert brief method: e.g., exploitation of a vulnerable web application]. Post-exploitation techniques allowed for privilege escalation, culminating in full administrative control of the host.

---

## 2. Reconnaissance

### 2.1 Nmap Scan

**Command Executed:**
```bash
nmap -sC -sV -oN nmap.txt [target-ip]
```

**Findings:**
```
[Insert sanitized and formatted Nmap output or key service/version findings]
```

### 2.2 Web & Directory Enumeration

**Command Executed:**
```bash
gobuster dir -u http://[target-ip] -w /usr/share/wordlists/dirb/common.txt
```

**Discovered Paths:**
```
[Insert discovered endpoints, pages, or directories of interest]
```

---

## 3. Service Enumeration

Detailed enumeration was performed on exposed services to identify misconfigurations, default credentials, and potential vulnerabilities.

**Services Identified:**
- HTTP on port 80 running Apache 2.4.49 (potential CVE-2021-41773)
- OpenSSH 7.9p1 on port 22 (default config)
- PHPMyAdmin interface exposed at `/phpmyadmin/`

---

## 4. Exploitation

**Attack Vector:**  
A vulnerability in [e.g., Apache path traversal] was exploited to gain code execution on the server.

**Steps Taken:**
1. Exploited [describe vuln/CVE] to access restricted files.
2. Uploaded a web shell via [file upload/LFI/RCE].
3. Established a reverse shell connection to attacker machine.

**Command(s) Used:**
```bash
curl -v "http://[ip]/cgi-bin/.%2e/%2e%2e/%2e%2e/etc/passwd"
nc -nlvp 4444
```

---

## 5. Privilege Escalation

**Vector:**  
Post-exploitation, enumeration scripts revealed [e.g., a misconfigured SUID binary or exposed credentials].

**Techniques Used:**
- Enumerated with `linpeas.sh`
- Identified writable cron jobs or backup scripts
- Exploited [e.g., tar, awk, sudo permissions, kernel exploit]

**Command(s) Used:**
```bash
find / -perm -4000 -type f 2>/dev/null
sudo -l
```

Result: Obtained root shell via [brief explanation].

---

## 6. Post-Exploitation & Data Access

Once root access was achieved, sensitive data was accessed to demonstrate impact. Examples include:

- `/etc/shadow`
- `/root/root.txt`
- Application config files with hardcoded credentials
- Database dumps or tokens

No destructive changes were made. Data exfiltration was simulated and documented only.

---

## 7. Flags (CTF Context)

```
User Flag: HTB{example-user-flag}
Root Flag: HTB{example-root-flag}
```

---

## 8. Remediation Recommendations

| Issue                                 | Recommendation                              |
|--------------------------------------|---------------------------------------------|
| Exposed vulnerable service            | Patch to latest secure version              |
| Default credentials / weak auth       | Enforce strong credential policies          |
| Misconfigured SUID permissions        | Remove unnecessary SUID binaries            |
| Unrestricted file uploads             | Validate file types and implement controls  |

---

## 9. Lessons Learned

- Validate input and sanitize file access to prevent LFI/RCE.
- Regularly patch and monitor exposed services.
- Audit privilege escalation vectors on Linux hosts.
- Use tools like AppArmor or SELinux to restrict shell access.

---

## 10. Appendix

- `nmap.txt` output
- Screenshot evidence
- Reverse shell transcript
- Enumeration logs (`linpeas`, manual notes)
- Exploit scripts or payloads used
