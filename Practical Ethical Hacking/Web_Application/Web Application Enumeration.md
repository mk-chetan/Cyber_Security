
Web Application related notes with tools:

- [[Ethical Hacking Methodology (Information Gathering - Reconnaissance)]]
- [[Ethical Hacking Methodology (Scanning & Enumeration)]]

#### Finding Subdomains with AssetFinder:

- AssetFinder, doesn't come preinstalled, assetFinder requires Go lang, so make sure we have golang installed in the machine or we can use pimpmykali tool to reinstall  or install Go.
	- Github for AssetFinder: https://github.com/tomnomnom/assetfinder
- We can use the command: `go get -u github.com/tomnomnom/assetfinder` and install assetfinder. If we face any issue while installing like 'go.mod file not found in current directory or any parent directory', run the command `sudo go mod init assetfinder`, before go get the assetfinder.
- Usage:
	- `assetfinder --subs-only <domain>`
	- `assetfinder <domain>`
- 
#### Finding Subdomains with Amass:

- Amass is similar tool as AssetFinder, which can be used for sub domain hunting, but its a heavy application. Amass also doesn't come preinstalled, if we have go installed in the machine, we can use the below command to install Amass.
	- `go install -v github.com/owasp-amass/amass/v4/...@master`
- Github page:
	- https://github.com/owasp-amass/amass
- Usage:
	- `amass enum -d tesla.com`


#### Finding Alive domains with Httprobe:

- Httprobe again doesn't come built in with kali, we are using the tomnomnom's github to install httprobe, this is mainly used to find the alive domains from the sub domains we obtained from the assetfinder and amass
	- Github page
		- https://github.com/tomnomnom/httprobe
	- Install:
		- `go install github.com/tomnomnom/httprobe@latest`
	- Usage:
		- we can cat the subdomains file we obtained with assetfinder or amass and feed it to the httprobe
		- `cat subdomains.txt | httprobe`
		- `cat subdomains.txt | httprobe -s -p https:443` {This is specifically search for https and port 443}

#### Screenshotting Websites with GoWitness:

- GoWitness is a tool, which can be used to capture the screenshot of the homepage of the web url, provided which can be helpful to automate the  process to check the alive subdomains and take screenshots of the webpage.
	- Github page:
		- [https://github.com/sensepost/gowitness](https://github.com/sensepost/gowitness)
	- Before Install:
		- `go get -u gorm.io/gorm`
			- To run the go get, run the `sudo go mod init assetfinder` first and use the go.mod file path to install the `go get -u gorm.io/gorm`
	- Install:
		- `go install github.com/sensepost/gowitness@latest`
	- Usage:
		- `gowitness single https://tesla.com`
		- After running the command, a screenshot folder will be created in the path and we can see the screenshot inside that folder.
		  
#### Automating the Enumeration Process ( #Resources):

Resources:
- sumrecon: [https://github.com/thatonetester/sumrecon](https://github.com/thatonetester/sumrecon)
- TCM's Modified full script for the sub domain hunting: [https://pastebin.com/MhE6zXVt](https://pastebin.com/MhE6zXVt)

Bash Script to find the subdomains using AssetFinder and amass easily and filter only sub domains:

		`#!/bin/bash`
		`url=$1`
		`if [ ! -d "$url" ];then`
			`mkdir $url`
		`fi`
		`if [ ! -d "$url/recon" ]; then`
			`mkdir $url/recon`
		`fi`
		`echo "[+] Retrieving subdomains with assetfinder....."`
		`assetfinder $url >> $url/recon/result.txt`
		`cat $url/recon/result.txt | grep $1 >> $url/recon/final.txt`
		`rm $url/recon/result.txt`
		
		`#echo "[+] Retrieving subdomains with amass....."`
		`#amass enum -d $url >> $url/recon/f.txt`
		`#sort -u $url/recon/f.txt >> $url/recon/final.txt`
		`#rm $url/recon/f.txt`
		
		`echo "[+] Probing for alive subdomains with httprobe....."`
		`cat $url/recon/final.txt | sort -u | httprobe -s -p https:443 | sed 's/https\?:\/\///' | tr -d ':443' >> $url/recon/alive.txt`



#### Additional Resources ( #Resources):

- The Bug Hunter's Methodology - [https://www.youtube.com/watch?v=uKWu6yhnhbQ](https://www.youtube.com/watch?v=uKWu6yhnhbQ)
- Nahamsec Recon Playlist - [https://www.youtube.com/watch?v=MIujSpuDtFY&list=PLKAaMVNxvLmAkqBkzFaOxqs3L66z2n8LA](https://www.youtube.com/watch?v=MIujSpuDtFY&list=PLKAaMVNxvLmAkqBkzFaOxqs3L66z2n8LA)