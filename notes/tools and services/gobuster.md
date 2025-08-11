- `gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt` – Directory brute-force  
- `gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt` – Subdomain scan  

vhost fuzzing:
```shell-session
gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
```
example:
```shell-session
gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

