
```powershell
Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
```
The `-s` tells it to print results to the console for us, the `-d` specifies the domain to search within, and the `-o` tells Snaffler to write results to a logfile. The `-v` option is the verbosity level. Typically `data` is best as it only displays results to the screen, so it's easier to begin looking through the tool runs. Snaffler can produce a considerable amount of data, so we should typically output to file and let it run and then come back to it later. It can also be helpful to provide Snaffler raw output to clients as supplemental data during a penetration test as it can help them zero in on high-value shares that should be locked down first.

```powershell
 .\Snaffler.exe  -d INLANEFREIGHT.LOCAL -s -v data
```

