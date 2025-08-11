

Academy link [here](https://academy.hackthebox.com/module/112/section/1069).

#### we can use dig command to query DNS:
```bash
dig soa www.inlanefreight.com

dig ns inlanefreight.htb @10.129.14.128

dig CH TXT version.bind 10.129.120.85

dig any inlanefreight.htb @10.129.14.128

dig axfr inlanefreight.htb @10.129.14.128

# and then we can specify with sub-doimain if we got any result from the upper command
dig axfr internal.inlanefreight.htb @10.129.14.128


```

#### Sub domain Brute Forcing
using bash for loop:
```shell-session
for sub in $(cat /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt);do dig $sub.inlanefreight.htb @10.129.14.128 | grep -v ';\|SOA' | sed -r '/^\s*$/d' | grep $sub | tee -a subdomains.txt;done
```
with DNSenum:
```shell-session
dnsenum --dnsserver 10.129.14.128 --enum -p 0 -s 0 -o subdomains.txt -f /opt/useful/seclists/Discovery/DNS/subdomains-top1million-110000.txt inlanefreight.htb
```
we can also use ffuf, gobuster etc etc for this.