# DanaBot Lab - Cyberdefenders

## Scenario
The SOC team has detected suspicious activity in the network traffic, revealing that a machine has been compromised. Sensitive company information has been stolen. Your task is to use Network Capture (PCAP) files and Threat Intelligence to investigate the incident and determine how the breach occurred.

- **Tools used:** Wireshark

---

### Q1 Which IP address was used by the attacker during initial access?

As it mention initial access it means we are looking for a phishing-kind of attack, as it is the first stage before the attacker manages to enter the network. , So we will check for the website that the victim visited, which gives us the IP for the attacker.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0ea99ff3-5b54-49c2-b21b-183f13abe4e2" />

I filtered for dns query to check which website the user visited and at the same get the IP for the website .
as we filtered for dns here we can see a malicious website and it has the IP 62.173.142.148 which the answer we want

**Answer:** `62.173.142.148`

---

### Q2 What is the name of the malicious file used for initial access?

We can switch to filter for dns as it we will show the files transfered between the victim and attacker , Here, we can see the victim requested login.php
from the server.
![[Pasted image 20260812214612.png]]


<img width="1149" height="366" alt="image" src="https://github.com/user-attachments/assets/27eec81e-78ec-418a-ab08-2a7acf3422c2" />

