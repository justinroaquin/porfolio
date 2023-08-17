

## 1. Receive Alert/Ticket
- Receive Alert from the following sources:
	- SIEM insights
	- Automated queries
	- zendesk tickets

## 2. Collect Contextual Information
- Access Sumo Logic and review the relevant logs associated with the alert. Analyze the failed login attempts, noting the source IP addresses, timestamps, and account names being targeted. Correlate this information with other logs, such as firewall logs or authentication logs, to gather more contextual information.

### Authentication Alerts (Brute Force attempts, Impossible travels, new location sign-ins, Risky user sign-ins, etc.)
- Collect metadata such username, email address, source and destination IP address, application accessed, user agent, 
	- Use Okta dashboard to determine login logs. Best used for authentication alerts and determining user location.
		- Find anomalies such as impossible travels.
		- Find common patterns such as known ISP from the users.
		- Suspicious IP addresses.
		- Determine what applications were access through SSO.
	- Use Azure AD logs lookup. Best used for Azure alerts, typically noninteractive sign-ins.
		- Determine where a user access teams, outlook, sharepoint using IP lookup.
	- Use O365 logs lookup. best used to look up o365 user activity.
		- Find anomalies.
			- External IP detected performing mass download/delete. This typically means that exfiltration is being performed.
			- Look for anonymous login locations.
			- Look for signs of data exfiltration.

### Email alerts (Phishing emails, spams, impersonation, etc.)
- Collect metadata such as sender, recipient(s), senderip, attachment hash, url, subject, etc.
	- Use inky dashboard to check if the email was previously reported.
	- Use inky logs lookup to see if the email was delivered to other users.
	- Check proofpoint logs lookup to check if proofpoint TAP flagged the email.
	- Determine if the email is delivered or not.

### Network Alerts
- Collect metadata such as user IP, server IP, url, connection type, port, server location.

## 3. Analysis
- Analyze collected metadata.
	- Use whois, abuseipdb, and alienvault otx to lookup IP address info. MX toolbox is also a great tool. Gather information about the IP such as location, abuse rating (abuseipdb), ISP, type of use (commercial, home, VPN, server, etc.).
	- Use urlscan.io or virustotal to scan url's. Gather information about the URL such as IP addresses, Domain name, CIDR, ASN, URL behaviors (redirects, cookie collection, Server locations).
	- Use virustotal to lookup hash. Virustotal holds collections of current and past known hashes of malicious files.

## 4. Document Findings
- Document all findings and steps of you did. Below is a template of questions to answer.
	- What happened? (technical details on the alert)
	- What is the Risk? (risk this alert poses to the org)
	- What are the steps you have taken (Document the collected information and analysis performed.)
	- What was your resolution (Based on the information gathered and analysis performed)

## 5. Resolution (True-Positive or False-Positive)
- What is the resolution based on your findings?
	- If **Yes**:
		- Remediate: if it is within your position's scope. 
			- Password reset, hard/soft delete emails, and account unlock for T1 analysts.
			- If it is a reported phishing email, thank user for reporting, hard/soft delete emails sent to recipient and close ticket.
		- Escalate: if required action is above analyst position, escalate to T2.
	- If **No**:
		- mark as false-positive and close ticket.
			- If it is a reported phishing email, thank user for reporting, ask to soft delete email, and close ticket.
