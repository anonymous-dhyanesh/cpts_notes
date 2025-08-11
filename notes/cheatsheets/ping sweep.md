host discovery:
Linux:
```bash 
for i in {1..254}; do ip=172.16.119.$i; ping -c1 -W1 $ip | grep "64 bytes from" | cut -d " " -f4 | tr -d ':'; done
```


