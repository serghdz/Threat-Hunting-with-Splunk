## Investigation 01

This is my inital investigation with the BOTSv3 dataset, for the purpose of practicing threat hunting and reporting skills using **Splunk**. The focus was to analyze activity from the AWS Cloudtrail logs, by first gaining familiarity with exisiting users. 

**Identifying Users**:

```splunk
index=botsv3 sourcetype=aws:cloudtrail | stats by userIdentity.userName
```
<img width="268" height="366" alt="image" src="https://github.com/user-attachments/assets/6a6ef5ab-fbda-4467-80b1-811290190f23" />

