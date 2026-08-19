# Environment Build Summary

This document condenses the completed build steps from the original project while omitting personal usernames, public IP addresses, VPC identifiers, and key-file names.

## 1. Kali Attacker

- Deployed Kali Linux on AWS EC2.
- Restricted SSH administration to an authorized source.
- Installed Nmap, curl, Git, Python, networking utilities, and supporting security tools.
- Verified network communication with the other project systems.

## 2. Ubuntu Target and Log Source

Installed and enabled logging services:

```bash
sudo apt update
sudo apt install -y rsyslog auditd
sudo systemctl enable --now rsyslog
sudo systemctl enable --now auditd
```

Generated and verified a test event:

```bash
logger "Purple Team pipeline validation event"
sudo tail -n 20 /var/log/syslog
```

Installed and enabled Apache for the controlled web-testing scenario:

```bash
sudo apt install -y apache2
sudo systemctl enable --now apache2
curl http://localhost
```

Configured rsyslog to forward events to the SIEM using TCP:

```text
*.* @@<SIEM-PRIVATE-IP>:514
```

## 3. ELK SIEM Server

The completed environment used:

- Elasticsearch for indexing and storage
- Logstash for syslog ingestion and processing
- Kibana for search and investigation

Elasticsearch was configured for a single-node lab deployment, Kibana was enabled for authorized access, and Logstash received syslog events from the Ubuntu system.

A `syslog-*` data view was created in Kibana using `@timestamp` as the time field.

## 4. Pipeline Validation

A test message was generated on the Ubuntu source and confirmed in Kibana before offensive activity began. This established a known-good data path:

```text
Ubuntu / Apache → rsyslog → Logstash → Elasticsearch → Kibana
```

## Security Notes

The original environment was temporary and educational. A production deployment should:

- avoid binding management interfaces to all addresses;
- use TLS and authentication for Elastic services;
- restrict security-group rules to approved sources;
- keep Elasticsearch and Kibana off the public internet;
- use supported package-signing and repository configuration;
- separate administrative, ingestion, and user-access paths.
