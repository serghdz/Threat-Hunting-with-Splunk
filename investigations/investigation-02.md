# Splunk Cybersecurity Dashboard: Firewall Traffic Analysis  

## Project Overview  
**Objective:** I built my **first real-time cybersecurity dashboard** in Splunk to **practice SOC investigation workflows** using the pre-loaded `botsv3` dataset. By analyzing firewall traffic, I learned how to turn raw logs into **actionable security insights** — answering three real-world SOC questions with **SPL queries** and **clear visualizations**.

**Tools Used:**  
- Splunk Enterprise (pre-configured local instance)  
- `botsv3` index (`sourcetype=firewall` logs)  
- AbuseIPDB Threat Intelligence App  
- SPL commands: `stats`, `iplocation`, `sort`, `head`, custom lookups  

**Outcome:** A fully functional **private classic dashboard** with **three interactive panels** — built from scratch to reinforce hands-on SIEM skills.


## Investigation 

> **“How would a real SOC analyst investigate suspicious traffic — and how can I build that into a dashboard?”**

With this mindset I asked the following questions: 
- *What looks suspicious in firewall logs?*  
- *How do I find it using SPL?*  
- *How do I present it so another analyst can act fast?*


## Step-by-Step Learning Process  

### 1. Log Discovery & Asking the Right Questions  
I opened the `botsv3` index and filtered to `sourcetype=stream:ip`. Each event showed fields like:  
- `src_ip` → Who’s initiating traffic?  
- `dest_ip` → Where are they going?  
- `_time` → When did it happen?  

<img width="876" height="798" alt="image" src="https://github.com/user-attachments/assets/9bd68871-b589-458f-8992-d01950aae02f" />

Follow-up questions:  
1. **Is any IP spamming destinations?** *(Scanning? Brute force?)*  
2. **Which countries are generating the most traffic?** *(Geo-risk context)*  
3. **Are high-volume IPs known to be malicious?** *(Threat intel check)*  


### 2. Building SPL Queries  

#### Top 10 Source IPs by Connection Count  
```spl
index=botsv3 source=stream:ip
| table _time src_ip dest_ip
| stats count by src_ip 
| sort - count 
| head 10
```
<img width="938" height="607" alt="image" src="https://github.com/user-attachments/assets/93c5bae3-2eb4-4158-91ab-d3757577601a" />

stats count by field → group and count events
sort - count → descending order
head 10 → limit to top 10
Result: One IP had 25,571 connections → clear red flag

Visualization: Bar Chart – “Top 10 IPs by Hit Count”
<img width="934" height="549" alt="image" src="https://github.com/user-attachments/assets/43953df2-7e0c-4366-8a0e-d7e79e735e99" />


Panel 2: Top 10 Countries by Source IP Volume
spl
index=botsv3 source=stream:ip
| iplocation src_ip 
| stats count by Country 
| sort - count 
| head 10
What I Learned:
iplocation → geolocates IPs automatically
Field names are case-sensitive (Country, not country)
Result: United States led with 516 unique IPs
Visualization: Bar Chart – “Top 10 Countries by Source IPs”

Panel 3: U.S. Top 10 IPs + Maliciousness Check
spl
index=botsv3 source=stream:ip
| iplocation src_ip 
| search Country="United States" 
| stats count by src_ip 
| sort - count 
| head 10 
| `abuseipdb_check_ip(src_ip)`
What I Learned:
How to chain commands in a pipeline
How to integrate threat intelligence via Splunk apps
Custom commands like `abuseipdb_check_ip()` run API lookups
Result: All 10 IPs had 0% abuse score → no known threats
Visualization: Table – “Top 10 U.S. IPs + Abuse Score”

3. Dashboard Construction
Dashboard Creation
Went to Dashboards > Create New
Named it: My First Firewall Dashboard
Chose Classic Dashboard (easier to control)
Set to Private (for learning)

