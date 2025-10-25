# Splunk Cybersecurity Dashboard: Firewall Traffic Analysis  

## Project Overview  
**Objective:** I built my **first real-time cybersecurity dashboard** in Splunk to **practice SOC investigation workflows** using the pre-loaded `botsv3` dataset. By analyzing firewall traffic, I learned how to turn raw logs into **actionable security insights** — answering three real-world SOC questions with **SPL queries** and **clear visualizations**.

**Tools Used:**  
- Splunk Enterprise (pre-configured local instance)  
- `botsv3` index (`sourcetype=firewall` logs)  
- AbuseIPDB Threat Intelligence App  
- SPL commands: `stats`, `iplocation`, `sort`, `head`, custom lookups  

**Outcome:** A fully functional **private classic dashboard** with **three interactive panels** — built from scratch to reinforce hands-on SIEM skills.


## Investigation Purpose & Context  
As someone **aspiring to become a SOC analyst**, I wanted to **learn by doing** — not just watch tutorials. Firewall logs are the **front door of network security**, so I chose them to practice **threat detection logic**.

My goal was simple:  
> **“How would a real SOC analyst investigate suspicious traffic — and how can I build that into a dashboard?”**

I started with **zero assumptions** and asked:  
- *What looks suspicious in firewall logs?*  
- *How do I find it using SPL?*  
- *How do I present it so another analyst can act fast?*

This project was my **structured learning lab** — every query, every panel, every decision was a lesson in **investigation, querying, and dashboard design**.



## Step-by-Step Learning Process  

### 1. Log Discovery & Asking the Right Questions  
I opened the `botsv3` index and filtered to `sourcetype=firewall`. Each event showed fields like:  
- `src_ip` → Who’s initiating traffic?  
- `dest_ip` → Where are they going?  
- `_time` → When did it happen?  

Instead of guessing, I **brainstormed real SOC questions**:  
1. **Is any IP spamming destinations?** *(Scanning? Brute force?)*  
2. **Which countries are generating the most traffic?** *(Geo-risk context)*  
3. **Are high-volume IPs known to be malicious?** *(Threat intel check)*  

> *This taught me: Good investigations start with **curiosity**, not code.*



### 2. Building SPL Queries (3 Core Panels)  

#### Panel 1: Top 10 Source IPs by Connection Count  
```spl
index=botsv3 sourcetype=firewall 
| stats count by src_ip 
| sort - count 
| head 10
What I Learned:
stats count by field → group and count events
sort - count → descending order
head 10 → limit to top 10
Result: One IP had 25,571 connections → clear red flag
Visualization: Bar Chart – “Top 10 IPs by Hit Count”

Panel 2: Top 10 Countries by Source IP Volume
spl
index=botsv3 sourcetype=firewall 
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
index=botsv3 sourcetype=firewall 
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

