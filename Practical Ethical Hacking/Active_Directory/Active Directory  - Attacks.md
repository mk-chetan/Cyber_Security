
#### LLMNR (Link Local Multicast Name Resolution) Poisoning:

- Used to identify hosts when DNS fails to do so.
- Previously known as NBT-NS
- Key flaw is that the services utilize a user's username and NTLMv2 hash when appropriately responded to
- Overview of LLMNR attack:
	- The victim/target machine is asking the server to connect the victim to \\hackm, and also server doesn't have the idea of the path, responds back with no details
	- The victim/target then broadcasts/multicast to everyone asking to help with the connection of \\hackm.
	- Hacker can intercept this request and responds to victim requesting the hash to connect to \\hackm.
	- As victim machine provides the hash, hacker can try to hack the hash and use it for unethical use cases.
- Process for the LLMNR poisoning:
	- Step 1: Run Responder
		- syntax: `sudo responder -I eth0 -dwPv`
			- -I is interface, if its a VPN, it will be tun0 (Tunnel0)
				- if its a network, it will be eth0
			- -d this is for DHCP
			- -w this is for wpad (Start the WPAD rogue proxy server.)
			- -P this is for ProxyAuth (Froce NTLM authentication for the proxy)
			- v - Verbosity
			- Both w and P cannot be used at the same.
		- Syntax: `sudo responder --help` this can be used for more details about the responder.
	- Step 2: Wait for an event to occur, where the victim tries to connect to a shared path
	- Step 3: Responder picks the Hash from victim 
	- Step 4: Crack the hashes with hashcat #HASHCat if its weak.
	
#### LLMNR Poisoning Mitigation:

