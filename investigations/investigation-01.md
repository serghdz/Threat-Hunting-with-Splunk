## Investigation 01

This is my inital investigation with the BOTSv3 dataset, for the purpose of practicing threat hunting and reporting skills using **Splunk**. The focus was to analyze activity from the AWS Cloudtrail logs, by first gaining familiarity with exisiting users. 

AWS Cloudtrail logs every API call made made to AWS services. The following displays which accounts are most active, and has a potential abuse of high privilege or service accounts, as well as anomalous activity. 

**Identifying Users**:

```splunk
index=botsv3 sourcetype=aws:cloudtrail | stats by userIdentity.userName
```
<img width="268" height="366" alt="image" src="https://github.com/user-attachments/assets/6a6ef5ab-fbda-4467-80b1-811290190f23" />



Web_admin investigation
query:
```splunk
index=botsv3 sourcetype=aws:cloudtrail userIdentity.userName=web_admin
| stats count by sourceIPAddress
| sort -count
```

<img width="950" height="159" alt="image" src="https://github.com/user-attachments/assets/4a7aa323-542d-4ed2-b712-d7c34cf2e27a" />


Investigate Uknown IP’s

index=botsv3 sourceIPAddress=209.107.196.112 | table _time, eventName, userIdentity.userName, userAgent

Unknown ip’s 
82.102.18.111
35.153.154.221
209.107.196.112
Investigate Uknown IP’s
index=botsv3 sourceIPAddress=209.107.196.112 | table _time, eventName, userIdentity.userName, userAgent


Output:

2018-08-20 09:16:18
ListAccessKeys
web_admin
Boto3/1.6.3 Python/2.7.14 Windows/2012ServerR2 Botocore/1.9.3



Event Analysis: ListAccessKeys Activity

Timestamp (_time): August 20, 2018, at 9:16:18 AM (likely UTC, per the dataset).

Timelines like this are key in investigations—they help spot patterns, such as bursts of activity that could signal an attack or unusual behavior.

Event Name: ListAccessKeys

This AWS API call lists access keys for a user. Access keys are essentially digital credentials (an ID and secret key) that enable programmatic access to AWS resources. 

It's a standard management task, but if it's unexpected, it might indicate reconnaissance—someone probing for valid keys to exploit.

User Identity: web_admin (IAM username)

This is the account that triggered the action. In our investigation, web_admin stands out with 646 events total. If it's meant strictly for web management, key-listing activity could be a red flag worth digging into.

User Agent (Software Fingerprint): Boto3/1.6.3 Python/2.7.14 Windows/2012ServerR2 Botocore/1.9.3

This reveals the tools behind the request: Boto3/1.6.3: AWS's open-source Python SDK for scripting API interactions. This version aligns with 2018 timelines.
Python/2.7.14: The runtime— an older Python version (end-of-life in 2020, but widely used back then).
Windows/2012ServerR2: The OS, a server edition common in enterprise or cloud environments.
Botocore/1.9.3: The core library powering Boto3's AWS calls.
In conclusion, at 9:16:18 AM on August 20, 2018, a Python script on a Windows server used the web_admin account to list its access keys, coming from IP 209.107.196.112. Since you checked VirusTotal and AbuseIPDB and found this IP malicious, this log is a strong clue that someone outside Frothly might be using (or testing) the account's credentials
Validating if action succeeded or failed:
index=botsv3 sourceIPAddress=209.107.196.112 | table _time, eventName, userAgent, errorCode

Investigating any other actions this userAgent performed:

index=botsv3 userAgent="Boto3/1.6.3 Python/2.7.14 Windows/2012ServerR2 Botocore/1.9.3" | table _time, sourceIPAddress, eventName




