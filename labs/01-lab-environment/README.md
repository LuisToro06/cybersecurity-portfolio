# Laboratory 01 - Cybersecurity Lab Environment

---

## Introduction

A well-designed laboratory environment is the foundation of any cybersecurity assessment. Before conducting vulnerability assessments, penetration testing, digital forensics, or defensive security activities, it is essential to establish a secure and isolated infrastructure where testing can be performed without affecting production systems.

This laboratory documents the virtual environment used throughout this cybersecurity portfolio. The infrastructure has been built using Oracle VirtualBox and consists of multiple virtual machines designed to simulate real-world enterprise environments for security testing, vulnerability assessment, network analysis, and cybersecurity research.

All future laboratories included in this repository will be performed within this isolated environment.

---

## Learning Objectives

Upon completing this laboratory, the reader will be able to:

- Understand the architecture of the virtual cybersecurity laboratory.
- Identify the virtual machines used throughout the portfolio.
- Understand the purpose of each virtual machine.
- Configure a secure virtual environment for cybersecurity testing.
- Recognize the importance of network isolation during security assessments.
- Prepare the laboratory for future vulnerability assessments and penetration testing exercises.

---

## Laboratory Environment

The laboratory has been implemented using Oracle VirtualBox as the virtualization platform.

### Virtual Machines

| Virtual Machine | Operating System | Purpose |
|-----------------|-----------------|----------|
| Kali Linux 2025.4 | Linux | Attacker Machine |
| Metasploitable 2 | Ubuntu Linux | Vulnerable Linux Server |
| Metasploitable 3 Ubuntu | Ubuntu Server | Vulnerable Enterprise Environment |
| Metasploitable 3 Windows | Windows Server | Vulnerable Active Directory Environment |

The attacker machine performs reconnaissance, vulnerability assessment, web security testing, packet analysis, digital forensics, and security validation against intentionally vulnerable systems.

---

## Laboratory Architecture

The laboratory follows a client-server architecture where Kali Linux acts as the attacking machine while the remaining virtual machines simulate vulnerable enterprise environments.

```
                         Oracle VirtualBox

                       Isolated Virtual Network

                                │
      ┌─────────────────────────┼─────────────────────────┐
      │                         │                         │
      │                         │                         │
┌───────────────┐      ┌────────────────┐      ┌──────────────────┐
│ Kali Linux    │      │ Metasploitable │      │ Metasploitable 3 │
│ 2025.4        │      │ 2              │      │ Ubuntu           │
│ Attacker      │      │ Vulnerable VM  │      │ Vulnerable VM    │
└───────────────┘      └────────────────┘      └──────────────────┘
                                   │
                          ┌──────────────────┐
                          │ Metasploitable 3 │
                          │ Windows Server   │
                          │ Vulnerable VM    │
                          └──────────────────┘
```

This architecture allows realistic cybersecurity exercises while maintaining complete isolation from production systems.

---

## Virtualization Platform

The laboratory uses Oracle VirtualBox to manage all virtual machines.

### Main Features

- Snapshot management
- Virtual networking
- Secure isolation
- Resource allocation
- Cross-platform compatibility

---

## Network Configuration

The virtual machines communicate through an isolated virtual network configured within Oracle VirtualBox.

This configuration allows:

- Host discovery
- Network scanning
- Service enumeration
- Vulnerability assessment
- Web application testing
- Packet capture
- Traffic analysis

No production devices participate in the laboratory.

---

## Tools Available

The following tools will be used throughout this cybersecurity portfolio.

| Tool | Purpose |
|------|----------|
| Nmap | Network Discovery |
| Wireshark | Packet Analysis |
| Burp Suite Community | Web Security Testing |
| OWASP ZAP | Web Vulnerability Assessment |
| OpenVAS | Vulnerability Scanning |
| Nikto | Web Server Assessment |
| Gobuster | Directory Enumeration |
| SQLMap | SQL Injection Testing |
| Wazuh | SIEM and Blue Team Monitoring |
| Autopsy | Digital Forensics |
| pfSense | Firewall Laboratory |

Additional tools will be introduced as new laboratories are developed.

---

## Security Considerations

This laboratory has been designed following responsible cybersecurity practices.

The following principles are applied:

- Complete isolation from production environments.
- Vulnerable machines are used exclusively for educational purposes.
- Security assessments are performed only against authorized systems.
- No offensive activities are conducted outside the laboratory.
- Ethical hacking principles are respected throughout every exercise.

---

## Laboratory Scope

The laboratory will support the development of practical exercises related to:

- Network Reconnaissance
- Service Enumeration
- Vulnerability Assessment
- Web Application Security
- Blue Team Operations
- Digital Forensics
- Active Directory Security
- Cloud Security
- Security Awareness
- Secure Configuration Assessment

---

## Expected Learning Outcomes

After completing this laboratory, the reader will have a fully documented cybersecurity environment capable of supporting practical security assessments.

This laboratory serves as the foundation for all subsequent exercises included in this repository.

---

## Next Laboratory

The next laboratory focuses on network reconnaissance using Nmap.

**Laboratory 02 – Reconnaissance**

Topics include:

- Host Discovery
- Port Scanning
- Service Detection
- Operating System Detection
- Nmap Scripting Engine (NSE)

---

## Lessons Learned

A properly documented laboratory environment is essential before conducting any cybersecurity assessment.

Establishing an isolated virtual infrastructure ensures that security testing can be performed safely while providing a realistic environment for learning, research, and professional practice.

Proper laboratory planning improves reproducibility, facilitates documentation, and supports the development of practical cybersecurity skills aligned with industry best practices.

---

## References

- Oracle VirtualBox Documentation
- Kali Linux Documentation
- OWASP Foundation
- NIST Cybersecurity Framework (CSF)
- NIST SP 800-115 Technical Guide to Information Security Testing and Assessment
- MITRE ATT&CK Framework
- CIS Controls Version 8
