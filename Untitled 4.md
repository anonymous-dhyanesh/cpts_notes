
### 1. Identifying Live Hosts
The first step is to discover which systems are active on the network. Several methods can be employed:

* **Traffic Analysis:** Use tools like **Wireshark** or **tcpdump** to analyze network traffic and identify hosts from protocols such as ARP or ICMP.
* **RESPONDER:** Run the **Responder** tool in analyze mode (`-A`) to passively identify hosts without altering any packets.
* **Fping:** Utilize the **fping** tool to send ping requests and scan for live hosts.
* **Nmap:** Once a list of active hosts is compiled, use **Nmap** to perform a comprehensive scan on each host.

> [!TIP]
> Use passive methods like Responder first to avoid creating noise on the network.

---

### 2. Gaining System-Level Access
The primary objective is to obtain system-level access, which opens up numerous opportunities.

* **Null Sessions:** Attempt to gain valuable information without credentials.
    * Through SMB null sessions, LDAP anonymous binds, or RPC null sessions, you may be able to retrieve the **password policy** and **user list**.

> [!MUST TRY]
> Sometimes, you can get the password policy and a user list without a password through null sessions. This is a crucial first step!

* **Username Enumeration:**
    * Use **Kerbrute** to check for valid usernames. A good source for pre-built username lists can be found [here](https://github.com/insidetrust/statistically-likely-usernames).
* **LLMNR Poisoning:**
    * If a password is not available, try **LLMNR poisoning** to capture or relay authentication hashes.
* **Password/Hash Spraying:**
    * Before proceeding, it is crucial to check the password policies if possible.
    * Perform **password spraying** or **hash spraying** to attempt logging in with a common password across multiple user accounts.

> [!IMPORTANT]
> Check password policies before password spraying to avoid locking out accounts.

---

### 3. Enumerating Security Controls
It's beneficial to enumerate security controls on Windows systems, as this can sometimes reveal vulnerabilities.

* **LAPS (Local Administrator Password Solution):** Sometimes, extended rights for LAPS can allow for the reading of passwords.

> [!REMEMBER]
> Enumerating security controls can reveal misconfigurations, such as extended rights for LAPS that allow password reads.

---

### 4. Post-Compromise Actions
Assuming you have a hash, a set of credentials, or system-level access on a domain-joined machine, you can perform the following:

* **Credentialed Enumeration:**
    * Explore the network and systems after obtaining credentials.
* **Kerberoasting:**
    * Attempt to extract service account hashes from Kerberos service tickets.

> [!KEY ACTION]
> Once you have credentials, credentialed enumeration and Kerberoasting are powerful ways to escalate privileges and move laterally.

---

### 5. Tools for Privilege Escalation and Lateral Movement
Once you have local administrator credentials, several tools can be used to further your access:

* **psexec.py:** A popular tool for executing processes on remote systems.
* **wmiexec.py:** Provides a semi-interactive shell for remote command execution.
* **windapsearch.py:** Can be used to list high-privileged groups and users (e.g., domain admins).
* **BloodHound:** A powerful tool for mapping and analyzing relationships within an Active Directory environment to find attack paths.
* **Snaffler:** Used for finding sensitive files and data on a network.

> [!TOOLKIT]
> These tools are essential for lateral movement and privilege escalation once you have local admin credentials. BloodHound is particularly useful for visualizing attack paths.
