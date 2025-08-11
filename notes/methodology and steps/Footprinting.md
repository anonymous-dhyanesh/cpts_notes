
####  [crt.sh](https://crt.sh/) for certificate lookups..go get info about certificates

####  [domain.glass](https://domain.glass/) to gather info about domains

#### [GrayHatWarfare](https://buckets.grayhatwarfare.com/) also worth looking this for domain or cloud searches

#### Bruteforcing user ids with RPC :
```shell-session
for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```
#### An alternative to this would be a Python script from [Impacket](https://github.com/SecureAuthCorp/impacket) called [samrdump.py](https://github.com/SecureAuthCorp/impacket/blob/master/examples/samrdump.py)..
```shell-session
samrdump.py 10.129.14.128
```

for the same we can also use SMBMAP:
```shell-session
smbmap -H 10.129.14.128
```

OR 
CRACKMAPEXEC:

```shell-session
 crackmapexec smb 10.129.14.128 --shares -u '' -p ''
```

#NFS
