
## Tools and Details:

#### 1) NMAP:

- Network mapping (NMAP) is used to discover hosts and services on a computer network by sending packets and analyzing the responses.
	- syntax:
		- `nmap -T4 -p- -A {ip-address}`
			- -T4 is for the speed (1 is slow, and 5 is fast)
			- -p- (Scan all ports)
			- -A (All the details)
		- `nmap --help`
#### 2) Net discover:

- Network discovery in Kali Linux is the process of identifying and mapping devices on a network, which can help with security and troubleshooting: 
	- Netdiscover- A tool built into Kali Linux that can be used to scan wireless and switched networks. It can passively detect hosts, or actively search for them by sending ARP requests. You can use Netdiscover to scan a range of IP addresses, do a passive scan, or inspect network ARP traffic
#### 3) Directory busting:\

- Directory busting, also known as directory brute forcing or directory enumeration, is a technique used in ethical hacking to find hidden or unprotected directories and files on a web server or application
	- Tools:
		- dirbuster
		- dirb
		- gobuster
		- fuff
			- We need to install fuff into kali linux with `apt install fuff` 
			- This is helpful to find one directory deep 
			- Syntax:
				- `fuff -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt:FUZZ -u http://{ip-address}/FUZZ`
				- -w is the wordlists path, we are using dirbusters medium wordlists file
				- :FUZZ at the end of the wordlists, mentions to use the wordlists at the FUZZ placeholder
				- -u - provide the ip-address/FUZZ (placeholder for the wordlists to attack)

#### 4) Nikto (Tool - Web Vulnerability Scanner):

- Nikto is a free, open-source tool that scans web servers for vulnerabilities. It can identify a variety of issues, including: Outdated software versions, Dangerous files and programs, Server configuration errors, Potential vulnerabilities, and Cookies received.
	- Syntax:
		- `nikto -h {ip-address}`
#### 5) Burp Suite (Enumeration and web proxy):

- Burp Suite is a software tool that helps identify vulnerabilities in web applications through security audits and penetration tests. It's a collection of tools that can be used together, and it's often used by professional pentesters, ethical hackers, and security researchers.
#### 6) Metasploit:

- Metasploit is a built in tool in Kali linux, which helps with the exploits, Auxiliary (Enumeration/gathering information), Payloads and etc
- Helpful Syntaxs:
	- `msfconsole` -  to open/start the metasploit console
	- `search smb_version` - This command can be used inside the msfconsole to find a module quickly
	- `use auxiliary/scanner/smb/smb_version` - Use this command to find the syntax to use the module, to found in the search results
		- `info` - can be used to provide more info of the module, we just used and info about the options as well
		- `options` - we can also use options check the setting of the module set
			- RHOSTS - this is the host/ip address
			- RPORTS & THREADS
		- `set RHOSTS {ip_address}` - This can be used to modify the options and then run the module
		- `run` - After setting the options using set for RHOSTS and other required options, provide run and metasploit, starts the scan for the ip address and tries to find the SMB version

#### 7)  smbclient:

-  This tool can be used to connect to a smb (File transfer), anonymously if we have access, it will output share-names present in the ip-address and we can use the second syntax to connect to each
	- Syntax:
		- `smbclient -L \\\\{ip-address}\\`
			- -L is used to list out the files
		- `smbclient \\\\{ip-address}\\ADMIN$`
		- `smbclient \\\\{ip-address}\\IPC$`

#### 8) SSH (Secure Shell):

- SSH stands for Secure Shell or Secure Socket Shell. It's a network protocol that allows two computers to communicate and share data securely, even over an unsecured network
	- Syntax:
		- `SSH {ip-address}`
		- If we observe an error for the "no matching key exchange method found"
			- We can add `SSH {ip-address} -oKexAlgorithms=+diffie-hellman-group1-sha1 -c aes128-cbc`
			- oKexAlgorithms is for the encrypion algorithm, we can choose from the they offer output of the 'no matching key exchange method found'
			- -c is for cipher, this can also be picked from they offer cipher error

#### 9) Searchsploit:

-  This is just a handy tool for searching exploits inside the kali machine, this tool search the database of exploits, that is stored locally
	- Syntax:
		- `searchsploit samba 2`
		- `searchsploit mod ssl 2`

#### 10) Nessus:

- Nessus is a vulnerability scanning tool that helps identify security weaknesses in networks and systems. It's used to check devices, applications, operating systems, and cloud services for vulnerabilities that could be exploited by hackers.
	- Syntax:
		- Download the nessus debain installation file from the website for nessus
			- `sudo dpkg -i Nessus-verison.deb`
				- `dpkg` - De-package and `-i` - Install
			- Once the install is completed, we can start nessus with below syntax:
				- `systemctl start nessusd`
			- And to access the nesses use the below url
				- `https://kali:8834`
- Once the installation is completed and the plugins are installed, we can start a new scan for a network/vulnerability scan and provide the required details, such as target-ip address and the ports to scan and the type of web vulnerabilities to scan.
- Once the scan is completed. we can use the report to view the critical and other vulnerabilities present in the provided server/ip-address of the server.

#### 11) Hash identifier:

- Hash identifier is a tool, built in kali systems. This can be used to identify a possible HASH.
	- Syntax:
		- `hash-identifier`
			- Once it loads up, provide the HASH to determine what type of HASH it is.

#### 12) Hash Cat:

- Hash cat is also a tool which is built in kali, and it is used to crack the Hashes.
	- Syntax:
		- `hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt`
			- -m  is module and we are using module 0, as its correlates to MD5
			- hashes.txt is the file containing the hashes we want to crack
			- /usr..../rockyou.txt - this is the wordlist provided to crack the MD5 Hash
	- You can google, hashcat MD5 crack and you can find the module to use and the wordlists to use for MD5 and other HASH types as required.
	- Note: HashCat uses CPU to crack passwords

#### 13) Hash Cracker:

- Hash Cracker is similar to the Hash Cat, but it runs with GPU, so make sure run the Hash Cracker in the local machine and not the virtual machine.


