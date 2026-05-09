# 🔐 API Security Risk Analysis

> **Future Interns Cyber Security Internship — Task 3 (2026)**

---

## 📋 Overview

This repository contains the deliverables for **Cyber Security Task 3** of the Future Interns internship program. The task involved performing a read-only API Security Risk Analysis against two publicly authorized test APIs, identifying common API security vulnerabilities, and producing a professional security report in the format used by real AppSec engineers and security consultants.

---

## 🎯 APIs Tested

| API | URL | Type |
|-----|-----|------|
| **JSONPlaceholder** | https://jsonplaceholder.typicode.com | Free fake REST API — no authentication |
| **ReqRes** | https://reqres.in | Hosted test API — authentication required |

Both APIs are publicly documented and explicitly maintained for developer testing and security learning. No real user data was accessed during this assessment.

---

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| **Postman** (Web) | Sending API requests and inspecting responses |
| **OWASP API Security Top 10 (2023)** | Industry-standard risk classification framework |
| **Browser DevTools** | Additional header and response inspection |

---

## 📊 Findings Summary

| # | Finding | Risk | OWASP Category |
|---|---------|------|----------------|
| F-01 | Unauthenticated access to all user data | 🔴 High | API2 — Broken Authentication |
| F-02 | Excessive data exposure (GPS, full address) | 🔴 High | API3 — Broken Object Property Auth |
| F-03 | IDOR — any user accessible by changing URL ID | 🔴 High | API1 — Broken Object Level Auth |
| F-04 | Framework disclosure (Express.js exposed) | 🟡 Medium | API8 — Security Misconfiguration |
| F-05 | Dangerous CORS misconfiguration | 🔴 High | API8 — Security Misconfiguration |
| F-06 | Missing security headers (CSP, HSTS, XFO) | 🟡 Medium | API8 — Security Misconfiguration |
| F-07 | Rate limit of 1000 low for sensitive data | 🟢 Low | API4 — Unrestricted Resource Consumption |

---

## 🧪 Tests Performed

### Test 1 — Unauthenticated Access
```
GET https://jsonplaceholder.typicode.com/users
```
Sent with zero authentication. Returned full personal data (name, email, address, GPS, phone) for all 10 users. Result: **200 OK** — immediate high-severity finding.

### Test 2 — IDOR (Insecure Direct Object Reference)
```
GET https://jsonplaceholder.typicode.com/users/1
GET https://jsonplaceholder.typicode.com/users/5
```
Changed the ID number in the URL. Received a different user's full data each time with no authorisation check. Any user ID from 1 to 10 returns complete PII with zero authentication.

### Test 3 — Response Header Analysis
Inspected the response headers from the JSONPlaceholder API. Found dangerous CORS settings (`access-control-allow-credentials: true`), framework disclosure (`x-powered-by: Express`), and multiple missing security headers.

### Test 4 — Authentication Comparison (ReqRes)
```
POST https://reqres.in/api/login
```
Sent without authentication. ReqRes correctly returned **401 Unauthorized** — no data was exposed. This demonstrated what correct API authentication behaviour looks like compared to JSONPlaceholder.

---

## 📁 Repository Structure

```
api-security-risk-analysis/
│
├── README.md                              ← This file
│
├── report/
│   └── API_SECURITY_RISK_ANALYSIS_REPORT.md  ← Full analysis report
│
└── evidence/
    ├── test1_all_users_no_auth.png        ← GET /users — 200 OK, no auth
    ├── test2_user1_idor.png               ← GET /users/1 — IDOR test
    ├── test2_user5_idor.png               ← GET /users/5 — IDOR confirmed
    ├── test3_response_headers.png         ← Response headers analysis
    └── test4_reqres_401.png               ← 401 Unauthorized — correct behaviour
```

---

## 🔍 Methodology

1. Identify endpoint to test
2. Send request via Postman with no authentication
3. Inspect response — status code, body, and headers
4. Document what data was returned and what controls were missing
5. Classify the risk level (High / Medium / Low)
6. Map to OWASP API Security Top 10 (2023)
7. Write a business-language explanation and remediation step

> All testing was **read-only and passive**. No data was modified, no systems were exploited, and no denial-of-service activity was performed.

---

## ⚖️ Ethics & Authorization

- JSONPlaceholder is a **free, public fake API** created specifically for testing
- ReqRes is a **hosted REST API** built specifically for testing and learning
- All requests were **GET or POST with no malicious payload**
- No authentication was bypassed — the absence of authentication was simply observed and documented
- This is the same methodology used by professional API security auditors

---

## 💼 Why This Skill Matters

API security is one of the fastest-growing areas of cybersecurity:
- Over **83% of internet traffic** is now API traffic
- OWASP lists API security as a distinct, critical discipline
- API breaches affect companies including Facebook, Twitter, Optus, and T-Mobile
- API security audits are **billable professional services** offered by agencies worldwide

This task demonstrates skills relevant to roles including:
- AppSec Engineer
- Security Analyst
- SOC Analyst
- SaaS Security Consultant
- Penetration Tester (API track)

---

## 🏫 Internship Program

**Program:** [Future Interns — Cyber Security Track](https://futureinterns.com/cyber-security-task-3-2026/)
**Task:** Task 3 — API Security Risk Analysis

---

*© 2026 — Submitted as part of Future Interns Cyber Security Internship*
