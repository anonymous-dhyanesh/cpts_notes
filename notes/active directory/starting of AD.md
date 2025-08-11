
* #### so after entering into network we need to check which hosts are live ...there are many methods to do this:

	1.we can use wireshark or tcp dump to analyze the traffic and indentify hosts from there(arp,icmp etc)

	2.using [RESPONDER](responder) tool..running responder in analyze mode so that it dont change any packets..(-A).

	3.Fping is a tool do scan live hosts via ping req...commands [here](fping)
	after making list of active hosts , we can pass whole list to [nmap](nmap)..i will scan every host in the list


- #### now our goal should to find system lvl access..it opens up many oppourtunities.
	- checking for valid username using [kerbrute](kerbrute). pre built usernames can be find [here](https://github.com/insidetrust/statistically-likely-usernames)
	- if we dont have password then we also try [LLMNR](obsidian://open?vault=cpts&file=notes%2Ftools%20and%20services%2FLLMNR%20poisoning) poisoning.(crack or relay more info in LLMNR notes)
	- 