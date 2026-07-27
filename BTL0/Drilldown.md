# BTL0 Investigation : DrillDown

## Scenario 
Your organization doesn't use Amazon Web Services, so when a Threat Hunter starts seeing connections to multiple EC2 instances, it's time to start hunting to understand what happened, so the information can be passed to the incident response team, and indicators can be gather for intelligence sharing.

## Tools Used
Splunk , Virustotal

## Investigation

 Q1 : WayneCorpInc doesn't use Amazon Web Service for cloud hosting, so when a threat hunting discovered outbound connections to EC2 instances they immediately began to drilldown into this activity so they can provide as much context for the Incident Response Team as possible. Using Sysmon logs, how many destination hostnames are found? (Format: # Destination EC2s)

Since this is an outbound connections I began by searching for Sysmon EventCodes=3 which list external connections

index=* EventCode=3 source="WinEventLog:Microsoft-Windows-Sysmon/Operational"
<img width="1918" height="631" alt="image" src="https://github.com/user-attachments/assets/5257c615-91a0-4256-96bd-cc8c8c8354a5" />
So it is confirmed that we have Network connection logs. To remove the noise, we filtered for EC2 instances specifically.
