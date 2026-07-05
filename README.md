# Altoro Mutual – Vulnerability Assessment Report

A basic, ethical, and non-intrusive vulnerability assessment of the Altoro Mutual demo banking application (`demo.testfire.net`), performed to identify common web security weaknesses from an external, unauthenticated perspective.

> **Disclaimer:** Altoro Mutual is a publicly available demo banking site published by HCL Technologies for the purpose of demonstrating web application vulnerability detection tools. It is not a real banking site. This assessment was conducted strictly using passive, read-only techniques for educational and portfolio purposes.

---

## 📋 Executive Summary

This assessment focused on the publicly accessible areas of the Altoro Mutual website using a read-only, ethical approach — no exploitation, brute-forcing, or intrusive actions were performed.

The analysis identified several common security weaknesses, including missing HTTP security headers, cookie misconfigurations, and minor exposure of technical information. None of these issues immediately compromise the system, but they increase the site's exposure to common web-based attacks if left unaddressed.

**Overall Security Level: Moderate**

---

## 🎯 Scope & Objective

**Scope**
- Assessment limited to the publicly accessible areas of `demo.testfire.net`
- Only client-side and openly available resources were reviewed
- No authenticated or restricted sections were accessed

**Objective**
- Identify common security weaknesses using non-intrusive methods
- Evaluate the website's security posture from an external user perspective
- Provide clear, practical recommendations for improvement

**Testing Approach**
- Passive analysis techniques used throughout
- No exploitation, brute-force attempts, or harmful activities performed
- Testing strictly followed ethical guidelines under a read-only approach

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| **Nmap** | Identify open ports and analyze exposed network services |
| **OWASP ZAP** (Passive Scan) | Detect common web vulnerabilities without active attacks |
| **Browser DevTools** | Inspect cookies, headers, and client-side behavior |
| **SecurityHeaders.com** | Evaluate presence and effectiveness of HTTP security headers |

---

## 🌐 Website Overview

- **Website Name:** Altoro Mutual
- **Website URL:** http://demo.testfire.net/

Altoro Mutual is a demo online banking application built for security testing and education. It simulates real-world banking functionality (account login, fund transfers, customer services) and is intentionally designed with known security weaknesses, making it a suitable target for vulnerability assessment practice.

---

## 🔍 Findings

### Finding #1 — Missing Security Headers
**Risk Level: Medium**

The site received an **F grade** from SecurityHeaders.com. Critical headers are not implemented:
- `Content-Security-Policy`
- `X-Frame-Options`
- `X-Content-Type-Options`
- `Referrer-Policy`
- `Permissions-Policy`

**Impact:** Increases exposure to XSS, clickjacking, and content-type spoofing attacks by reducing browser-level enforcement of security rules.

**Recommendation:** Configure the missing headers at the server (Apache/Nginx) or application level.

---

### Finding #2 — Open Ports Exposure
**Risk Level: Low**

An Nmap scan (`nmap -F demo.testfire.net`) identified:

```
PORT     STATE SERVICE
80/tcp   open  http
443/tcp  open  https
```

**Impact:** Standard ports are open as expected for a web application, but HTTP (port 80) without enforced redirection to HTTPS increases the risk of data interception and man-in-the-middle attacks.

**Recommendation:** Enforce HTTPS by redirecting all HTTP traffic to port 443, and regularly audit open ports/firewall rules to minimize unnecessary exposure.

---

### Finding #3 — OWASP ZAP Passive Scan Results
**Risk Level: Medium**

Automated passive scanning surfaced multiple issues across risk tiers:
- **High Risk:** 1
- **Medium Risk:** 3 (Missing Security Headers, Cookie No HttpOnly Flag)
- **Low Risk:** 5 (X-Content-Type-Options Missing, CSP Not Set)
- **Informational:** 2

**Impact:** Combined, these weaknesses raise the risk of XSS, session hijacking, and clickjacking.

**Recommendation:** Prioritize remediation of medium-risk findings first, followed by low-risk items. Adopt secure coding practices and recurring automated scans.

---

### Finding #4 — Insecure Cookies
**Risk Level: Medium**

Browser DevTools inspection showed cookies (e.g., `JSESSIONID`) missing the `HttpOnly` and `Secure` flags.

**Impact:** Cookies without `HttpOnly` are accessible via JavaScript (XSS risk); cookies without `Secure` may be transmitted over unencrypted connections (interception risk). Together these can enable session hijacking.

**Recommendation:** Enforce `HttpOnly` and `Secure` flags on all session cookies, and review session management practices.

---

### Finding #5 — Network Response Headers
**Risk Level: Medium**

Network traffic analysis via DevTools confirmed the absence of `Content-Security-Policy`, `X-Frame-Options`, and `Strict-Transport-Security` in server responses.

**Impact:** Absence of CSP increases XSS risk; missing X-Frame-Options allows clickjacking; missing HSTS permits insecure HTTP communication, enabling MITM attacks.

**Recommendation:** Implement and validate all required security headers as part of standard deployment/testing practices.

---

## ✅ Conclusion

**Overall Security Level:** Moderate — no critical vulnerabilities were found, but multiple medium- and low-risk issues span security headers, cookie configuration, and network responses. The application is functional but lacks several security best practices needed to defend against common web-based attacks.

**Priority Fixes**
1. Implement `Content-Security-Policy`, `Strict-Transport-Security`, and `X-Frame-Options`
2. Secure cookies with `HttpOnly` and `Secure` flags
3. Enforce HTTPS redirection across all traffic
4. Establish recurring automated security testing (e.g., OWASP ZAP) and configuration reviews

---

## 👤 Author

**Mohammed Wajihuddin (Shzzib)**
Cyber Security Intern | B.Tech CSE (Cyber Security)
[GitHub](https://github.com/Shzzib)
