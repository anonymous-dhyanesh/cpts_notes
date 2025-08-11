
```shell-session
 for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
```
its better to use openssl for openssl protected files with for loop as shown above