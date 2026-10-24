# Open-Source SOC & SIEM Lab with Wazuh

## Overview

This project implements an open-source Security Operations Center (SOC) and SIEM laboratory based on Wazuh.

The objective is to deploy a complete monitoring architecture, collect security events from a Linux endpoint, detect suspicious activities, correlate events, create custom detection rules, and investigate alerts through the Wazuh Dashboard.

The environment uses Docker, WSL2, and an Ubuntu endpoint monitored by the Wazuh Agent.

## Key Features

- Wazuh single-node SIEM deployment using Docker
- Ubuntu endpoint monitoring with Wazuh Agent
- Centralized collection of system and authentication logs
- Real-time File Integrity Monitoring (FIM)
- Custom Wazuh detection rules
- SSH brute-force detection through event correlation
- Monitoring of privileged sudo activity
- MITRE ATT&CK mapping
- Threat Hunting and SOC-style alert investigation

## Detection Scenarios

| Scenario | Detection | Wazuh Rule | Severity | MITRE ATT&CK |
|---|---|---:|---:|---|
| Critical file modification | Real-time FIM + custom detection | 100002 | 12 | T1565.001 - Stored Data Manipulation |
| SSH brute-force | Correlation of repeated failed SSH authentication attempts | 5712 | 10 | T1110 - Brute Force |
| Privileged command execution | Custom detection of monitored sudo command | 100003 | 10 | T1548.003 - Sudo and Sudo Caching |

## Architecture

The laboratory follows the following security event pipeline:

```text
Ubuntu Endpoint
      |
      | Wazuh Agent
      | journald / FIM
      v
Wazuh Manager
      |
      | Decoders / Rules / Correlation
      v
Wazuh Indexer
      |
      | Alert storage and indexing
      v
Wazuh Dashboard
      |
      v
Threat Hunting / SOC Investigation
```

## Technologies

- Wazuh 4.14.8
- Docker and Docker Compose
- Ubuntu 24.04 LTS / WSL2
- Wazuh Indexer (OpenSearch-based)
- OpenSSH
- systemd / journald
- MITRE ATT&CK
- DQL for Threat Hunting
