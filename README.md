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

After downloading the package, we are going to install it. The process is simple, just a three click step.
Check the license box, and click next

<img alt="Screenshot_3" src="https://github.com/user-attachments/assets/b74c62ee-26d8-4592-aa53-6d3f03a4e9ff" />

In the next step, we are going to setup the username and password that will be required to access the web GUI of Splunk:

<img alt="Screenshot_4" src="https://github.com/user-attachments/assets/234a583e-8eda-4a8a-b02d-990020dfee6b" />

Hit the Install Button

<p align="center">
  <img width="40%" alt="Screenshot_5" src="https://github.com/user-attachments/assets/cd8076a9-41a2-4e16-960a-781ed22c367b" />
  <img width="40%" alt="Screenshot_6" src="https://github.com/user-attachments/assets/879f6a6d-c608-45ec-9dee-bc5e85cffcc5" />
</p>

After The installation process Finishو Splunk GUI will open in the browser.
Here’s the SPLUNK login page and we will require the credentials that was setup during installation process:

<img width="1400" height="611" alt="Screenshot_7" src="https://github.com/user-attachments/assets/538fd714-a483-4219-9bf9-fe241cbe9653" />

At this point, Splunk Enterprise is installed but not yet configured to receive logs.

## Step 2: Access Splunk Web and Perform Initial Configuration
Splunk Enterprise is managed through its web interface, called Splunk Web.
1. Open your browser and navigate to: http://localhost:8000
2. Log in using the credentials you created during installation.

<img width="1365" height="623" alt="Screenshot_8" src="https://github.com/user-attachments/assets/ca0dd4a1-4db2-4e6b-843a-08ddad5854e5" />

### Configure Receiving Port
To allow log ingestion from forwarders:
1. Go to -> Settings -> Forwarding and Receiving

<img width="1363" height="615" alt="Screenshot_9" src="https://github.com/user-attachments/assets/c0d48916-a038-48ad-aa10-973ad4aeafe6" />

Set port 9997 as the listening port for incoming logs.

<img width="1365" height="621" alt="Screenshot_10" src="https://github.com/user-attachments/assets/3a17531a-d11d-456f-98df-04eebe1e213b" />
<img width="1365" height="629" alt="Screenshot_11" src="https://github.com/user-attachments/assets/975a5346-6c81-4032-9c51-f2597248ea51" />

Configure Windows Firewall

### Configure Windows Firewall
Make sure the port is accessible:
1. Open Windows Defender Firewall
Click on Inbound Rules -> New Rule -> Port and hit next

<img width="1213" height="728" alt="Screenshot_12" src="https://github.com/user-attachments/assets/37eb8791-8cf9-496f-a9bf-cffa9389f9fa" />
choose TCP and The Port we sit in splunk (9997)

<img width="750" height="610" alt="Screenshot_13" src="https://github.com/user-attachments/assets/f5045738-53bf-4bac-97cc-170c96c4a01f" />

Next -> "Allow The Connection" -> Next -> Next -> Pick a Name Like ("Splunk Port") And Hit Finish

<img width="1025" height="349" alt="Screenshot_14" src="https://github.com/user-attachments/assets/f31c9d6b-2021-4edf-a228-e61dfafa92c6" />

## Step 3: Install Splunk Universal Forwarder on Windows 10 VM










