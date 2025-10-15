# Threat Hunting with Splunk: Analyzing IOCs in the BOTSv3 Dataset

This documentation is a walkthrough on setting up a simple yet effective Splunk lab using Virtual Box, using the Splunk Boss of the SOC (BOTSv3) dataset, emphasizing on threat hunting, skill-building with Splunk, and the creation of detailed reports.  With over 2 million events from various log sources, the project aims to identify indicators of compromise, become proficient in Splunk’s search processing language, and develop practical reporting skills for effective cybersecurity analysis and response.
## Key Components
- ### VirtualBox: Virtualization platform hosting the lab environment
- ### Ubuntu Server: Virtual Machine running Splunk Enterprise (e.g., version 7.1.7+ recommended), 
- ### BOTSv3 dataset link: Available via Github, containing over 2 million+ events
- ### Splunk App/Add-on (refer to dataset link above)- Required Add-ons for log source compatability
## Splunk Virtual Machine Network Configuration
- ### Bridged Adapter: Allows the VM to connect directly to the same physical network as the host, enabling communication between the two. Assigns an IP from the hosts subnet by dhcp if available.
## Splunk Web interface
- ### Host Web Browser:  Splunk Web GUI is accessed from the host pc using the assigned IP given to the Splunk server.
-- http://<Splunk Server IP>:8000
