
### IP Address (Layer 3):

- Commands for IP address in terminal:
	- `windows: ipconfig`
	- `linux: ifconfig`

- IPv4 and IPV6 are two versions of the internet protocol and which is the underlying protocol that enables communication on the internet

- IPV4 address are 32 bit numerical addresses, such as *192.168.0.1*, each section or octet of the address consists of 8 bits, range from 0 to 255, which allows for 4.3 billion unique addresses.

- Each section of the IPV4 address can hold 8 bits such as *11111111.11111111.11111111.11111111*, if we calculate the each section with having 8 (1) bits, it will hold a max number of 255, as each bit goes as 2^0, 2^1 and so on.....

- IPV6 address are 128 bit hexa-decimal address such as "2001:0db8:85a3:0000:0000:8a2e:0370:7334" which are divided into 8 groups of 4 hexadecimal digits

- We have NAT (Network Address Translation) to help us communicate to the internet with a public IP address, but all the devices in the network gets a private IP address, which is classified as below:
	- Class A: Starts from *10.0.0.0* owned by business 
		- Network Mask: *255.0.0.0*
		- No of Networks: *126*
		- No of hosts per network: *16,646,146*
	- Class B: Starts from *172.16.0.0* to *172.31.0.0*
		- Network Mask: *255.255.0.0*
		- No of Networks: *16,383*
		- No of hosts per network: *65,024*
	- Class C: Starts from *192.168.0.0* to *192.168.255.255* suitable for household
		- Network Mask: *255.255.255.0*
		- No of Networks: *2,097,151*
		- No of hosts per network: *254*
	- Class D & E......


### MAC Address (Layer 2):

- A MAC(Media Access Control) address is a unique identifier assigned to network interface controllers (NICs) of network devices. It is a hardware/physical address that is permanently assigned by the manufacturer and is stored in the device's firmware or read-only-memory (ROM).

- MAC address are used at the data link layer of the OSI model to ensure data is delivered to the correct device within the local network

- MAC addresses are typically 48 bits in length and are expressed as a sequence of six pairs of hexadecimal digits separated by colons or hyphens. For example, a MAC address may look like "00:1A:2B:3C:4D:5E". 

- The first three pairs of digits identify the manufacturer of the network interface card, while the last three pairs provide a unique identifier for the specific device, we can search in google with the first three pairs of MAC address in *mac address lookup*, to show the type of device.

- Commands for MAC Address:
	- `Windows: getmac /v` or `ipconfig /all`
	- `Linux: ifconfig` (and we can find it, with the field *ethr*)


### TCP, UDP and the Three-Way Handshake:

- TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are two commonly used transport layer protocols in computer networks.

- TCP is a connection-oriented protocol that provides reliable, ordered, and error-checked delivery of data packets over an IP network. It guarantees that data sent from one device is received correctly by the destination device. TCP achieves this reliability through mechanisms like acknowledgement, retransmission, and flow control. It breaks data into smaller packets, assigns sequence numbers to them, and ensures they are reassembled correctly at the receiving end. TCP is widely used for applications that require guaranteed delivery, such as web browsing, email, file transfer, and remote login.

- UDP, on the other hand, is a connectionless protocol that does not provide the same level of reliability as TCP. It is simpler and more lightweight, making it suitable for applications that can tolerate some data loss or delay. UDP does not establish a connection or guarantee delivery of packets. It simply sends data packets from one device to another without waiting for acknowledgements or retransmissions. UDP is commonly used for real-time applications like streaming media, online gaming, DNS (Domain Name System), and VoIP (Voice over IP).

- The three-way handshake is a process used by TCP to establish a connection between two devices. It is a sequence of three steps that takes place before data transmission can begin. Here's how the three-way handshake works:

	1. **SYN (Synchronize):** The initiating device (often referred to as the client) sends a TCP packet with the SYN flag set to the destination device (often referred to as the server). This packet indicates the desire to establish a connection and includes an initial sequence number.
	2. **SYN-ACK (Synchronize-Acknowledge):** Upon receiving the SYN packet, the destination device responds with a TCP packet that has both the SYN and ACK (acknowledge) flags set. This packet acknowledges the receipt of the initial SYN packet and also includes its own initial sequence number.
	3. **ACK (Acknowledge):** Finally, the initiating device acknowledges the SYN-ACK packet by sending an ACK packet back to the destination. This packet confirms the establishment of the connection and typically contains an incremented sequence number.

	Once the three-way handshake is complete, the connection is established, and both devices are ready to exchange data. The sequence numbers exchanged during the handshake are used to ensure that data is transmitted and received in the correct order.


### Common Ports and Protocols:

Below are the commonly used ports and protocols associated:
- FTP (File Transfer Protocol): Port 21 (TCP)
- SSH (Secure Shell): Port 22 (TCP)
- Telnet: Port 23 (TCP)
- SMTP (Simple Mail Transfer Protocol): Port 25 (TCP)
- DNS (Domain Name System): Port 53 (TCP and UDP)
- HTTP (Hypertext Transfer Protocol): Port 80 (TCP)
- HTTPS (Hypertext Transfer Protocol Secure): Port 443 (TCP)
- DHCP (Dynamic Host Configuration Protocol): Port 67 (UDP) and Port 68 (UDP)
- POP3 (Post Office Protocol version 3): Port 110 (TCP)
- IMAP (Internet Message Access Protocol): Port 143 (TCP)
- SNMP (Simple Network Management Protocol): Port 161 (UDP)
- RDP (Remote Desktop Protocol): Port 3389 (TCP)
- NTP (Network Time Protocol): Port 123 (UDP)
- SMB (Server Message Block): Port 445 (TCP)
- FTPS (FTP over SSL/TLS): Port 990 (TCP)
- TFTP (Trivial File Transfer Protocol): Port 69 (UDP)
- LDAP (Lightweight Directory Access Protocol): Port 389 (TCP and UDP)
- MySQL: Port 3306 (TCP)

Test