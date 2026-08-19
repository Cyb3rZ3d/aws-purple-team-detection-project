
# AWS Purple Team Detection Lab Architecture

## Overview

This lab uses separate attacker, target, and defensive systems hosted in AWS to demonstrate a complete Purple Team workflow.

The attacker performs reconnaissance and exploitation against the target system. The target generates security telemetry, which is forwarded into the ELK Stack for centralized logging, investigation, IOC analysis, and detection engineering.

## Architecture Diagram

```mermaid
flowchart LR

    subgraph AWS["AWS Environment"]

        subgraph RED["Attacker"]
            KALI["Kali Linux<br/>Nmap • curl"]
        end

        subgraph TARGET["Target"]
            UBUNTU["Ubuntu Linux"]
            APACHE["Apache HTTP Server"]
        end

        subgraph DEFENDER["Defender / SIEM"]
            RSYSLOG["rsyslog<br/>Log Collection"]
            LOGSTASH["Logstash<br/>Log Processing"]
            ELASTIC["Elasticsearch<br/>Indexing & Storage"]
            KIBANA["Kibana<br/>SIEM Analysis"]
        end

    end

    KALI -->|"Reconnaissance<br/>Nmap"| APACHE
    KALI -->|"HTTP Exploitation<br/>CVE Testing"| APACHE

    APACHE --> UBUNTU

    UBUNTU -->|"System / Apache Logs"| RSYSLOG
    RSYSLOG -->|"Forwarded Events"| LOGSTASH
    LOGSTASH -->|"Processed Events"| ELASTIC
    ELASTIC -->|"Searchable Events"| KIBANA

    KIBANA --> ANALYSIS["IOC Analysis<br/>KQL Investigation<br/>MITRE ATT&CK Mapping<br/>Detection Engineering"]



flowchart TD

    A["1. Reconnaissance"] --> B["2. Service Enumeration"]
    B --> C["3. Vulnerability Identification"]
    C --> D["4. Exploitation Attempt"]
    D --> E["5. Security Telemetry Generated"]
    E --> F["6. Logs Forwarded to SIEM"]
    F --> G["7. Kibana Investigation"]
    G --> H["8. IOC Identification"]
    H --> I["9. MITRE ATT&CK Mapping"]
    I --> J["10. Detection Development"]
