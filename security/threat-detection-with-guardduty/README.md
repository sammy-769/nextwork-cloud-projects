<picture><source media="(prefers-color-scheme: dark)" srcset="https://nextwork.ai/static/nextwork-logo-paper.svg" /><img src="https://nextwork.ai/static/nextwork-logo-leather.svg" alt="NextWork" width="300" /></picture>

# Threat Detection with GuardDuty

**Project Link:** [View Project](https://nextwork.ai/projects/d3694045-8dc9-5910-99bc-061c82f1bccd)

**Author:** Sammy Shah Saleh

---

## Introducing Today's Project!

### ✍️ What are we here to do today?

In this project, I deployed an intentionally vulnerable web application in AWS to explore how security threats can occur. I simulated attacks to obtain credentials and access sensitive data, then used Amazon GuardDuty to detect and analyse these threats. Finally, I explored GuardDuty's malware protection features to help safeguard my data.

### ✍️ What are we doing in this step?

In this step, I deployed a web application using an AWS CloudFormation template. I also explored the AWS resources created by the template to understand how they work together to run the application.

### Tools and concepts

I learnt how to use AWS CloudFormation to deploy a web application and its supporting infrastructure. I also explored Amazon GuardDuty to detect and analyse suspicious and potentially malicious activities, investigate security findings, and enable Malware Protection for S3 to scan uploaded objects for potential threats.

### Project reflection

The project took me more than two hours to complete.

I wanted to gain practical experience with AWS security services, understand how attackers could exploit vulnerabilities and stolen credentials, and learn how Amazon GuardDuty detects suspicious activities and malware.

## Project Setup

I deployed a CloudFormation template that created an intentionally vulnerable **OWASP Juice Shop** web application environment in AWS. The three main components were:

1. **Web application infrastructure:** OWASP Juice Shop hosted on an EC2-based web server, supported by networking resources, an Elastic Load Balancer, an Auto Scaling Group and a CloudFront distribution to make the application publicly accessible.
2. **Amazon S3 bucket:** Stored a file named `important-information.txt` containing simulated sensitive information. The EC2 instance had access to this bucket.
3. **Amazon GuardDuty:** Enabled threat detection and security monitoring to identify suspicious activities and potential attacks against the deployed environment.

I tested and exploited the intentionally vulnerable OWASP Juice Shop application. This allowed me to spend more time exploring Amazon GuardDuty and analysing whether it detected the suspicious activities and attacks I performed.

Amazon GuardDuty is a managed AWS security service that continuously monitors AWS accounts, workloads and data for suspicious activity, potential threats and malicious behaviour. It uses threat intelligence and machine learning to detect security risks and generate findings.

![Project Setup](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_n1o2p3q4)

### ✍️ What are we doing in this step?

I opened the running OWASP Juice Shop web application and used a hacking technique to bypass the admin portal's login process, allowing me to explore how an attacker could gain unauthorised access.

![Project Setup](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_d7e8f9g0)

## SQL Injection

SQL injection is a security vulnerability that occurs when an application does not properly handle user input before including it in an SQL query. An attacker can exploit this weakness to manipulate database queries and potentially gain unauthorised access to data or accounts.

I entered `' or 1=1;--` into the email field on the login page. This manipulated the SQL query because `1=1` was always true, while `--` commented out the remaining part of the query. As a result, I bypassed the normal login process and gained access to the admin portal.

### ✍️ What are we doing in this step?

In this step, I attempted to obtain AWS credentials from the vulnerable environment and then used the stolen credentials to access AWS resources, simulating how an attacker could exploit exposed credentials.

![SQL Injection](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_h1i2j3k4)

## Command Injection

Next, I used command injection, which is a security vulnerability that occurs when an application passes untrusted user input to the operating system without properly validating it. An attacker can exploit this weakness to execute unauthorised system commands on the server, potentially accessing sensitive information or compromising the system.

I entered a command injection payload in the username field. It exploited the application's ability to execute operating system commands and accessed the EC2 instance metadata service to retrieve temporary AWS IAM credentials. The credentials were then saved to a file within the web application's directory.

![Command Injection](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_t3u4v5w6)

## Attack Verification

I opened `[JuiceShopURL]/assets/public/credentials.json` in my browser and checked whether the file contained the AWS credentials retrieved by the command injection attack. Seeing the credentials in the file confirmed that the attack had succeeded.

### ✍️ What are we doing in this step?

In this step, I opened AWS CloudShell and configured the AWS CLI using the stolen credentials. I then used the CLI to access the S3 bucket and copy the sensitive file stored in it, demonstrating the potential impact of compromised AWS credentials.

![Attack Verification](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_x7y8z9a0)

## Using CloudShell for Advanced Attacks

I used AWS CloudShell to simulate an attacker accessing my AWS environment from an external environment using stolen EC2 credentials. I configured the AWS CLI with those credentials and used them to access private data in the S3 bucket. This created suspicious activity that Amazon GuardDuty could detect and analyse.

I used `wget` to download the stolen AWS credentials file from the Juice Shop web application. I then used `cat` and `jq` to display and format the JSON file, making the credentials easier to read and use when configuring the AWS CLI.

I created a new AWS CLI profile called `stolen` to store and use the stolen EC2 credentials separately from my default CloudShell profile. This allowed me to simulate an attacker using compromised credentials while keeping my normal AWS authentication settings unchanged.

![Using CloudShell for Advanced Attacks](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_j9k0l1m2)

### ✍️ What did you do using the 'stolen' profile?

I used the `stolen` profile to copy `secret-information.txt` from the web application's S3 bucket to my CloudShell environment. I then used the `cat` command to view the contents of the downloaded file, confirming that I could access the private information using the stolen credentials.

### ✍️ What are we doing in this step?

In this step, I explored Amazon GuardDuty's findings to identify and analyse the suspicious activities generated during my simulated attacks. I examined the detected threats to understand what happened and how GuardDuty reported them.

## GuardDuty's Findings

GuardDuty reported a finding in less than a minute in my test, although detection times can vary depending on the activity. A finding is a security alert that describes suspicious activity or a potential threat detected in my AWS environment.

The finding indicated that credentials belonging to the EC2 instance role `NextWork-GuardDuty-project-sam-TheRole-fSR97MhxAs9T` had been used from a different AWS account. This suggested that the EC2 credentials had been stolen and used by an external party to access AWS resources.

The finding indicated that credentials associated with an EC2 instance role had been used from an unexpected AWS location. This suggested that the credentials had been exposed and used by someone other than the intended EC2 instance.

![GuardDuty's Findings](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_v1w2x3y4)

## Extra: Malware Protection

I enabled **Malware Protection for Amazon S3** in GuardDuty for the S3 bucket used by the web application. This feature scans newly uploaded objects for potential malware. Malware is malicious software designed to harm systems, steal information or gain unauthorised access to resources.

I uploaded the `EICAR-test-file.txt` file to the S3 bucket. It contained a standard antivirus test string designed to be recognised as malware by security software. Although it was harmless and not actual malware, it allowed me to test whether GuardDuty's Malware Protection could detect it.

GuardDuty successfully detected the test file almost instantly. It reported: `A malware scan on your S3 object EICAR-test-file.txt has detected a security risk: EICAR-Test-File (not a virus)`. This confirmed that Malware Protection was working and had correctly identified the harmless EICAR test file.

![Extra: Malware Protection](https://nextwork.ai/sammy769/uploads/d3694045-8dc9-5910-99bc-061c82f1bccd_sm42x3y4)
