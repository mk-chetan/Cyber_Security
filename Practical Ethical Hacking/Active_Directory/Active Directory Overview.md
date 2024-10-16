
#### What is Active Directory:

- Directory service developed by Microsoft to manage Windows domain networks
- Stores information related to objects, such as computers, users, printers and etc
- Authenticates using kerberos tickets.
	- Non-Windows devices, such as Linux machines, firewalls, etc, can also authenticate to Active Directory via RADIUS or LDAP
- Active Directory is the most commonly used identity management service in the world
- Can be exploited without ever attacking patchable exploits.
	- Instead, we abuse features, trusts, components and more.

#### Active Directory Components:

- Active Directory is composed of both physical and logical components:
	- Physical Components:
		- Data store
		- Domain controllers
		- Global catalog server
		- Read-Only Domain Controller (RODC)
	- Logical Components:
		- Partitions
		- Schema
		- Domains
		- Domain trees
		- Forests
		- Sites
		- Organization Units (OUs)

#### Physical Active Directory Components:

- Below are the important physical components of the Active Directory:
	- **Domain Controller:**
		- A domain controller is a server with the AD DS server role installed that has specifically been promoted to a domain controller
		- Useful for:
			- Host a copy of  the AD DS directory store
			- Provide authentication and authorization services
			- Replicate updates to other domain controllers in the domain and forest
			- Allow administrative access to manage user accounts and network resources.
	- **AD DS Data Store:**
		- The AD DS data store contains the database files and processes that store and manage directory information for users, services and applications.
		- Details:
			- Consists of the Ntds.dit file
			- Is stored by default in the %SystemRoot%\NTDS folder on all domain controllers
			- Is accessible only through the domain controller processes and protocols.

#### Logical Active Directory Components:

- Below are the important logical components of the Active Directory:
	- **AD DS Schema:**
		- Defines every type of object that can be stored in the directory
		- Enforces rules regarding object creation and configuration.
		- Object Types:
			- Class Object:
				- Function: What objects can be created in the directory
				- Example: User, Computer
			- Attribute Object:
				- Function: Information that can be attached to an object.
				- Example: Display name
	- **Domains**:
		- Domains are used to group and manage objects in an organization.
			- Ex: local.com/  domain.local etc
			- Details:
				- An administrative boundary for applying policies to groups of objects
				- A replication boundary for replicating data between domain controllers
				- An authentication and authorization boundary that provides a way to limit the scope of access to resources.
	- **Trees**:
		- A domain tree is a hierarchy of domains in AD DS/ similar to domain and sub-domains.
			- Ex: sub.local.com / sub.domain.local etc
			- All domains in the tree:
				- Share a contiguous namespace with the parent domain
				- Can have additional child domains
				- By default create a two-way transitive trust with other domains
	- **Forests**:
		- A forest is a collection of one or more domain trees.
		- Details:
			- Share a common schema
			- Share a common configuration partition
			- Share a common global catalog to enable searching
			- Enable trusts between all domains in the forest
			- Share the enterprise admins and schema admins groups.
	- **Organizational Units (OUs)**:
		- OUs are Active Directory containers that can contain users, groups, computers and other OUs
			- Used to:
				- Represent the organization hierarchically and logically
				- Manage a collection of objects in a consistent way
				- Delegate permissions to administer groups of objects
				- Apply policies
	- **Trusts**:
		- Trusts provide a mechanism for users to gain access to resources in another domain.
			- Types of Trusts:
				- Directional:
					- The trust direction flows from trusting domain to the trusted domain
				- Transitive:
					- The trust relationship is extended beyond a two-domain trust to include other trusted domains
		- All domains in a forest trust all other domains in the forest
		- Trusts can extend outside the forest
	- **Objects**:
		- Objects live within an organization unit (OU) and they contain a lot of different things and can be lot of different things
			- Example:
				- User:
					- Enables network resource access for a user
				- InetOrgPerson:
					- Similar to a user account
					- Used for compatibility with other directory services
				- Contacts:
					- Used primarily to assign e-mail addresses to external users
					- Does not enable network access
				- Groups:
					- Used to simplify the administration of access control
				- Computers:
					- Enables authentication and auditing of computer access to resources.
				- Printers:
					- Used to simplify the process of locating and connecting to printers
				- Shared folders:
					- Enables users to search for shared folders based on properties.

#### Kerberos vs NTLM ( #AD_Important_terms):

Kerberos and NTLM are both authentication protocols used to verify the identity of users and services in network environments, particularly in Microsoft Windows networks.
##### Kerberos

1. **Overview**: Kerberos is a network authentication protocol designed to provide secure authentication over non-secure networks. It uses secret-key cryptography to provide strong authentication for client-server applications.
    
2. **How It Works**:
    
    - **Ticket Granting Ticket (TGT)**: When a user logs in, they receive a TGT from the Key Distribution Center (KDC).
    - **Service Tickets**: When accessing services, the user requests a service ticket from the KDC using the TGT.
    - **Mutual Authentication**: Both the user and the service verify each other's identities using the tickets, ensuring secure communication.
3. **Security Features**:
    
    - Protects against eavesdropping and replay attacks.
    - Supports mutual authentication.

##### NTLM (NT LAN Manager)

1. **Overview**: NTLM is an older authentication protocol that is used primarily in Windows environments for legacy systems and applications. It is based on challenge-response mechanisms.
    
2. **How It Works**:
    
    - **Challenge-Response**: When a user tries to authenticate, the server sends a challenge (a random number). The client then encrypts this challenge using the user's password and sends the response back.
    - The server performs the same encryption and checks if the responses match.
3. **Security Features**:
    
    - Less secure than Kerberos, as it is susceptible to pass-the-hash attacks and other vulnerabilities.
    - Lacks mutual authentication; only the client proves its identity.

##### Key Differences

- **Security**: Kerberos is generally considered more secure than NTLM.
- **Mutual Authentication**: Kerberos supports mutual authentication, while NTLM does not.
- **Use Cases**: Kerberos is used in environments that require strong security, while NTLM may still be used for compatibility with older systems.

Overall, Kerberos is the preferred method in modern Windows environments, while NTLM is being phased out for new applications due to its vulnerabilities.



#### What is SMB ( #AD_Important_terms ):

**SMB (Server Message Block)** is a network protocol primarily used for sharing files, printers, and other resources between nodes on a network. It allows applications on one computer to read and write to files on a server and to request services from server programs.

##### Key Features of SMB:

1. **File Sharing**: SMB enables users to share files and directories over a network. It provides mechanisms for file access and file locking.
    
2. **Printer Sharing**: It allows multiple clients to share printers connected to a server, facilitating print jobs over the network.
    
3. **Inter-process Communication**: SMB can be used for communication between processes on different machines, supporting features like named pipes.
    
4. **Authentication and Security**: SMB supports various authentication methods, including NTLM and Kerberos, to ensure secure access to shared resources.
    
5. **Versions**: SMB has undergone several iterations, with SMB 1.0 being the original version. Later versions, such as SMB 2.0 and SMB 3.0, introduced improvements in performance, security, and functionality. SMB 3.0, for example, added features like encryption and improved handling of large files.
    

- ##### How SMB Works:

	- **Client-Server Model**: SMB operates on a client-server model, where the client sends requests to the server for file or resource access.
	- **Transport Protocol**: SMB can operate over TCP/IP (port 445) and also historically over NetBIOS over TCP/IP (port 137-139).
	- **Message Structure**: SMB messages are structured in a way that allows clients to request various services, such as file reads, writes, and directory listings.

- ##### Use Cases:

	- **Windows Networking**: SMB is widely used in Windows environments for file and printer sharing.
	- **Cross-platform Support**: SMB is also supported on various operating systems, including Linux and macOS, allowing cross-platform file sharing.

- ##### Security Considerations:

	- Older versions of SMB (especially SMB 1.0) have known vulnerabilities and are considered insecure. It's recommended to use the latest versions (SMB 2.0 or SMB 3.0) for improved security features, including encryption and better authentication mechanisms.

	In summary, SMB is a crucial protocol for networked environments, facilitating resource sharing and communication across various systems.



#### LSASS (Local Security Authority Subsystem service) ( #AD_Important_terms):

The **Local Security Authority Subsystem Service (LSASS)** manages security policies on a Windows computer, including user authentication. The **Security Account Manager (SAM)** is a database file that stores user account information and security descriptors for users on a Windows machine. Here’s a closer look at what the Local SAM entails:

- #### What is the Local SAM?

1. **Storage of User Accounts**: The Local SAM holds information about local user accounts, including usernames, passwords (hashed), and group memberships. It's primarily used for systems not connected to a domain, where user accounts are stored locally rather than on a centralized server.
    
2. **Authentication**: When a user attempts to log in, Windows checks the credentials against the Local SAM. If the provided credentials match the stored hash, access is granted.
    
3. **Location**: The SAM database is located in the Windows directory, typically at `C:\Windows\System32\config\SAM`. It's a protected file, and only the operating system and specific system processes can access it directly.
    
4. **Security**:
    - The passwords stored in the SAM are not kept in plain text; instead, they are hashed to enhance security. This means that even if someone gains access to the SAM, they cannot easily retrieve user passwords.
    - Windows uses a variety of security measures to protect the SAM, including encryption and access controls.
5. **Limitations**: The Local SAM is used primarily in standalone systems or workgroups. In a domain environment, user accounts are typically managed by a domain controller, which maintains its own SAM.

- ##### Use Cases:

	- **Local Accounts**: When a computer is not joined to a domain, users are managed through the Local SAM.
	- **Administrator Access**: The Local SAM allows for the management of local administrators and other user roles, crucial for system maintenance.
- ##### Summary:

	The Local SAM is an essential component of Windows security, providing a mechanism for user account management and authentication on standalone machines. Its structure and protective measures play a critical role in maintaining the integrity and security of local user data.


#### LDAP ( #AD_Important_terms ):

**LDAP (Lightweight Directory Access Protocol)** is a protocol used to access and manage directory information over a network. It is widely used for storing and retrieving user and group information, typically in environments that require centralized management, such as corporate networks.

##### Key Features of LDAP:

1. **Directory Services**: LDAP is designed to work with directory services, which store information about resources such as users, computers, and services in a hierarchical format.
    
2. **Hierarchical Structure**: LDAP organizes data in a tree-like structure, with entries represented as nodes. Each entry has a unique identifier called a **Distinguished Name (DN)**, which reflects its position in the hierarchy.
    
3. **Attributes**: Each entry in an LDAP directory consists of attributes, which hold specific pieces of information. For example, a user entry might include attributes like `cn` (common name), `mail` (email address), and `uid` (user ID).
    
4. **Protocols**: LDAP operates over standard protocols, primarily TCP/IP, and uses port 389 for unencrypted communication and port 636 for secure communications (LDAPS).
    
5. **Authentication and Access Control**: LDAP supports various authentication mechanisms and allows for fine-grained access control, enabling administrators to manage who can read or modify directory entries.
    
##### Common Use Cases:

1. **User Authentication**: Many organizations use LDAP for centralized user authentication, allowing users to log in to multiple services with a single set of credentials.
    
2. **Directory Services**: LDAP is often used in directory services like Microsoft Active Directory, OpenLDAP, and others to manage user accounts and resources.
    
3. **Group Management**: It helps manage groups and permissions, making it easier to control access to resources across the network.
    
4. **Contact Information**: LDAP can also be used to store and retrieve contact information for users and organizations.
    
##### Benefits:

- **Centralization**: LDAP allows for centralized management of user accounts and resources, simplifying administration.
- **Interoperability**: Being an open standard, LDAP is supported by many applications and systems, enabling integration across diverse environments.
- **Scalability**: It can handle large amounts of data efficiently, making it suitable for organizations of all sizes.

##### Summary:

LDAP is a powerful and flexible protocol used for accessing and managing directory information in a network. Its hierarchical structure and support for various authentication methods make it an essential tool for user management, especially in enterprise environments.


#### What is DHCP ( #AD_Important_terms):

**DHCP (Dynamic Host Configuration Protocol)** is a network management protocol used to automate the assignment of IP addresses and other network configuration parameters to devices on a network. This process allows devices to communicate on an IP network without needing manual configuration.

##### Key Features of DHCP:

1. **Dynamic IP Address Assignment**:
    
    - DHCP automatically assigns IP addresses to devices (clients) when they join a network. This eliminates the need for network administrators to manually configure IP settings.
2. **Configuration Parameters**:
    
    - In addition to IP addresses, DHCP can provide clients with other essential information, such as subnet masks, default gateways, DNS server addresses, and more.
3. **Lease Duration**:
    
    - IP addresses are typically assigned for a limited time, known as a "lease." When the lease expires, the device must request a new IP address. This helps manage IP address availability on the network.
4. **DHCP Server**:
    
    - The DHCP server is responsible for managing IP address assignments and leases. It maintains a pool of available IP addresses and allocates them to clients as they request them.
5. **Broadcast Communication**:
    
    - When a device connects to a network, it sends a broadcast message to discover available DHCP servers. The server responds with an offer that includes an available IP address and configuration settings.

- ##### How DHCP Works:

1. **DHCP Discover**: The client broadcasts a request to find available DHCP servers.
2. **DHCP Offer**: DHCP servers respond with an offer containing an available IP address and configuration settings.
3. **DHCP Request**: The client selects one of the offers and sends a request to the chosen DHCP server to accept the offered parameters.
4. **DHCP Acknowledgment**: The server acknowledges the request, confirming the IP address assignment and other settings.

- ##### Benefits of DHCP:

	- **Ease of Management**: Automates IP address allocation, reducing the administrative burden on network managers.
	- **Efficiency**: Reduces the likelihood of IP address conflicts and ensures efficient use of available IP addresses.
	- **Flexibility**: Supports a wide range of network configurations and can easily accommodate new devices joining the network.

- ##### Use Cases:

	- **Enterprise Networks**: Commonly used in corporate environments to manage large numbers of devices.
	- **Home Networks**: Used by home routers to assign IP addresses to devices like smartphones, tablets, and computers.

- ##### Summary:

	DHCP is a crucial protocol for managing IP address assignments in both small and large networks. It simplifies network management, enhances efficiency, and helps ensure seamless connectivity for devices as they join the network.


#### What is WPAD ( #AD_Important_terms ):

**WPAD (Web Proxy Auto-Discovery Protocol)** is a protocol used by web browsers and other applications to automatically detect and configure proxy settings. This simplifies the process of setting up internet connections for users, especially in enterprise environments where proxy servers are commonly used.

- ##### Key Features of WPAD:

	1. **Automatic Configuration**: WPAD allows devices to automatically discover proxy settings without requiring manual configuration. This is especially useful in environments with multiple proxy servers or changing network configurations.
    
	2. **Configuration File**: WPAD uses a configuration file, typically a **PAC (Proxy Auto-Configuration)** file, which contains rules that define how requests should be routed. The file specifies which proxy server to use for different types of traffic.
    
	3. **Discovery Methods**:
    
	    - **DHCP**: WPAD can be discovered using DHCP, where a DHCP server provides the URL of the WPAD configuration file.
	    - **DNS**: Alternatively, WPAD can be discovered via DNS by querying for a specific hostname (e.g., `wpad.<domain>`). The server can then respond with the location of the PAC file.
	4. **Dynamic Changes**: Since the configuration is dynamically discovered, network administrators can change proxy settings centrally without needing to update individual devices.
	
- ##### How WPAD Works:

	1. **Client Request**: When a device connects to a network, it looks for proxy settings using the WPAD protocol.
	2. **Discovery**:
	    - If using DHCP, the client sends a request to the DHCP server to receive the WPAD URL.
	    - If using DNS, the client queries for the `wpad` hostname to find the configuration file.
	3. **Configuration File Retrieval**: Once the URL is discovered, the client retrieves the PAC file, which contains the necessary proxy settings.
	4. **Proxy Configuration**: The client applies the settings from the PAC file to route web traffic through the specified proxy server.

- ##### Benefits of WPAD:

	- **Ease of Use**: Simplifies network configuration for users by automating proxy setup.
	- **Centralized Management**: Administrators can manage proxy settings centrally, reducing the need for individual device configuration.
	- **Flexibility**: Adapts easily to changes in network infrastructure, making it suitable for dynamic environments.
	
- ##### Security Considerations:

	- **Potential Risks**: WPAD can introduce security vulnerabilities if not properly configured, as it could be exploited to redirect traffic through malicious proxy servers. It’s essential to secure the discovery methods (DHCP/DNS) and validate the configuration files.

- ##### Use Cases:

	- **Enterprise Networks**: Commonly used in corporate environments where web traffic is routed through proxy servers for security, monitoring, and content filtering.
	- **Educational Institutions**: Schools and universities may use WPAD to manage web access for students and faculty.

- ##### Summary:

	WPAD is a useful protocol for automatically discovering and configuring proxy settings, making it easier for devices to connect to the internet in managed environments. Its ability to centralize configuration and adapt to changes helps streamline network management, but it requires careful implementation to avoid security risks.


##### What is Responder and its uses ( #Tools)

- "Responder" is a tool used for network attacks, particularly for capturing and analyzing authentication credentials over a local network. It's part of the toolkit for penetration testers and ethical hackers.

- Responder works primarily by leveraging vulnerabilities in protocols such as LLMNR (Link-Local Multicast Name Resolution) and NBNS (NetBIOS Name Service). When a device on the network tries to resolve a hostname and fails, it may broadcast a request. Responder listens for these requests and responds with fake information, potentially capturing usernames and NTLM hashes in the process.

- This tool is particularly useful in testing the security of networks, identifying weak configurations, and performing network reconnaissance. However, it's essential to use it responsibly and only on networks where you have permission to test.


##### Difference between netcat and responder ( #Information)

- Responder and Netcat serve different purposes and are not direct alternatives to each other.
- **Responder** is specifically designed for capturing authentication credentials in network environments, primarily exploiting weaknesses in protocols like LLMNR and NBNS.
- **Netcat**, on the other hand, is a versatile networking utility often referred to as the "Swiss Army knife" of networking. It can be used for various tasks, including:

	- Creating TCP/UDP connections
	- Port scanning
	- Banner grabbing
	- Transferring files
	- Setting up reverse shells

- While both tools can be used in network testing and penetration testing, they focus on different aspects of network interactions. If you're looking to capture credentials, Responder is the tool for that. If you need a general-purpose networking tool, Netcat is the way to go.

