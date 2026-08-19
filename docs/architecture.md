
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
