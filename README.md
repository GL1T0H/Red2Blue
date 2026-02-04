# Introduction
This documentation provides detailed guidance step by step through building your SOC lab from scratch — from setting up the infra, to designing and running attack scenarios, and finally learning how to detect and analyze them using Splunk and more.

## Table of Contents

- [Introduction](#introduction)
  - [Table of Contents](#table-of-contents)
- [Project Overview](#project-overview)
  - [What is Red2Blue?](#what-is-red2blue)
  - [What You Will Learn](#what-you-will-learn)
  - [Lab Architecture](#lab-architecture)
  - [Lab Requirements](#lab-requirements)
- [Infrastructure](#infrastructure)
  - [Infrastructure Diagram (Architecture Schema)](#infrastructure-diagram-architecture-schema)
  - [SIEM Server (Windows 10 Host)](#siem-server-windows-10-host)
    - [Splunk Enterprise Setup & Installation](#splunk-enterprise-setup--installation)
    - [Initial Splunk Configuration](#initial-splunk-configuration)
    - [Add-ons & Apps Configuration](#add-ons--apps-configuration)
  - [Endpoint (Windows 10 VM)](#endpoint-windows-10-vm)
    - [Setting Up Windows 10 on VMware](#setting-up-windows-10-on-vmware)
    - [Sysmon Installation & Configuration](#sysmon-installation--configuration)
    - [Splunk Universal Forwarder Setup & Installation](#splunk-universal-forwarder-setup--installation)
    - [Splunk Universal Forwarder Configuration](#splunk-universal-forwarder-configuration)
  - [Testing & Log Verification](#testing--log-verification)
- [Attack Scenarios](#attack-scenarios)
  - [Attack Scenario: Phishing via Malicious Word Attachment](#attack-scenario-phishing-via-malicious-word-attachment)
    - [Overview](#overview)
      - [What is Phishing?](#what-is-phishing)
      - [Why Is Phishing So Effective?](#why-is-phishing-so-effective)
    - [Attack Flow](#attack-flow)
      - [MITRE ATT&CK Mapping](#mitre-attck-mapping)
      - [Create the Malicious Word Document](#create-the-malicious-word-document)
      - [Configure Macro and PowerShell Payload](#configure-macro-and-powershell-payload)
      - [Set Up the C2 Server on Kali Linux](#set-up-the-c2-server-on-kali-linux)
      - [Malware Behavior](#malware-behavior)
        - [Host Information Collection](#host-information-collection)
        - [Registry Run Key Persistence](#registry-run-key-persistence)
        - [Beaconing to C2 Server](#beaconing-to-c2-server)
- [Detection & Analysis](#detection--analysis)
  - [Context Concepts](#context-concepts)
  - [Alerts and Detection](#alerts-and-detection)
  - [Use Case 1: Phishing via Malicious Word Attachment](#use-case-1-phishing-via-malicious-word-attachment)
    - [Scenario](#scenario)


# Project Overview

## What is Red2Blue?
**Red2Blue** is a hands-on home SOC lab designed to demonstrate how common attack techniques generate logs and telemetry, and how defenders can detect, investigate, and respond to these attacks using **Splunk**.

This project bridges the gap between **Red Team attack simulation** and **Blue Team detection engineering**, focusing on attacker behavior rather than tools.

## What You Will Learn
By working through this lab, you will learn how to:
- Build a complete SOC lab at home using Splunk
- Simulate realistic attack scenarios in a controlled environment
- Analyze attacker-generated telemetry
- Write effective SPL-based detections
- Think like a SOC analyst and detection engineer

## Lab Architecture
Before we dive into the technical steps, let’s quickly understand the architecture:
- Windows 10 Host (Soc analyst)
  - Runs Splunk Enterprise
  - Acts as the SIEM server
- Windows 10 Virtual Machine (Endpoint)
  - Runs Splunk Universal Forwarder
  - Generates logs (Windows Events + Sysmon)
- Communication
  - Logs are sent from the VM to the host via TCP Port (e.g., 9997)

<img width="1536" height="1024" alt="ChatGPT Image Feb 4, 2026, 12_01_00 AM" src="https://github.com/user-attachments/assets/440b54e4-a3ba-460f-8b35-79f5a5532cdb" />

## Lab Requirements
### Hardware
- Minimum 16 GB RAM (8 GB possible with limitations)
- 100+ GB available disk space
- CPU with virtualization support enabled

## Host Operating System
- Windows 10 or 11

## Virtualization
- VMware Workstation (recommended)
- VirtualBox (alternative)

## Software
- Splunk Enterprise
- Splunk Universal Forwarder
- Windows 10 ISO
- Sysmon
- Some plugs in splunk Enterprise

---

# Infrastructure

## Infrastructure Diagram (Architecture Schema)

#### The infrastructure diagram illustrates a simple SOC lab architecture where a Windows 10 endpoint generates security telemetry using Sysmon and Windows Event Logs. This telemetry is collected and forwarded by the Splunk Universal Forwarder to a centralized Splunk Enterprise SIEM server over TCP port 9997. The SIEM server indexes, stores, and analyzes the incoming logs, enabling detection and alerting use cases.

<img width="1536" height="1024" alt="ChatGPT Image Feb 4, 2026, 12_01_00 AM" src="https://github.com/user-attachments/assets/440b54e4-a3ba-460f-8b35-79f5a5532cdb" />

## SIEM Server (Windows 10 Host)

### Splunk Enterprise Setup & Installation
First, in our **Windows 10 (HOST)** download the latest version of Splunk Enterprise
([https://www.splunk.com/en_us/download.html](https://www.splunk.com/en_us/download.html))

<p align="left">
  <img width="40%" alt="Screenshot_1" src="https://github.com/user-attachments/assets/946943b0-35c6-453e-a38a-c1c6068afdc9" />
  <img width="40%" alt="Screenshot_2" src="https://github.com/user-attachments/assets/24ef079d-6e64-4543-9550-fb4a201d4321" />
</p>

After downloading the .MSI, we are going to setup it. The process is simple, just a three click step.
1. Accept license agreement., and click next

<img alt="Screenshot_3" src="https://github.com/user-attachments/assets/b74c62ee-26d8-4592-aa53-6d3f03a4e9ff" />

2. In the next step, we are going to setup the username and password that will be required to access the web GUI of Splunk

<img alt="Screenshot_4" src="https://github.com/user-attachments/assets/234a583e-8eda-4a8a-b02d-990020dfee6b" />

3. Hit the Install Button, And Finish

<p align="left">
  <img width="40%" alt="Screenshot_5" src="https://github.com/user-attachments/assets/cd8076a9-41a2-4e16-960a-781ed22c367b" />
  <img width="40%" alt="Screenshot_6" src="https://github.com/user-attachments/assets/879f6a6d-c608-45ec-9dee-bc5e85cffcc5" />
</p>

> [!NOTE]
> During the Splunk Enterprise Security (ES) setup, it is recommended to specify the log sources from which telemetry will be collected.  
However, in this lab, this step was intentionally **postponed** to a later stage.

After The installation process Finish, Splunk GUI will open in the browser.
Here’s the SPLUNK login page and we will require the credentials that was setup during installation process:

<img width="1400" height="611" alt="Screenshot_7" src="https://github.com/user-attachments/assets/538fd714-a483-4219-9bf9-fe241cbe9653" />

At this point, Splunk Enterprise is installed but not yet configured to receive logs.

### Initial Splunk Configuration
Splunk Enterprise is managed through its web interface, called Splunk Web.
1. Open your browser and navigate to: http://localhost:8000
2. Log in using the credentials you created during installation.

<img width="1365" height="623" alt="Screenshot_8" src="https://github.com/user-attachments/assets/ca0dd4a1-4db2-4e6b-843a-08ddad5854e5" />

#### Configure Receiving Port
To allow log ingestion from forwarders:
1. Go to -> Settings -> Forwarding and Receiving

<img width="1363" height="615" alt="Screenshot_9" src="https://github.com/user-attachments/assets/c0d48916-a038-48ad-aa10-973ad4aeafe6" />

Set port 9997 as the listening port for incoming logs.

<img width="1365" height="621" alt="Screenshot_10" src="https://github.com/user-attachments/assets/3a17531a-d11d-456f-98df-04eebe1e213b" />
<img width="1365" height="629" alt="Screenshot_11" src="https://github.com/user-attachments/assets/975a5346-6c81-4032-9c51-f2597248ea51" />


#### Configure Indexes
In Splunk, indexes are storage locations where incoming data is organized and kept for searching and analysis. They help Splunk quickly retrieve and manage large volumes of log or event data efficiently.W
We will Configure 4 indexes
- windows_system
- windows_security
- windows_application
- sysmon
I will explain how to do it once and you can do the same for the 4 indexes
1. Go to -> Settings -> Indexes

<img width="1361" height="616" alt="Screenshot_15" src="https://github.com/user-attachments/assets/56b0981b-5e4e-45f3-9a17-cd8215a8d3e5" />

2. From The Indexes window click **New Index**

<img width="1363" height="620" alt="Screenshot_16" src="https://github.com/user-attachments/assets/b3c5c9ea-9784-4455-a6fc-4bfe8ae1fc74" />

3. From The new Window type the index name (i.s sysmon) and keep everything in default and hit **Save**

<img width="1052" height="597" alt="Screenshot_17" src="https://github.com/user-attachments/assets/6db9fae2-c3a6-462a-a424-8e8e0c0ba8f9" />

Do the same with the 3 left indexes
> [!NOTE]
> Tall now the indexes will not work until we configure it on splunk UF so do it and keep going with me

#### Configure Windows Firewall
Make sure the port is accessible:
1. Open Windows Defender Firewall
Click on Inbound Rules -> New Rule -> Port and hit next

<img width="1213" height="728" alt="Screenshot_12" src="https://github.com/user-attachments/assets/37eb8791-8cf9-496f-a9bf-cffa9389f9fa" />
choose TCP and The Port we sit in splunk (9997)

<img width="750" height="610" alt="Screenshot_13" src="https://github.com/user-attachments/assets/f5045738-53bf-4bac-97cc-170c96c4a01f" />

Next -> "Allow The Connection" -> Next -> Next -> Pick a Name Like ("Splunk Port") And Hit Finish

<img width="1025" height="349" alt="Screenshot_14" src="https://github.com/user-attachments/assets/f31c9d6b-2021-4edf-a228-e61dfafa92c6" />

### Add-ons & Apps Configuration






## Endpoint (Windows 10 VM)

### Setting Up Windows 10 on VMware

### Sysmon Installation & Configuration

### Splunk Universal Forwarder Setup & Installation

### Splunk Universal Forwarder Configuration




## Testing & Log Verification











