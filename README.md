# Enterprise SIEM Deployment: Wazuh Manager & Windows Server Agent with FIM

## Project Overview
A hands-on security and systems administration lab demonstrating the deployment of a centralized **Wazuh SIEM Manager** (hosted on Kali Linux) and a **Wazuh Security Agent** on **Windows Server 2022**. This project showcases live telemetry streaming, XML service configuration, and **Real-Time File Integrity Monitoring (FIM)**.

## Lab Architecture
* **SIEM Manager:** Kali Linux VM (Wazuh Manager v4.x)
* **Target Endpoint:** Windows Server 2022 Standard Evaluation
* **Communication:** TCP Port 1514 (Encrypted Agent-to-Manager Handshake)
* **Monitoring Scope:** System logs, service health, and real-time folder integrity.

## Configuration & Implementation Highlights

### 1. Agent Installation & Endpoint Enrollment
* Deployed the Wazuh agent package via PowerShell using the automated installer property:
  ```powershell
  msiexec.exe /i wazuh-agent-4.14.9-1.msi /q WAZUH_MANAGER="192.168.132.130"
2. Real-Time File Integrity Monitoring (FIM)
Configured the <syscheck> engine inside ossec.conf to track real-time file creation, modification, and deletion events on a custom target directory:

XML
<syscheck>
  <disabled>no</disabled>
  <frequency>43200</frequency>
  <scan_on_start>yes</scan_on_start>
  <directories realtime="yes">C:\Users\Administrator\Desktop\WAZUH-Test</directories>
</syscheck>
Troubleshooting & Engineering Solutions
Placeholder IP Fix: Resolved initialization errors ((4112): Invalid server address found: '0.0.0.0') by enforcing strict static IP routing to the kali manager.

XML Encoding Resilience: Overcame configuration file corruption and parser errors (Error reading XML file 'ossec.conf': (line 0)) caused by text editor encoding conflicts. Solved by programmatically writing UTF-8 compliant configurations via PowerShell scripts.

Service Lifecycle Management: Stabilized the WazuhSvc Windows background service from an unstable crash loop to a healthy, continuous Running state.

Verification & Proof of Concept
Connection Handshake Log
Plaintext
2026/10/25 18:35:22 wazuh-agent: INFO: Using AES as encryption method.
2026/10/25 18:35:22 wazuh-agent: INFO: Trying to connect to server ([192.168.132.130]:1514/tcp).
2026/10/25 18:35:23 wazuh-agent: INFO: Connected to the server ([192.168.132.130]:1514/tcp).
2026/10/25 18:35:23 wazuh-agent: INFO: Started (pid: 3132).
Dashboard Status
Check the screenshots folder in this repository for visual verification of the active agent and live telemetry logs..
