https://academy.hackthebox.com/course/preview/active-directory-bloodhound complete this
custom queries resource link: [custom Cypher queries](https://hausec.com/2019/09/09/bloodhound-cypher-cheatsheet/)

there are two ways to collect data:
- first is two send sharphound.exe to windows host and run there...it will collect and generate files ...then transfer those file into our local machine
- using bloohound-python on our linux attacking machine. provide creds and ip will automatically collect data and spit out files.. then upload it to bloodhound and we are good to go.

bloodhound-python:
```shell
sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all 
```

sharphound:
```powershell
 .\SharpHound.exe --help
```

```powershell-session
.\SharpHound.exe -c All --zipfilename ILFREIGHT
```


![[Pasted image 20250819194424.png]]
