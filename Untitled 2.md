# 🪟 Post-Exploitation Workflow (Windows)

***[WADComs](https://wadcoms.github.io/)***  
_This resource shows what actions are possible once we gain access to a Windows system._

---

## 🔎 Host Discovery (After Entering the Network)

1. Use **Wireshark** or **tcpdump** to analyze traffic and identify hosts (ARP, ICMP, etc).
2. Run **[Responder](responder)** in analyze mode (`-A`) so it does not modify packets.
3. Use **[Fping](fping)** to scan live hosts via ping requests.
4. After making a list of active hosts, pass it to **[Nmap](nmap)** → scan every host in the list.

---

## 🎯 Goal: System-Level Access
_System-level access opens many opportunities._

- **MUST TRY**  
  Sometimes without passwords, via:
  - **SMB Null Session**  
  - **LDAP Anonymous Bind**  
  - **RPC Null Session**  

  We can often retrieve:  
  - **Password Policy** → [details](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FEnumerating%20%26%20Retrieving%20Password%20Policies)  
  - **User List** → [details](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2Fgetting%20or%20making%20user%20list)  

1. Check for valid usernames with **[Kerbrute](kerbrute)**.  
   - Pre-built username lists: [statistically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames)  
2. Try **[LLMNR poisoning](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2FLLMNR%20poisoning)** (crack or relay → see LLMNR notes).

---

## 🔑 Password Attacks

- **Check password policies first** (important before spraying).  
- **Password Spraying / Hash Spraying** → [detailed notes](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FPassword%20Spraying)

---

## 🛡️ Enumerating Security Controls

- Enumerate if possible:  
  - Example: **LAPS extended rights** may allow us to read stored passwords.  
- [Detailed notes here](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2Fenumerating%20security%20controls)

---

## 🧰 With Hashes / Credentials / System-Level Access

- **Credentialed Enumeration** → [details](obsidian://open?vault=cpts&file=notes%2Factive%20directory%2FCredentialed%20Enumeration)  
- **[Kerberoasting](kerberoasting)**  

---

## ⚙️ Tools with Local Admin Credentials

- [psexec.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fpsexec)  
- [wmiexec.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fimpacket%2Fwmiexec) → semi-interactive shell  
- [windapsearch.py](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fimpacket%2Fwindapsearch) → lists high-priv groups and users  
- [BloodHound](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fbloodhound)  
- [Snaffler](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2Fsnaffler)  
