# IoT Software Defined Network IPS

An integrated security architecture for Internet of Things (IoT) networks combining Software Defined Networking (SDN), firewalls, and Intrusion Prevention Systems (IPS) to detect and block anomalous traffic at the network edge.

## Overview

The programmability resulting from the Software Defined Networking (SDN) approach facilitates the integration of the functionalities of firewalls, Intrusion Prevention Systems (IPS) and switching gear, allowing fast reconfiguration of the network in case of anomaly detection. This project implements a distributed security measure integrating firewall, IPS, switches and a controller entity to support Internet of Things (IoT) instances. The architecture enables identification of anomalous behavior of IoT devices by the IPS, leading the SDN to block attacks as near as possible to the sources, reducing malicious traffic volume and isolating infected devices from the rest of the network.

**Published paper:** https://ieeexplore.ieee.org/document/8896297/

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Features](#features)
- [System Requirements](#system-requirements)
- [Hardware Requirements](#hardware-requirements)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Setup Instructions](#setup-instructions)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contributing](#contributing)
- [License](#license)

## Architecture

The system is composed of three main components:

1. **SDN Controller** - Manages network flow rules and coordinates security policies
2. **Intrusion Prevention System (IPS)** - Monitors and analyzes traffic for anomalies using Snort
3. **Network Switches** - OpenFlow-enabled switches for enforcing security rules and blocking malicious traffic

The architecture operates in a distributed manner, allowing real-time threat detection and response with minimal latency.

## Features

- **Real-time Threat Detection** - Continuous monitoring of network traffic for anomalous behavior
- **Automated Response** - Automatic blocking of detected threats at network edge
- **Distributed Architecture** - Scalable design supporting multiple IoT devices
- **SDN Integration** - Dynamic network reconfiguration based on security events
- **Centralized Management** - Controller-based policy management and coordination

## System Requirements

### Software Requirements

- **Operating System:** Linux (Ubuntu 16.04 LTS or later recommended)
- **Python:** 3.5 or higher
- **OpenFlow Controller:** Ryu (included)
- **IPS Engine:** Snort 2.9.x or later
- **Network Switch:** OpenFlow 1.3 compatible switches

### Python Dependencies

The project requires the following Python packages:

| Package | Version | Purpose |
|---------|---------|---------|
| Flask | 1.0+ | REST API framework for controllers |
| Ryu | 4.0+ | OpenFlow SDN controller framework |
| Paramiko | 2.0+ | SSH/SFTP for device management |
| SQLite3 | Built-in | Database for rules, logs, and configuration |
| Scapy | 2.4+ | Network packet manipulation and analysis |
| Statistics | Built-in | Statistical analysis for anomaly detection |

#### Install Python Dependencies

```bash
pip install Flask>=1.0
pip install ryu>=4.0
pip install paramiko>=2.0
pip install scapy>=2.4
```

Or create a `requirements.txt` file with the following content and install:

```
Flask>=1.0
ryu>=4.0
paramiko>=2.0
scapy>=2.4
```

Then install with:
```bash
pip install -r requirements.txt
```

## Hardware Requirements

### Minimum Configuration

For testing and small-scale deployments:

| Component | Specification |
|-----------|---------------|
| **Controller Server** | 2+ CPU cores, 4GB RAM, 10GB storage |
| **IPS/Sniffer Server** | 2+ CPU cores, 4GB RAM, 10GB storage |
| **Network Switch** | 1 OpenFlow-enabled switch (e.g., OVS, Pica8) |
| **IoT Devices** | 2+ devices (physical or virtual) |
| **Network Connection** | Gigabit Ethernet (1Gbps minimum) |

### Recommended Configuration (Real-World Production)

For production IoT deployments with multiple sites:

| Component | Specification |
|-----------|---------------|
| **Main Controller Server** | Intel Xeon / ARM (8+ cores), 16-32GB RAM, 256GB SSD |
| **Regional Controllers** | Intel i7 / Ryzen 7 (4+ cores), 8GB RAM, 128GB SSD (per site) |
| **IPS/Sniffer Appliance** | Specialized IPS hardware or 2x Xeon (8+ cores), 16GB RAM, 1TB SSD |
| **Network Switches** | Enterprise OpenFlow switches (48+ ports) with redundancy |
| **Firewalls** | Hardware firewall appliances (5-10Gbps throughput) |
| **IoT Gateway Devices** | 1-2 gateways per IoT site, 2+ cores, 2GB RAM |
| **Network Infrastructure** | 10Gbps backbone, Gigabit to edge devices |
| **Backup/Storage** | RAID-enabled NAS for log storage and redundancy |
| **UPS/Power Supply** | Uninterruptible power supply for 24/7 operation |

### Network Architecture

```
┌─────────────────┐
│  IoT Devices    │
│  (Sensors/etc)  │
└────────┬────────┘
         │
    ┌────▼─────┐
    │  Gateway  │
    │ (OpenFlow)│
    └────┬─────┘
         │
    ┌────▼──────────────┐
    │  Network Switch   │
    │ (OpenFlow 1.3)    │
    └────┬──────────────┘
         │
    ┌────┴────────────────────┐
    │                         │
┌───▼────────┐      ┌────────▼────┐
│ SDN        │      │ IPS/Sniffer │
│ Controller │◄────►│ (Snort)     │
└────────────┘      └─────────────┘
```

## Prerequisites

Before installation, ensure you have:

- Root or sudo access on Linux servers
- Network connectivity between all components
- OpenFlow-enabled network switches
- Snort IPS installed and configured
- Git for cloning the repository

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd IoT-Software-Defined-Network--IPS
```

### 2. Install Dependencies

```bash
pip install --upgrade pip
pip install Flask>=1.0
pip install ryu>=4.0
pip install paramiko>=2.0
pip install scapy>=2.4
```

### 3. Install Snort IPS

```bash
sudo apt-get install snort
# Configure Snort with appropriate rules for IoT traffic
```

### 4. Configure Ryu Controller

Update configuration files in the Controller directory with your network topology and security policies.

## Setup Instructions

### 1. Controller Setup

Navigate to the Controller directory and configure:

```bash
cd Code/Controller
# Edit database configuration
nano database/config.conf
# Edit security policies
nano interpreter/systemapiOptions.py
```

### 2. Global Controller Setup

Configure the centralized control policies:

```bash
cd Code/ControllerGlobal
# Edit global settings
nano controllerglobal.py
```

### 3. IPS Configuration

Configure Snort rules for IoT traffic detection:

```bash
cd Code/SnortAPI
# Update Snort configuration
nano database/snort.conf
```

### 4. Network Topology

Define your network topology in the SDN controller before starting:

```bash
cd Code/SDN
# Configure switch connections and ports
nano switchL4.py
```

### 5. System API

Configure the API endpoints for management:

```bash
cd Code/SystemAPI
# Edit API settings
nano main.py
```

## Usage

### Start the SDN Controller

```bash
cd Code/Controller
python main.py
```

### Start the Global Controller

```bash
cd Code/ControllerGlobal
python main.py
```

### Start the IPS/Snort Listener

```bash
cd Code/SnortAPI
python main.py
```

### Start the System API

```bash
cd Code/SystemAPI
python main.py
```

### Verify System Status

```bash
# Check controller logs
tail -f logs/controller.log

# Monitor IPS alerts
tail -f logs/snort_alerts.log

# Check SDN flows
ovs-ofctl dump-flows <bridge-name>
```

## Project Structure

```
Code/
├── apimetrics.py              # API performance monitoring
├── Controller/                # Local SDN controller
│   ├── main.py
│   ├── interpreter/           # Command processing
│   ├── parallel/              # Health monitoring & logging
│   ├── SDN/                   # OpenFlow operations
│   └── database/
├── ControllerGlobal/          # Global coordination controller
│   ├── main.py
│   ├── controllerglobal.py
│   ├── ruleApply.py
│   └── database/
├── SnortAPI/                  # IPS integration
│   ├── main.py
│   ├── classes/
│   ├── interpreter/
│   └── ruleParser/
├── SystemAPI/                 # System management API
│   ├── main.py
│   ├── test.py
│   └── parallel/
└── SDN/                       # SDN utilities
    ├── ryu_test.py
    ├── switchL4.py
    └── ovs_scripts/
```

## Configuration Files

Key configuration files to modify for your deployment:

- `Controller/database/` - Database schema and settings
- `ControllerGlobal/` - Global policies and rules
- `SnortAPI/database/` - IPS rules and thresholds
- `SystemAPI/` - API endpoints and permissions

## Troubleshooting

### Controller Connection Issues
- Verify network connectivity between controller and switches
- Check OpenFlow port (default 6633/6653)
- Verify firewall rules allow controller traffic

### IPS Detection Issues
- Ensure Snort is running and properly configured
- Check Snort rule files for syntax errors
- Monitor CPU/memory usage on IPS server

### Performance Issues
- Monitor controller CPU and memory usage
- Check network bandwidth utilization
- Optimize Snort rule sets for your IoT traffic patterns

## Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -am 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Submit a pull request

## License

This project is released under the MIT License. See LICENSE file for details.

## References

- **Published Paper:** https://ieeexplore.ieee.org/document/8896297/
- **Ryu Framework:** https://osrg.github.io/ryu/
- **OpenFlow Specification:** https://www.opennetworking.org/
- **Snort IPS:** https://www.snort.org/
