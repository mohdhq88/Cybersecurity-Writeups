<img width="468" height="94" alt="image" src="https://github.com/user-attachments/assets/5d09e07c-c515-44f9-a3b3-cdc794f6ebfe" />

# HTB SHERLOCK: Holmes 2025 2: The Watchman's Residue
## Scenario
With help from D.I. Lestrade, Holmes acquires logs from a compromised MSP connected to the city’s financial core. The MSP’s AI helpdesk bot looks to have been manipulated into leaking remote access keys - an old trick of Moriarty’s.

## Tools 
Wireshark , Zimmerman Tools

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

Q6 / 





 


