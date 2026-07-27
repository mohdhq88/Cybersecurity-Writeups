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







