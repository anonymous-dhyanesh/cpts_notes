> [!TIP]
> ***we can also try to steal creds from memory of logged on users.*** 

academy link: https://academy.hackthebox.com/module/143/section/1269

***once we have domain user creds we can :***
	 - run [bloodhound](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fbloodhound)
	 - enumerate users with various tools...later in this note
	 - 
# From linux:

## netexec or crackmapexec:
enumerating users 
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

spidering through shares: 
for this we must read at lest READ access:
```shell
sudo netexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```
When completed, CME writes the results to a JSON file located at `/tmp/cme_spider_plus/<ip of host>`


## smbmap:
this tool also has ability to upload and download files.

listing shares:
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```
***i think we can also list shares without -d ...not sure***

recursive listing of share:
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares'
```
to only list dirs we can user --dir-only flag:
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```



