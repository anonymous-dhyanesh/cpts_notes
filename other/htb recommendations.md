
## Resources

When starting, the sheer amount of content available on the web can be overwhelming. Furthermore, it isn't easy to know where to start and the quality of materials available. What follows are some resources outside of HTB that we recommend to anyone starting on their journey or looking to enhance their skillset and pick up new tricks.

#### Vulnerable Machines/Applications

There are many resources available to practice common web and network vulnerabilities in a safe, controlled setting. The following are some examples of purposefully vulnerable web applications and vulnerable machines that we can set up in a lab environment for extra practice.

|||
|---|---|
|[OWASP Juice Shop](https://owasp.org/www-project-juice-shop/)|Is a modern vulnerable web application written in Node.js, Express, and Angular which showcases the entire [OWASP Top Ten](https://owasp.org/www-project-top-ten) along with many other real-world application security flaws.|
|[Metasploitable 2](https://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/)|Is a purposefully vulnerable Ubuntu Linux VM that can be used to practice enumeration, automated, and manual exploitation.|
|[Metasploitable 3](https://github.com/rapid7/metasploitable3)|Is a template for building a vulnerable Windows VM configured with a wide range of [vulnerabilities](https://github.com/rapid7/metasploitable3/wiki/Vulnerabilities).|
|[DVWA](https://github.com/digininja/DVWA)|This is a vulnerable PHP/MySQL web application showcasing many common web application vulnerabilities with varying degrees of difficulty.|

It is worth learning how to set these up in your lab environment to gain extra practice setting up VMs and working with common configurations such as setting up a web server. Aside from these vulnerable machines/applications, we can also set up many machines and applications in a lab environment to practice configuration, enumeration/exploitation, and remediation.

---

#### YouTube Channels

There are many YouTube channels out there that showcase penetration testing/hacking techniques. A few worth bookmarking are:

|||
|---|---|
|[IppSec](https://www.youtube.com/channel/UCa6eh7gCkpPo5XXUDfygQQA)|Provides an extremely in-depth walkthrough of every retired HTB box packed full of insight from his own experience, as well as videos on various techniques.|
|[VbScrub](https://www.youtube.com/channel/UCpoyhjwNIWZmsiKNKpsMAQQ)|Provides HTB videos as well as videos on techniques, primarily focusing on Active Directory exploitation.|
|[STÖK](https://www.youtube.com/channel/UCQN2DsjnYH60SFBIA6IkNwg)|Provides videos on various infosec related topics, mainly focusing on bug bounties and web application penetration testing.|
|[LiveOverflow](https://www.youtube.com/channel/UClcE-kVhqyiHCcjYwcpfj9w)|Provides videos on a wide variety of technical infosec topics.|

---

#### Blogs

There are too many blogs out there to list them all. If you do a Google search for a walkthrough of most any retired HTB box, you will usually come across the same blogs time and time again. These can be great for seeing another person's perspective on the same topic, especially if their posts contain "extra" information about the target that other blogs do not cover.  
One great blog worth checking out is [0xdf hacks stuff](https://0xdf.gitlab.io/). This blog has fantastic walkthroughs of most retired HTB boxes, each with a "Beyond Root" section covering some unique aspect of the box that the author noticed. The blog also has posts on various techniques, malware analysis, and write-ups from past CTF events.

At any point in the learning process, it is worth reading as much material as possible to understand a subject better and gain different perspectives. Aside from blogs related to retired HTB boxes, it is also worth seeking out blog write-ups on recent exploits/attacks, Active Directory exploitation techniques, CTF event write-ups, and bug bounty report write-ups. These can all contain a wealth of information that may help connect some dots in our learning or even teach us something new that can come in handy on an assessment.

---

#### Tutorial Websites

There are many tutorial websites out there for practicing fundamental IT skills, such as scripting.  
Two great tutorial websites are [Under The Wire](https://underthewire.tech/wargames) and [Over The Wire](https://overthewire.org/wargames/). These websites are set up to help train users on using both Windows `PowerShell` and the Linux command line, respectively, through various scenarios in a "war games" format.  
They take the user through various levels, consisting of tasks or challenges to training them on fundamental to advanced Windows and Linux command line usage and `Bash` and `PowerShell` scripting. These skills are paramount for anyone looking to succeed in this industry.

---

#### HTB Starting Point

[Starting Point](https://app.hackthebox.com/starting-point) is an introduction to HTB labs and basic machines/challenges. After completing a tutorial covering connecting to VPN, enumeration, gaining a foothold, and privilege escalation against a single target, we are presented with several easy-rated machines that can be attacked before accessing the rest of the HTB platform.

---

#### HTB Tracks

On the main HTB platform [Tracks](https://app.hackthebox.com/tracks), "selections of machines and challenges tied together for users to progress through, mastering a particular subject." Tracks cover a variety of topics and are continually being added to the platform. Their goal is to help students stay focused on a specific goal in a structured way while following an exploratory learning approach.

---

#### Beginner Friendly HTB Machines

There are many beginner-friendly machines on the main HTB platform. Some recommended ones are:

|[Lame](https://app.hackthebox.com/machines/1)|[Blue](https://app.hackthebox.com/machines/51)|[Nibbles](https://app.hackthebox.com/machines/121)|[Shocker](https://app.hackthebox.com/machines/108)|[Jerry](https://app.hackthebox.com/machines/144)|
|---|---|---|---|---|

If you prefer to watch a video walkthrough while working on an easy machine, the below playlists from IppSec's channel have a walkthroughs for various Linux/Windows easy HTB boxes:

|**<br><br>Easy Linux Boxes<br><br>**|**<br><br>Easy Windows Boxes<br><br>**|
|---|---|

 

---

#### Beginner Friendly HTB Challenges

The HTB platform contains one-off challenges in a variety of categories. Some beginner-friendly challenges include:

|[Find The Easy Pass](https://app.hackthebox.com/challenges/5)|[Weak RSA](https://app.hackthebox.com/challenges/6)|[You know 0xDiablos](https://app.hackthebox.com/challenges/106)|
|---|---|---|

---

#### Dante Prolab

The HTB platform has various Pro Labs that are simulated enterprise networks with many interconnected hosts that players can use to practice their skills in a network containing multiple targets.  
Successful exploitation of specific hosts will yield information that will help players when attacking hosts encountered later in the lab.

The [Dante Pro Lab](https://app.hackthebox.com/prolabs/overview/dante) is the most beginner-friendly lab offered to date. This lab is geared towards players with some experience performing network and web application attacks and an understanding of networking concepts and the basics of penetration methodologies such as scanning/enumeration, lateral movement, privilege escalation, post-exploitation, etc.