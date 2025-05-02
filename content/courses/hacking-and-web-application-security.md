---
date: 2025-04-22
title: Hacking & web application security
images: [/assets/img/courses/hacking-websec.png]
tags:
- courses
- cybersecurity
---

This intensive 3-day training is designed for __developers__ and __system administrators__ aiming to enhance the security of their web applications.
Combining __theoretical knowledge__ with __practical exercises__, the course covers __common vulnerabilities__, utilization of __essential tools__, __best practices__ in secure web development​.
Participants will learn to __identify__, __exploit__, and __remediate__ security flaws, gaining hands-on experience to proactively defend against real-world threats.​
<!--more-->

<br>
<center>
    <img src="/assets/img/courses/hacking-websec.png" alt="Hacking & web application security" width="500" />
</center>


## Targeted audience:

- Web developers
- System administrators
- Dev(Sec)Ops engineers
- Any cybersecurity enthusiasts


## Educational goals:
- Understand the main web application vulnerabilities
- Learn how to detect and exploit common security flaws
- Use tools like Burp Suite and SQLmap effectively
- Apply secure coding practices to prevent attacks
- Improve overall application resilience against threats


## Prerequisites:
- Basic knowledge of web development (HTML, CSS, JavaScript)
- Familiarity with web protocols (HTTP, HTTPS)
- Experience with using command-line tools


## Program:
#### Day 1 - The basics of web security
###### The landscape of web insecurity
- Web security: a vast topic
- Architecture of a typical Web app
- Open Web Application Security Project
- CWEs, CVEs & CVSS
- External and internal attackers
- The different steps of an attack
- An attacker’s bag of tricks
- Common misconceptions

###### Security’s place in the development cycle
- The importance of inventory
- Shadow IT
- Production bias & incompatibility with Agile approaches
- The idea of DevSecOps
- The importance of testing & code reviews
- Static & dynamic analysis

###### Outside help
- Security audits and _Bug Bounty_
- Vulnerability disclosure policy

###### HTTP protocol
- Reminder about the HTTP(s) protocol
- Anatomy of HTTP requests & responses
- Semantics & status codes
- Introduction to Dev Tools
- Introduction to Burp Suite

###### URLs
- Anatomy of a URL
- Open redirects exploitation

###### Disclosure of sensitive information
- Sequential IDs
- Verbose error messages
- Directory listing
- Clear-text logging of sensitive information
- Secrets on the client side
- API Excessive data exposure
- Public Git repositories & history
- Git directory deployed

#### Day 2 - User input flaws
###### Input validation
- Client-side, server-side, or both?
- HTTP parameter pollution
- Mass assignment

###### Cross-Site Scripting (XSS)
- Cross-site scripting is everywhere
- Types of cross-site scripting
- What is the impact?
- How to protect & how to bypass

###### SQL Injection (SQLi)
- What is it?
- What is the impact?
- How to protect & how to bypass
- Presentation of SQLMap
- NoSQL injection

###### Server-Side Request Forgery (SSRF)
- What is it?
- What is the impact?
- How to protect & how to bypass

###### Other forms of injection
- HTML injection
- Code injection
- OS command injection

###### Files
- Common file issues
- Arbitrary file upload
- Path traversal
- Local & remote file inclusion

#### Day 3 - authenticated user vulnerabilities
###### Authentication & vulnerabilities
- Authentication
- User enumeration
- Rate limiting and security questions
- Multiple-factor authentication

###### Passwords
- Cryptography in a nutshell: encoding, hashing, encryption
- Password-strength policy
- Leaked passwords
- Password storage
- Cracking tools

###### Cookies
- Cookies attributes: secure, httponly, domain, samesite...
- Session hijacking

###### Cross-Site Request Forgery (CSRF)
- What is it?
- What is the impact?
- How to protect & how to bypass

###### Clickjacking
- What is it?
- What is the impact?
- How to protect & how to bypass

###### Authorization & access control
- Authentication vs authorization
- Roles and permissions
- Insecure direct object reference

###### Cross-Origin resource sharing (CORS)
- Refresher on the _Same-Origin Policy_
- Possible misconfigurations
- What is the impact?
- How to protect

<br><br>

Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - quotes on request - SIRET 79778302400038
