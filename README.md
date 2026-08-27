# Web Application Vulnerability Assessment & Penetration Testing (VAPT)

A black-box/gray-box security assessment conducted against the **OWASP Juice Shop** web application to identify critical security flaws mapped against the **OWASP Top 10** framework.

---

## 📑 Full Assessment Report
- **Download PDF Report:** [VAPT_Report_JuiceShop_LargeImages.pdf](./VAPT_Report_JuiceShop_LargeImages.pdf)

---

## 🎯 Executive Summary
The objective of this assessment was to evaluate authentication resilience, access control boundaries, and input sanitization mechanisms. Two high-impact vulnerabilities were identified and validated using proof-of-concept (PoC) exploits.

---

## 🛡️ Findings Summary Table

| Finding ID | Vulnerability Title | OWASP Category | Affected Endpoint | Severity / CVSS v3.1 |
| :--- | :--- | :--- | :--- | :--- |
| **SEC-01** | SQL Injection (Auth Bypass) | A03:2021 – Injection | `POST /rest/user/login` | **9.8 (Critical)** |
| **SEC-02** | Insecure Direct Object Reference (IDOR) | A01:2021 – Broken Access Control | `GET /rest/basket/{id}` | **7.5 (High)** |

---

## 🔍 Key Technical Findings

### 1. SEC-01: SQL Injection Leading to Admin Takeover
- **Vulnerability Type:** SQL Injection (CWE-89)
- **Vector:** `email` parameter on `/rest/user/login`
- **Payload:** `' or 1=1--`
- **Impact:** Allowed an unauthenticated attacker to bypass password verification and receive an administrative JSON Web Token (JWT) session.
- **Remediation:** Enforce Parameterized Queries (Prepared Statements) or use ORM abstraction to separate query logic from user input.

### 2. SEC-02: IDOR on Shopping Basket API
- **Vulnerability Type:** Broken Object Level Authorization (CWE-639)
- **Vector:** Path parameter `basketId` on `/rest/basket/{basketId}`
- **Impact:** Allowed an authenticated user to inspect the cart contents, product IDs, and quantities of arbitrary users by tampering with the numerical identifier.
- **Remediation:** Implement server-side session checks verifying `basket.UserId === req.user.id` before returning database records.

---

## 🛠️ Tools & Environment
- **Interception Proxy:** Burp Suite Community Edition (Repeater, Proxy History)
- **Target Application:** OWASP Juice Shop (Node.js/Express)
- **Methodology:** OWASP Testing Guide (OTG)
