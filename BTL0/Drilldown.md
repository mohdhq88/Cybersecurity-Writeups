# BTL0 Investigation : DrillDown

## Scenario 
Your organization doesn't use Amazon Web Services, so when a Threat Hunter starts seeing connections to multiple EC2 instances, it's time to start hunting to understand what happened, so the information can be passed to the incident response team, and indicators can be gather for intelligence sharing.

## Tools Used
Splunk , Virustotal

## Investigation

 Q1 : WayneCorpInc doesn't use Amazon Web Service for cloud hosting, so when a threat hunting discovered outbound connections to EC2 instances they immediately began to drilldown into this activity so they can provide as much context for the Incident Response Team as possible. Using Sysmon logs, how many destination hostnames are found? (Format: # Destination EC2s)

Since this is an outbound connections I began by searching for Sysmon EventCodes=3 which list external connections

 __index=* EventCode=3 source="WinEventLog:Microsoft-Windows-Sysmon/Operational"__
<img width="1918" height="631" alt="image" src="https://github.com/user-attachments/assets/5257c615-91a0-4256-96bd-cc8c8c8354a5" />
So it is confirmed that we have Network connection logs. To remove the noise, we filtered for EC2 instances specifically.

<img width="1376" height="537" alt="image" src="https://github.com/user-attachments/assets/0d4c73ae-43be-4119-9a9e-0f4a80942103" />

So as we can see here there is a connection to 3 different EC2 instances 

#### Question 1 / 3

Q2 : Enter the hostnames (excluding '.compute-1.amazon.aws.com') in the order of event count, with the highest first (Format: Hostname1, Hostname2, ...) 

From the same results from above we will able to see the hostnames of the 3 instances 

#### Question 2 / ec2-23-22-63-114, ec2-184-72-234-184, ec2-52-70-175-181

Q3 : Look at the Image 'interesting field' to see what files are initiating these connections. What is the Image value with the lowest count? (Format: Image Value)

Since it is mentioning Image field we can filter by Image field of this 3 EC2 instances and see what we got
<img width="1919" height="515" alt="image" src="https://github.com/user-attachments/assets/9a271089-806f-4915-acf2-e84f59952962" />

Out of the 5 process found initiating connections to the EC2 services 3791.exe stands out because it is located inside \wwwroot\joomla\3791.exe which should only contain PHP , HTML , images files and not .exe files so I tried this answer and turn out it is correct

#### Question 3 / C:\inetpub\wwwroot\joomla\3791.exe

Q4 :  What is the hostname and internal IP address of the system that initiated this connection? (Format: Hostname, X.X.X.X) 

We can find the hostname and IP address by checking the same event 
<img width="1380" height="692" alt="image" src="https://github.com/user-attachments/assets/5e78b5c7-c68f-4710-ab92-9fe91e1b8ae5" />
<img width="998" height="301" alt="image" src="https://github.com/user-attachments/assets/c7bb5a61-35fd-4bfb-b3b3-cc7c21044317" />

#### Question 4 / we1149srv.waynecorpinc.local, 192.168.250.70

Q5 :  What time was this connection event? Use TimeCreated SystemTime (Format: YYYY-MM-DDTHH:MM:SS)

Using the same event I found systemtime which I tried and turned out be the correct answer
<img width="910" height="347" alt="image" src="https://github.com/user-attachments/assets/5cf89b14-dcb9-471d-b71f-3a33fc73c8cb" />

Q6 : What is the destination hostname and IP address of the AWS EC2 instance? (Format: Hostname, X.X.X.X)

Another easy question we can find the destination hostname and IP address using the same event 
<img width="1000" height="347" alt="image" src="https://github.com/user-attachments/assets/ea5e73a8-4292-4f4c-b8ce-baff5b9f0da0" />
<img width="925" height="222" alt="image" src="https://github.com/user-attachments/assets/cd232fa8-0a65-4dde-9cd2-0c50536a7c10" />

#### Question 6 / ec2-23-22-63-114.compute-1.amazonaws.com, 23.22.63.114

Q7 : Utilize Sysmon logs to find the SHA256 hash of the executable making this connection. What is the hash value? (Format: SHA256 Hash) 

For finding the hash value I used sysmon event 1 which process creation as it records every information for any executable created 
<img width="781" height="214" alt="image" src="https://github.com/user-attachments/assets/eb2a887a-1f5f-4b45-a98d-d0ea580bcb03" />

#### Question 7 / EC78C938D8453739CA2A370B9C275971EC46CAF6E479DE2B2D04E97CC47FA45D

Q8:  Search this hash online to find more about its reputation. On the Behaviour tab look at the results for Microsoft Sysinternals. What two IPv4 addresses are listed, that begin with 23.216.? (Format: X.X.X.X, X.X.X.X)  

We used VirusTotal to look up the reputation of the SHA256 hash On the "Behavior" tab VirusTotal shows results from multiple sandbox engines that each independently executed the file. Under the "Microsoft Sysinternals" section specifically, two IPv4 addresses in the 23.216.x.x range were observed being contacted by the malware during sandbox execution.

<img width="551" height="448" alt="image" src="https://github.com/user-attachments/assets/e72107bf-0c99-4e9d-9c51-17633500029f" />

#### Question 8 / 23.216.147.64, 23.216.147.76

Q9: Using these two gathered IPs, check to see if there is any activity from them in Splunk, which there might not be! What is the number of events per IP where the address is mentioned ANYWHERE in a log? (Format: IP1EventCount, IP2EventCount)

I searched for the both the IP addresses but there was no result found so it turned out that there was no connection to this two IP addressess from the affected hostnames

#### Question 9 / 0, 0

Q10 : At what time was this file uploaded to the web server? (Use 'timestamp' value) (Format: YYYY-MM-DDTHH:MM:SS) (4 points)

This one confused me a bit at first. I searched through the HTTP logs and found a POST request event related to 3791.exe. I assumed this was the log entry recording the file being uploaded to the server, so I checked its timestamp value and submitted that as the answer.

<img width="1622" height="316" alt="image" src="https://github.com/user-attachments/assets/f59a43ba-3f5d-4ceb-99c3-c3492f13b4a9" />

#### Question 10 / 2016-08-10T21:52:45

















