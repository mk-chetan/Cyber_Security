
#### Tools that offer quick and efficient enumeration ( #Tools):

- Bloodhound
- Plumhound
- Ldapdomaindump
- PingCastle
- etc


#### ldapdomaindump:

- This tool is built in kali linux and is useful to grab all the domain detail dump with the exploited account.
- Syntax:
	- `sudo ldapdomaindump ldaps://{dc-ip-address} -u '{domain\username}' -p {password}`
	- If we face any issue with the MD4 Hashes (ValueError: unsupported hash type MD4)
		- Make sure to upgrade the ldapdomaindump - `sudo pip3 install --upgrade ldapdomaindump` 
		- Make sure to install the ldap dependencies - `sudo apt-get install python3-ldap`
		- Once done and you have latest version of ldapdomaindump, try to run it using python3 instead of python2
			- Syntax:
				- `python3 -m ldapdomaindump ldaps://{dc-ip-address} -u '{domain\username}' -p '{password}'
				- ex: `python3 -m ldapdomaindump ldaps://10.0.0.1 -u 'MARVEL\ppeter' -p 'password1'`
- Once the domain dump is finished, we can see all the domain documents in the path.


#### Bloodhound:

- BloodHound is a tool used in cybersecurity to analyze and visualize Active Directory (AD) relationships and permissions. The term "ingestors" refers to components or scripts that collect data from various sources and prepare it for analysis within BloodHound. Here’s a breakdown of their uses:

###### Purpose of BloodHound Ingestors

1. **Data Collection**: Ingestors gather information about Active Directory environments, including users, groups, computers, and their relationships. This data is crucial for understanding how permissions are structured.
    
2. **Visualization**: After collecting data, ingestors help format it for visualization in BloodHound's interface. This allows security professionals to see potential attack paths, identify misconfigurations, and assess overall security posture.
    
3. **User and Group Information**: Ingestors retrieve details about users and groups, including memberships, group policies, and delegated permissions.
    
4. **Session Information**: They can also collect session information, such as which users are logged into which machines, helping to map out the attack surface.
    
5. **Kerberos Tickets**: Some ingestors collect information about Kerberos tickets and their permissions, which can help identify escalation paths.
    
6. **Scripted Data Collection**: BloodHound provides various ingestors (like `SharpHound`, the most commonly used one) that can be run in different environments, including domain-joined machines or from remote locations, to gather necessary data for analysis.
    
###### Common Ingestors

- **SharpHound**: The primary ingestor for BloodHound, written in C#. It can run on domain-joined machines to collect detailed information about AD objects.
    
- **LDAP Enumeration**: Some scripts can enumerate information directly from the LDAP service to gather user and group data without executing code on domain machines.

 
  #### Steps:
	- Make sure, we have the latest bloodhound installed in the system
		- `sudo pip install bloodhound`
	- Once, we have the latest bloodhound installed, we need `neo4j` in order to make the bloodhound work
		- `sudo neo4j console`
		- Once started, look for the remote interface url and use that to open a user interface for neo4j in the browser: `http://localhost:7474/`
		- Once we have the page opened in browser, it will prompt for a username and password, we can use the default username (neo4j) and password (neo4j)
			- Once done, it will prompted to create a new password for the account.
	- Once neo4j is up, spin up bloodhound as well.
		- `sudo bloodhound`
		- It will prompt for a neo4j username and password, provide the one that is created in last step for neo4j and login into bloodhound
		- Now, we will be running the bloodhound ingestors with the below syntax:
			- `sudo bloodhound-python -d {domain} -u {username} -p {password} -ns {dc-ip-address} -c all`
				- -ns (Name server)
				- -c (collect)
			- Ex: `sudo bloodhound-python -d MARVEL.local -u ppeter -p password1 -ns 10.0.0.1 -c all`
		- If the bloodhound ingestors command fails for some reason, try using the python3 instead of python2
			- `sudo python3 -m bloodhound -d 'MARVEL.local' -u 'ppeter' -p 'password1' -ns 10.0.0.1 -c all`
		- Once it ran succesfully, we should be able to see computers.json, containers.json and etc files in the directory.
		- Open bloodhound and on the right side, click on upload data and navigate to the path, where we have the ingestor generated files and select all and import.
		- Once that is completed, we can go to the options on the left and see the database info, which we just imported updated into the database and also we can go to the analysis tab, and use some pre-built analytic queries, to check the graphical maps about the data we just imported, we can play around with these queries to get more info.
- Note: So basically, bloodhound is an graphical user interface application which requires neo4j to run, and bloodhound-ingestors are used to grab the data from the domain controllers with the exploited account and that data can be used to query in bloodhound UI with the graphs.


#### Plumhound (sister of bloodhound):

- Plumhound is bloodhound for blue and purple teams
- We need to grab this from github, as it doesn't come built-in
- Steps:
	- Search google for plumhound and copy the github repo url to clone it to the machine 
		- `https://github.com/PlumHound/PlumHound.git`
		- `cd /opt`
		- `sudo git clone https://github.com/PlumHound/PlumHound.git`
		- Once we have the git cloned, cd to the PlumHound directory to install it
			- `cd PlumHound`
			- `sudo pip3 install -r requirments.txt --break-system-packages`
				- --break-system-packages is required to install using pip3
		- Once the plumhound is installed, we can use a test command to check of its running as expected, but before that make sure that neo4j and bloodhound are up and running, as plumhound will be grabbing the data from bloodhound to analyze.
			- `sudo python3 PlumHound.py --easy -p neo4j1`
			- -p will be the password, we have updated for the neo4j application
			- After running the command, it should show that it ran 1 task and grabbed the users details, this confirms that plumhound is working.
		- Now to run few tasks and generate report using plumhound, we can use the below syntax:
			- `sudo python3 PlumHound.py -x tasks/default.tasks -p neo4j1`
			- Once this run is completed, it should have ran around 119 tasks and report is generated in the reports folder with Reports.zip
		- To check on to reports, navigate to reports folder inside the plumhound folder and ls 
			- `cd reports`
			- `ls` - to check all the reports created.
			- But to easily navigate through the report, we can use the index.html file in the reports folder.
				- `firefox index.html`
				- This will open a web page, with all the reports combined, which can navigated to grab more info, similar to bloodhound.

#### Ping Castle:

**Ping Castle** is a cybersecurity tool primarily used for assessing the security posture of Active Directory (AD) environments. It focuses on identifying vulnerabilities, misconfigurations, and potential attack vectors within an AD infrastructure. Here are the main uses and features of Ping Castle:

##### Key Uses of Ping Castle

1. **Security Assessment**: Ping Castle performs comprehensive security assessments of Active Directory environments, analyzing configurations, permissions, and user accounts.
    
2. **Vulnerability Identification**: The tool identifies potential vulnerabilities such as weak password policies, excessive privileges, and insecure configurations that could be exploited by attackers.
    
3. **Risk Scoring**: Ping Castle provides a risk scoring system that helps organizations prioritize their security issues based on the severity and potential impact of identified vulnerabilities.
    
4. **Report Generation**: After the assessment, Ping Castle generates detailed reports that outline security findings, recommended actions, and overall security scores. These reports can help organizations in understanding their security posture and compliance requirements.
    
5. **Continuous Monitoring**: Organizations can use Ping Castle for regular assessments, helping to ensure that any changes in the AD environment do not introduce new vulnerabilities over time.
    
6. **Ease of Use**: The tool is designed to be user-friendly, with a simple interface that allows both security professionals and less technical users to perform assessments and understand the results.
##### Common Features

- **User and Group Analysis**: Evaluates user accounts, group memberships, and their permissions to identify excessive access rights.
- **Password Policy Review**: Analyzes password policies to ensure they meet security best practices.
- **Historical Analysis**: Offers insights into historical changes within the AD environment, which can help in tracking potential security incidents.
- **Graphical Reports**: Provides visual representations of the security posture, making it easier to communicate findings to stakeholders.
##### Summary

Overall, Ping Castle is a valuable tool for organizations looking to enhance their Active Directory security. By identifying vulnerabilities and providing actionable insights, it helps organizations mitigate risks and strengthen their overall security posture.

##### Note:
We can download ping castle application and run it in the Domain Controller machine to scan and generate a health report.


