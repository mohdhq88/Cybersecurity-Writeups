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

Q1 / 62.173.142.148

---

###
