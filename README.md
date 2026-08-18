# AWS Purple Team Detection Lab

An AWS-based Purple Team lab designed to simulate adversary activity, collect security telemetry, analyze attack behavior in a SIEM, and map observed activity to the MITRE ATT&CK framework.

## Overview

This project demonstrates a hands-on Purple Team workflow combining offensive security testing and defensive detection analysis in an AWS environment.

The lab was built using separate virtual systems for attack simulation, target exploitation, centralized logging, and SIEM analysis. The objective was to generate realistic attack activity, capture the resulting telemetry, investigate indicators of compromise, and evaluate defensive visibility.

## Objectives

* Build an isolated AWS-based Purple Team environment
* Simulate reconnaissance and exploitation activity
* Generate security-relevant telemetry from the target system
* Centralize logs using the ELK Stack
* Analyze attack activity using Kibana and KQL
* Identify indicators of compromise
* Map attack behavior to MITRE ATT&CK
* Evaluate detection opportunities based on observed telemetry

## Lab Architecture

The lab consisted of three primary systems:

* **Attacker:** Kali Linux
* **Target:** Ubuntu Linux running Apache
* **Defender / SIEM:** ELK Stack

High-level data flow:

```text
Kali Linux
    |
    | Reconnaissance / Attack Activity
    v
Ubuntu + Apache
    |
    | rsyslog
    v
Logstash
    |
    v
Elasticsearch
    |
    v
Kibana
    |
    v
Detection Analysis / IOC Review / ATT&CK Mapping
```

A visual architecture diagram will be added to the `diagrams/` directory.

## Technologies Used

### Cloud

* Amazon Web Services
* Amazon EC2
* VPC networking
* Security Groups

### Offensive Security

* Kali Linux
* Nmap
* curl
* Linux command-line tools

### Target Environment

* Ubuntu Linux
* Apache HTTP Server

### Logging and SIEM

* rsyslog
* Logstash
* Elasticsearch
* Kibana
* Kibana Query Language

### Analysis

* Indicators of Compromise
* CVE analysis
* MITRE ATT&CK
* Security event investigation

## Attack Scenario

The attack workflow focused on identifying and testing vulnerabilities affecting an Apache web server.

The lab included activity related to:

* CVE-2021-41773
* CVE-2021-42013

These vulnerabilities affected specific Apache HTTP Server versions and were used in the lab to demonstrate how exploitation activity could be observed and investigated from the defensive side.

## Reconnaissance

Reconnaissance activity was performed from the Kali Linux attacker system.

Examples of activity included:

* Host discovery
* Port scanning
* Service enumeration
* Web service identification
* Target validation prior to exploitation

Nmap was used to identify exposed services and gather information about the target environment.

## Exploitation

After reconnaissance, the target Apache service was tested using crafted HTTP requests.

The objective of this stage was to generate realistic attack traffic and validate whether the attack behavior could be observed through collected logs.

The project focused on defensive analysis of the activity rather than persistence or destructive actions.

## Logging Pipeline

Security telemetry was centralized using the ELK Stack.

The general log flow was:

```text
Ubuntu / Apache
      |
      v
   rsyslog
      |
      v
   Logstash
      |
      v
Elasticsearch
      |
      v
    Kibana
```

This allowed attack-related activity to be reviewed and analyzed from a centralized interface.

## SIEM Analysis

Kibana was used to review and investigate events generated during the attack simulation.

Analysis focused on:

* Suspicious HTTP requests
* Source IP activity
* Request paths
* Attack timing
* Repeated reconnaissance behavior
* Exploitation attempts
* Relationships between attacker actions and target logs

Kibana Query Language was used to filter and investigate relevant events.

## Detection Engineering

The project evaluated how observed attacker behavior could be converted into detection opportunities.

Detection concepts included:

* Repeated reconnaissance from a single source
* Suspicious HTTP request paths
* Requests associated with path traversal behavior
* Unusual command execution patterns
* High-frequency access attempts
* Attack activity mapped to known adversary techniques

Future versions of this repository will include reusable KQL and Sigma detection logic in the `detection-rules/` directory.

## Indicators of Compromise

Indicators observed during the lab included data such as:

* Source IP addresses
* Suspicious HTTP request paths
* Unusual web requests
* Exploitation patterns
* Event timestamps
* Targeted services

Sanitized IOC examples will be stored in the `iocs/` directory.

## MITRE ATT&CK Mapping

Observed activity was analyzed using the MITRE ATT&CK framework.

Relevant behavior included categories such as:

* Reconnaissance
* Network Service Scanning
* Exploitation of Public-Facing Applications
* Command and Scripting Interpreter

Additional ATT&CK mappings will be documented as the repository is expanded.

## Results

The lab demonstrated the full lifecycle of a Purple Team exercise:

```text
Attack Simulation
       |
       v
Telemetry Generation
       |
       v
Centralized Logging
       |
       v
SIEM Investigation
       |
       v
IOC Identification
       |
       v
MITRE ATT&CK Mapping
       |
       v
Detection Development
```

The exercise showed how offensive activity can be translated into defensive detection and investigation workflows.

## Repository Structure

```text
aws-purple-team-detection-lab/
|
|-- README.md
|-- LICENSE
|-- .gitignore
|
|-- docs/
|-- screenshots/
|-- detection-rules/
|-- scripts/
|-- iocs/
|-- diagrams/
```

## Planned Improvements

Future improvements include:

* Add architecture diagrams
* Add sanitized Kibana screenshots
* Add attack timeline documentation
* Add KQL detection queries
* Add Sigma detection rules
* Add IOC datasets
* Add MITRE ATT&CK technique mapping
* Add Python-based log analysis
* Expand attack scenarios
* Add automated detection validation

## Skills Demonstrated

* Purple Team Operations
* Security Operations
* Detection Engineering
* SIEM Analysis
* Cyber Threat Analysis
* Vulnerability Analysis
* Network Reconnaissance
* Linux
* AWS
* ELK Stack
* Kibana
* KQL
* MITRE ATT&CK
* IOC Analysis
* Technical Documentation

## Disclaimer

This project was conducted in an authorized lab environment for educational and defensive cybersecurity purposes.

Do not perform security testing against systems you do not own or have explicit permission to test.
