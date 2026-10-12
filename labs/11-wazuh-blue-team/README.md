# Laboratory 11 --- Wazuh SIEM & Blue Team Operations

![Wazuh](https://img.shields.io/badge/Wazuh-SIEM%20%7C%20XDR-005571)
![Blue Team](https://img.shields.io/badge/Focus-Blue%20Team-blue)
![Environment](https://img.shields.io/badge/Environment-VirtualBox-orange)

## 1. Overview

This laboratory documents a controlled Wazuh security-monitoring
environment with a Windows Server endpoint. The work covers network
validation, Wazuh Dashboard access, agent connectivity, Windows
security-event collection, failed-authentication alert investigation,
MITRE ATT&CK mapping review, incident triage, and final security
findings.

The documentation distinguishes observed evidence from interpretation.
An alert or framework mapping is not, by itself, proof of a successful
attack.

## 2. Table of Contents

-   [1. Overview](#1-overview)
-   [2. Table of Contents](#2-table-of-contents)
-   [3. Objectives](#3-objectives)
-   [4. Lab Architecture](#4-lab-architecture)
-   [5. Environment](#5-environment)
-   [6. Tools and Components](#6-tools-and-components)
-   [7. Methodology](#7-methodology)
-   [8. Implementation and Validation](#8-implementation-and-validation)
    -   [8.1 Network validation](#81-network-validation)
    -   [8.2 Wazuh Dashboard](#82-wazuh-dashboard)
    -   [8.3 Windows agent installation](#83-windows-agent-installation)
    -   [8.4 Agent connection
        validation](#84-agent-connection-validation)
    -   [8.5 Windows security events](#85-windows-security-events)
    -   [8.6 Authentication-failure
        alert](#86-authentication-failure-alert)
    -   [8.7 Alert investigation](#87-alert-investigation)
    -   [8.8 MITRE ATT&CK mapping
        review](#88-mitre-attck-mapping-review)
    -   [8.9 Incident response and
        triage](#89-incident-response-and-triage)
    -   [8.10 Final security findings](#810-final-security-findings)
-   [9. Findings and Recommendations](#9-findings-and-recommendations)
-   [10. Limitations and Evidence
    Integrity](#10-limitations-and-evidence-integrity)
-   [11. Evidence Index](#11-evidence-index)
-   [12. Repository Structure](#12-repository-structure)
-   [13. Security and Privacy](#13-security-and-privacy)
-   [14. Conclusion](#14-conclusion)
-   [15. References](#15-references)
-   [16. Disclaimer](#16-disclaimer)

## 3. Objectives

1.  Validate connectivity between the virtual lab systems.
2.  Access the Wazuh Dashboard and confirm the monitored endpoint is
    connected.
3.  Install and configure the Wazuh Agent on Windows Server 2022.
4.  Review Windows Security events in Wazuh.
5.  Investigate a failed-authentication alert and its underlying event.
6.  Review the rule's MITRE ATT&CK association critically.
7.  Document a proportionate incident-triage workflow.
8.  Summarize endpoint configuration-assessment results and
    recommendations.

## 4. Lab Architecture

``` text
                  Isolated VirtualBox Lab Network
                             |
          +------------------+------------------+
          |                  |                  |
          v                  v                  v
   Kali Linux           Wazuh Server        Windows Server
   Analyst / Browser    Manager, Indexer    DC01
   10.0.2.6             Dashboard           10.0.2.15
                        10.0.2.9             Wazuh Agent
```

**Data flow:** the Wazuh Agent on `DC01` sends configured endpoint
telemetry to the Wazuh server (`10.0.2.9`). The Manager analyzes events
using decoders and rules, the Indexer stores searchable data, and the
Dashboard provides the analyst interface.

This is a logical architecture diagram, not a packet capture.

## 5. Environment

  -----------------------------------------------------------------------
  Component         Operating system  Address           Purpose
                    / role                              
  ----------------- ----------------- ----------------- -----------------
  Kali Linux        Kali Linux 2025.4 `10.0.2.6`        Analyst
                                                        workstation and
                                                        browser

  Wazuh Server      Ubuntu Server     `10.0.2.9`        Wazuh Manager,
                    24.04 LTS                           Indexer,
                                                        Dashboard

  `DC01`            Windows Server    `10.0.2.15`       Monitored
                    2022 Standard                       endpoint
                    Evaluation                          
  -----------------------------------------------------------------------

Observed agent properties:

  Property                  Value
  ------------------------- -----------------------
  Agent name                `DC01`
  Agent ID                  `002`
  Group                     `default`
  Agent version             `4.14.8`
  Status at capture         `Active`
  Domain context in event   `CORP` / `corp.local`

These are the values recorded during this lab session; addresses can
change if the virtual network or DHCP configuration changes.

## 6. Tools and Components

  Tool / component       Purpose
  ---------------------- ---------------------------------------------------
  Wazuh Manager          Event analysis and alert generation
  Wazuh Indexer          Indexing and searching events and alerts
  Wazuh Dashboard        Monitoring, threat hunting, and investigation
  Wazuh Agent            Endpoint telemetry and log collection
  Kali Linux             Analyst workstation
  Windows Event Viewer   Local validation of Windows Security events
  Oracle VirtualBox      Virtualized lab environment
  MITRE ATT&CK           Framework for interpreting tactics and techniques

## 7. Methodology

``` text
Network validation
       ↓
Dashboard access
       ↓
Agent installation and enrollment
       ↓
Agent connectivity validation
       ↓
Windows event collection
       ↓
Authentication alert investigation
       ↓
MITRE ATT&CK mapping review
       ↓
Incident triage
       ↓
Final findings and recommendations
```

## 8. Implementation and Validation

### 8.1 Network validation

Connectivity between the lab systems was checked, including
communication from Windows Server to the Wazuh server. The figure should
show the actual validation output collected during the lab.

**Evidence:** Figure 1 --- `01-lab-network-validation.png`![Figura 1](images/01-lab-network-validation.png)

### 8.2 Wazuh Dashboard

The Dashboard was accessed from Kali Linux at:

``` text
https://10.0.2.9
```

The interface provides access to agent management, Threat Hunting, MITRE
ATT&CK, Vulnerability Detection, and Security Configuration Assessment.

**Evidence:** Figure 2 --- `02-wazuh-dashboard.png`

### 8.3 Windows agent installation

The Wazuh Agent was installed on Windows Server 2022 (`DC01`) and
configured to communicate with the Wazuh server at `10.0.2.9`. Do not
publish passwords, enrollment secrets, or authentication keys.

**Evidence:** Figure 3 --- `03-windows-agent-installation.png`

### 8.4 Agent connection validation

The Dashboard showed `DC01`, agent ID `002`, IP `10.0.2.15`, Windows
Server 2022 Standard Evaluation, agent version `4.14.8`, and status
`Active`.

An active status confirms connectivity at the time of capture; it does
not independently prove that every Windows event channel is configured
for collection.

**Evidence:** Figure 4 --- `04-agent-connected.png`

### 8.5 Windows security events

Windows Security events were reviewed locally and in Wazuh to confirm
visibility of records from `DC01`. Use the collected event evidence and
avoid substituting sample events for actual lab output.

**Evidence:** Figure 5 --- `05-windows-security-events.png`

### 8.6 Authentication-failure alert

The captured Wazuh search returned three records for rule `60122` in the
selected time range.

  Property                    Observed value
  --------------------------- --------------------------------------------------------
  Rule ID                     `60122`
  Description                 `Logon Failure - Unknown user or bad password`
  Alert level                 `5`
  Groups                      `authentication_failed`, `windows`, `windows_security`
  Event IDs matched by rule   `529` or `4625`

These records are alert matches, not necessarily three distinct attacks.
Their meaning depends on timestamps, account context, and related
events.

**Evidence:** Figure 6 --- `06-authentication-alert.png`

### 8.7 Alert investigation

The detailed event reviewed in the lab contained the following fields:

  Field              Observed value
  ------------------ ---------------------------------------
  Windows Event ID   `4625`
  Event channel      `Security`
  Computer           `DC01.corp.local`
  Agent              `DC01` (`002`)
  Target account     `CORP\Administrador`
  Logon type         `7`
  Source IP          `127.0.0.1`
  Status             `0xc000006d`
  Substatus          `0xc000006a`
  Provider           `Microsoft-Windows-Security-Auditing`
  Wazuh rule         `60122`

Event ID `4625` indicates a failed logon. Logon type `7` is associated
with workstation unlock activity; `127.0.0.1` is a loopback address, and
substatus `0xc000006a` indicates an incorrect password. This is
consistent with a local authentication failure, but surrounding events
should be checked before making a final attribution.

The event alone does not prove remote intrusion, successful compromise,
or malicious intent.

**Evidence:** Figure 7 --- `07-alert-investigation.png`

### 8.8 MITRE ATT&CK mapping review

The rule details displayed the following mapping:

  Property    Value displayed by Wazuh
  ----------- ------------------------------------
  Rule ID     `60122`
  Technique   `T1531` --- Account Access Removal
  Tactic      Impact

This is documented as the rule-level association shown by Wazuh. The
investigated event `4625` records a failed authentication attempt and
does not, by itself, demonstrate account removal or access revocation.
The mapping must not be interpreted as proof that `T1531` was executed.

**Evidence:** Figure 8 --- `08-mitre-attack-analysis.png`

### 8.9 Incident response and triage

A failed-authentication alert should be investigated before deciding
whether containment is required. Recommended triage:

1.  Confirm timestamp, target account, logon type, source address, and
    failure status.
2.  Correlate with nearby successful logons, additional failed logons,
    and account-lockout events if present.
3.  Determine whether the event is expected local activity or requires
    escalation.
4.  If unexpected, notify the authorized administrator and follow the
    applicable incident-handling procedure.
5.  Change credentials or block accounts only when justified,
    authorized, and proportionate.
6.  Record evidence, assessment, decision, and any action actually
    performed.

No account blocking, password reset, or containment action is claimed
unless shown in the collected evidence. The event alone is insufficient
to confirm an attack.

**Evidence:** Figure 9 --- `09-incident-response.png`

### 8.10 Final security findings

The final Dashboard review included Security Configuration Assessment
(SCA) results for Windows Server 2022.

  Property                                                Observed value
  ----------------- ----------------------------------------------------
  Policy              CIS Microsoft Windows Server 2022 Benchmark v2.0.0
  Passed                                                            `87`
  Failed                                                           `270`
  Not applicable                                                     `2`
  Score displayed                                                  `24%`

These values represent the captured assessment snapshot. Failed checks
require individual review; they do not automatically mean that the
server is compromised or that each check is an exploitable
vulnerability.

The Dashboard also displayed a Vulnerability Detection package/product
entry for Windows Server 2022 with a count of `1315`. This is recorded
as displayed by the interface and should not be described as 1,315
unique vulnerabilities without validating the underlying inventory
records.

**Evidence:** Figure 10 --- `10-security-findings.png`

## 9. Findings and Recommendations

  ----------------------------------------------------------------------------
  ID                Observation          Interpretation    Recommendation
  ----------------- -------------------- ----------------- -------------------
  F-01              Agent `DC01` (`002`) Connectivity      Monitor agent
                    appeared as Active   confirmed at      availability and
                                         capture time      event-ingestion
                                                           health

  F-02              Windows Security     Evidence supports Periodically verify
                    events were visible  endpoint event    required channels
                    in Wazuh             visibility        and collection
                                                           settings

  F-03              Rule `60122`         Failed logons     Review adjacent
                    reported             need context and  events and repeated
                    authentication       correlation       or unexpected
                    failures                               failures

  F-04              Event `4625` showed  Consistent with a Validate whether
                    logon type `7`,      local             the activity was
                    loopback IP, and     authentication    expected
                    incorrect-password   failure           
                    substatus                              

  F-05              Wazuh displayed a    Mapping alone     Validate
                    `T1531` mapping for  does not prove    rule-to-technique
                    rule `60122`         account access    associations
                                         removal           against event
                                                           semantics

  F-06              SCA displayed 87     Multiple          Prioritize
                    passed, 270 failed,  benchmark checks  applicable checks
                    2 not applicable,    require review    and apply approved
                    and 24%                                hardening

  F-07              Vulnerability        The count needs   Inspect individual
                    Detection displayed  inventory-level   records, CVEs,
                    a count of 1315 for  interpretation    severity, and
                    the listed                             affected versions
                    package/product                        
  ----------------------------------------------------------------------------

### Recommended priorities

1.  Review SCA failures and prioritize them according to severity,
    exposure, and operational impact.
2.  Correlate authentication events to establish expected behavior and
    identify suspicious patterns.
3.  Validate ATT&CK mappings before classifying a technique as observed.
4.  Review individual vulnerability records and applicable vendor fixes.
5.  Preserve timestamps and event context while redacting sensitive
    information before publication.

These are recommendations based on the evidence summarized here, not a
claim that all remediation steps have already been completed.

## 10. Limitations and Evidence Integrity

-   This is a controlled, single-endpoint demonstration, not a full
    production security assessment.
-   The observed alerts do not independently confirm a successful
    compromise.
-   A failed authentication event does not establish malicious intent.
-   A rule's ATT&CK mapping is not proof that the mapped technique
    occurred.
-   SCA failures require individual review before assigning risk.
-   Dashboard counts should not be treated as unique vulnerability
    counts without validating the underlying records.
-   Screenshots represent the environment at the time of capture.
-   Response actions must be reported only if actually performed and
    verified.

## 11. Evidence Index

  -------------------------------------------------------------------------------------
  Figure                  Evidence                File
  ----------------------- ----------------------- -------------------------------------
  Figure 1                Lab Network Validation  `01-lab-network-validation.png`

  Figure 2                Wazuh Dashboard         `02-wazuh-dashboard.png`

  Figure 3                Windows Agent           `03-windows-agent-installation.png`
                          Installation            

  Figure 4                Agent Connected         `04-agent-connected.png`

  Figure 5                Windows Security Events `05-windows-security-events.png`

  Figure 6                Authentication Alert    `06-authentication-alert.png`

  Figure 7                Alert Investigation     `07-alert-investigation.png`

  Figure 8                MITRE ATT&CK Analysis   `08-mitre-attack-analysis.png`

  Figure 9                Incident Response       `09-incident-response.png`

  Figure 10               Security Findings       `10-security-findings.png`
  -------------------------------------------------------------------------------------

## 12. Repository Structure

``` text
11-wazuh-blue-team/
├── images/
│   ├── 01-lab-network-validation.png
│   ├── 02-wazuh-dashboard.png
│   ├── 03-windows-agent-installation.png
│   ├── 04-agent-connected.png
│   ├── 05-windows-security-events.png
│   ├── 06-authentication-alert.png
│   ├── 07-alert-investigation.png
│   ├── 08-mitre-attack-analysis.png
│   ├── 09-incident-response.png
│   └── 10-security-findings.png
└── README.md
```

## 13. Security and Privacy

Before publishing the evidence to a public GitHub repository, check that
screenshots do not expose passwords, enrollment secrets, authentication
keys, tokens, private keys, or unnecessary personal information. Redact
sensitive details where appropriate and never commit credentials.

## 14. Conclusion

Laboratory 11 demonstrated a defensive monitoring workflow using Wazuh
and a Windows Server endpoint. The work covered network validation,
agent connectivity, Windows event visibility, failed-authentication
alert investigation, ATT&CK mapping review, incident triage, and
configuration-assessment findings.

The central lesson is that SIEM alerts require contextual investigation.
Analysts must distinguish between an event, a rule match, a framework
mapping, and a confirmed incident. Configuration-assessment results and
vulnerability counts must likewise be validated at the finding level
before conclusions or remediation priorities are assigned.

This lab complements the portfolio's earlier work on virtual lab
infrastructure, vulnerability assessment, web application security,
Active Directory, and cloud security by adding a Blue Team monitoring
and investigation workflow.

## 15. References

-   [Wazuh Documentation --- Navigating the Wazuh
    Dashboard](https://documentation.wazuh.com/current/user-manual/wazuh-dashboard/navigating-the-wazuh-dashboard.html)
-   [Wazuh Documentation --- Wazuh
    Agent](https://documentation.wazuh.com/current/user-manual/agent/index.html)
-   [Wazuh Documentation --- Security Configuration
    Assessment](https://documentation.wazuh.com/current/user-manual/capabilities/sec-config-assessment/index.html)
-   [Wazuh Documentation --- Vulnerability
    Detection](https://documentation.wazuh.com/current/getting-started/use-cases/vulnerability-detection.html)
-   [MITRE ATT&CK --- Account Access Removal
    (T1531)](https://attack.mitre.org/techniques/T1531/)
-   [Microsoft Learn --- Audit
    Logon](https://learn.microsoft.com/en-us/windows/security/threat-protection/auditing/audit-logon)

## 16. Disclaimer

This laboratory was conducted for educational purposes in a controlled
virtual environment. Security monitoring, testing, and response actions
should only be performed on systems that you own or are explicitly
authorized to administer. The findings documented here are limited to
the evidence collected during this exercise and do not certify the
security posture of a production system.

