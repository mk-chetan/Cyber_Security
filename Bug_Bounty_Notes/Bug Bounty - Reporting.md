
## CVSS ( Common Vulnerability Scoring System )

|  Rating  | CVSS Score |
| :------: | :--------: |
|   None   |    0.0     |
|   Low    | 0.1 - 3.9  |
|  Medium  | 4.0 - 6.9  |
|   High   | 7.0 - 8.9  |
| Critical |  9.0 - 10  |

**CVSS Calculator :** 

- https://www.first.org/cvss/calculator/4.0#
- https://gitlab-com.gitlab.io/gl-security/product-security/appsec/cvss-calculator/#
- CVSS Advisor - https://cvssadvisor.com/

##### CVSS Components:

 - **Base Metrics:** The inherent qualities of a vulnerability that remain constant. The  characteristics that contribute to the vulnerability's impact and exploitability
 - **Temporal Metrics:** The characteristics of a vulnerability that may change over time. Provides additional context on exploitability, remediation etc.,
 - **Environmental Metrics:** Considers the impact of the vulnerability within a specific environment. Considers system configuration, compensating controls etc.,

##### Base Metrics:
###### Base Metrics: Exploitability

-  **Attack Vector (AV):**
	- Network (N)
	- Adjacent (A)
	- Local (L)
	- Physical (P)
- **Attack Complexity (AC):**
	- Low (L)
	- High (H)
- **Privileges Required (PR):**
	- None (N)
	- Low (L)
	- High (H)
- **User Interaction (UI):**
	- None (N)
	- Required (R)

###### Base Metrics: Impact

- **Confidentiality (C):**
	- None (N)
	- Low (L)
	- High (H)
- **Integrity (I):**
	- None (N)
	- Low (L)
	- High (H)
- **Availability (A):**
	- None (N)
	- Low (L)
	- High (H)

###### Base Metrics: Scope

- **Scope (C):**
	- Unchanged (U)
	- Changed (C)


##### Temporal Metrics:

- **Exploit Code Maturity (E):**
	- Not Defined (X)
	- Unproven (U)
	- Proof-of-Concept (P)
	- Functional (F)
	- High (H)

- **Remediation Level (RL):**
	- Not Defined (X)
	- Official Fix (O)
	- Temporary Fix (T)
	- Workaround (W)
	- Unavailable (U)

- **Report Confidence (RF):**
	- Not Defined (X)
	- Unknown (U)
	- Reasonable (R)
	- Confirmed (C)

##### Environmental Metrics: Modified

- **Base Metrics:**
	- Modified Attack Vector (MAV)
	- Modified Attack Complexity (MAC)
	- Modified Privileges Required (MPR)
	- Modified User Interaction (MUI)
	- Modified Scope (MS)
	- Modified Confidentiality (MC)
	- Modified Integrity (MI)
	- Modified Availability (MA)

##### Environmental Metrics: Requirements

- **Confidentiality Requirement (CR):**
	- Not Defined (X)
	- Low (L)
	- Medium (M)
	- High (H)
- **Integrity Requirement (IR):**
	- Not Defined (X)
	- Low (L)
	- Medium (M)
	- High (H)
- **Availability Requirement (AR):**
	- Not Defined (X)
	- Low (L)
	- Medium (M)
	- High (H)

##### CVSS and Bug Bounty:

- Bounties are evaluated based on the overall severity of the vulnerability
- Researches (optionally) submit their self-calculated CVSS score
- The CVSS may be updated by triage and/or the impacted organization
- In case of a dispute, the bug bounty platform will provide mediation
- Imperfect system but currently the most reliable, consistent approach


##### CVSS Limitations:

- Subjectivity
- Complexity
- Lack of context
- Limited Scope
- Lack of real-time updates
- Dependency on vulnerability data
- Misdirecting priorities risk

##### CVSS Versions:

v3 - released in 2015 and updated in 2019 (v3.1)
v4 - launched in end of 2023


##### Communicating with Clients and Triagers:

- **Effective Communication:**
	- Use official communication channels
	- Clarity and precision
	- Honest reporting
	- Be patient and professional
- **Reporting Vulnerabilities:**
	- Structured vulnerability report:
		- Title
		- Description
		- Steps of reproduce
		- Impact
		- CWE and CVSS
		- Mitigation suggestions
		- Proof-of-Concept (PoC)
- **Client Expectations:**
	- Timely and responsive communication
	- Compliance with reporting guidelines
	- Adherence to confidentiality requirements
	- Understanding of business impact
	- Professionalism and respect.
- **Triager Role:**
	- Triagers act as a bridge between researchers and clients
	- They evaluate (triage) incoming bug reports
	- Sometimes require additional information
	- Send valid submissions directly to the client
- **Responsiveness:**
	- Timely communication results in faster resolution
	- Two-way street, but patience is also important
	- Adhere to the community code of conduct (30 day wait)
- **Coordinated Disclosure:**
	- A structed process for publicly disclosing vulnerabilities
	- Ensures clients have sufficient opportunity to fix the bug
	- Allows researchers to gain recognition for their work, and help to educate the community
	- Many programs do not allow disclosure! Follow the CoC.

