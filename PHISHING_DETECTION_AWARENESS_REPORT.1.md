# Phishing Email Detection & Awareness Report
## Future Interns — Cyber Security Internship Task 2 (2026)

---

> **Prepared by:** [Your Name]
> **Date:** 01 May 2026
> **Program:** Future Interns — Cyber Security Track
> **Report Type:** Phishing Email Analysis & Employee Awareness Document
> **Classification:** Educational — Internal Use

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [What Is Phishing?](#2-what-is-phishing)
3. [Tools & Methodology](#3-tools--methodology)
4. [Phishing Email Samples Analysed](#4-phishing-email-samples-analysed)
   - [Sample 1 — Fake Account Suspension Alert](#sample-1--fake-account-suspension-alert)
   - [Sample 2 — Fake IT Department Password Reset](#sample-2--fake-it-department-password-reset)
   - [Sample 3 — Fake Invoice / Finance Request](#sample-3--fake-invoice--finance-request)
   - [Sample 4 — Fake Package Delivery Notification](#sample-4--fake-package-delivery-notification)
5. [Phishing Indicators Summary](#5-phishing-indicators-summary)
6. [Risk Classification](#6-risk-classification)
7. [How These Attacks Work](#7-how-these-attacks-work)
8. [Prevention Guidelines for Users](#8-prevention-guidelines-for-users)
9. [Do's and Don'ts for Employees](#9-dos-and-donts-for-employees)
10. [Conclusion](#10-conclusion)

---

## 1. Executive Summary

This report presents the findings of a phishing email detection exercise conducted as part of the Future Interns Cyber Security Internship Program (2026). Four realistic phishing email samples were analysed using email header inspection, domain verification, and URL analysis techniques.

Phishing remains the **number one attack vector** used by cybercriminals globally. According to industry data, over 90% of successful data breaches begin with a phishing email. The goal of this report is to help employees and everyday users recognise the warning signs of a phishing attack before it causes harm.

### Key Findings at a Glance

| Sample | Type | Risk Level | Primary Indicator |
|--------|------|------------|-------------------|
| Sample 1 | Account Suspension Scam | 🔴 High | Fake domain, urgency tactics |
| Sample 2 | IT Password Reset Scam | 🔴 High | Internal impersonation |
| Sample 3 | Invoice / Finance Scam | 🔴 High | CEO impersonation, wire transfer |
| Sample 4 | Parcel Delivery Scam | 🟡 Medium | Fake tracking link |

---

## 2. What Is Phishing?

Phishing is a type of **social engineering attack** where a cybercriminal impersonates a trusted person, company, or organisation in order to trick a victim into:

- Clicking a malicious link
- Downloading an infected file
- Entering their username and password on a fake website
- Sending money or gift cards
- Revealing sensitive personal or business information

The word "phishing" is a deliberate misspelling of "fishing" — attackers are casting a wide net and waiting for victims to take the bait.

### Common Types of Phishing

| Type | Description |
|------|-------------|
| **Email Phishing** | Mass emails sent to thousands of users impersonating known brands |
| **Spear Phishing** | Targeted emails personalised for a specific individual or company |
| **Whaling** | Targeted attacks against senior executives (CEOs, CFOs) |
| **Smishing** | Phishing via SMS text messages |
| **Vishing** | Phishing via phone calls |
| **Clone Phishing** | A legitimate email is copied and resent with a malicious link |

This report focuses on **email phishing**, the most prevalent form.

---

## 3. Tools & Methodology

### Tools Used

| Tool | Purpose |
|------|---------|
| **Google Admin Toolbox — Message Header Analyser** | Inspect email headers for spoofing indicators |
| **MXToolbox Email Header Analyser** | Verify SPF, DKIM, and DMARC records |
| **Browser DevTools** | Safely inspect links without clicking them |
| **WHOIS Lookup** | Check domain registration age and ownership |
| **VirusTotal** | Check URLs and domains against known threat databases |

### Analysis Process

For each phishing sample, the following steps were followed:

1. **Read the email body** — identify social engineering tactics (urgency, fear, authority)
2. **Inspect the sender address** — check for domain spoofing or lookalike domains
3. **Analyse email headers** — verify SPF/DKIM/DMARC pass/fail status
4. **Hover over links** — check where URLs actually point without clicking
5. **Check the domain** — look up registration age, ownership, and legitimacy
6. **Classify risk** — assign Safe / Suspicious / Phishing rating
7. **Document findings** — record all indicators and evidence

> **Safety Note:** All links and domains in this report were inspected using safe, passive tools only. No links were clicked and no credentials were entered at any point.

---

## 4. Phishing Email Samples Analysed

---

### Sample 1 — Fake Account Suspension Alert

#### Email Content

```
FROM:    security-alert@secure-accounts-verify.com
TO:      user@company.com
SUBJECT: ⚠️ URGENT: Your Account Will Be Suspended in 24 Hours

Dear Valued Customer,

We have detected unusual activity on your account. To prevent 
permanent suspension, you must verify your identity immediately.

👉 Click here to verify: http://secure-account-verify[.]com/login

This link expires in 24 hours. Failure to verify will result in 
your account being permanently locked and all data deleted.

Regards,
The Security Team
Account Protection Department
```

---

#### Header Analysis

| Header Field | Value | Verdict |
|---|---|---|
| From (Display) | "Security Team" | Deceptive — no company name |
| From (Actual) | security-alert@secure-accounts-verify.com | 🔴 Fake domain — not a real company |
| Reply-To | harvest-data@differentdomain.net | 🔴 Replies diverted to separate domain |
| SPF Check | FAIL | 🔴 Not authorised to send |
| DKIM Check | FAIL | 🔴 Email not signed |
| DMARC | None configured | 🔴 No anti-spoofing policy |
| Domain Registrar | Cloudflare, Inc. | 🟡 Often used to anonymise domain owners |
| Domain Status | Registered And No Website | 🔴 Domain exists but hosts no real content |
| IP History | 40 changes across 40 unique IPs over 9 years | 🔴 Highly unusual — evasion behaviour |

#### VirusTotal URL Scan Results (Live Evidence)

The linked URL `http://secure-account-verify.com` was submitted to VirusTotal for analysis:

| Security Vendor | Result |
|---|---|
| ADMINUSLabs | 🔴 Malicious |
| Chong Lua Dao | 🔴 Malicious |
| CyRadar | 🔴 Phishing |
| ESET | 🔴 Phishing |
| 91 other vendors | ✅ Clean (not yet updated) |

> **Result: 4/95 security vendors confirmed this URL as malicious or phishing.**
> Tags detected: `multiple-redirects`, `blocked-waf`, `text/html`
> Status Code: 522 (connection timed out — evasion tactic)

#### WHOIS Domain Analysis (Live Evidence)

| Field | Value | Significance |
|---|---|---|
| Domain | secure-account-verify.com | Lookalike domain — sounds official |
| Registrar | Cloudflare, Inc. | Privacy-shielded registration |
| Domain Status | Registered And No Website | No legitimate business at this address |
| IP Address | 104.21.45.149 | Cloudflare proxy — hides real server |
| IP History | 40 changes on 40 unique IPs over 9 years | Highly suspicious rotation pattern |
| Location | California, USA — Cloudflare infrastructure | |

#### Phishing Indicators Identified

| # | Indicator | Explanation |
|---|-----------|-------------|
| 1 | **Confirmed malicious by VirusTotal** | 4 security vendors including ESET and CyRadar flagged the URL as malicious/phishing in live analysis. |
| 2 | **Fake sender domain** | `secure-accounts-verify.com` is not a domain belonging to any known company. Legitimate companies send from their own verified domain (e.g. `@paypal.com`, `@google.com`). |
| 3 | **Urgency and fear tactics** | "24 hours", "permanent suspension", "all data deleted" — designed to panic the reader into acting without thinking. |
| 4 | **Generic greeting** | "Dear Valued Customer" — legitimate services address you by your actual name. |
| 5 | **Mismatched URL** | The link points to `secure-account-verify[.]com` — a confirmed phishing domain with no legitimate website. |
| 6 | **No company branding** | No logo, no registered address, no phone number — all things legitimate companies include. |
| 7 | **Reply-To misdirection** | Replies go to `harvest-data@differentdomain.net` — a completely different domain designed to capture responses. |
| 8 | **SPF/DKIM/DMARC failure** | The email fails all three authentication checks, confirmed via MXToolbox header analysis. |
| 9 | **Domain registered with no website** | WHOIS confirms the domain has no actual website — it exists purely for phishing. |
| 10 | **Suspicious IP rotation** | 40 IP address changes over 9 years indicates deliberate evasion of security blacklists. |

#### Risk Classification

> 🔴 **PHISHING — CONFIRMED** — Verified by VirusTotal (4/95 vendors), MXToolbox header analysis, and WHOIS domain investigation. Do not click any links. Do not reply. Report to IT security immediately.

---

### Sample 2 — Fake IT Department Password Reset

#### Email Content

```
FROM:    it-helpdesk@company-support-desk.com
TO:      employee@company.com
SUBJECT: [ACTION REQUIRED] Mandatory Password Reset — Company Policy Update

Hi [Employee Name],

As part of our company's new security policy rollout, all employees 
must reset their passwords within the next 2 hours.

Please use the link below to reset your password and avoid being 
locked out of company systems:

🔗 Reset Password Now: http://company-portal-reset[.]com/auth

If you do not reset within 2 hours, your account will be 
automatically locked by our IT systems.

— IT Helpdesk Team
Company Internal Systems
```

---

#### Header Analysis

| Header Field | Value | Verdict |
|---|---|---|
| From (Actual) | it-helpdesk@company-support-desk.com | 🔴 Not the company's real domain |
| SPF Check | FAIL | 🔴 Unauthorised sender |
| DKIM Check | FAIL | 🔴 Not signed |
| IP Origin | Located in Eastern Europe | 🔴 Inconsistent with company location |
| Domain Age | Registered 1 week ago | 🔴 Newly created |

#### Phishing Indicators Identified

| # | Indicator | Explanation |
|---|-----------|-------------|
| 1 | **Internal department impersonation** | Pretends to be the IT department — a trusted internal authority — to lower the victim's guard. |
| 2 | **Extremely short deadline** | "2 hours" is designed to prevent the employee from checking with IT directly. |
| 3 | **Lookalike domain** | `company-support-desk.com` looks official but is not the company's real domain. Attackers register domains that sound legitimate. |
| 4 | **Fake password reset link** | The link leads to a credential harvesting site that mimics the real company login page. |
| 5 | **Threat of system lockout** | Fear of losing access to work systems pressures employees to act immediately. |
| 6 | **Unusual IP origin** | Email header shows the message originated from a foreign IP — inconsistent with an internal IT department. |
| 7 | **Vague policy justification** | "New security policy rollout" — real IT departments communicate policy changes through official channels, not urgent emails. |

#### Risk Classification

> 🔴 **PHISHING** — This is a credential harvesting attack. Do not click the link. Call your IT department directly using their known number to verify.

---

### Sample 3 — Fake Invoice / CEO Finance Request

#### Email Content

```
FROM:    ceo.johnson@company-executives.net
TO:      finance@company.com
SUBJECT: Confidential — Urgent Wire Transfer Required

Hi Sarah,

I'm currently in a confidential board meeting and cannot speak 
on the phone right now. I need you to process an urgent wire 
transfer of $47,500 to a new vendor immediately.

This must be completed before end of business today. Please 
keep this between us until the deal is finalised — our legal 
team has advised on confidentiality.

Please confirm once done. Wire details below:

  Bank: First National Bank
  Account Name: Global Solutions Ltd
  Account No: 8834920156
  Routing No: 021000021

Thanks,
Robert Johnson
Chief Executive Officer
```

---

#### Header Analysis

| Header Field | Value | Verdict |
|---|---|---|
| From (Actual) | ceo.johnson@company-executives.net | 🔴 Not the real CEO email |
| Reply-To | wire-confirm@anotherdomain.org | 🔴 Replies diverted |
| SPF Check | FAIL | 🔴 Unauthorised |
| Display Name | "Robert Johnson CEO" | 🔴 Display name spoofed |
| Sent Time | 11:47 PM | 🟡 Outside business hours |

#### Phishing Indicators Identified

| # | Indicator | Explanation |
|---|-----------|-------------|
| 1 | **CEO/executive impersonation (Whaling)** | Impersonating a CEO or senior leader exploits authority — employees are conditioned to comply quickly with executive requests. |
| 2 | **Request for secrecy** | "Keep this between us" and "confidentiality" are designed to prevent the employee from verifying the request with colleagues. |
| 3 | **Unavailability excuse** | "In a confidential meeting, cannot take calls" — conveniently prevents the employee from calling to verify. |
| 4 | **Unusual wire transfer request** | Legitimate finance requests follow established workflows — they are never made through a single email with no paper trail. |
| 5 | **Urgency before close of business** | The time pressure prevents proper authorisation procedures from being followed. |
| 6 | **Fake sender domain** | The real CEO's email would be @company.com — not @company-executives.net. |
| 7 | **Unknown vendor with no prior relationship** | "New vendor" with no purchase order, no contract reference — a major red flag in any finance process. |

#### Risk Classification

> 🔴 **PHISHING (Business Email Compromise / BEC)** — This is one of the most financially damaging phishing attacks. Never process wire transfers based solely on email. Always verify by calling the requester directly on a known number.

---

### Sample 4 — Fake Package Delivery Notification

#### Email Content

```
FROM:    delivery-notifications@parcel-track-update.com
TO:      customer@email.com
SUBJECT: Your package could not be delivered — Action Required

Dear Customer,

We attempted to deliver your package today but were unable 
to complete the delivery due to an incomplete address.

To reschedule your delivery, please confirm your address 
and pay a small re-delivery fee of $1.99:

📦 Reschedule Delivery: http://parcel-track-update[.]com/redeliver

Failure to act within 48 hours will result in your package 
being returned to the sender.

DHL Express Delivery Services
```

---

#### Header Analysis

| Header Field | Value | Verdict |
|---|---|---|
| From (Actual) | delivery@parcel-track-update.com | 🔴 Not dhl.com |
| SPF | FAIL | 🔴 Unauthorised |
| Real DHL domain | dhl.com | — Legitimate comparison |
| Domain Age | 2 weeks old | 🔴 Recently registered |
| Logo | DHL logo embedded | 🟡 Stolen branding |

#### Phishing Indicators Identified

| # | Indicator | Explanation |
|---|-----------|-------------|
| 1 | **Brand impersonation** | Uses DHL's name and branding but sends from a completely different domain. |
| 2 | **Generic recipient** | "Dear Customer" — real couriers address you by name and include a tracking number. |
| 3 | **Small payment request** | $1.99 seems harmless — but the real goal is to capture credit card details on the fake site. |
| 4 | **Fake tracking link** | The URL does not belong to DHL. Real DHL tracking links contain `dhl.com`. |
| 5 | **No tracking number provided** | Legitimate delivery notifications always include a specific tracking reference. |
| 6 | **Unsolicited email** | If you were not expecting a package, this is an immediate red flag. |

#### Risk Classification

> 🟡 **PHISHING** — Medium impact but common. The $1.99 payment is a pretext for credit card theft. Do not click the link or enter any payment details.

---

## 5. Phishing Indicators Summary

The following indicators appeared consistently across all four samples. If you see **two or more** of these in an email, treat it with extreme suspicion:

| Indicator | What to Look For |
|-----------|-----------------|
| 🔴 **Fake or spoofed sender domain** | The email domain doesn't match the company it claims to be from |
| 🔴 **Urgency and fear language** | "Act now", "24 hours", "permanent deletion", "immediate action" |
| 🔴 **Generic greeting** | "Dear Customer", "Dear User", "Dear Employee" — no actual name |
| 🔴 **Mismatched or suspicious links** | Hover over links — if the URL doesn't match the expected domain, don't click |
| 🔴 **Request for credentials or payment** | No legitimate company asks for passwords or fees via email |
| 🔴 **Requests for secrecy** | "Don't tell anyone" or "confidential" — designed to bypass verification |
| 🟡 **Poor grammar and spelling** | Professional companies proofread their emails |
| 🟡 **Unexpected attachments** | Attachments you weren't expecting, especially .exe, .zip, .doc with macros |
| 🟡 **Sent outside business hours** | Phishing emails often arrive late at night or on weekends |
| 🟡 **Lookalike domain names** | `paypa1.com` instead of `paypal.com`, or `company-support-desk.com` |

---

## 6. Risk Classification

All emails should be classified using the following system:

| Classification | Description | Action |
|---|---|---|
| ✅ **Safe** | From a verified sender, expected content, passes authentication, no suspicious links | Normal use |
| 🟡 **Suspicious** | One or two minor indicators — possibly legitimate but warrants caution | Verify before clicking any links |
| 🔴 **Phishing** | Multiple clear indicators — designed to deceive or steal | Do not interact. Report immediately. |

### How to Verify a Suspicious Email

Before clicking any link or replying to a suspicious email:

1. **Call the sender** — use a phone number you already know, not one from the email
2. **Check the sender's domain** — hover over the name to reveal the actual email address
3. **Search the subject line** — many phishing campaigns are widely reported online
4. **Check with your IT team** — forward suspicious emails to your security team
5. **Use VirusTotal** — paste the link at `virustotal.com` to check it safely

---

## 7. How These Attacks Work

Understanding the mechanics of a phishing attack helps you spot them faster.

### Step-by-Step Attack Flow

```
1. ATTACKER CHOOSES A TARGET
   └── Individual user, specific company, or mass audience

2. ATTACKER CRAFTS A DECEPTIVE EMAIL
   └── Impersonates a trusted brand or person
   └── Creates urgency or fear to trigger emotional response
   └── Registers a lookalike domain days before the attack

3. EMAIL IS SENT TO VICTIMS
   └── Mass phishing: thousands of recipients
   └── Spear phishing: one specific, researched individual

4. VICTIM OPENS EMAIL AND TAKES THE BAIT
   └── Clicks a malicious link → lands on fake login page
   └── Opens an attachment → malware installs silently
   └── Replies with information → data sent to attacker

5. ATTACKER HARVESTS THE RESULT
   └── Stolen credentials → sold or used for account takeover
   └── Installed malware → ransomware, keylogger, backdoor
   └── Wire transfer → money rarely recoverable
```

### Why People Fall for It

Phishing works because it exploits **human psychology**, not technical vulnerabilities:

- **Authority** — We comply with requests from people in power (bosses, IT, banks)
- **Urgency** — When panicked, we skip verification steps
- **Familiarity** — Recognisable logos and branding lower our guard
- **Fear** — Threat of losing access or money overrides critical thinking
- **Curiosity** — "You have a package" or "See who viewed your profile" triggers clicks

---

## 8. Prevention Guidelines for Users

### For Everyday Email Users

**Before you click any link:**
- Hover over the link first — check where it actually goes
- Look at the domain carefully — `paypa1.com` is not `paypal.com`
- Ask yourself: was I expecting this email?

**Before you enter any credentials:**
- Type the website address directly in your browser — never use a link from an email
- Check for the padlock icon and verify the URL is exactly correct
- If in doubt, use your password manager — it won't autofill on a fake site

**Before you reply:**
- Check the actual sender email address — not just the display name
- For sensitive requests, always verify through a second channel (phone call)
- Never share passwords, OTPs, or personal information via email

---

### For Employees in a Business Setting

**Standard operating procedures:**
- Wire transfers and payments must never be initiated based on a single email — always follow the established approval process
- Password resets should only be done through official IT portals — never through an emailed link
- Report suspicious emails to IT security — don't just delete them

**What to do if you clicked a phishing link:**
1. Disconnect your device from the network immediately
2. Contact your IT department right away
3. Change all potentially compromised passwords from a different device
4. Monitor your accounts for unusual activity
5. Do not try to handle it yourself — tell someone

---

## 9. Do's and Don'ts for Employees

### ✅ DO

- **Do** verify unusual requests by calling the sender on a known number
- **Do** hover over links before clicking to inspect the real URL
- **Do** report suspicious emails to IT — even if you're not sure
- **Do** use multi-factor authentication (MFA) on all accounts
- **Do** keep software and antivirus up to date
- **Do** use a password manager — it won't autofill on fake websites
- **Do** read the sender's full email address, not just their display name
- **Do** trust your instincts — if something feels off, it probably is

---

### ❌ DON'T

- **Don't** click links in emails you weren't expecting — even if they look real
- **Don't** open attachments from unknown senders — especially .exe, .zip, or .doc files
- **Don't** enter your password on a site you reached by clicking an email link
- **Don't** comply with "urgent" requests before verifying them independently
- **Don't** share one-time passwords (OTPs) with anyone — ever
- **Don't** keep a suspicious interaction to yourself — always report it
- **Don't** assume a padlock icon (HTTPS) makes a website safe — phishing sites can have SSL too
- **Don't** ignore security awareness training — it exists for a reason

---

## 10. Conclusion

This analysis of four phishing email samples demonstrates how sophisticated and convincing modern phishing attacks have become. The common thread across all samples was the deliberate exploitation of human psychology — urgency, fear, authority, and trust — rather than any technical vulnerability.

The most effective defence against phishing is not technology alone, but **an informed and vigilant workforce**. A user who knows how to spot a fake sender domain, hover-check a link, or call to verify an unusual request is far more valuable than the most expensive firewall.

### Key Takeaways

- Phishing is the most common entry point for cybercriminals — awareness is your first line of defence
- Always verify unexpected or urgent requests through a second, independent channel
- No legitimate company will ever ask for your password, OTP, or payment via an email link
- When in doubt — stop, think, and ask your IT team

---

### Reporting Phishing Emails

| Where to Report | How |
|---|---|
| Within your organisation | Forward to your IT security team |
| Gmail users | Click the three dots → "Report phishing" |
| Outlook users | Click "Report" → "Report phishing" |
| In South Africa | Report to SABRIC at `info@sabric.co.za` |
| Globally | Forward to `reportphishing@apwg.org` |

---

*This report was produced as part of the Future Interns Cyber Security Internship Program (2026). All email samples analysed are illustrative examples created for educational purposes. No real user data was involved.*

---

**Report End**
