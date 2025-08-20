academy link: https://academy.hackthebox.com/module/143/section/1485

## Enumeration:

###### Powerview:

importing PowerView:
```powershell
Import-Module .\PowerView.ps1
```
massive info output :
```powershell
Find-InterestingDomainAcl
```
converting name to sid:
```powershell
$sid = Convert-NameToSid wley
```
exploring domain user rights:
```powershell
Get-DomainObjectACL -Identity * | ? {$_.SecurityIdentifier -eq $sid}

# we can also use "ResolveGUIDs" flag to automatically that numbers into names
Get-DomainObjectACL -ResolveGUIDs -Identity * | ? {$_.SecurityIdentifier -eq $sid} 
```
creating a list of domain users:
```powershell
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt
```
getting details for each user in the list:
```powershell
foreach($line in [System.IO.File]::ReadLines("C:\Users\htb-student\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match 'INLANEFREIGHT\\wley'}}
```

