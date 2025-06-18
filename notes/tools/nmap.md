## nmap module cheat sheet [Here](obsidian://open?vault=cpts&file=notes%2Fcheatsheets%2Fnmap)
- `nmap 10.129.42.253` – Basic scan  
- `nmap -sV -sC -p- 10.129.42.253` – Full script scan  
- `locate scripts/citrix` – Locate Nmap scripts  
- `nmap --script smb-os-discovery.nse -p445 10.10.10.40` – Run specific script  

#### **scan the entire network and give the hosts available in the formatted format([example](obsidian://open?vault=cpts&file=images%2FPasted%20image%2020250616140153.png)):** 
it will scan all the hosts and will just return the hosts ips in formatted way
```shell-session
sudo nmap 10.129.2.0/24 -sn -oA tnet | grep for | cut -d" " -f5
```
#### **scanning all hosts from a file([example](obsidian://open?vault=cpts&file=images%2FPasted%20image%2020250616140153.png)):**
```shell-session
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5
```
#### **scanning multiple ips with entering multiple ips manually([example](obsidian://open?vault=cpts&file=images%2FPasted%20image%2020250616140901.png)):**
```shell-session
sudo nmap -sn -oA tnet 10.129.2.18 10.129.2.19 10.129.2.20| grep for | cut -d" " -f5

#we can also specify ranges if ips are sequentials or next to each other:
sudo nmap -sn -oA tnet 10.129.2.18-20| grep for | cut -d" " -f5
```

##### By default nmap scans with ARP ping request not with ICMP..so we need to disable arp req and enable ICMP maybe we find some more targets:



#### nmap for banner grabbing :
```
nmap -sV --script=banner -p21 10.10.10.0/24
```



