# Cybersecurity Terminologies

## 1) Cyber Attack

A cyber attack is an offensive act to make a computing device function maliciously, with the intent of stealing user credentials, gaining unauthorized access to a target system, disrupting services, and so many other intentions.

## 2) Exploit 

### Definition: 

An exploit is a specific method or tool used by an attacker to take advantage of a vulnerability in a system. Exploits can be code, scripts, or techniques.

### Examples: 

Buffer overflow attacks, SQL injection, cross-site scripting (XSS), privilege escalation exploits.

### Role: 

Exploits are used by attackers to carry out their threats, turning potential risks into actual incidents. Understanding how exploits work is vital for creating defenses and response strategies.

## 3) Cyber Threat

### Definition: 

A cyber threat is any potential danger or harm to an information system or network that could result in a negative impact, such as data loss, financial damage, or reputation damage.

### Examples: 

Malware, phishing attacks, ransomware, insider threats, nation-state attacks.

### Role: 

Threats are external or internal actors or factors that could exploit vulnerabilities within a system. Understanding threats helps in identifying and preparing for possible attacks.

## 4) Risk

### Definition: 

Risk is the potential for loss or damage when a threat exploits a vulnerability. It is often calculated as a combination of the probability of the threat occurring and the impact it would have.

Formula: Risk = Threat × Vulnerability × Impact

### Examples: 

The risk of a data breach if weak passwords are used; the risk of a DDoS attack on an unsecured server.

### Role: 

Risk assessment is critical for prioritizing resources and actions. High-risk areas should be addressed more urgently.

## Vulnerability

### Definition: 

A vulnerability is a weakness or gap in a system's defenses that could be exploited by a threat to gain unauthorized access or cause harm.

### Examples: 

Unpatched software, default configurations, weak encryption, poor access controls.

### Role:

Identifying and mitigating vulnerabilities is a core part of cybersecurity practices. Regular vulnerability assessments and patch management are essential.

## Comparison

### Interrelationship:

- Threats are potential sources of harm.
- Vulnerabilities are the weaknesses that threats can exploit.
- Exploits are the actual methods used to attack those vulnerabilities.
- Risk is the potential impact of a threat successfully exploiting a vulnerability.

### Focus in Cybersecurity:

- Cyber Threats: Identifying and understanding the actors and methods.
- Risk: Evaluating the likelihood and impact of different threats exploiting vulnerabilities.
- Vulnerability: Finding and fixing weaknesses in systems.
- Exploit: Analyzing attack methods to develop defenses.

# Types of Exploits

### 1. Local Exploitation

#### Description: 

Exploitation that requires the attacker to have direct access to the target system or network, typically as an authenticated user or someone with physical access.

#### Examples:

- Exploiting a privilege escalation vulnerability to gain administrative rights on a local machine.
- Executing malicious code by inserting a USB drive into a computer.

### 2. Remote Exploitation

#### Description: 

Exploitation that can be carried out over a network without physical access to the target system. The attacker can initiate the exploit from anywhere.

#### Examples:

- Exploiting a remote code execution (RCE) vulnerability in a web server.
- Launching a Distributed Denial of Service (DDoS) attack against an online service.

### 3. Client-Side Exploitation

#### Description: 

Exploitation that targets client applications, typically requiring user interaction, such as opening a malicious email attachment, visiting a compromised website, or running a vulnerable software.

Examples:

- Exploiting a browser vulnerability when a user visits a malicious website.
- Attacking a PDF reader via a crafted PDF file sent to the user.

### 4. Server-Side Exploitation

#### Description: 

Exploitation that targets server applications or services, often without requiring any user interaction. These exploits typically target vulnerabilities in the software running on the server.

#### Examples:

- SQL injection in a web application to gain unauthorized access to the database.
- Exploiting a buffer overflow vulnerability in a web server to execute arbitrary code.

### 5. Social Engineering Exploitation

#### Description: 

Exploitation that relies on manipulating individuals into performing actions or divulging confidential information that can be used to gain access to systems or data.

#### Examples:

- Phishing attacks where users are tricked into revealing their credentials.
- Pretexting to obtain sensitive information over the phone by pretending to be a trusted figure.

### 6. Physical Exploitation

#### Description: 

Exploitation that requires physical access to the target environment, device, or network. This type of exploitation often involves tampering with hardware or physically compromising the security of a facility.

#### Examples:

- Installing a hardware keylogger on a workstation to capture keystrokes.
- Bypassing a locked door or secure entry system to access restricted areas.

# Common Computer Attacks 

Cyber attacks are deliberate attempts by individuals or groups to breach the information systems of another individual or organization. Here are some of the most common types of cyber attacks:

## 1. Phishing

### Description: 

Phishing attacks involve tricking individuals into providing sensitive information, such as passwords or credit card numbers, often through deceptive emails, websites, or messages.

### Example: 

An email that appears to be from a trusted source, asking you to click a link and enter your login credentials.

## 2. Malware

### Description: 

Malware is malicious software designed to harm, exploit, or otherwise compromise a system. Types of malware include viruses, worms, trojans, ransomware, spyware, and adware.

### Example: 

Ransomware encrypts a user's data and demands payment for the decryption key.

## 3. Denial of Service (DoS) and Distributed Denial of Service (DDoS)

### Description: 

DoS and DDoS attacks aim to overwhelm a system, network, or website with traffic, rendering it unavailable to users. DDoS involves multiple compromised systems attacking simultaneously.

### Example: 

Flooding a website with so many requests that it crashes or becomes too slow to use.

## 4. Man-in-the-Middle (MitM) Attack

### Description: 

In MitM attacks, the attacker intercepts and potentially alters communication between two parties without their knowledge. This can occur through unsecured Wi-Fi, fake websites, or compromised devices.

### Example:

An attacker intercepting data sent from a user's device to a website, such as login credentials.

## 5. SQL Injection

### Description: 

SQL injection attacks involve inserting malicious SQL queries into input fields on websites or applications, enabling the attacker to gain unauthorized access to the database and manipulate or steal data.

### Example: 

Inputting SQL code into a login form to bypass authentication and access user data.

## 6. Cross-Site Scripting (XSS)

### Description: 

XSS attacks involve injecting malicious scripts into websites that are then executed by the browser of a visiting user. These scripts can steal data, manipulate content, or redirect users.

### Example: 

An attacker injecting a script into a comment section that steals session cookies from other users who view the comment.

## 7. Brute Force Attack

### Description: 

Brute force attacks involve systematically trying every possible combination of passwords or encryption keys until the correct one is found. These attacks are often automated.

### Example: 

Using a tool to try all possible passwords until the correct one is found and access is gained.

## 8. Credential Stuffing

### Description: 

Credential stuffing uses lists of previously breached username and password combinations to try to gain access to multiple accounts, exploiting the fact that many users reuse passwords across different sites.

### Example: 

Using credentials from a data breach to log into a user's social media or banking account.

## 9. Ransomware

### Description:

Ransomware is a type of malware that encrypts the victim's data and demands a ransom for the decryption key. It's a prevalent and highly disruptive form of cyber attack.

### Example: 

The WannaCry ransomware attack that affected organizations worldwide, encrypting data and demanding payment in Bitcoin.

## 10. Zero-Day Exploit

### Description: 

A zero-day exploit takes advantage of a previously unknown vulnerability in software or hardware, for which no patch or fix is available. These are highly dangerous because they are typically exploited before the vendor can address the issue.

### Example: 

Exploiting a flaw in a popular operating system that the vendor is unaware of, allowing the attacker to gain control of the system.

## 11. Social Engineering

### Description: 

Social engineering attacks manipulate individuals into performing actions or divulging confidential information, often bypassing technological security measures by exploiting human psychology.

### Example: 

A phone call pretending to be from IT support, asking for a user's password to fix a non-existent problem.

## 12. Advanced Persistent Threat (APT)

### Description: 

APTs are prolonged and targeted attacks where an intruder gains unauthorized access to a network and remains undetected for an extended period, often to steal data or spy on the organization.

### Example:

A nation-state actor infiltrating a government agency's network to gather intelligence over several months.

## 13. Insider Threats

### Description: 

Insider threats involve individuals within an organization, such as employees, contractors, or partners, who misuse their access to harm the organization, whether intentionally or accidentally.

### Example: 

An employee downloading sensitive data to sell to a competitor.

## 14. Session Hijacking

### Description: 

In session hijacking, an attacker takes control of a user's session by stealing session cookies or tokens, allowing them to impersonate the user and access restricted areas.

### Example: 

An attacker capturing a session token over an unsecured Wi-Fi connection and using it to access a victim's online banking account.

These attacks highlight the importance of maintaining robust cybersecurity measures, such as regular software updates, strong passwords, multi-factor authentication, and employee training on recognizing and avoiding phishing attempts and social engineering.

# How to Prevent Cyber Attacks?

- Always ensure your system and software are up to date.
- Use strong passwords with upper and lowercase alphabets mixed with numbers and symbols. An example is "!Cyb3r-T4l3n75".
- Avoid clicking on suspicious links or opening a suspicious email.
- The use of a VPN (Virtual Private Network) is a great way to prevent MITM.
- Install firewalls and good anti-virus software.