# BTL0 Investigation : DrillDown

## Scenario 
Your organization doesn't use Amazon Web Services, so when a Threat Hunter starts seeing connections to multiple EC2 instances, it's time to start hunting to understand what happened, so the information can be passed to the incident response team, and indicators can be gather for intelligence sharing.

## Tools Used
Splunk , Virustotal

## Investigation

### Question 1) WayneCorpInc doesn't use Amazon Web Service for cloud hosting, so when a threat hunting discovered outbound connections to EC2 instances they immediately began to drilldown into this activity so they can provide as much context for the Incident Response Team as possible. Using Sysmon logs, how many destination hostnames are found? (Format: # Destination EC2s)
