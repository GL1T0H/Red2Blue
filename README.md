# 🔴🔵 Red2Blue | Attack Simulation & Detection Lab (Splunk)

## 📌 Project Overview

### 1. What is Red2Blue?
**Red2Blue** is a hands-on home SOC lab designed to demonstrate how common attack techniques generate logs and telemetry, and how defenders can detect, investigate, and respond to these attacks using **Splunk**.

This project bridges the gap between **Red Team attack simulation** and **Blue Team detection engineering**, focusing on attacker behavior rather than tools.

### 2. Who is it for?
This lab is intended for:
- SOC Analysts (Junior / Entry-level)
- Blue Team & Detection Engineering learners
- Cybersecurity students
- Anyone seeking hands-on SOC experience without enterprise infrastructure

### 3. What you will learn
By working through this lab, you will learn how to:
- Build a complete SOC lab at home using Splunk
- Simulate realistic attack scenarios in a controlled environment
- Analyze attacker-generated telemetry
- Write effective SPL-based detections
- Reduce false positives and improve detection quality
- Think like a SOC analyst and detection engineer

### 4. Lab Architecture (High Level)

```text
+-------------------+        Logs / Events        +----------------------+
|                   |  ----------------------->  |                      |
|  Windows 10 VM    |                            |   Splunk Enterprise  |
|  (Attack Target)  |  Splunk Universal Forwarder|   (Host Machine)     |
|                   |                            |                      |
+-------------------+                            +----------------------+
```

---

## 🧩 Lab Requirements

### Hardware
- Minimum 16 GB RAM (8 GB possible with limitations)
- 100+ GB available disk space
- CPU with virtualization support enabled

### Host Operating System
- Windows 10
- Windows 11

### Virtualization
- VMware Workstation (recommended)
- VirtualBox (alternative)

### Software
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows 10 ISO
- Sysmon
- PowerShell (built-in)
- Optional: Wireshark or other network tools

----

## 🏗️ Lab Architecture & Design

### Network Topology
- Host-only network configuration
- Isolated lab environment
- Windows 10 VM can communicate only with the host
- No direct internet exposure (recommended)

### Data Flow
1. User activity or simulated attack occurs on the Windows 10 VM  
2. Telemetry is generated (Windows Events, Sysmon, PowerShell)  
3. Splunk Universal Forwarder sends logs to Splunk Enterprise  
4. Logs are indexed and analyzed in Splunk  

### Why this design?
- Safe and isolated for learning purposes
- No enterprise infrastructure required
- Focuses on detection logic rather than complex networking
- Closely resembles real SOC data collection workflows

### Security Considerations
- All attacks are simulated and non-destructive
- No real malware or live command-and-control infrastructure
- The lab should never be exposed to production or personal networks

---

## ⚙️ Lab Setup & Configuration

### Installing Splunk
- Install Splunk Enterprise on the host machine
- Access Splunk Web via `http://localhost:8000`
- Enable receiving on port `9997`
- Create dedicated indexes for incoming telemetry

### Network Configuration
- Configure the VM network as **Host-only**
- Verify connectivity between the VM and the host
- Ensure the forwarding port is reachable

### Windows 10 VM Setup
- Install Windows 10 on the virtual machine
- Apply basic system configuration
- Optional: reduce background noise (updates, telemetry)

### Splunk Universal Forwarder
- Install the Universal Forwarder on the Windows 10 VM
- Configure forwarding to the host Splunk instance
- Validate that logs are successfully received

### Sysmon
- Install Sysmon with a community-based configuration
- Enable detailed logging for:
  - Process creation
  - Network connections
  - Registry changes
- Forward Sysmon logs to Splunk

### Logging Configuration
- Windows Security, System, and Application logs
- PowerShell Operational logs
- Sysmon Operational logs

---
