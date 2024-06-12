# Sentinel Incident Investigation Using NIST 800-61 Incident Management Lifecycle

## Introduction
In this activity, I used Microsoft Sentinel (SIEM) to investigate an alert indicating a possible successful brute force attack on my virtual machine (VM). I applied the NIST 800-61 Incident Management Lifecycle to conduct a thorough investigation and draw conclusions about the incident.

## Steps

### Step 1: Receive Alert and Assign Investigation
Inside Microsoft Sentinel, I received an alert of a possible successful brute force attack after leaving my VM open to the internet. I assigned myself as the person in charge of investigating this alert.

![Incident-Investigation 1](https://github.com/RafaelValera15/Sentinel-Incident-Investigation/assets/170124569/5a459bb6-08d7-4873-87d7-30ce61d2887b)


### Step 2: Locate Malicious IP Address
In the overview section, I located the IP address of the malicious actor that “broke” into my VM. The IP address originated from Vietnam.

![Incident-Investigation 2](https://github.com/RafaelValera15/Sentinel-Incident-Investigation/assets/170124569/72247649-770e-40a3-8d93-fb8215e9e82e)



### Step 3: Investigate Related Incidents
I further investigated the incident to see what other attacks were related to this malicious IP address. 

![Incident-Investigation 3](https://github.com/RafaelValera15/Sentinel-Incident-Investigation/assets/170124569/bddb4518-fd43-493c-a053-dfd7f7ef2e08)


### Step 4: Analyze Additional Attacks
The malicious IP address was associated with numerous other brute force attacks across the internet. The left image shows the IP address where the attack originated, and the right image displays other attacks this actor has attempted.

![Incident-Investigation 4](https://github.com/RafaelValera15/Sentinel-Incident-Investigation/assets/170124569/832c0bb2-f43e-46e0-b79f-3a298b554f04)
![Incident-Investigation 5](https://github.com/RafaelValera15/Sentinel-Incident-Investigation/assets/170124569/ea72750c-0b4e-43b9-9427-595d521d35ef)


### Step 5: Review Logs in Log Analytics Workspace
To dig deeper, I reviewed the logs in my Log Analytics Workspace. I wrote a KQL script to query logs from this IP address and found that after the "successful attempt," the actor continued to try logging into the VM, resulting in more EventID 4625 (Failed attempt).

![Incident-Investigation 6](https://github.com/RafaelValera15/Sentinel-Incident-Investigation/assets/170124569/ed281a5d-9a30-47fc-a5a9-8aaa006fa860)

### Step 6: Conclusion of the investigation
At the end of my investigation I’ve concluded that the “Successful attempts” have been a false positive. After looking at the logs I noticed that they continued to brute force for over 100 times unsuccessfully(4625). The reason for this “successful logon” is because when a user initiates an RDP or SMB connection, the connection via RDP/SMB will be logged as a successful connection, BEFORE the user is prompted to enter their password. This means a successful(4624) will be logged for type 3 as an anonymous logon.

![Incident-Investigation 7](https://github.com/RafaelValera15/Sentinel-Incident-Investigation/assets/170124569/2f085b3b-7dbf-4e08-9084-8b6611702fd9)


## Final Conclusion
For this exercise, I left my virtual machine open to the public internet for 24 hours to gather logs and attract real malicious activity. Applying the NIST 800-61 Incident Management Lifecycle, I investigated the incident and concluded that despite being a false positive, my VM was too exposed. Consequently, I will change my Network Inbound Rules to harden my security posture and that way minimize the risk.
