---
date: 2015-02-09
title: OWASP Top 10
tags:
- owasp
---
The Open Web Application Security Project (OWASP) is an open community dedicated to enabling organizations to develop, purchase, and maintain applications that can be trusted.
All of the OWASP tools, documents, forums, and chapters are free and open to anyone interested in improving application security.

The [OWASP Top 10 project](https://www.owasp.org/index.php/Top_10_2013-Table_of_Contents "OWASP Top 10 project") references the most security issues and widespread on the web.
Most safety audits and specialized tools are based on the Top 10.
The primary aim is to educate developers, designers, architects, managers, and organizations about the consequences of the most important web application security weaknesses.
The Top 10 also provides basic techniques to protect against these high risk problem areas.

For each risks, OWASP provides generic information about likelihood and technical impact using the following simple ratings scheme, which is based on the Rating Methodology:

![OWASP top10 risk](/images/owasp-top10-risk.png)

<!--more-->

**Only you* know the specifics of your environment and your business.
For any given application, there may not be a threat agent that can perform the relevant attack, or the technical impact may not make any difference to your business.
Therefore, you should evaluate each risk **for yourself**, focusing on the threat agents, security controls, and business impacts in your enterprise.

Below the 2013 top 10 list:

1. **Injection**
    Injection flaws, such as SQL, OS, and LDAP injection occur when untrusted data is sent to an interpreter as part of a command or query. The attacker’s hostile data can trick the interpreter into executing unintended commands or accessing data without proper authorization.

3. **Broken Authentication and Session Management**
    Application functions related to authentication and session management are often not implemented correctly, allowing attackers to compromise passwords, keys, or session tokens, or to exploit other implementation flaws to assume other users’ identities.

5. **Cross-Site Scripting (XSS)**
    XSS flaws occur whenever an application takes untrusted data and sends it to a web browser without proper validation or escaping. XSS allows attackers to execute scripts in the victim’s browser which can hijack user sessions, deface web sites, or redirect the user to malicious sites.

7. **Insecure Direct Object References**
    A direct object reference occurs when a developer exposes a reference to an internal implementation object, such as a file, directory, or database key. Without an access control check or other protection, attackers can manipulate these references to access unauthorized data.

9. **Security Misconfiguration**
    Good security requires having a secure configuration defined and deployed for the application, frameworks, application server, web server, database server, and platform. Secure settings should be defined, implemented, and maintained, as defaults are often insecure. Additionally, software should be kept up to date.

11. **Sensitive Data Exposure**
    Many web applications do not properly protect sensitive data, such as credit cards, tax IDs, and authentication credentials. Attackers may steal or modify such weakly protected data to conduct credit card fraud, identity theft, or other crimes. Sensitive data deserves extra protection such as encryption at rest or in transit, as well as special precautions when exchanged with the browser.

13. **Missing Function Level Access Control**
    Most web applications verify function level access rights before making that functionality visible in the UI. However, applications need to perform the same access control checks on the server when each function is accessed. If requests are not verified, attackers will be able to forge requests in order to access functionality without proper authorization.

15. **Cross-Site Request Forgery (CSRF)**
    A CSRF attack forces a logged-on victim’s browser to send a forged HTTP request, including the victim’s session cookie and any other automatically included authentication information, to a vulnerable web application. This allows the attacker to force the victim’s browser to generate requests the vulnerable application thinks are legitimate requests from the victim.

17. **Using Components with Known Vulnerabilities**
    Components, such as libraries, frameworks, and other software modules, almost always run with full privileges. If a vulnerable component is exploited, such an attack can facilitate serious data loss or server takeover. Applications using components with known vulnerabilities may undermine application defenses and enable a range of possible attacks and impacts.

19. **Unvalidated Redirects and Forwards**
    Web applications frequently redirect and forward users to other pages and websites, and use untrusted data to determine the destination pages. Without proper validation, attackers can redirect victims to phishing or malware sites, or use forwards to access unauthorized pages.

**Don’t stop at 10.** There are hundreds of issues that could affect the overall security of a web application as discussed in the OWASP Developer’s Guide and the OWASP Cheat Sheet Series.
These are essential reading for anyone developing web applications.
Guidance on how to effectively find vulnerabilities in web applications is provided in the [OWASP Testing Guide](https://www.owasp.org/images/5/56/OWASP_Testing_Guide_v3.pdf "OWASP Testing Guide") and the [OWASP Code Review Guide](https://www.owasp.org/images/f/fa/Code_Review_Guide_Pre-AlphaV2_(1).pdf "OWASP Code Review Guide").

**Constant change.** This Top 10 will continue to change.
Even without changing a single line of your application’s code, you may become vulnerable as new flaws are discovered and attack methods are refined: "Scanned today Hacked tomorrow".

<br>

Source: [OWASP](https://www.owasp.org/index.php/Main_Page "OWASP")
