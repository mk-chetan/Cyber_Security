
## Websites/Tools Used & Useful:

- bugcrowd.com

#### Discovering Email Addresses:

- hunter.io
- phonebook.cz
- voilanorbert.com
- Clearbit connect (Chrome extension)
- tools.verifyemailaddress.io (This is used to verify the email address)
- email-checker.net/validate (This is also used to verify the email address)
- hashes.org ( Search for Hashes )

#### Hunting Subdomains:

- **sublist3r** is a tool/app can be used from the terminal for finding subdomains
	- To install:
		- `apt install sublist3r`
	- To use:
		- `sublist3r -h`
		- `sublist3r -d telsa.com`
- **https://crt.sh** - This website can be used to search subdomains using certificate fingerprint
	- Example:
		- %.tesla.com
- **owasp amass** is a github project, that can be installed in kali, which works similar to sublist3r for subdomain hunting
	- Website:
		- https://github.com/owasp-amass/amass
- **tomnomnom httprobe** - this github tool helps with the probing of domains, to check if the domains are up and running, this can be useful with sublist3r or owasp amass to find the active sub domains.
	- Website:
		- https://github.com/tomnomnom/httprobe

#### Identify Website Technologies:

- **builtwith** website helps to identify the technologies being used by a website
	- Website:
		- https://builtwith.com
- **wappalyzer** is a extension, which does the same as builtwith for identifying all the technologies used in a website
- **whatweb** is a tool built in kali linux system to identify the website technologies as others:
	- Command:
		- whatweb --aggression 1 https://google.com




