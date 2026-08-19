# Purple Team Attack Scenario

## Overview

This scenario demonstrates a controlled Purple Team exercise against an Apache HTTP Server hosted on an Ubuntu Linux system in AWS.

The exercise was designed to connect offensive security activity with defensive monitoring. Reconnaissance and exploitation activity generated telemetry that could be collected, centralized in the ELK Stack, investigated using Kibana Query Language (KQL), and analyzed for indicators of compromise (IOCs) and MITRE ATT&CK behaviors.

> **Scope:** All activity documented in this project was performed within an authorized lab environment.

---

## Scenario Objectives

The objectives of the exercise were to:

* Perform network reconnaissance against the target system.
* Identify the exposed Apache HTTP service.
* Investigate vulnerabilities associated with the identified service.
* Test Apache CVE-2021-41773 and CVE-2021-42013 in the lab.
* Generate security telemetry from controlled attack activity.
* Forward target logs to the ELK Stack.
* Investigate attack activity using Kibana and KQL.
* Identify useful indicators and attack patterns.
* Map observed behavior to MITRE ATT&CK.
* Identify opportunities for defensive detection.

---

## Environment

| Role           | System             | Purpose                                         |
| -------------- | ------------------ | ----------------------------------------------- |
| Attacker       | Kali Linux         | Reconnaissance and controlled attack simulation |
| Target         | Ubuntu Linux       | Hosts the target environment                    |
| Web Service    | Apache HTTP Server | Web application attack surface                  |
| Log Collection | rsyslog            | Collects and forwards target telemetry          |
| Log Processing | Logstash           | Processes incoming log events                   |
| Data Store     | Elasticsearch      | Indexes and stores security events              |
| Analysis       | Kibana             | Event investigation and visualization           |

For the complete architecture, see [Architecture Documentation](01-architecture.md).

---

# Phase 1 — Reconnaissance

The exercise began with reconnaissance from the Kali Linux attacker.

Nmap was used to identify accessible network services and determine the attack surface presented by the target.

The reconnaissance process focused on:

* Identifying reachable services
* Enumerating open ports
* Identifying the web service
* Gathering service information
* Establishing potential attack paths

### Defensive Significance

Reconnaissance provides defenders with an early opportunity to identify suspicious activity before exploitation occurs.

Potential indicators include:

* Repeated connection attempts
* Sequential port access
* Unusual connection frequency
* Connections originating from unexpected systems
* Service enumeration patterns

---

# Phase 2 — Vulnerability Analysis

After identifying the Apache HTTP service, vulnerabilities associated with the target configuration were investigated.

The lab focused on:

* **CVE-2021-41773**
* **CVE-2021-42013**

These vulnerabilities affect specific vulnerable versions/configurations of Apache HTTP Server and can involve path traversal behavior, with exploitation impact depending on server configuration.

The purpose of the lab was not simply to demonstrate exploitation, but to examine how exploitation attempts appear from a defender's perspective.

---

# Phase 3 — Controlled Exploitation

Controlled HTTP requests were generated from the Kali Linux attacker against the Apache target.

The activity was designed to produce observable telemetry associated with exploitation attempts.

Testing focused on identifying evidence such as:

* Suspicious HTTP request paths
* Path traversal patterns
* Requests targeting sensitive resources
* Abnormal HTTP behavior
* Repeated requests from the attacker
* Timestamps corresponding with known attack activity

No persistence, destructive actions, or attacks against systems outside the authorized lab were performed.

---

# Phase 4 — Telemetry Generation

Attack activity against the target generated logs that could be analyzed from the defensive side.

Relevant telemetry included Apache and Linux system activity associated with the exercise.

The defensive pipeline followed:

```text
Attack Activity
      ↓
Apache / Ubuntu
      ↓
Security Telemetry
      ↓
rsyslog
      ↓
Logstash
      ↓
Elasticsearch
      ↓
Kibana
```

This provided a centralized location for investigating events associated with the simulated attack.

---

# Phase 5 — SIEM Investigation

Kibana was used to investigate the events generated during reconnaissance and exploitation.

Analysis focused on correlating known attacker activity with events recorded by the target.

Investigation areas included:

* Source addresses
* Event timestamps
* HTTP requests
* Request paths
* Web server activity
* Repeated requests
* Suspicious patterns
* Activity occurring during the known attack window

KQL was used to filter the available telemetry and isolate events relevant to the investigation.

---

# Phase 6 — IOC Analysis

Observed activity was reviewed for information that could assist future investigations and detection development.

Potential indicators included:

| Indicator Type    | Defensive Value                                     |
| ----------------- | --------------------------------------------------- |
| Source address    | Identifies the origin of observed lab activity      |
| Timestamp         | Establishes attack chronology                       |
| HTTP request      | Shows attacker interaction with the service         |
| Request path      | May reveal traversal or exploitation behavior       |
| Target service    | Identifies the attacked service                     |
| Repeated activity | Helps distinguish scanning or exploitation patterns |

Any examples published in this repository should be sanitized before being made public.

---

# Phase 7 — MITRE ATT&CK Mapping

Observed behavior was mapped to the MITRE ATT&CK framework to describe attacker actions using standardized terminology.

Relevant behaviors included:

| Activity                           | ATT&CK Mapping                                                                                                |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Network/service enumeration        | Network Service Discovery                                                                                     |
| Exploitation of the Apache service | Exploitation for Client Execution / Exploit Public-Facing Application, as applicable to the observed behavior |
| Shell or command activity          | Command and Scripting Interpreter, if demonstrated by collected evidence                                      |

ATT&CK mappings should be based on the behavior actually demonstrated in the lab rather than simply the vulnerability being tested.

---

# Phase 8 — Detection Opportunities

The final stage evaluated how observed attacker activity could be transformed into defensive analytics.

Potential detection opportunities included:

* Suspicious path traversal sequences
* Requests containing abnormal encoded characters
* Multiple suspicious requests from the same source
* Web requests associated with known exploitation patterns
* Reconnaissance followed by exploitation activity
* Unusual requests targeting sensitive filesystem locations

These observations can be used to develop reusable KQL or Sigma detections.

The completed investigation query is maintained under:

- [Apache path-traversal KQL](../02-detection-rules/kql/apache-path-traversal.kql)

The exercise confirmed that the patched Apache 2.4.58 target rejected both controlled requests while still generating searchable GET and POST telemetry with HTTP 400 responses.

---

# Attack-to-Detection Workflow

```mermaid
flowchart LR

    A["Reconnaissance"] --> B["Vulnerability Analysis"]
    B --> C["Controlled Exploitation"]
    C --> D["Telemetry Generation"]
    D --> E["Log Collection"]
    E --> F["SIEM Investigation"]
    F --> G["IOC Analysis"]
    G --> H["ATT&CK Mapping"]
    H --> I["Detection Engineering"]
```

---

# Key Takeaways

This exercise demonstrated how offensive testing can directly support defensive security operations.

Rather than treating penetration testing and monitoring as separate activities, the Purple Team approach connected:

**Attacker behavior → telemetry → investigation → intelligence → detection**

The exercise provided hands-on experience with reconnaissance, vulnerability analysis, centralized logging, SIEM investigation, IOC analysis, MITRE ATT&CK mapping, and detection engineering.
