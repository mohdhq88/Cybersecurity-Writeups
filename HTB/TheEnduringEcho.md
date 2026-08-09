# HTB SHERLOCK: Holmes 2025 3: The Enduring Echo
## Scenario
LetStrade passes a disk image to Holmes. It's one of the identified breach points, now showing abnormal CPU activity and anomalies in process logs.

## Tools Used
Zimmerman Tools 

Splunk

## Investigation

Q1/ What was the first (non cd) command executed by the attacker on the host?

So we'll be using cmd commands here. First, I converted the Security.evtx logs provided in the lab files to CSV using EvtxECmd from Eric Zimmerman's tools, then uploaded them to Splunk to make it easier to investigate the logs

<img width="1916" height="1028" alt="image" src="https://github.com/user-attachments/assets/bbcf25b2-98f7-403b-81e2-a44b8bc5dc61" />

After uploading it to Splunk, I filtered for Event ID 4688, which logs process creationas each time a command is run through cmd, it is treated as a process being created

<img width="1660" height="898" alt="image" src="https://github.com/user-attachments/assets/7bcd3976-5205-4098-a4db-4d833d270e4d" />

As we can right here there is a lot of noises so I tried to filter for the cmd.exe 

<img width="1698" height="846" alt="image" src="https://github.com/user-attachments/assets/209f483c-6258-4f0d-af20-819cb3011a15" />

I noticed the commands followed this pattern: /Q /c <command>
<img width="403" height="348" alt="image" src="https://github.com/user-attachments/assets/bd3a78b4-7f3d-4a62-8136-a0abd0faccf3" />

So I thought filtering for this pattern and went to find the earliest log and as I though it has the commands that we need which is systeminfo

<img width="809" height="91" alt="image" src="https://github.com/user-attachments/assets/d52e1317-14f4-4daa-be4e-cefaf448422d" />
<img width="1426" height="170" alt="image" src="https://github.com/user-attachments/assets/64459e20-41fb-4653-b299-31d5d7cb2275" />

**Answer:** `systeminfo`


Q2 / Which parent process (full path) spawned the attacker’s commands?
We can find the parent process in the same log that we got for the previous question 
<img width="1430" height="183" alt="image" src="https://github.com/user-attachments/assets/bf709ddf-9e4f-47ba-b1f5-6a56bef120b9" />

Q2 / C:\Windows\System32\wbem\WmiPrvSE.exe









