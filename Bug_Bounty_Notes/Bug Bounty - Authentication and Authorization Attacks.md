
## Authentication:

- Authentication is the process of an entity proving they who they claim to be.
- There are three main factors used for authentication. something you know (password), something you have (token), and something you are (biometrics)
- Just because a web application used one or more factors, it does NOT mean its authentication mechanism is secure
#### Common types of Authentication:

- Password-based authentication
- Multi-factor authentication
- Application behavior / side-channel analysis

#### Common weaknesses:

- Lack of brute-force protection
- Logic flaws

#### General Process:

- Map the entire authentication attack surface
- Create multiple accounts
- Is the application using a standard library/framework?
- Check for logic issues
- Inspect tokens

#### Brute-force Attacks:

- Using trial & error to guess credentials
		- Target usernames, passwords, email addresses, etc
		- Use information we've found or from lists and public sources
		
- Watch for changes in:
		- Status codes
		- Error messages
		- Content length
		- Response times
		
- Wordlists to save us time:
		- Assetnote:
			- https://wordlists.assetnote.io/
		- Seclists:
			- `sudo apt install seclists`

#### Response Timing / Side-Channel Analysis:

- Response timing can give us insights into what the application is doing and potentially leak information
		- Use brute-force techniques
		- Monitor the application for differences in response times
		- Repeat the test to verify

#### Rate Limiting Bypasses:

- Rate limiting prevents us from sending large numbers of requests to a target. It can also be referred to as throttling.
		- Can we identify how the rate limit is being applied?
		- Is it based on an input we control?
		- If yes.. we can potentially bypass it!
	- #Resources : [https://appsecexplained.gitbook.io/appsecexplained/bypassing-controls/rate-limiting](https://appsecexplained.gitbook.io/appsecexplained/bypassing-controls/rate-limiting)
	- Application can rate limit using IP address
	- Turning off and on the router helps to get a new IPv6 address
	- If X-Forwarded-For and any other IP related headers are not available in the request, try to add one and see if the server accepts it, if it does we can use this header and bypass the IP-based rate limiting
	- Pitchfork attack type can be used to iterate through all the payloads simultaneously, useful for finding username by iterating through the X-Forwarded-For: IP header and also iterating through usernames to bypass the IP-based rate limiting for brute force

#### Sequencer:

- Sometimes we want to understand if a token just appears to be a random and we can use sequencer to achieve this.
		- When should we do this?
			- If an app looks like it's running its own session management
			- if the session token is short, repetitive or doesn't appear to be random
			- MD5Sum:
				- `echo -n "peter" | md5sum`

#### Multi-Factor Authentication:

- MFA can also be vulnerable to broken logic/implementation flaws and brute-force attacks
		- Approach MFA in the same way except we analyze each step for weaknesses
	- Things we might try:
		- Forceful browsing
		- Changing parameters & body content
		- Brute-forcing codes
		- Testing for predictability
		- Testing backup codes
		- Testing codes multiple times or against different accounts
		- Triggering errors or erroneous behavior
		- Test other functionality like enrolment


## Authorization / Access Control:

#### Access Control:

- Also known as "Authorization"
	- In a nutshell. It's what you're allowed to do
	- Common finding in modern and complex applications
	- Different types of access control exist:
		- Horizontal, Vertical and context-dependent
##### Typical Attacks:

- Forceful browsing
- IDOR ( Insure Direct Object Reference ) / BOLA( Broken Object Level Authorization ) / BFLA ( Broken Function Level Authorization )
- Trusting user input (E.g. headers, parameters)

##### IDOR - Insecure Direct Object Reference:

- Sometimes applications use user-supplied input to access objects directly.
- Often used to access information of other objects (e.g. another user's account information)
- May need to be combined with another weakness if the object ID is not easy to guess or brute-force
- Can also impact files and work in various other contexts
- Known as BOLA in APIs
- BAC (Broken Access Control) / BFLA ( Broken Function Level Authorization )

- IDOR is used when we access objects directly. BFLA or BAC is used when we can abuse functionality, such as updating the account of another user.

##### Weak Access Controls:

- Sometimes applications will user input we can control for access control, such as HTTP methods or headers.
- We should check to see if modifying the HTTP request will lead to unintended behavior
		- HTTP Method
		- Headers (e.g. Referrer, X-Origin-URL)
- Comparing requests between different users may help us uncover weaknesses such as these.

