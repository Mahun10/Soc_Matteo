
---

# SOC Infrastructure Implementation: Wazuh, Opensearch, and Logstash Pipeline

## Project Overview

This project documents the deployment of a centralized Security Operations Center (SOC) infrastructure. The architecture utilizes a distributed SIEM/XDR model where log collection, processing, and indexing are decoupled to ensure modularity and scalability.

The solution integrates the Wazuh Manager with a Logstash pipeline for data transformation, using OpenSearch as the primary data store.

## System Architecture

The infrastructure is built as a multi-stage data pipeline designed for real-time telemetry analysis.

### Data Flow and Components

1. **Endpoint (Windows VM)**: The Wazuh Agent collects security events and system telemetry.
2. **Wazuh Manager (Debian)**: Receives and analyzes events. It generates alerts based on rule matching and decoders.
3. **Logstash Pipeline**: Acts as the bridge between the Manager and the Indexer. It receives alerts via filebeat or syslog, processes them, and pushes them to the OpenSearch API.
4. **OpenSearch**: Manages data indexing and storage. It receives alert data from Logstash and metadata directly from the management services.
5. **Wazuh Dashboard**: A web interface connected to OpenSearch using dedicated credentials. It allows for visualization of security alerts and agent management.

### Technology Stack

| Component | Technology | Role | Port | Protocol |
| --- | --- | --- | --- | --- |
| **SIEM Engine** | Wazuh Manager | Threat detection and rule engine | 1514/1515 | TCP |
| **Data Transport** | Logstash | ETL: Routing logs to OpenSearch | - | - |
| **Search Engine** | OpenSearch | Data indexing and storage | 9200 | TCP |
| **Visualization** | Wazuh Dashboard | Web UI for analytics | 5601 | HTTP |
| **Server OS** | Debian 12 | Hosting the management stack | - | - |
| **Endpoint OS** | Windows Server 2022 | Monitored target | - | - |

---

## Implementation Details

### Server Configuration (Debian)

The management stack ensures seamless communication between the detection engine and the storage layer.

* **Logstash Integration**: Configured with a dedicated pipeline (`01-wazuh.conf`) to ingest Wazuh alerts and output them to the OpenSearch cluster.
* **Storage Layer**: OpenSearch is configured to index security alerts and maintain agent status information.
* **Dashboard Access**: The dashboard is configured to communicate with the Indexer via internal credentials defined in the configuration files. It is accessible on port **5601** over **HTTP**.

### Network Hardening

The following ports were opened on the Debian host to ensure connectivity:

* **1514/TCP**: Agent communication (Events).
* **1515/TCP**: Agent registration.
* **5601/TCP**: Dashboard web access.
* **9200/TCP**: OpenSearch REST API (Restricted to internal traffic).

### Endpoint Monitoring (Windows)

* **Agent Deployment**: Installed on Windows 10/11, pointing to the Debian Manager's static IP.
* **Monitoring Scope**: Real-time tracking of Windows Event Logs (System, Security, Application) and File Integrity Monitoring (FIM) for critical system directories.

---

## Key SOC Features Demonstrated

* **Automated Log Pipeline**: Implementation of a Manager -> Logstash -> OpenSearch flow for optimized data handling.
* **Vulnerability Management**: Automated detection of known CVEs and outdated software on the Windows endpoint.
* **File Integrity Monitoring**: Tracking unauthorized changes to critical system files and registry keys.
* **Centralized Incident Response**: Capability to query indexed security events and visualize attack patterns through the dashboard.

---

## Repository Structure

```plaintext
.
├── configs/
│   ├── wazuh_manager/      # Wazuh Manager settings (ossec.conf)
│   ├── logstash/           # Logstash pipeline (01-wazuh.conf)
│   ├── opensearch/         # OpenSearch communication settings
│   └── wazuh_dashboard/    # Dashboard connection & credentials config
├── docs/
│   └── report_soc.pdf      # Technical documentation in LaTeX format
├── images/                 # Architecture diagrams and dashboard captures
└── README.md               # Project documentation

```

## Author

**Matteo PADONOU**
Cybersecurity and SOC Engineering Project

---

