
```
curl -k 'imaps://10.129.42.195' --user robin:robin -v
```

can also connect with ssl:
```
openssl s_client -connect 10.129.42.195:995
```
```
openssl s_client -connect 10.129.42.195:993 -showcerts
```

```
curl -k --user robin:robin "imaps://10.129.42.195/INBOX"
```
```
* 1 EXISTS
* 2 RECENT
curl -k --user robin:robin "imaps://10.129.42.195/INBOX;MAILINDEX=2"
```
*TIP: we can go further with mail index 1,2,3,4 etc*



