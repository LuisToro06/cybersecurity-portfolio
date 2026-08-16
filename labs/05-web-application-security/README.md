# 🌐 Lab 05 — Web Application Security Assessment

This laboratory focuses on the security assessment of an intentionally vulnerable web application using **Damn Vulnerable Web Application (DVWA)**.

The objective is to identify, analyze, and document common web application security weaknesses in a controlled and isolated laboratory environment.

The assessment was performed from **Kali Linux** against the DVWA instance hosted on **Metasploitable 2**.

---

## 🎯 Objectives

The main objectives of this laboratory are:

- Identify the target web application.
- Discover accessible web resources and directories.
- Analyze the application's exposed attack surface.
- Perform passive web security analysis.
- Identify common web application vulnerabilities.
- Demonstrate reflected and stored Cross-Site Scripting (XSS).
- Test SQL Injection using manual techniques and SQLMap.
- Demonstrate command injection in the vulnerable application.
- Perform a consolidated security assessment using OWASP ZAP.
- Document the evidence and security implications of the findings.

---

## 🧪 Laboratory Environment

| Component | Description |
|---|---|
| Attacker | Kali Linux |
| Target | Metasploitable 2 |
| Web Application | Damn Vulnerable Web Application (DVWA) |
| Target IP | `10.0.2.4` |
| Web Server | Apache/PHP |
| Database | MySQL |
| Security Testing Tools | Gobuster, OWASP ZAP, SQLMap, Browser Developer Tools |
| Network | Isolated virtual laboratory |

> **Important:** This laboratory is intended exclusively for authorized security testing in an isolated environment. DVWA is intentionally vulnerable and must not be exposed to the Internet or production networks.

---

# 1. 🎯 DVWA Target

The first step was to verify access to the DVWA web application hosted on the Metasploitable 2 virtual machine.

### Target

```text
http://10.0.2.4/dvwa/
