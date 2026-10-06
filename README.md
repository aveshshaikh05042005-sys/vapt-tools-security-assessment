# 🛡️ Vulnerability Assessment & Penetration Testing (VAPT) Tools

**Author:** Mohammed Avesh Shaikh  
**Role:** Cybersecurity Analyst  
**Contact:** aveshshaikh05042005@gmail.com  
**Target Host:** `http://testasp.vulnweb.com` (IP: `44.238.29.244`)  
**Date:** October 2026  

---

## 🎯 Executive Summary
This project demonstrates hands-on practical methodology for Vulnerability Assessment and Penetration Testing (VAPT). The assessment evaluates an authorized live target using industry-standard security tools: **Nmap** (network & service reconnaissance), **Nikto** (automated web vulnerability scanning), and **Burp Suite** (HTTP proxy & traffic interception).

---

## 🛠️ Tools Utilized
- **Nmap 7.99:** Network port scanning, service versioning (`-sV`), aggressive enumeration (`-A`), and script scanning.
- **Nikto 2.6.0:** Web server misconfiguration checks, header analysis, and outdated component discovery.
- **Burp Suite Community Edition:** Local proxy listener (`127.0.0.1:8080`), HTTP request interception, and header forensics.

---

## 📋 Summary of Practical Tasks

### 1. Network Reconnaissance (Nmap)
- **Commands Executed:**
  - `nmap -sS testasp.vulnweb.com` (Stealth SYN Scan)
  - `nmap -sV testasp.vulnweb.com` (Service Version Detection)
  - `nmap -A testasp.vulnweb.com` (Aggressive Audit)
- **Key Findings:**
  - Discovered Port 80/tcp OPEN running `Microsoft IIS httpd 8.5` on Windows OS.
  - Detected active HTTP `TRACE` method (enables Cross-Site Tracing risks).
  - Identified missing `HttpOnly` flag on session cookie `ASPSESSIONIDACBRSCSB`.

### 2. Automated Web Vulnerability Scanning (Nikto)
- **Command Executed:** `nikto -h http://testasp.vulnweb.com`
- **Key Discoveries:**
  - Server banner disclosure (`Server: Microsoft-IIS/8.5`).
  - Technology header leakage (`X-Powered-By: ASP.NET`).
  - Insecure cookie attribute: `Cookie ASPSESSIONIDACBRSCSB created without the httponly flag`.
  - Absence of anti-clickjacking and MIME protection headers (`X-Frame-Options`, `X-Content-Type-Options`).

### 3. HTTP Request Interception (Burp Suite)
- Configured local proxy on `127.0.0.1:8080`.
- Intercepted live HTTP request: `GET /Default.asp HTTP/1.1` targeting `testasp.vulnweb.com:80`.
- Dissected client request headers (`User-Agent`, `Accept`, `Accept-Encoding`, `Connection`) to validate parameter delivery and session structure.

---

## 🔍 Consolidated Findings Matrix (F-01 to F-07)
| ID | Finding | Source | Risk | Description |
| :--- | :--- | :--- | :--- | :--- |
| **F-01** | Server Version Disclosure | Nikto / Nmap | Low | Server exposes exact banner `Microsoft-IIS/8.5`. |
| **F-02** | Backend Technology Disclosure | Nikto | Low | `X-Powered-By: ASP.NET` reveals framework version. |
| **F-03** | Insecure Cookie Attribute | Nikto / Nmap | Medium | `ASPSESSIONIDACBRSCSB` lacks `HttpOnly` flag (XSS risk). |
| **F-04** | Risky HTTP Methods (TRACE) | Nmap (-A) | Medium | `TRACE` method active, creating Cross-Site Tracing exposure. |
| **F-05** | Missing Security Headers | Nikto | Low | Missing `X-Frame-Options` and `X-Content-Type-Options`. |
| **F-06** | Cleartext HTTP Protocol | Nmap / Burp | Medium | Port 80 lacks TLS encryption, leaving data vulnerable to MITM. |
| **F-07** | HTTP Request & Headers Inspected | Burp Suite | Info | Validated live client-server communication wireframes. |

---

## 🛡️ Enterprise Hardening Recommendations
1. **Enforce HTTPS Everywhere:** Redirect all Port 80 traffic to Port 443 with TLS 1.3 and HSTS.
2. **Secure Cookie Attributes:** Enforce `HttpOnly`, `Secure`, and `SameSite=Strict` on all session cookies.
3. **Disable Risky HTTP Methods:** Turn off `TRACE` and `TRACK` in web server configuration.
4. **Suppress Server Signatures:** Remove `Server` and `X-Powered-By` headers to prevent technology profiling.
5. **Implement Comprehensive Security Headers:** Deploy `Content-Security-Policy`, `X-Frame-Options`, and `X-Content-Type-Options`.

---

## 📁 Repository Deliverables
- `VAPT_Tools_Practical_Assessment_Report.pdf`: 18-Page Comprehensive Technical Report
- `VAPT_Tools_Practical_Assessment_Slides.pptx`: 8-Slide Executive Presentation Deck
- Screenshot evidence of Nmap, Nikto, and Burp Suite operations
