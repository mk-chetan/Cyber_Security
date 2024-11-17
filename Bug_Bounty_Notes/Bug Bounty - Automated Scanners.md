Automated scanners for bug bounty:

**Free Tier:**

- *OWASP ZAP (Zed Attack Proxy):*  To find security vulnerabilities in web applications during development and testing phases
- *Arachni:* A modular Ruby framework aimed towards helping penetration testers and administrators evaluate the security of web applications.
- *Wapiti:* Allows you to audit the security of your web applications. It performs " black-box " scans. i.e., it does not study the source code of the application but will scan the web pages of the deployed application
- *Vega:* Another free and open-source web security scanner and web security testing platform to test the security of web applications
- *SQLMap:* An open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws
- *Skipfish:* An active web application security reconnaissance tool. It prepares an interactive sitemap for the targeted site by carrying out a recursive crawl and dictionary-based probes.

**Commercial:**

- *Burp Suite:* A tool for web security testing, Burp Suite provides a variety of features of web application security checks. The community edition is free but limited, while the professional version is commercial.
- *Nessus*: While not strictly a web application scanner, Nessus does provide a range of scans that include detecting vulnerabilities in web applications
- *Acunetix:* A fully automated web vulnerability scanner that detects and reports on over 4500 web application vulnerabilities, including all variations of SQL Injection and XSS.
- *AppScan:* Developed by HCL, previously IBM, it provides dynamic application security testing.
- *Veracode*: Provides multiple scanning technologies (e.g., SAST, DAST, SCA) on a single platform, aiming to offer a comprehensive web application security solution
- *Netsparker:* A scalable, multi-user web application security solution with built-in workflow and reporting tools ideal for security teams
- *Qualys Web Application Scanning (WAS):* Provides automated crawling and testing of custom web applications to identify vulnerabilities.

## Automating the Enumeration Process:

Resources:
- sumrecon: [https://github.com/thatonetester/sumrecon](https://github.com/thatonetester/sumrecon)
- TCM's Modified full script for the sub domain hunting: [https://pastebin.com/MhE6zXVt](https://pastebin.com/MhE6zXVt)

Bash Script to find the subdomains using AssetFinder and amass easily and filter only sub domains:

		`#!/bin/bash
		 url=$1
		 if [ ! -d "$url" ];then
			`mkdir $url
		 fi
		 if [ ! -d "$url/recon" ]; then
			`mkdir $url/recon
		 fi
		 echo "[+] Retrieving subdomains with assetfinder....."
		 assetfinder $url >> $url/recon/result.txt
		 cat $url/recon/result.txt | grep $1 >> $url/recon/final.txt
		 rm $url/recon/result.txt
		
		 #echo "[+] Retrieving subdomains with amass....."
		 #amass enum -d $url >> $url/recon/f.txt
		 #sort -u $url/recon/f.txt >> $url/recon/final.txt
		 #rm $url/recon/f.txt
	
		 echo "[+] Probing for alive subdomains with httprobe....."
		 cat $url/recon/final.txt | sort -u | httprobe -s -p https:443 | sed     s/https\?:\/\///' | tr -d ':443' >> $url/recon/alive.txt'
