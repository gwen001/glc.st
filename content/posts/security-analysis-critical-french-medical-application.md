---
date: 2025-04-21
title: Analysis of a critical french medical application
images: [/images/optimed-preview.png]
tags:
- pii
- leak
- git
- sqli
- rce
- file upload
- php
---
During my regular activities as a bug bounty hunter and security auditor for sensitive institutions, I came across a healthcare application called __OptiMed__ (redacted), developed by __Synaptek Digital__ (redacted), and used in numerous medical imaging centers in France. Unfortunately, this software demonstrates a catastrophic level of security, putting __millions of sensitive medical records__ at risk.
<!--more-->


## Introducing OptiMed

According to the vendor's website:
> Synaptek Digital offers a fully integrated global solution covering all the needs of an imaging center or department.

_OptiMed_ handles medical exams like CT scans and MRIs, with multiple user roles: patients, physicians, administrative staff, system admins, etc.

Still on their website:
> Synaptek Digital has been an expert in the interoperability and security of healthcare information systems for over 10 years.

However, in reality the implementation is a real __security nightmare__.


## Critical issues

### Source code leak

<div style="float:left;width:550px;">
    First critical issue: the <code>.git</code> directory was publicly accessible at the root of the web portal:
    <code>
        https://www.irm-redacted.fr/.git/
    </code>
    <p>
        Problem when this happens, most of the time the source code of the application can be retrieved.
        Using some Git-Fu and hacking tools, I quickly got everything.
    </p>
    <p>
        Directories and sub, PHP files, hidden files, config files... not a single file was missing and more the source code was intact.
    </p>
    <p>
        According to the mess in the root directory, I already knew this would be severe.
        Time for a "serious" code audit.
    </p>
</div>

<img src="/images/optimed-ls.png" alt="OptiMed file listing" width="300" style="float:right;" />

<div style="clear:both;"></div>

### Secrets

<div style="float:left;width:550px;">
    Since I often perform code audit, I have some custom regexps in my pockets that I use to detect secrets: <i>usernames</i>, <i>passwords</i>, <i>tokens</i>, <i>API keys</i>...
    <p>
        Basically a huge <code>grep</code> at the root directory will output those sensitive informations.
    </p>
    <p>
        Result was unexpected (well, not really) as I got some nice hits:
        <br>ftp/smtp/bdd credentials, admin credentials and more.
    </p>
</div>

<img src="/images/optimed-secrets-1.png" alt="OptiMed secrets" width="300" style="float:right;" vspace="3" />
<img src="/images/optimed-secrets-2.png" alt="OptiMed secrets" width="300" style="float:right;" vspace="3" />
<img src="/images/optimed-secrets-4.png" alt="OptiMed secrets" width="300" style="float:right;" vspace="3" />

<div style="clear:both;"></div>

### Massive SQL injections

Randomly opening PHP files to check the quality on the code, a piece of s**t jumped into my eyes.
Most pages contained unfiltered SQL queries, clearly lacking input validation.
```php
$lastname		= isset($_POST['nom']) ? $_POST['nom'] : "";
$firstname		= isset($_POST['prenom']) ? $_POST['prenom'] : "";
$email			= isset($_POST['email']) ? $_POST['email'] : "";

$request = "SELECT * FROM radio ";
$request.= " WHERE lastname='$lastname' ";
$request.= " AND firstname='$firstname' ";
$request.= " AND email='$email' ";

$result = $mysql->query($request);
```
These are classic SQL Injection vulnerabilities that can allow:
- Data extraction
- Authentication bypass
- Admin access


### Passwords

Exploiting one of the numerous SQL injection in order to check how the database looked like.
I triggered [SQLMap](https://sqlmap.org/) to grab some informations about the structure of the database when the table `utilisateur` catched my attention.

![OptiMed utilisateurs](/images/optimed-utilisateurs.png)

Passwords were obviously stored using a weak hasing algorithm. With the helps of [hashes.com](https://hashes.com/en/decrypt/hash) I could get some passwords decrypted in a second:

![OptiMed passwords](/images/optimed-passwords.png)

_administrateur_ password retrieved, admin panel unlocked!


### Arbitrary file upload

An arbitrary file upload vulnerability happens when a web application allows users to upload any file without proper checks. Attackers can exploit it to upload malicious files like web shells, leading to remote code execution, data theft, or server compromise.

Several unprotected file upload scripts like this one were available in the app, deadly.
```php
$uploadDir = "./";
if(isset($_FILES['FileToImport']))
{
	$fichier = $uploadDir.basename($_FILES['FileToImport']['name']);
	if(move_uploaded_file($_FILES['FileToImport']['tmp_name'], $fichier)) {
		echo 'Upload effectué avec succés !<br>';
	}
	else {
		echo 'Echec de l\'upload !<br>';
		return;
	}
```


### System interaction

At worst, command injection vulnerabilities allows arbitrary execution of system commands from user input:
```php
$PID = $_POST['PID'];
exec("taskkill /F /PID $PID" );
```
An attacker could then inject any command to takedown the system.

Some critical admin tools were also publicly available without any authentication system.
No need to say what would happen if a malicious user found this:
![OptiMed services](/images/optimed-services.png)


## A huge attack surface

How many medical centers were using _OptiMed_, how many instances have been deployed and how many users would have been impacted by a leak?
Using some dorks on Shodan, including the good old [favicon hash trick](https://github.com/gwen001/favicon-hashtrick), I discovered about __190 medical centers in France__ and a few in Switzerland that were using this application.

![OptiMed Shodan](/images/optimed-shodan.png)

<div style="float:left;width:450px;">
    Using a SQL injection on all of them, I got a vague idea of how many users were at risk host by host.
    Numbers were scary, I definitely didn't expect that.
</div>

<img src="/images/optimed-count.png" alt="OptiMed count" width="400" style="float:right;" />

<div style="clear:both;"></div>

Finally all added, about __28 millions of patients and 77 millions of results of medical exams__.
This was huge!


## What this reveals

As an old school web developper and now security expert I can easily understand where the problems come from:
- No input validation
- No privilege separation
- Years of accumulated bad practices
- No code reviews, no security test
- Countless developers with no security knowledge

At that point we can just say that the software editor doesn't care as there was absolutely __no effort put in security at all__.


## What's next?

I reported the situation to ANSSI - France's national cybersecurity agency - in 2022 following responsible disclosure principles.

I did not access any personal data, nor did I modify, delete, or interfere with any system elements. My goal was purely to prevent a disaster.

As far as I can see now in 2025, many medical centers have change the software, for those who are still using it, I remain available to both the authorities and the vendor to help address this issue.


## Conclusion

Security in medical applications cannot be optional.
It must be integrated from day one, embedded in development practices, and regularly audited.

In a world where health data is a critical high-value target, _OptiMed_ is a textbook case of what not to do.