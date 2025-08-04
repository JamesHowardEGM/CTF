# Reconnaissance Report – HTB: Editor

## Overview

This report documents the reconnaissance phase of the Editor machine on Hack The Box. The objective of this phase is to systematically identify all externally exposed services, enumerate potential vulnerabilities, and develop an informed plan of attack based on the findings.

---

## Target Information

- **Machine Name**: Editor
- **Operating System**: Linux
- **Difficulty**: Easy
- **IP Address**: `10.10.11.80`
- **Date of Test**: [Insert date]

---

## Methodology

The following steps were taken during reconnaissance:

1. **Host Discovery**
2. **Port Scanning**
3. **Service Enumeration**
4. **Directory & File Brute Forcing**
5. **Technology Fingerprinting**
6. **Vulnerability Research**

---

## 1. Host Discovery

Initial ping test to confirm host availability:

```bash
ping -c 4 10.10.11.80
```

> Host responded to ICMP echo requests. Proceeded to port scan.

---

## 2. Port Scanning

### Full TCP Scan (Top 1000 ports)

```bash
nmap -Pn -sS -T4 -p- --min-rate=1000 -oN full-port-scan.txt 10.10.11.80
```

### Targeted Service Version Scan

```bash
nmap -sCV -p 22,80 -oN targeted-service-scan.txt 10.10.11.80
```

**Open Ports Identified**:
| Port | Service | Version |
|------|---------|---------|
| 22   | SSH     | OpenSSH 8.x |
| 80   | HTTP    | Apache 2.x |

---

## 3. Service Enumeration

### HTTP Enumeration

- Visited `http://10.10.11.80` in browser.
- Ran directory brute force:
```bash
gobuster dir -u http://10.10.11.80 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 40 -x php,html,txt -o gobuster.txt
```

#### Findings:
- `/admin` returned HTTP 200
- `/editor` exposed a WYSIWYG CMS interface
- Possible CMS in use (TinyMCE or similar)

### SSH Enumeration

- Banner showed OpenSSH running on a modern Linux distribution
- Password authentication enabled (to be validated with brute force only if authorized)

---

## 4. Web Technology Fingerprinting

```bash
whatweb http://10.10.11.80
```

**Identified Technologies**:
- Apache Web Server
- Potential CMS (TinyMCE/WYSIWYG editor)
- jQuery 3.x (via source inspection)

---

## 5. Additional Scans

### Nikto

```bash
nikto -h http://10.10.11.80
```

- Identified outdated Apache version with known vulnerabilities
- Directory listing was disabled
- Found `/config.php.bak` (potential misconfigured backup)

---

## 6. Summary of Key Findings

- **Web application is publicly exposed and likely editable**
- **Potential CMS or admin portal at `/editor`**
- **Outdated Apache version may be exploitable**
- **Found potentially sensitive files during brute force**

---

## Next Steps

1. Investigate CMS/editor interface for upload vulnerabilities or XSS.
2. Examine source code or JS for hidden credentials.
3. Check Apache version for known exploits.
4. Begin local file inclusion (LFI) or command injection testing if input vectors found.

---

## Notes

All scans were performed from a Kali Linux environment using only passive and active reconnaissance tools within scope. No intrusive payloads or brute force methods were employed at this phase.

---

