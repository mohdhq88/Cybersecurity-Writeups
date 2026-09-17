<img width="468" height="94" alt="image" src="https://github.com/user-attachments/assets/5d09e07c-c515-44f9-a3b3-cdc794f6ebfe" />

# HTB SHERLOCK: Holmes 2025 2: The Watchman's Residue
## Scenario
With help from D.I. Lestrade, Holmes acquires logs from a compromised MSP connected to the city’s financial core. The MSP’s AI helpdesk bot looks to have been manipulated into leaking remote access keys - an old trick of Moriarty’s.

cWireshark , Zimmerman Tools

## Evidence
<img width="691" height="115" alt="image" src="https://github.com/user-attachments/assets/1d665327-e1e4-4ac6-b034-39c9629f3942" />

## Investigation

Q1 /What was the IP address of the decommissioned machine used by the attacker to start a chat session with MSP-HELPDESK-AI?

I started by opening the packet capture in Wireshark and filtering on `http` to see which IPs were communicating with the helpdesk website.

<img width="1904" height="633" alt="image" src="https://github.com/user-attachments/assets/85381be9-638a-4fae-91b2-e8016cb7de31" />

From this, I identified `172.18.0.2` as the AI helpdesk bot's backend — it was serving the actual web app content. I then scrolled through the capture to look for any other IPs communicating with the bot across different sessions.

<img width="1136" height="106" alt="image" src="https://github.com/user-attachments/assets/a9791fda-adb7-4fa4-ac8b-919b92288b0a" />


We can see anomalous packets from a new IP that appears to be entering the website for the first time, which is suspicious, so I tried it and it turned out to be our answer."

**Answer:** `10.0.69.45`

---

Q2 / What was the hostname of the decommissioned machine?

To find the hostname, I used NBNS. I first tried DNS, but there were no records for 10.0.69.45 — I think this is because it's a decommissioned machine, so it likely wouldn't have an active DNS entry anymore.

<img width="1919" height="174" alt="image" src="https://github.com/user-attachments/assets/1e374647-f5ab-4ce1-906b-2dee01ebcde7" />

As we can see here we found it 

**Answer:** `WATSON-ALPHA-2`

---

Q3 /  What was the first message the attacker sent to the AI chatbot?

I went back and filtered for HTTP packets to check the messages being sent to the chatbot.
<img width="1919" height="746" alt="image" src="https://github.com/user-attachments/assets/16c5a2d4-5166-4a02-9bcf-3ba26e4f8bad" />

I then right-clicked the first POST request from the attacker's machine  and followed the HTTP stream to read the actual message content.
<img width="1248" height="1001" alt="image" src="https://github.com/user-attachments/assets/206c55d8-9a31-4056-8a34-a1411b8fc8c9" />

I found it and it was "Hello Old Friend"

**Answer:** `Hello Old Friend`

---

Q4 / When did the attacker's prompt injection attack make MSP-HELPDESK-AI leak remote management tool info?

This one confused me a bit , I figured it out by searching through the POST packets where each packets shows a part of the conversation until I found the one we need

<img width="1919" height="919" alt="image" src="https://github.com/user-attachments/assets/7dffb7e8-645c-48fe-91e2-7253451df0f7" />

**Answer:** `2025-08-19 12:02:06`

---

Q5 / What is the Remote management tool Device ID and password?

This one is already revealed in the previous question 

![[Pasted image 20260829082308.png]]

**Answer:** `565963039:CogWork_Central_97&65`

---

Q6 / What was the last message the attacker sent to MSP-HELPDESK-AI?

I did the same process as Q3 — checking the last POST request and following the stream to check the content.
<img width="449" height="205" alt="image" src="https://github.com/user-attachments/assets/7225a1b9-3391-4295-a97b-235b962b63d8" />

**Answer:** `JM WILL BE BACK`

---

Q7 / When did the attacker remotely access Cogwork Central Workstation?

In our disk image in progame files there is file called connection_incoming.txt so I checked it and it lead us to our answer
<img width="1919" height="386" alt="image" src="https://github.com/user-attachments/assets/c7a39868-5276-400c-880e-86f1ad2f30e3" />

**Answer:** `2025-08-20 09:58:25`

---

Q8 / What was the RMM Account name used by the attacker?

<img width="710" height="124" alt="image" src="https://github.com/user-attachments/assets/2cb215bb-f5a0-4c04-9e6f-6d7691d69bee" />

**Answer:** `James Moriarty`

---

Q9 / What was the machine's internal IP address from which the attacker connected?

Alongside `Connections_incoming.txt`, I found another file called `TeamViewer15_Logfile.log`. I searched through it using the same timeframe from Q7 and found an IP address in the logs.

> **Note:** The timestamps in the TeamViewer log are UTC+1, so the 
> times appear one hour ahead compared to the other evidence sources.


<img width="1919" height="1015" alt="image" src="https://github.com/user-attachments/assets/a4e80149-aaa2-4cc4-b708-3dc53a0428c6" />

**Answer:** `192.168.69.213`

---

Q10 / The attacker brought some tools to the compromised workstation to achieve its objectives. Under which path were these tools staged?

Moving down after we checked the connection from `192.168.69.213` we can see being staged through a temp folder

<img width="1919" height="422" alt="image" src="https://github.com/user-attachments/assets/cf034735-2c98-4dd8-94bc-dae66bc7e321" />

**Answer:** `C:\Windows\temp\safe\`

---

Q11 / The attacker staged a browser credential harvesting tool on the compromised system. How long did this tool run before it was terminated? (Provide your answer in milliseconds, rounded to the nearest thousand)

Lol this one really confused me 

I first checked the Prefetch folder, but discovered there were no Prefetch files available. After some research, I found that the `NTUSER.DAT` registry hive contains an artifact called **UserAssist**, which records execution information for programs launched through the Windows GUI

I loaded `NTUSER.DAT` using registory explorer

<img width="1560" height="358" alt="image" src="https://github.com/user-attachments/assets/8596ae17-6079-458f-952c-7c4ba9a5b715" />

I went to Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist

<img width="1878" height="906" alt="image" src="https://github.com/user-attachments/assets/e3c1fd6b-b26d-458f-a7df-e7dc0579ded5" />

**Answer:** `8000`

---

َQ12 / The attacker executed a OS Credential dumping tool on the system. When was the tool executed?

One mistake I made was assuming that the Prefetch folder was the only way to find evidence of program execution. After some research, I discovered that the `$J` (USN Journal) can also help — even if the actual Prefetch file no longer exists on disk, the journal still records that the Prefetch file was created at some point, since file creation is a filesystem event that gets logged regardless.

<img width="1457" height="590" alt="image" src="https://github.com/user-attachments/assets/26ccec84-b575-428e-b704-f9dfd8857b68" />

Then I loaded it into timeline explorer and filter for .pf files

<img width="1919" height="1013" alt="image" src="https://github.com/user-attachments/assets/5799c8ba-2d90-47e6-8769-8474a6dfed2e" />

as we can see mimikatz were run at 2025-08-20 10:07:08

**Answer:** `2025-08-20 10:07:08`

---

Q13 / The attacker exfiltrated multiple sensitive files. When did the exfiltration start? (UTC)

For this one I went back to the logs and checked there and found it 

<img width="1915" height="1009" alt="image" src="https://github.com/user-attachments/assets/dfec7141-e1a3-4969-a1df-3d5e97cff818" />

**Answer:** `2025-08-20 10:12:07`

---

Q14 / Before exfiltration, several files were moved to the staged folder. When was the Heisen-9 facility backup database moved to the staged folder for exfiltration?

I went back to the `$J` (USN Journal) output in Timeline Explorer to check when the file was moved to the staging folder. At first I questioned why this wouldn't be visible in the logs  then I realised that moving a file from one folder to another on the same computer is purely a filesystem event,  which is why the `$J` is the right source here rather than the TeamViewer log

<img width="1917" height="635" alt="image" src="https://github.com/user-attachments/assets/385dffa8-50bd-43e7-835d-fb9c5a2e1c15" />

I searched for "Heisen" in Timeline Explorer, found the database backup file's filesystem activity, and confirmed the correct entry by checking the **Update Reasons** column for a `FileCreate` event.

<img width="1919" height="537" alt="image" src="https://github.com/user-attachments/assets/9777a15b-939d-4e1f-a835-ccb4871039ca" />

**Answer:** `2025-08-20 10:11:09`

---

Q15 / When did the attacker access and read a txt file, which was probably the output of one of the tools they brought, due to the naming convention of the file?

This one was easy , I just searched for mimikatz and checked which .txt files runs after it 

<img width="1919" height="997" alt="image" src="https://github.com/user-attachments/assets/66fb62db-1b01-4fcd-ade9-b3f024e22726" />

Then I searched for it and checked the update reason same process as the previous question 

<img width="1913" height="621" alt="image" src="https://github.com/user-attachments/assets/001c4a33-cdf4-416c-9308-e2293a5d8e31" />

<img width="1912" height="481" alt="image" src="https://github.com/user-attachments/assets/481bb713-5b84-4fe1-bc0e-fea4e7f9a2a7" />

**Answer:** `2025-08-20 10:08:06`


---

Q16 / The attacker created a persistence mechanism on the workstation. When was the persistence setup?

I started by checking the `Run` key in the SOFTWARE hive, but nothing suspicious was there
<img width="1886" height="900" alt="image" src="https://github.com/user-attachments/assets/52f8bbba-074c-4053-910d-3eb936d2399e" />

I then checked a less obvious but well-known persistence location —  `Microsoft\Windows NT\CurrentVersion\Winlogon` , and here the attacker appears to modify the userinit value 
<img width="1879" height="934" alt="image" src="https://github.com/user-attachments/assets/5f40f874-ce77-44d9-b93b-505338552fdc" />
<img width="753" height="20" alt="image" src="https://github.com/user-attachments/assets/f22eec5c-5bba-4191-b8e6-05732607574d" />


**Answer:** `2025-08-20 10:13:57`

---

Q17 / What is the MITRE ID of the persistence subtechnique? 

**Answer:** `T1547.004`

---

Q18 / When did the malicious RMM session end?

This one we can agein the check the first Incoming Connection txt file 

<img width="1919" height="393" alt="image" src="https://github.com/user-attachments/assets/775f51c8-5ad4-481d-972c-80b2a8b2406b" />


**Answer:** `2025-08-20 10:14:27`

---

Q19 / The attacker found a password from exfiltrated files, allowing him to move laterally further into CogWork-1 infrastructure. What are the credentials for Heisen-9-WS-6?

For this we need to crack through the database file , First I got the hash for the database file using kee2pass

<img width="1914" height="251" alt="image" src="https://github.com/user-attachments/assets/fcee0d0d-2ff8-4be5-aa2c-87e9d6825d30" />

Then I used john the reaper to crack the hash as we can here 

<img width="877" height="251" alt="image" src="https://github.com/user-attachments/assets/24b4805f-c0b3-4dc9-ab99-0a2f32f3248e" />

The password for the database is cutiepie14

Using this password we can access and get the credinials 

<img width="608" height="764" alt="image" src="https://github.com/user-attachments/assets/3d2d9f85-5a4e-4be5-858d-578b95f4bf16" />

And we got it 

**Answer:** `Werni:Quantum1!`

---

## Attack Flow 

- The attacker turned on a retired/decommissioned machine (WATSON-ALPHA-2) and used it to start a chat with the MSP's AI helpdesk bot
- Through a carefully crafted conversation, the attacker tricked the AI bot into giving away the remote access tool's login credentials, then signed off with "JM WILL BE BACK"
- The next day, the attacker used those stolen credentials to remotely log into the Cogwork Central Workstation through TeamViewer
- Once inside, the attacker dropped several hacking tools onto the machine, including one that steals saved browser passwords and another that dumps system credentials
- The attacker also modified a Windows registry setting to make their malicious program (JM.exe) run automatically every time the computer starts
- The attacker then stole several sensitive files off the machine and sent them out through the remote session
- One of the stolen files was a password manager database — after cracking it open, the attacker found login credentials for other computers on the network

---

## MITRE ATTACK

Tactic - Initial access
Technique - Valid Accounts (stolen RMM credentials) 
ID -  T1078

Tactic - Lateral Movement
Technique - Remote Services (TeamViewer RMM abuse)
ID -  T1021

Tactic - Credential Access
Technique - OS Credential Dumping (Mimikatz) 
ID - T1003

Tactic - Persistence
Technique - Winlogon Helper DLL
ID - T1547.004

Tactic -  Exfiltration
Technique - Exfiltration Over C2 Channel
ID - T1041

---
Feel free to check out my other investigations in this repository, and connect with me on [LinkedIn](https://www.linkedin.com/in/mohd-mutasem-356346250/) if you have any feedback or questions.









 


