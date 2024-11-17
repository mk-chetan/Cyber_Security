

**Link to Extra Notes:**
[[Web Application Enumeration]]


#### Identify Website Technologies:

- **builtwith** website helps to identify the technologies being used by a website
	- https://builtwith.com
- **wappalyzer** is a extension, which does the same as builtwith for identifying all the technologies used in a website
- **whatweb** is a tool built in kali Linux system to identify the website technologies as others:
	- Command:
		- whatweb --aggression 1 https://google.com
- `curl -I -L https://example.com` - this can help to find the details about the website and the technologies (-L is used for redirect)
-  https://securityheaders.com/ (This website helps to security rating of a website based on the security headers being utilized by the website )


#### Directory Busting / Directory Enumeration & Brute Forcing:

- NMAP:
	- Network mapping (NMAP) is used to discover open ports.
	- syntax:
		- `nmap -T4 -p- -A {ip-address}`
			- -T4 is for the speed (1 is slow, and 5 is fast)
			- -p- (Scan all ports)
			- -A (All the details)
		- `nmap --help`
- Directory busting, also known as directory brute forcing or directory enumeration, is a technique used in ethical hacking to find hidden or unprotected directories and files on a web server or application
	- **Tools:**
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
				- -u - provide the ip-address or website url/FUZZ (placeholder for the wordlists to attack)
				- -recursion -  can be used to scan recursively and we can set the recursion depth using the -recursion-depth tag
- Based on the technology utilized in the web application, we can search the files with that extensions while directory busting like .html files, .php and .aspx for Microsoft ISS and etc.


#### Sub domain hunting:

- We can use google like `site:google.com`, this will extract all the sites along with the sub domains
	- `site:google.com -www -dev,` this will ignore the subdomains provided
	- `site:google.com filetype:pdf` this will find any pdf entries in the website
- **sublist3r** is a tool/app can be used from the terminal for finding subdomains
	- To install:
		- `apt install sublist3r`
	- To use:
		- `sublist3r -h`
		- `sublist3r -d telsa.com`
- https://crt.sh - This website can be used to search subdomains using certificate fingerprint
	- Example:
		- %.tesla.com
- **subfinder** is a tool in kali used to find the sub domains of a webpage:
	- To install:
		- `apt install subfinder`
	- To use:
		- `subfinder -d google.com`
- **assetfinder** is a tool useful to find the sub domains and in the mean while, check which domains are up and running
	- https://github.com/tomnomnom/assetfinder
	- Install the tool using the instructions provided in the github.
		- To use:
			- `assetfinder example.com | grep example.conm | sort -u > example.txt`
- **owasp amass** is a github project, that can be installed in kali, which works similar to sublist3r for subdomain hunting
	- Website:
		- https://github.com/owasp-amass/amass
	- To use:
		- `amass enum -d example.com > example.txt`
- **tomnomnom httprobe** - this github tool helps with the probing of domains, to check if the domains are up and running, this can be useful with sublist3r or owasp amass to find the active sub domains.
	- Website:
		- https://github.com/tomnomnom/httprobe
	- To use:
		- Once we have all the subdomains using the sublis3r, assetfinder, amass and other tools, grab all the data and filter https entries and provide it to the httprobe to filter out the live subdomains:
		- `cat exampletfinal.txt | grep example.com | sort -u | httprobe -prefer-https | grep https > examplelive.txt`


