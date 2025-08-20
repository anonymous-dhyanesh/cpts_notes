# Windows Post-Exploitation — Methodology (linked)

***[WADComs](https://wadcoms.github.io/)***  
_This is also give tells us what we can do after got into Windows system._

**Contents**
- [[#step-1--host-discovery-after-entering-the-network]]
- [[#step-2--goal-system-lvl-access]]
- [[#step-3--password-spraying-or-hash-spraying]]
- [[#step-4--enumerating-security-controls-on-windows]]
- [[#step-5--assuming-we-have-hash-or-credentials-or-system-lvl-access]]
- [[#step-6--once-we-have-local-admin-credentials-we-can-use-sometools-like]]

---

> [!info] Step 1 — Host discovery (after entering the network)
- Use **Wireshark** or **tcpdump** to analyze traffic and identify hosts (ARP, ICMP, etc).  
- Run **[Responder](responder)** in analyze mode (`-A`) so it does not modify packets.  
- Use **[Fping](fping)** to scan live hosts via ping requests.  
- After making a list of active hosts, pass it to **[Nmap](nmap)** → scan every host in the list.  

**Next →** [[#step-2--goal-system-lvl-access]] | **Back to top →** [[#windows-post-exploitation--methodology-linked]]

---

> [!info] Step 2 — Goal: system lvl access
_System-level access opens many opportunities._

- **MUST TRY**: sometimes without password, through:
  - **SMB Null Session**  
  - **LDAP Anonymous Bind**  
  - **RPC Null Session**  

  You can often retrieve:  
  - **Password Policy** → [detail](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FEnumerating%20%26%20Retrieving%20Password%20Policies)  
  - **User List** → [detail](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2Fgetting%20or%20making%20user%20list)  

- Check valid usernames with **[Kerbrute](kerbrute)**.  
  - Pre-built username lists → [statistically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames)  
- Try **[LLMNR poisoning](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2FLLMNR%20poisoning)** (crack or relay).  

**Previous ←** [[#step-1--host-discovery-after-entering-the-network]] | **Next →** [[#step-3--password-spraying-or-hash-spraying]] | **Back to top →** [[#windows-post-exploitation--methodology-linked]]

---

> [!info] Step 3 — Password spraying or hash spraying
- Before spraying, **check password policies if possible**.  
- Detailed notes → [Password Spraying](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FPassword%20Spraying)

**Previous ←** [[#step-2--goal-system-lvl-access]] | **Next →** [[#step-4--enumerating-security-controls-on-windows]] | **Back to top →** [[#windows-post-exploitation--methodology-linked]]

---

> [!info] Step 4 — Enumerating security controls on windows
- It’s good to enumerate if possible.  
- Example: **LAPS extended rights** may allow reading passwords.  
- Detailed notes → [Enumerating Security Controls](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2Fenumerating%20security%20controls)

**Previous ←** [[#step-3--password-spraying-or-hash-spraying]] | **Next →** [[#step-5--assuming-we-have-hash-or-credentials-or-system-lvl-access]] | **Back to top →** [[#windows-post-exploitation--methodology-linked]]

---

> [!info] Step 5 — Assuming we have hash / credentials / system lvl access
- **Credentialed Enumeration** → [details](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FCredentialed%20Enumeration)  
- **[Kerberoasting](kerberoasting)**  

**Previous ←** [[#step-4--enumerating-security-controls-on-windows]] | **Next →** [[#step-6--once-we-have-local-admin-credentials-we-can-use-sometools-like]] | **Back to top →** [[#windows-post-exploitation--methodology-linked]]

---

> [!info] Step 6 — Once we have local admin credentials we can use sometools like
- [psexec.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fpsexec)  
- [wmiexec.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fimpacket%2Fwmiexec) (semi-interactive shell)  
- [windapsearch.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fimpacket%2Fwindapsearch) (enumerate high-priv groups and users)  
- [BloodHound](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fbloodhound)  
- [Snaffler](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fsnaffler)  

**Previous ←** [[#step-5--assuming-we-have-hash-or-credentials-or-system-lvl-access]] | **Back to top →** [[#windows-post-exploitation--methodology-linked]]
