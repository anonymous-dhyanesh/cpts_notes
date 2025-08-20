academy link: https://academy.hackthebox.com/module/143/section/1490
### From linux:
```shell
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```
*we can do this with netexec as well.*

enumerating via rpcclient:
```shell
rpcclient -U "" -N 172.16.5.5
```
after login in to rpcclient we can do:
```shell
querydominfo

getdompwinfo
```

via enum4linux:
```shell
enum4linux -P 172.16.5.5
```

via enum4ilnux-ng(its upgraded version of enum4linux with some additional functionalities)
 repo link: [enum4linux-ng](https://github.com/cddmp/enum4linux-ng)
```shell
enum4linux-ng -P 172.16.5.5 -oA ilfreight
```

 enumerating using ldap:
```shell
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```

### From windows:
by null session
null session:
```cmd
net use \\DC01\ipc$ "" /u:""
```
with username:
```cmd
net use \\DC01\ipc$ "" /u:guest
```
with password:
```cmd
net use \\DC01\ipc$ "password" /u:guest
```

can also use:
```cmd-session
net accounts
```

by using powerview tool:
```powershell
import-module .\PowerView.ps1
Get-DomainPolicy
```

