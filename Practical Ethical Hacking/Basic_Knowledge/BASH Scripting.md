
Below are the scripts and commands which i found helpful:
### IP Sweep script:

`#!/bin/bash if [ "$1" == "" ]` 

`then echo "You forgot an IP address!"` 

`echo "Syntax: ./ipsweep.sh 192.168.1"` 

`else` 

`for ip in ``seq 1 254``;` 

`do ping -c 1 $1.$ip | grep "64 bytes" | cut -d " " -f 4 | tr -d ":" &` 

`done` 

`fi`

- Command to have the nmap use the ip address captured from the ip sweep script:
	- `for ip in ``(cat ips_available.txt)``; do echo $ip; nmap $ip; done`


