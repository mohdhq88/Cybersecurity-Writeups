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

We can switch to filter for http as it we will show the files transfered between the victim and attacker , Here, we can see the victim requested login.php
from the server.
<img width="1792" height="304" alt="image" src="https://github.com/user-attachments/assets/c6fb789f-c968-4780-aad6-1788ad26aaea" />

Then I checked the http stream for one that requested the login.php and I found a malicious attachment 

<img width="1149" height="366" alt="image" src="https://github.com/user-attachments/assets/27eec81e-78ec-418a-ab08-2a7acf3422c2" />

This reveals our answer

**Answer:** `allegato_708.js`

---

### Q3 What is the SHA-256 hash of the malicious file used for initial access?

I exported the login.php file from wireshark and then I used sha256sum tool
<img width="717" height="302" alt="image" src="https://github.com/user-attachments/assets/b3e368c3-c9f8-4e28-ae2d-89f9a724cbf9" />

*Answer:** `847b4ad90b1daba2d9117a8e05776f3f902dda593fb1252289538acf476c4268`

---

### Q4 Which process was used to execute the malicious file?

Then one I just used google to search what process execute js as you cant see the process using wireshark
<img width="695" height="352" alt="image" src="https://github.com/user-attachments/assets/4f3d73b6-6eaf-4b02-9ac7-d9a5c9abb066" />

*Answer:** `wscript.exe`

---

### Q5 What is the file extension of the second malicious file utilized by the attacker?
If we look back at the http packets we can see another two GET request after the login.php one for a hash file and one for .dll file which is the likely the second malicious files used by the attacker 

<img width="1856" height="213" alt="image" src="https://github.com/user-attachments/assets/a377c19e-75d7-4536-bb9a-9ab8a1de62ad" />

*Answer:** `.dll`

### Q6 What is the MD5 hash of the second malicious file?

I exported the resources.dll file and then found the MD5 hash using the same way for question 3

<img width="717" height="361" alt="image" src="https://github.com/user-attachments/assets/349c0cc3-2db9-44a5-a55a-d6db254ad083" />


## Attack Flow
The victim visits (`62.173.142.148`) and requests `/login.php`, which returns a file disguised as an email attachment — `allegato_708.js`, . Once executed, the script reaches out to a different IP, `188.114.97.3`, and downloads the real payload, `resources.dll`.  The DLL is then executed, and DanaBot becomes active on the machine, beginning communication with attacker infrastructure.



## Mitre Attack

ID - T1566.001
Tactic - Initial access
Technique - Spear Phishing - Attachment




