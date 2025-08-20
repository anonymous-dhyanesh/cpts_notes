
#### blank enumeration
-  sometimes without password, through smb null session or LDAP anonymous bind or rpc null session we can get **password policy([detail](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FEnumerating%20%26%20Retrieving%20Password%20Policies))** and **user list([detail](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2Fgetting%20or%20making%20user%20list))** as well.*
-  we can use wireshark or tcp dump to analyze the traffic and indentify hosts from there(arp,icmp etc)
-  using [RESPONDER](responder) tool..running responder in analyze mode so that it dont change any packets..(-A).
- Fping is a tool do scan live hosts via ping req...commands [here](fping)
- after making list of active hosts , we can pass whole list to [nmap](nmap)..i will scan every host in the list
- snaffler can also give results

#### with low lvl creds:
- collect data with [bloodhound](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fbloodhound%20or%20sharphound) or sharphound
- explore smb shares
- check credentialed enumeration [here](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FCredentialed%20Enumeration) in more detail.
- 

#### enumerating using powersell active directory cmdlet:
htb academy link: https://academy.hackthebox.com/module/143/section/1421

first we need to load that module:
```powershell
Import-Module ActiveDirectory
```

check if module loaded successfully..if yes it will show module name in outpout:
```powershell
Get-Module
```

lets start enumerating:
```powershell
Get-ADDomain
```

filtering accounts which can we suspecticle to kerbroast attack:
```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```

checking for trust and relationships:
```powershell
Get-ADTrust -Filter *
```

getting group information:
```powershell
Get-ADGroup -Filter * | select name
```

checking particular group:
```powershell
Get-ADGroup -Identity "Backup Operators"
```

get all group members:
```powershell
Get-ADGroupMember
```


