# SharpHound-detection

Scenario execution:
Exécution of sharphound from target machine in AD Infrastructure.

Evidences output :

    1.	Sysmon Logs (DC + target machine)
    2.	Security Logs (DC + target machine)
    3.	System Logs  (DC + target machine)
    4.	Logs Directory Service (DC)

(we might not need this, because we have our SIEM reporting the logs from all Domain endpoints)

The configuration needed for LDAP auditing on the “DC” Server before the scenario deployment: 
•	Open « Registry Editor » 
•	Navigate to “HKEY_LOCAL_MACHINE -> SYSTEM -> CurrentControlSet -> Services -> NTDS -> Diagnostics”
•	Add in the field '15 Field Engineering' the value '5'


After analyzing the logs generated in our SIEM, we can implement some rules to detect the “sharphound” in our Domain. 

Rules to implement :

•	Server Side (DC) : 
    Contain the Event id 5145
    2.	With same Account Name, src and ds tip under interval of 1 min
    3.	Account Name not contains $
    4.	Share name : \\*\IPC$



I have implemented the filters in QRadar and this is the result I got : 
•	With Duration of 19 ms and 35 total results the filter has shown us the right result we want in the DC log sources.

![alt text](https://github.com/chnz2k/SharpHound-detection/blob/main/1.png?raw=true)

 
•	Client side (target machine) :
    1.	Multiple connections to LDAP(S) and SMB  (ports 445, 389, 636)
    2.	Multiple application name used “lsass” and “srvsvc”

I have implemented the filters in QRadar and this is the result I got : 
•	With Duration of 62 ms and 129 total results the filter has shown us the right result we want in the target log sources.

![alt text](https://github.com/chnz2k/SharpHound-detection/blob/main/2.png?raw=true)
 

Hope this research might be useful for you fellas 😉
