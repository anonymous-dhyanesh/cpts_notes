***https://wadcoms.github.io/***
***this is also give tells us what we can do after got into windows system.***

* ### so after entering into network we need to check which hosts are live,there are many methods to do this:

	1. we can use wireshark or tcp dump to analyze the traffic and indentify hosts from there(arp,icmp etc)
	2. using [RESPONDER](responder) tool..running responder in analyze mode so that it dont change any packets..(-A).
	3. Fping is a tool do scan live hosts via ping req...commands [here](fping)
	4. after making list of active hosts , we can pass whole list to [nmap](nmap)..i will scan every host in the list

---
- ### now our goal should to find system lvl access..it opens up many oppourtunities.
	* **MUST TRY** : sometimes without password, through smb null session or LDAP anonymous bind or rpc null session we can get **password policy([detail](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FEnumerating%20%26%20Retrieving%20Password%20Policies))** and **user list([detail](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2Fgetting%20or%20making%20user%20list))** as well.*
	1. checking for valid username using [kerbrute](kerbrute). pre built usernames can be find [here](https://github.com/insidetrust/statistically-likely-usernames)
	2. if we dont have password then we also try [LLMNR](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2FLLMNR%20poisoning) poisoning.(crack or relay more info in LLMNR notes)

- *before going for password spraying we should check password policies if possible, it can be done like this:*
- ### Password spraying or hash spraying:
	- more detailed notes [here](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FPassword%20Spraying)

- ### Enumerating security controls on windows: 
	- its just good to enumerate if possible sometimes laps extended right allow us to read passwords. detailed notes [here](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2Fenumerating%20security%20controls).

- ### assuming we have hash or a set of credentials or system lvl access on a domain joined machine we can do:
	- Credentialed Enumeration. (exploring after getting creds)detailed note [here](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FCredentialed%20Enumeration).
	- [kerberoasting](kerberoasting).  
	
- ### once we have local admin credentials we can use sometools like:
	- [psexec.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fpsexec)
	- [wmiexec.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fimpacket%2Fwmiexec) (gives semi interactive shell)
	- [windapsearch.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fimpacket%2Fwindapsearch)(it can give us list of high privilaged groups and users like admins etc etc)
	- [bloodhound](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fbloodhound) 
	- [snaffler](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fsnaffler)

- 