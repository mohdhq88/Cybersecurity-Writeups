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



