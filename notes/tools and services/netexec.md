
netexec
nxc

usage:
```bash
netexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```
```bash
netexec winrm 10.129.42.197 -u user.list -p password.list
```

password spraying:
```shell
sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```

local auth spraying (to all local administrator account once only):
```shell
sudo netexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```

enumerating users:
```shell
sudo netexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```

enumerating groups:
```shell
sudo netexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```

checking for loggedon users:
```shell
sudo netexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```

enumerating shares or listing shares:
```shell
sudo netexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```


