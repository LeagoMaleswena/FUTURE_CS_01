# API Security Risk Analysis Report
## Future Interns — Cyber Security Internship Task 3 (2026)

---

> **Leago Maleswena**
> **Date:** 09 May 2026
> **Program:** Future Interns — Cyber Security Track
> **Report Type:** Read-Only API Security Risk Analysis
> **Classification:** Educational — Authorized Public APIs

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Scope & Methodology](#2-scope--methodology)
3. [APIs Tested](#3-apis-tested)
4. [Tools Used](#4-tools-used)
5. [Test Results & Findings](#5-test-results--findings)
6. [Risk Classification Summary](#6-risk-classification-summary)
7. [OWASP API Top 10 Mapping](#7-owasp-api-top-10-mapping)
8. [Business Impact Analysis](#8-business-impact-analysis)
9. [Remediation Recommendations](#9-remediation-recommendations)
10. [Conclusion](#10-conclusion)

---

## 1. Executive Summary

A read-only API Security Risk Analysis was conducted against two publicly authorized test APIs — **JSONPlaceholder** and **ReqRes** — as part of the Future Interns Cyber Security Internship Program (2026). All testing was passive and non-destructive, performed using Postman against APIs explicitly maintained for security testing and learning.

The analysis revealed **7 security findings** ranging from critical authentication failures to missing security headers and sensitive data over-exposure.

### Findings at a Glance

| Risk Level | Count | Examples |
|------------|-------|---------|
| 🔴 High | 4 | No authentication, IDOR, CORS misconfiguration, excessive data exposure |
| 🟡 Medium | 2 | Missing security headers, framework disclosure |
| 🟢 Low | 1 | Rate limit present but low for sensitive data |

### Key Finding

JSONPlaceholder returns the **full personal data of all users** — including names, emails, home addresses with GPS coordinates, and phone numbers — to **any request with zero authentication**. In a real production application, this single flaw could constitute a reportable data breach under POPIA and GDPR.

---

## 2. Scope & Methodology

### What Was Tested (In Scope)
- Public GET endpoints without authentication
- Individual resource access by ID (IDOR testing)
- Response headers and security controls
- Authentication mechanisms — comparing both APIs
- Error responses for information leakage

### What Was NOT Tested (Out of Scope)
- SQL injection or malicious input
- Authentication bypass or credential stuffing
- Data modification with malicious payloads
- Denial-of-service or rate limit abuse
- Any private or production API

### Methodology

Each test followed this structured process:

```
1. Identify endpoint to test
2. Send request via Postman with no authentication
3. Inspect the response — status code, body, and headers
4. Document what data was returned and what controls were missing
5. Classify the risk level (High / Medium / Low)
6. Map to relevant OWASP API Security Top 10 category
7. Write business-language explanation and remediation step
```

---

## 3. APIs Tested

| API | Base URL | Authentication Required |
|-----|----------|------------------------|
| **JSONPlaceholder** | https://jsonplaceholder.typicode.com | None |
| **ReqRes** | https://reqres.in | API Key required |

Both APIs are publicly documented and maintained for developer testing. No real user data was accessed during this assessment.

---

## 4. Tools Used

| Tool | Purpose |
|------|---------|
| **Postman** (Web) | API request testing and response inspection |
| **OWASP API Security Top 10 (2023)** | Risk classification framework |
| **Browser DevTools** | Additional header inspection |

---

## 5. Test Results & Findings

---

### Test 1 — Unauthenticated Access to All Users

**Request:**
```
GET https://jsonplaceholder.typicode.com/users
Authentication: None
```

**Response:**
```
Status: 200 OK | Time: 89ms | Size: 2.93 KB
```

The API returned complete personal data for all 10 users with zero authentication — including full names, usernames, email addresses, home addresses, GPS coordinates, phone numbers, websites, and company information.

**Sample data returned (User 1):**
```json
{
  "id": 1,
  "name": "Leanne Graham",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "city": "Gwenborough",
    "geo": { "lat": "-37.3159", "lng": "81.1496" }
  },
  "phone": "1-770-736-0831 x56442"
}
```

---

**Finding F-01: Unauthenticated Access to User Data**

| Field | Detail |
|-------|--------|
| **Risk Level** | 🔴 High |
| **OWASP Category** | API2:2023 — Broken Authentication |
| **Endpoint** | GET /users |

**Description:** Any person on the internet with no account or credentials can retrieve the complete personal profile of every user in the system with a single request. No username, password, token, or API key is required.

**Business Impact:** In a real application, this constitutes a data breach under POPIA (South Africa) and GDPR (Europe). The company faces regulatory fines, legal liability, and reputational damage. Exposed GPS coordinates could also enable physical harm to users.

**Remediation:** Require a valid authentication token on every endpoint before returning data. Unauthenticated requests must return `401 Unauthorized`.

---

**Finding F-02: Excessive Data Exposure**

| Field | Detail |
|-------|--------|
| **Risk Level** | 🔴 High |
| **OWASP Category** | API3:2023 — Broken Object Property Level Authorization |
| **Endpoint** | GET /users |

**Description:** Even if authentication were required, the API returns far more data than necessary — GPS coordinates, full addresses including suite numbers, and internal company metadata are all returned by default.

**Business Impact:** Excessive data exposure increases breach severity. Attackers receive a complete dossier on every user rather than just what is needed.

**Remediation:** Implement response filtering — only return fields explicitly needed by the requesting client. Sensitive fields like GPS coordinates should require elevated permissions.

---

### Test 2 — Insecure Direct Object Reference (IDOR)

**Requests:**
```
GET https://jsonplaceholder.typicode.com/users/1
GET https://jsonplaceholder.typicode.com/users/5
```

**Responses:**
```
/users/1 → 200 OK → Leanne Graham (full profile)
/users/5 → 200 OK → Chelsey Dietrich (full profile)
```

By simply changing the number in the URL — from `/users/1` to `/users/5` — a completely different user's full data was returned instantly. No authorisation check was performed.

---

**Finding F-03: Insecure Direct Object Reference (IDOR)**

| Field | Detail |
|-------|--------|
| **Risk Level** | 🔴 High |
| **OWASP Category** | API1:2023 — Broken Object Level Authorization |
| **Endpoints** | GET /users/1, GET /users/5, GET /users/{any id} |

**Description:** There is no check to verify whether the person making the request is authorised to view a specific user's record. An attacker can enumerate through every user ID (1, 2, 3, 4...) and systematically harvest every user's data from the entire database.

**Business Impact:** IDOR is the #1 API vulnerability in the OWASP Top 10. It has caused some of the largest data breaches in history. A single IDOR flaw can expose an entire user database to anyone who discovers it.

**Remediation:** After authenticating, verify the user owns the requested resource before returning it. Return `403 Forbidden` if a user requests another user's data:
```javascript
if (requestedUserId !== authenticatedUserId) {
  return res.status(403).json({ error: 'Access forbidden' });
}
```

---

### Test 3 — Response Header Security Analysis

**Request:**
```
GET https://jsonplaceholder.typicode.com/users/5
```

**Headers Returned (Response):**

| Header | Value | Assessment |
|--------|-------|------------|
| `x-content-type-options` | nosniff | ✅ Present |
| `x-ratelimit-limit` | 1000 | ✅ Rate limiting active |
| `x-ratelimit-remaining` | 999 | ✅ Counter working |
| `x-powered-by` | Express | 🔴 Framework disclosed |
| `server` | cloudflare | 🟡 Infrastructure disclosed |
| `access-control-allow-credentials` | true | 🔴 Dangerous CORS setting |
| `Content-Security-Policy` | MISSING | 🔴 Absent |
| `Strict-Transport-Security` | MISSING | 🟡 Absent |
| `X-Frame-Options` | MISSING | 🟡 Absent |

---

**Finding F-04: Framework and Technology Disclosure**

| Field | Detail |
|-------|--------|
| **Risk Level** | 🟡 Medium |
| **OWASP Category** | API8:2023 — Security Misconfiguration |
| **Header** | `x-powered-by: Express` |

**Description:** The API advertises it is built on Express.js in every response. Attackers can look up all known Express.js vulnerabilities and version-specific exploits.

**Remediation:** Disable the header in Express:
```javascript
app.disable('x-powered-by');
```

---

**Finding F-05: Dangerous CORS Misconfiguration**

| Field | Detail |
|-------|--------|
| **Risk Level** | 🔴 High |
| **OWASP Category** | API8:2023 — Security Misconfiguration |
| **Header** | `access-control-allow-credentials: true` |

**Description:** This setting allows any website on the internet to make authenticated requests to this API on behalf of a logged-in user — enabling Cross-Site Request Forgery (CSRF) attacks where a malicious site silently makes API calls as the victim.

**Remediation:** Restrict CORS to specific trusted origins only:
```javascript
// Never use wildcard with credentials: true
cors({ origin: 'https://yourtrustedapp.com', credentials: true })
```

---

**Finding F-06: Missing Critical Security Headers**

| Field | Detail |
|-------|--------|
| **Risk Level** | 🟡 Medium |
| **OWASP Category** | API8:2023 — Security Misconfiguration |

**Description:** Three important security headers were absent from all responses:

| Missing Header | Impact |
|---|---|
| `Content-Security-Policy` | No XSS protection |
| `Strict-Transport-Security` | HTTPS not enforced |
| `X-Frame-Options` | Clickjacking possible |

**Remediation:** Use the `helmet` middleware to add all security headers at once:
```javascript
const helmet = require('helmet');
app.use(helmet());
```

---

### Test 4 — Authentication Control Comparison (ReqRes)

**Request:**
```
POST https://reqres.in/api/login
Authentication: None
```

**Response:**
```
Status: 401 Unauthorized | Time: 496ms
```

**Response Body:**
```json
{
  "message": "The x-api-key header is required for this endpoint.",
  "hint": "Send x-api-key for admin calls or Authorization: Bearer <token>"
}
```

---

**Finding F-07: Positive Control — Proper Authentication Enforced**

| Field | Detail |
|-------|--------|
| **Risk Level** | ✅ Informational (Positive Finding) |
| **Status** | 401 Unauthorized — correct and expected behaviour |

**Description:** Unlike JSONPlaceholder, ReqRes correctly rejected the unauthenticated request with `401 Unauthorized`. No user data was returned. This is the expected behaviour for a secure API.

**Comparison:**

| Behaviour | JSONPlaceholder | ReqRes |
|-----------|----------------|--------|
| Unauthenticated request result | Returns all user data | 401 Unauthorized |
| Data returned without auth | Yes — full PII | No |
| Authentication required | No | Yes |
| Behaviour correct | No | Yes |

---

## 6. Risk Classification Summary

| # | Finding | Risk | OWASP |
|---|---------|------|-------|
| F-01 | Unauthenticated access to all user data | 🔴 High | API2 |
| F-02 | Excessive data exposure (GPS, full address) | 🔴 High | API3 |
| F-03 | IDOR — any user's data accessible by ID | 🔴 High | API1 |
| F-04 | Framework disclosure (Express.js) | 🟡 Medium | API8 |
| F-05 | Dangerous CORS misconfiguration | 🔴 High | API8 |
| F-06 | Missing security headers (CSP, HSTS, XFO) | 🟡 Medium | API8 |
| F-07 | Rate limit of 1000 low for sensitive data | 🟢 Low | API4 |

---

## 7. OWASP API Top 10 Mapping

| OWASP ID | Category | Finding |
|----------|----------|---------|
| API1:2023 | Broken Object Level Authorization | F-03 (IDOR) |
| API2:2023 | Broken Authentication | F-01 (No auth) |
| API3:2023 | Broken Object Property Level Authorization | F-02 (Excessive exposure) |
| API4:2023 | Unrestricted Resource Consumption | F-07 (Rate limiting) |
| API8:2023 | Security Misconfiguration | F-04, F-05, F-06 |

---

## 8. Business Impact Analysis

**Scenario 1 — Data Breach via Unauthenticated Access (F-01, F-02)**
An attacker calls `/users` once and downloads the complete user database in under a second — names, emails, addresses, GPS locations. The company faces mandatory breach notification, regulatory fines up to 4% of annual revenue under GDPR, and customer trust loss.

**Scenario 2 — Mass Account Harvesting via IDOR (F-03)**
A malicious user loops through `/users/1` through `/users/10000` and harvests every account's data while appearing as a legitimate user in the logs. No alarms are triggered because each request looks normal individually.

**Scenario 3 — Invisible Attack via CORS Misconfiguration (F-05)**
A malicious website loads invisibly in a victim's browser and silently makes authenticated API calls on their behalf — reading their data, draining accounts, or posting on their behalf — without the user ever knowing.

---

## 9. Remediation Recommendations

| Priority | Finding | Fix | Complexity |
|----------|---------|-----|-----------|
| Immediate | F-01: No authentication | Add auth middleware to all routes | Medium |
| Immediate | F-03: IDOR | Add object-level authorisation checks | Medium |
| Immediate | F-05: CORS | Restrict to trusted origins only | Low |
| High | F-02: Excessive data | Implement response field filtering | Medium |
| Medium | F-04: Framework disclosure | `app.disable('x-powered-by')` | Low |
| Medium | F-06: Missing headers | Add `helmet` middleware | Low |
| Low | F-07: Rate limiting | Reduce limit on sensitive endpoints | Low |

---

## 10. Conclusion

This API security assessment identified **7 findings**, including 4 rated High severity. The most critical issues — unauthenticated access, IDOR, and CORS misconfiguration — represent foundational security failures that would be classified as reportable incidents under most data protection frameworks.

The comparison with ReqRes demonstrated clearly what secure API behaviour looks like — a `401 Unauthorized` response that protects data without leaking system information. This is the standard all production APIs must meet.

APIs are the backbone of modern software. A single insecure endpoint can expose an entire database and bypass all authentication at scale. The findings and remediations in this report reflect the standard of care expected of a professional AppSec engineer or API security consultant.

---

*This report was produced as part of the Future Interns Cyber Security Internship Program (2026). All testing was conducted ethically within the explicitly authorized scope of publicly available test APIs.*

**Report End**
