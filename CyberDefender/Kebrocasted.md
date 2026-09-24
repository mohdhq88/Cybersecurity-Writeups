
Q1 / To mitigate Kerberoasting attacks effectively, we need to strengthen the encryption Kerberos protocol uses. What encryption type is currently in use within the network?

<img width="1880" height="830" alt="image" src="https://github.com/user-attachments/assets/c21c3dac-e50b-4dbf-bff4-39c6e39848b8" />

<img width="1533" height="816" alt="image" src="https://github.com/user-attachments/assets/7f7244bd-bb16-4438-baa9-5d7f27f02b02" />

<img width="1865" height="323" alt="image" src="https://github.com/user-attachments/assets/6ceca7fb-6dc3-4bae-9f76-d3669efac963" />


**Answer:** `RC4-HMAC`

---

Q2 / What is the username of the account that sequentially requested Ticket Granting Service (TGS) for two distinct application services within a short timeframe?
<img width="1917" height="727" alt="image" src="https://github.com/user-attachments/assets/366f6d4a-ec40-4058-9d9f-caa3b738a994" />





<img width="1917" height="842" alt="image" src="https://github.com/user-attachments/assets/e928ac6f-77fd-4041-86de-df2e11a617c8" />


**Answer:** `johndoe`


---

Q3/ We must delve deeper into the logs to pinpoint any compromised service accounts for a comprehensive investigation into potential successful kerberoasting attack attempts. Can you provide the account name of the compromised service account?

As we can two services were requuest in similar timing one of them got compromised 

<img width="1913" height="511" alt="image" src="https://github.com/user-attachments/assets/ac147cf0-ab1f-4cc8-93d0-18347562caa4" />

**Answer:** `SQLService`

---

Q4 / To track the attacker's entry point, we need to identify the machine initially compromised by the attacker. What is the machine's IP address?
<img width="1557" height="872" alt="image" src="https://github.com/user-attachments/assets/888b366a-943f-4a82-8dd9-210b30d21ebc" />








