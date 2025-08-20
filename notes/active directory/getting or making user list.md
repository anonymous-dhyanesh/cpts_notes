academy link: https://academy.hackthebox.com/module/143/section/1455
### From linux:

**trying to get user list with smb null session :**

via netexec or crackmapexec:
```shell
crackmapexec smb 172.16.5.5 --users
```

via enum4linux:
```shell
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```

via rpcclient:
```shell
rpcclient -U "" -N 172.16.5.5
```
after login  to rcpclient we should run these command to get users:
```shell
rpcclient $> enumdomusers 
```

**trying to get user list via ldap anonymous:**
```shell
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```

tools like windapsearch makes this a bit easier:
```shell
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```

