
## CSRF ( Cross Site Request Forgery ):

**CSRF** stands for **Cross-Site Request Forgery**. It's a type of security vulnerability that tricks a user into performing an unwanted action on a website or web application where they are authenticated (i.e., logged in).

##### How CSRF works:

1. **Authenticated User**: The attacker targets a user who is already logged in to a website (e.g., a bank account, social media, etc.).
2. **Malicious Request**: The attacker creates a malicious link or form, which sends a request (such as a transfer of funds, changing a password, or updating personal information) to the web application the user is authenticated on.
3. **Exploitation**: When the authenticated user clicks on the malicious link or submits the form, the browser automatically includes the user's session cookies (which prove they are logged in) with the request. Since the website doesn't verify whether the request is intentionally coming from the user, it processes the request as though the user initiated it.
##### Example:

1. The attacker creates a form on their website that submits a request to transfer money from a victim's bank account to the attacker's account.
2. If the victim is logged into their bank account and visits the attacker's site, the form is submitted without their knowledge or consent.
3. The bank processes the request and transfers the money to the attacker's account, believing the user (victim) initiated the action.

##### Prevention of CSRF:

1. **Anti-CSRF Tokens**: Generate unique tokens that are included with every form or state-changing request. The server checks that the token sent with the request matches the one stored in the user's session, ensuring the request was legitimate.
2. **SameSite Cookies**: Use the `SameSite` cookie attribute to restrict how cookies are sent with cross-site requests. This makes it harder for an attacker to send authenticated requests from a different site.
3. **Referer and Origin Header Checks**: Ensure that the request originates from the expected domain by checking the `Referer` or `Origin` headers.
4. **User Interaction**: In some cases, requiring the user to take a specific action (like re-entering a password) before performing a sensitive operation can help mitigate CSRF.


## SSRF (Server Side Request Forgery):

**SSRF** stands for **Server-Side Request Forgery**. It's a type of security vulnerability where an attacker is able to manipulate a server into making requests to internal or external resources that it would not normally be able to access, or shouldn't be accessing. This is typically exploited when an application allows users to provide URLs or other inputs that the server then uses to fetch data or interact with other systems.

##### How SSRF works:

1. **Exploiting Input Handling**: An attacker provides a malicious URL or input to a server-side function that is designed to make requests to other servers (like an API or internal service). For example, a web application might have a feature that lets users input a URL to fetch metadata, like a URL to an image or a webpage.

2. **Server Makes Request**: The server, acting as a proxy, makes a request to the provided URL. If the attacker controls the input and the server has the ability to access internal services (e.g., an internal API, database, or private network resources), the attacker can get the server to make requests to these internal services, potentially leaking sensitive information or even performing unauthorized actions.

3. **Sensitive Data Exposure or Attack Execution**: SSRF can be used to:
    
    - Access internal systems or services (e.g., APIs, databases) that are normally not exposed to the outside world.
    - Perform port scanning on internal networks.
    - Interact with cloud metadata services, which may expose sensitive information like credentials or configuration details (e.g., AWS metadata service).
    - Bypass access control restrictions by making requests as if they come from the server itself, bypassing firewalls or network security rules.
##### Example:

Suppose an application allows a user to input a URL, and the server fetches the contents of that URL to display it to the user (e.g., a service that fetches metadata from a provided URL).

1. The attacker provides a URL like `http://localhost:8080/admin` (pointing to an internal service that only the server can access).
2. The server, thinking it's just fetching some external data, makes the request to `localhost:8080/admin`, which might expose sensitive internal data, such as configuration files, APIs, or admin interfaces.
3. If the internal service does not have proper security, the attacker could gain access to sensitive data or potentially execute malicious actions.

##### Potential Consequences of SSRF:

- **Exposure of Internal Systems**: The attacker may gain access to internal servers or services that are not exposed to the outside world.
- **Information Disclosure**: SSRF can be used to fetch internal data, such as database information, metadata, credentials, or configuration details.
- **Network Attacks**: The attacker may be able to launch network attacks from the server’s IP, potentially bypassing firewalls or access controls.
- **Cloud Metadata Service Exploitation**: In cloud environments (e.g., AWS, GCP), SSRF can be used to access metadata services, which may expose sensitive information such as API keys, instance metadata, and environment variables.

##### Prevention of SSRF:

1. **Input Validation and Whitelisting**: Carefully validate and sanitize user inputs. Ensure that URLs or other user-controlled inputs are strictly validated. For instance, disallow requests to internal IP ranges (e.g., `127.0.0.1`, `localhost`, private IP ranges) and only allow trusted external domains.

2. **URL Filtering**: Implement filters to ensure that users can only submit URLs that point to safe and trusted destinations. Avoid letting users directly input raw URLs to internal services.

3. **Network Segmentation**: If the server needs to access internal resources, consider isolating the vulnerable server or application from sensitive internal systems, limiting its access to only what is necessary.

4. **Use of Proxy Servers**: In cases where external requests are needed, use a proxy server that can enforce strict rules and sanitization before any requests are made.

5. **Metadata Service Protections**: For cloud-based applications, make sure that the application cannot access sensitive metadata services (e.g., AWS instance metadata) unless explicitly required, and apply proper access control to prevent exploitation.


## Open Redirects:

**What is an Open Redirect:**
- Open Redirects occur when an application accepts untrusted input that could cause a redirection to an external URL.
- We can exploit this to redirect users to sites that attempt to capture credentials or deliver malware.


## WAF Evasion:

**WAF Evasion** refers to techniques used by attackers to bypass or evade detection and blocking mechanisms employed by a **Web Application Firewall (WAF)**. A WAF is designed to protect web applications from malicious web traffic by filtering, monitoring, and blocking potential attacks, such as **SQL injection**, **Cross-Site Scripting (XSS)**, **CSRF**, and other common web-based vulnerabilities. However, attackers may attempt to evade WAF protections by exploiting weaknesses in the WAF itself or by obfuscating their malicious traffic to avoid detection.

##### Common WAF Evasion Techniques:

1. **Encoding and Obfuscation**
    
    - **URL Encoding**: Attackers can encode malicious payloads in ways that might bypass WAF rules, e.g., using `%20` for spaces or `%3C` for `<`. Some WAFs may not decode the input correctly, allowing the attack to go undetected.
    - **Double Encoding**: Sometimes attackers encode data multiple times to confuse the WAF's decoding mechanisms. For example, encoding `%3C` (the encoded value for `<`) into `%253C` (which, when decoded twice, results in `<`).
    - **Base64 Encoding**: Attackers may encode payloads in Base64 to obscure them from pattern-matching detection systems.
2. **HTTP Parameter Pollution (HPP)**
    
    - In HPP, attackers send multiple, potentially malicious values for the same parameter (e.g., `user=admin&user=evil`), exploiting how some WAFs or back-end applications process HTTP parameters. The goal is to make it harder for the WAF to accurately parse and analyze the input.
    
3. **Splitting Payloads Across Multiple Requests**
    
    - An attacker may split a malicious request across several smaller requests to evade detection. For example, breaking up a SQL injection payload into parts that are submitted across multiple HTTP requests, with each request appearing harmless on its own.
4. **Using Alternative HTTP Methods**
    
    - Many WAFs focus on standard HTTP methods like `GET`, `POST`, `PUT`, etc., and might not properly inspect or block less commonly used HTTP methods like `OPTIONS`, `TRACE`, `CONNECT`, or `PATCH`. Attackers can exploit this by sending malicious payloads using these methods.
5. **Obfuscating or Modifying Headers**
    
    - WAFs often check HTTP headers like `User-Agent`, `Referer`, `X-Forwarded-For`, or custom headers for suspicious patterns. Attackers might alter, spoof, or fragment headers to bypass detection (e.g., using an unusual `User-Agent` string or manipulating `X-Forwarded-For` headers to hide the true source of the request).
6. **Evasion via WebSockets**
    
    - Some WAFs may not properly inspect traffic over WebSockets. Attackers can exploit this by sending malicious payloads over WebSocket connections, bypassing traditional HTTP-based WAF rules.
7. **Polymorphic and Metamorphic Attacks**
    
    - In polymorphic attacks, attackers modify their payloads slightly for each request (e.g., changing variable names, adding random characters) to prevent WAF signature-based detection.
    - Metamorphic attacks go further, re-writing the attack payload in a completely different form that still performs the same malicious action but is more difficult for the WAF to identify.
8. **Using Comments and Whitespace in Payloads**
    
    - Attackers might inject comments, whitespace, or other non-functional characters in SQL, JavaScript, or other payloads to evade detection by WAFs. For example, inserting SQL comments (`--`) or using excessive whitespace in an SQL injection payload (`OR 1=1 --`).
9. **Time-Based Evasion (Rate Limiting or Delay Evasion)**
    
    - Some WAFs may have rate-limiting or anti-DoS mechanisms that block requests after a certain threshold is reached. Attackers may use techniques like **slowloris** or send requests at a rate too low to trigger blocking, attempting to evade rate-based protections.
10. **Exploiting WAF Misconfigurations**
    
    - If a WAF is misconfigured (e.g., not properly inspecting all input parameters, not using the most up-to-date threat signatures, or having weak rule sets), attackers can exploit these weaknesses to bypass the WAF entirely.

##### Example of a WAF Evasion Attack:

###### Scenario: SQL Injection Evasion

1. **Payload Before Evasion**:
    
    `' OR 1=1 --`
    
    - This is a common SQL injection payload. A WAF that checks for typical SQL injection patterns would detect this and block it.
2. **Payload After Evasion (Encoding)**:
    
    `'%20OR%201%3D1%20--'`
    
    - This payload is URL-encoded, which can bypass some WAFs that do not decode the request properly.
3. **Payload After Evasion (Obfuscation)**:
    
    `' OR 1 /* comment */ = 1 --`
    
    - The payload includes SQL comments to bypass simple signature-based detection by breaking the pattern the WAF is looking for.

##### Mitigating WAF Evasion:

1. **Deep Packet Inspection (DPI)**: Use advanced DPI techniques that inspect traffic at multiple layers (not just HTTP), allowing the WAF to detect attacks even if they are obfuscated.
    
2. **Behavioral Analysis**: Rather than relying solely on signature-based detection, modern WAFs often use machine learning or heuristics to detect anomalies in behavior. This approach can help detect suspicious patterns even when traditional evasion techniques are used.
    
3. **Rate-Limiting and Blocking Suspicious IPs**: Implement additional measures, such as rate-limiting, geo-blocking, and IP reputation-based blocking, to reduce the chances of successful evasion.
    
4. **Regular WAF Rule Updates**: Make sure the WAF is continuously updated with the latest threat intelligence and signatures. Many WAF providers offer automatic rule updates to keep pace with new attack techniques.
    
5. **Input Validation**: Ensure that input validation is performed at both the WAF and application levels to prevent malicious data from being processed. Reject suspicious or malformed inputs early in the process.
    
6. **Layered Security**: Combine WAFs with other security mechanisms, such as intrusion detection systems (IDS), application security testing, and logging/monitoring to provide multiple layers of defense against evasion attempts.
    
7. **Custom Rule Creation**: Customize WAF rules to better reflect the unique application and its attack surface. Generic rules might not be sufficient for more sophisticated attacks.

##### Tool used to detect the WAF:

wafwoof:
- `wafwoof {website-url}`

