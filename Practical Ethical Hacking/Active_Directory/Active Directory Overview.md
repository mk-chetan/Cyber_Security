
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