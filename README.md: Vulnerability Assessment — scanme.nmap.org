# 🔐 Vulnerability Assessment — scanme.nmap.org

> **Future Interns Cyber Security Internship — Task 1 (2026)**

---

## 📋 Overview

This repository contains the deliverables for **Cyber Security Task 1** of the Future Interns internship program. The task involved conducting a passive, read-only vulnerability assessment against an authorized public target and producing a professional security audit report.

---

## 🎯 Website Tested

| Field | Detail |
|-------|--------|
| **Target** | [scanme.nmap.org](http://scanme.nmap.org) |
| **IP Address** | 45.33.32.156 |
| **Authorization** | Explicitly permitted by the Nmap Project for practice scanning |
| **Assessment Date** | 30 April 2026 |

> `scanme.nmap.org` is maintained by Insecure.org specifically to allow people to legally practice network scanning techniques. See: http://scanme.nmap.org

---

## 🔍 Scope

### ✅ In Scope
- Public-facing web pages
- Port and service identification (passive)
- HTTP response header analysis
- Cookie security flag inspection
- Automated passive vulnerability scanning

### ❌ Out of Scope
- Login bypass or authentication attacks
- Exploitation of any vulnerability
- Brute-force or denial-of-service attacks
- Any modification of server data or configuration

---

## 🛠️ Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| **Nmap** | 7.98 | Port scanning, service identification |
| **Firefox DevTools** | Built-in | HTTP header & cookie analysis |
| **OWASP ZAP** | 2.17.0 | Passive automated vulnerability scanning |
| **Kali Linux** | 2026.1 | Assessment environment |

---

## 📊 Findings Summary

| Risk Level | Count | Examples |
|------------|-------|---------|
| 🔴 **High** | 4 | Outdated Apache, No HTTPS, Insecure Cookies |
| 🟡 **Medium** | 5 | Missing Security Headers, SSH Exposed |
| 🟢 **Low / Info** | 1 | SameSite: None on Cookies |

Full findings are documented in the report.

---

## 📁 Repository Structure

```
vulnerability-assessment-scanme/
│
├── README.md                          ← This file
│
├── report/
│   └── VULNERABILITY_ASSESSMENT_REPORT.md   ← Full audit report
│
└── evidence/
    ├── nmap_scan_1.png                ← First Nmap scan output
    ├── nmap_scan_2.png                ← Second Nmap scan (extended)
    ├── devtools_headers.png           ← HTTP response headers
    ├── devtools_cookies.png           ← Cookie security flags
    ├── zap_spider.png                 ← OWASP ZAP spider running
    └── nmap_output.txt                ← Raw Nmap output (text)
```

---

## 📄 Report

The full vulnerability assessment report can be found here:
👉 [`report/VULNERABILITY_ASSESSMENT_REPORT.md`](./report/VULNERABILITY_ASSESSMENT_REPORT.md)

The report includes:
- Executive Summary
- Scope & Methodology
- 10 Documented Findings with risk classifications
- Evidence references
- Remediation recommendations
- Conclusion

---

## ⚖️ Ethics & Disclaimer

This assessment was conducted strictly within ethical boundaries:
- Only publicly accessible pages were analysed
- No exploitation, brute-force, or denial-of-service was attempted
- The target (`scanme.nmap.org`) is **explicitly authorized** for this type of scanning
- All activity was passive and read-only

This project was completed as part of an educational internship program and is intended to demonstrate security consulting skills, not offensive hacking.

---

## 🏫 Internship Program

**Program:** [Future Interns — Cyber Security Track](https://futureinterns.com/cyber-security-task-1-2026/)
**Task:** Task 1 — Vulnerability Assessment Report for a Live Website

---

*© 2026 — Submitted as part of Future Interns Cyber Security Internship*
