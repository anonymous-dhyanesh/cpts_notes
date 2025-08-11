
have a look on this may help [example](obsidian://open?vault=cpts&file=images%2Fnfs%20image):
```shell-session
'/mnt/nfs  10.129.14.0/24(sync,no_subtree_check)' >> /etc/exports
```

get more info with nmap:
```shell-session
sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049
```

checking for mount [example](obsidian://open?vault=cpts&file=images%2FPasted%20image%2020250625202036.png):
```shell-session
showmount -e 10.129.14.128
```


```shell-session

#making dir
mkdir target-NFS

# mounting folder to local
sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock

# unmount
sudo umount ./target-NFS
```

