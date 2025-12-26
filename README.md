
# SOC Infrastructure Implementation: Wazuh, Indexer, and Logstash

## Project Overview

This project documents the deployment of a centralized Security Operations Center (SOC) infrastructure. It features a distributed SIEM (Security Information and Event Management) and XDR (Extended Detection and Response) architecture.

The solution is built on the Wazuh ecosystem, integrated with Logstash for log processing and Wazuh Indexer for data storage. The primary objective is to establish a robust security monitoring environment capable of real-time threat detection, log analysis, and vulnerability assessment across a hybrid network.

## System Architecture

The infrastructure is designed as a modular data pipeline to ensure scalability and efficient log management.

### Data Flow Pipeline

1. **Endpoint (Windows VM)**: The Wazuh Agent collects system logs, file integrity data, and security events.
2. **Processing (Debian VM)**: The Wazuh Manager analyzes incoming telemetry against pre-defined decoders and rules.
3. **Transformation (Logstash)**: Acts as the ETL (Extract, Transform, Load) layer to normalize data before storage.
4. **Indexing (Wazuh Indexer)**: A search and analytics engine that stores, indexes, and organizes security alerts.
5. **Visualization (Wazuh Dashboard)**: The central web interface for security monitoring and incident response.

### Technology Stack

| Component | Technology | Role |
| --- | --- | --- |
| SIEM / XDR | Wazuh Manager | Threat detection and centralized management |
| Indexer | Wazuh Indexer | Data indexing and storage (OpenSearch-based) |
| Log Pipeline | Logstash | Data transformation and routing |
| Visualization | Wazuh Dashboard | Web UI for analytics and monitoring |
| Server OS | Debian 12 GNU/Linux | Hosting the management stack |
| Endpoint OS | Windows Server 2022 | Monitored target via Wazuh Agent |

---

## Implementation Details

### Server Deployment (Debian)

The management stack was installed on a Debian environment. The configuration focuses on the seamless integration between the Manager, Logstash, and the Indexer.

* **Service Management**: All core services (wazuh-manager, wazuh-indexer, logstash, wazuh-dashboard) were configured to start on boot.
* **Network Hardening**: The Debian host was configured to allow inbound traffic on essential ports:
* Port 1514 (TCP): Agent communication.
* Port 1515 (TCP): Agent registration.
* Port 443 (HTTPS): Dashboard access.



### Endpoint Configuration (Windows)

The monitoring of the Windows VM was achieved through the deployment of the Wazuh Agent.

* **Connectivity**: The agent was pointed to the Manager's static IP address via the `ossec.conf` configuration file.
* **Network Validation**: Verified that both VMs reside on the same subnet to ensure low-latency communication.
* **Monitoring Scope**: Configured to monitor Windows Event Logs (System, Security, Application) and File Integrity Monitoring (FIM).

---

## Key SOC Features Demonstrated

* **Real-time Threat Detection**: Immediate alerts for suspicious activities such as brute-force attacks or unauthorized access.
* **Vulnerability Management**: Automated detection of known CVEs and outdated software on the Windows endpoint.
* **Log Pipeline Management**: Demonstrated proficiency in using Logstash to bridge the management engine and the indexing layer.
* **File Integrity Monitoring**: Tracking unauthorized changes to critical system files and registry keys.

---

## Repository Structure

```plaintext
.
├── configs/
│   ├── wazuh_manager/      # Sanitized ossec.conf for the manager
│   ├── wazuh_agent/        # Windows agent configuration snippets
│   └── logstash/           # Logstash pipeline (01-wazuh.conf)
├── docs/
│   └── report_soc.pdf      # Detailed technical report in LaTeX format
├── images/                 # Architecture diagrams and dashboard captures
└── README.md               # Project documentation

```

## Technical Documentation

A detailed technical report including step-by-step installation, troubleshooting procedures, and configuration logic is available in the documentation folder.

**[View the Full Technical Report (PDF)](https://www.google.com/search?q=docs/report_soc.pdf)**

---

## Author

**Matteo PADONOU** Cybersecurity and SOC Engineering Project

---

