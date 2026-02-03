# Red2Blue | Attack Simulation & Detection Lab (Splunk)

## 📌 Project Overview

### 1. What is Red2Blue?
**Red2Blue** is a hands-on home SOC lab designed to demonstrate how common attack techniques generate logs and telemetry, and how defenders can detect, investigate, and respond to these attacks using **Splunk**.

This project bridges the gap between **Red Team attack simulation** and **Blue Team detection engineering**, focusing on attacker behavior rather than tools.

### 2. Who is it for?
This lab is intended for:
- SOC Analysts (Junior / Entry-level)
- Blue Team & Detection Engineering learners
- Anyone seeking hands-on SOC experience without enterprise infrastructure

### 3. What you will learn
By working through this lab, you will learn how to:
- Build a complete SOC lab at home using Splunk
- Simulate realistic attack scenarios in a controlled environment
- Analyze attacker-generated telemetry
- Write effective SPL-based detections
- Think like a SOC analyst and detection engineer

### 4. Lab Architecture

Before we dive into the technical steps, let’s quickly understand the architecture:
- Windows 10 Host (Soc analyst)
  - Runs Splunk Enterprise
  - Acts as the SIEM server
- Windows 10 Virtual Machine (Victim - log sender)
  - Runs Splunk Universal Forwarder
  - Generates logs (Windows Events + Sysmon)
- Communication
  - Logs are sent from the VM to the host via TCP Port (e.g., 9997)

<img width="1536" height="1024" alt="ChatGPT Image Feb 4, 2026, 12_01_00 AM" src="https://github.com/user-attachments/assets/440b54e4-a3ba-460f-8b35-79f5a5532cdb" />

---

### 5. Lab Requirements

### Hardware
- Minimum 16 GB RAM (8 GB possible with limitations)
- 100+ GB available disk space
- CPU with virtualization support enabled

### Host Operating System
- Windows 10 or 11

### Virtualization
- VMware Workstation (recommended)
- VirtualBox (alternative)

### Software
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows 10 ISO
- Sysmon
- Some plugs in splunk Enterprise

----

# Setup
Now lets talk about how to build our project
## Step 1: Install Splunk Enterprise on Windows 10 Host
First, in our Windows 10 (HOST) download the latest version of Splunk Enterprise
([https://www.splunk.com/en_us/download.html](https://www.splunk.com/en_us/download.html))
<p align="center">
  <img width="40%" alt="Screenshot_1" src="https://github.com/user-attachments/assets/946943b0-35c6-453e-a38a-c1c6068afdc9" />
  <img width="40%" alt="Screenshot_2" src="https://github.com/user-attachments/assets/24ef079d-6e64-4543-9550-fb4a201d4321" />
</p>









