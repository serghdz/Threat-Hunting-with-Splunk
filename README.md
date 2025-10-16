# Threat Hunting with Splunk: Analyzing IOCs in the BOTSv3 Dataset

This documentation is a walkthrough on setting up a simple yet effective Splunk lab using Virtual Box, using the Splunk Boss of the SOC (BOTSv3) dataset, emphasizing on threat hunting, skill-building with Splunk, and the creation of detailed reports.  With over 2 million events from various log sources, the project aims to identify indicators of compromise, become proficient in Splunk’s search processing language, and develop practical reporting skills for effective cybersecurity analysis and response.
## Key Components
- **VirtualBox**: Virtualization platform hosting the lab environment
- **Ubuntu Server**: Virtual Machine running Splunk Enterprise (e.g., version 7.1.7+ recommended), 
- **BOTSv3 dataset link**: Available via Github, containing over 2 million+ events
- **Splunk App/Add-on** (refer to dataset link above)- Required Add-ons for log source compatability
## Splunk Virtual Machine Network Configuration
- **Bridged Adapter**: Allows the VM to connect directly to the same physical network as the host, enabling communication between the two. Assigns an IP from the hosts subnet by dhcp if available.
## Splunk Web interface
- **Host Web Browser**:  Splunk Web GUI is accessed from the host pc using the assigned IP given to the Splunk server.
  - http://<SplunkServerIP>:8000

<img width="1560" height="629" alt="botsv3lab" src="https://github.com/user-attachments/assets/51fe3166-fb22-4894-902f-5d3542a58137" />

## Scenario:
The Splunk Boss of the SOC (BOTS) Version 3 is a Capture The Flag (CTF) challenge designed for security professionals to practice threat hunting and incident response using Splunk. The dataset simulates a cyber attack on Frothly, a fictional beer manufacturing company, by a North Korean threat actor group known as Taedonggang (also referred to as a DPRK-linked group). The events unfold on August 20, 2018, primarily between 09:00 and 16:00 UTC, and involve a range of tactics including credential compromise, phishing, remote code execution (RCE), privilege escalation, cryptocurrency mining, data exfiltration, and website defacement. The dataset includes over 2 million events from sources like AWS CloudTrail, Windows Event Logs, Syslog, Microsoft Office 365, Suricata, and more, ingested into Splunk under the index=botsv3


## Investigations:
- [Investigation-01](https://github.com/serghdz/Threat-Hunting-with-Splunk/blob/main/investigation-01.md)
