# 🛡️ Purple Team Lab Scenario 3: Sysmon Telemetry Detection Engineering

📌 Project Overview

This project focuses on enhancing endpoint visibility by deploying Sysmon (System Monitor) on a Windows Server and integrating it with Wazuh SIEM. The objective was to improve detection capabilities by capturing detailed process, network, and registry telemetry during simulated attack activity.
Unlike standard Windows event logging, Sysmon provides rich telemetry that enables defenders to detect attacker behavior with greater precision and context. This scenario demonstrates how enhanced endpoint monitoring strengthens detection engineering and forensic investigations.

🎯 Objectives

⚙️ Install and configure Sysmon on Windows Server
📡 Forward Sysmon logs to Wazuh SIEM
🔍 Monitor process creation and command-line execution
🧠 Detect PowerShell activity and system enumeration
📌 Track registry changes and persistence attempts
📊 Improve forensic visibility of attacker behavior

🖥️ Lab Environment

Component
Technology
Attacker Machine
Kali Linux
Target Machine
Windows Server
Endpoint Monitoring
Sysmon
SIEM
Wazuh
Log Collection
Wazuh Agent
Detection Framework
MITRE ATT&CK


⚙️ Sysmon Deployment

Installing Sysmon
Sysmon was installed using Microsoft Sysinternals along with a community configuration file to capture detailed endpoint telemetry.
Sysmon64.exe -accepteula -i sysmonconfig.xml
📸 Screenshot 1 – Sysmon Installation
Insert Screenshot:
<img width="1361" height="602" alt="Screenshot 2026-06-29 075806" src="https://github.com/user-attachments/assets/1f664584-2a41-44f8-ba98-d2a768e0264e" />

Caption: Figure 1. Successful Sysmon installation using Sysinternals.

Wazuh Integration
The Wazuh agent was configured to monitor the Sysmon Operational event channel.
<localfile>
 <location>Microsoft-Windows-Sysmon/Operational</location>
 <log_format>eventchannel</log_format>
</localfile>
📸 Screenshot 2 – Wazuh Agent Configuration
Insert Screenshot:
<img width="979" height="128" alt="Screenshot 2026-06-29 081133" src="https://github.com/user-attachments/assets/faa3e8dc-cd85-4305-8840-e692756b297a" />

Caption: Figure 2. Wazuh agent configured to collect Sysmon Operational logs.

⚔️ Detection Scenario 1 – Process Execution Monitoring
The following commands simulated attacker reconnaissance activity:
whoami
ipconfig
Get-Process
Sysmon generated Event ID 1, capturing:
Executable path
Parent process
Command-line arguments
Process GUID
User context
📸 Screenshot 3 – Sysmon Event ID 1
Insert Screenshot:
<img width="1902" height="977" alt="Screenshot 2026-06-29 075849" src="https://github.com/user-attachments/assets/121a8f59-f903-4407-8458-f27d935c658c" />

or


Caption: Figure 3. Sysmon Event ID 1 capturing process creation with full command-line visibility.

⚡ Detection Scenario 2 – PowerShell Activity

PowerShell commands were executed to simulate attacker activity.
Sysmon captured:
PowerShell executable
Command-line parameters
Parent-child process relationship
User account
📸 Screenshot 4 – PowerShell Detection
Insert Screenshot:
<img width="1355" height="625" alt="Screenshot 2026-06-29 084702" src="https://github.com/user-attachments/assets/37549d80-90d1-472f-a36c-e6dc1a2b8b68" />

and/or



<img width="590" height="761" alt="Screenshot 2026-06-29 084535" src="https://github.com/user-attachments/assets/487c3195-e097-4266-9eb9-6cccfa3abb70" />



<img width="541" height="750" alt="Screenshot 2026-06-29 084229" src="https://github.com/user-attachments/assets/9d8b0abf-bfbe-4e02-b045-fa9c12ee131a" />

Caption: Figure 4. PowerShell activity detected and logged by Sysmon.

📌 Detection Scenario 3 – Registry Persistence

Persistence was simulated by creating a Run registry key.
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\Run ^
/v TestApp ^
/t REG_SZ ^
/d notepad.exe
Sysmon recorded:
Registry path
Registry value
Process responsible
User context
using Event ID 13.
📸 Screenshot 5 – Registry Modification
Insert Screenshot:
<img width="1243" height="250" alt="Screenshot 2026-06-29 085321" src="https://github.com/user-attachments/assets/f08d9857-5bd0-45ba-a7d3-ee72dafe6b6d" />

or

<img width="801" height="757" alt="Screenshot 2026-06-29 090305" src="https://github.com/user-attachments/assets/bead3153-22e9-442a-8f59-9df2ba87b085" />

Caption: Figure 5. Registry modification detected, indicating a potential persistence mechanism.

🌐 Detection Scenario 4 – Network Connections

Sysmon monitored outbound network connections associated with running processes.
Information captured included:
Source IP
Destination IP
Destination Port
Process Image
Process ID
using Event ID 3.
📸 Screenshot 6 – Network Connection Event
Insert Screenshot:
<img width="1354" height="403" alt="Screenshot 2026-06-29 090721" src="https://github.com/user-attachments/assets/bb0d9f19-3eae-46e2-87c1-f6b1beffcd5a" />







<img width="715" height="756" alt="Screenshot 2026-06-29 090924" src="https://github.com/user-attachments/assets/b7cb70fa-7307-41ab-a25d-129bafd1000d" />

or


<img width="1538" height="931" alt="Screenshot 2026-06-29 090826" src="https://github.com/user-attachments/assets/1a3ffe45-da18-44eb-a7cb-9a920d87aae6" />

Caption: Figure 6. Network connection telemetry correlated with process execution.

📊 Detection Summary

Activity
Sysmon Event ID
Visibility
Process Creation
1
Full command line
Network Connections
3
Source process mapping
File Creation
11
File path tracking
Registry Changes
13
Persistence detection


🎯 MITRE ATT&CK Mapping

Technique
ID
Command and Scripting Interpreter
T1059
Process Discovery
T1057
System Information Discovery
T1082
Registry Run Keys / Startup Folder
T1547.001


🛡️ Key Improvements

Compared to basic Windows event logging, deploying Sysmon significantly enhanced endpoint visibility by providing:
✔ Full process command-line logging
✔ Parent-child process relationships
✔ PowerShell execution visibility
✔ Registry persistence monitoring
✔ Network process correlation
✔ Rich forensic telemetry

📚 Skills Demonstrated
🧠 Detection Engineering
⚙️ Sysmon Deployment & Configuration
📊 Wazuh SIEM Integration
🔍 Endpoint Telemetry Analysis
🪟 Windows Event Logging
🐉 Attack Simulation
🎯 MITRE ATT&CK Mapping

🚀 Outcome

This lab demonstrated how Sysmon dramatically improves endpoint visibility compared to default Windows logging. By integrating Sysmon with Wazuh SIEM, process execution, PowerShell activity, registry persistence, and network connections were captured in near real time, providing analysts with richer telemetry for threat hunting, incident response, and forensic investigations.
The experience reinforced practical skills in detection engineering, log analysis, and MITRE ATT&CK mapping while illustrating the value of comprehensive endpoint monitoring in a modern SOC environment.

📸 Recommended Screenshot Checklist

✅ Sysmon installation in PowerShell
✅ ossec.conf showing Sysmon configuration
✅ Event Viewer – Sysmon Operational log
✅ Wazuh dashboard with Sysmon alerts
✅ Process Creation (Event ID 1)
✅ PowerShell execution alert
✅ Registry modification (Event ID 13)
✅ Network Connection (Event ID 3)
✅ MITRE ATT&CK mapping within Wazuh (if available)
✅ Wazuh security events dashboard (optional)


👩‍💻 Author

Michael Stromer

Aspiring SOC Analyst | Blue Team | Detection Engineering | Threat Hunting
