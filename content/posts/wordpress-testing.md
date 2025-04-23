---
date: 2017-09-12
title: Wordpress testing
tags:
- wordpress
- upload
- rce
- sql injection
see_also:
  -
    link: OneLogin authentication bypass on WordPress sites
    url: https://hackerone.com/reports/136169
  -
    link: Image Gallery SQL Injection
    url: /cves/cve-2016-11018/
---
Here is the way I usually follow to test a Wordpress install.

## Information gathering

Get basic informations with [WPScan](https://wpscan.org/):
```none
wpscan -r --enumerate u --url http://www.example.com
```

If it can't retrieve the user list, try to use provided script `stop_user_enumeration_bypass.rb`
For each user found, try to brute with basic passwords:
```none
wpscan -r --url http://www.example.com --wordlist /Wordlists/Passwords/best1050.txt --username <username>
```

If the version of Wordpress has been found, download it from the official archive directory:
[https://wordpress.org/download/release-archive/](https://wordpress.org/download/release-archive/)

Open the tested website and take a look at the source to find those strings:
`wp-content/themes` and `wp-content/plugins`
This could reveal more themes/plugins.

<!--more-->

## Replication

For each themes found, download them from the official archive directory:
[https://wordpress.org/themes/](https://wordpress.org/themes/)  
Try to download the exact same version from the SVN repository (ex: [https://themes.svn.wordpress.org/matala/](https://themes.svn.wordpress.org/matala/))  
If you can't find it, try to download the version before and after.
If the theme can't be found or if it's not free, try to find it from Google or on some illegal website (take care of backdoors).

For each plugins found, download them from the official archive directory:
[https://wordpress.org/plugins/](https://wordpress.org/plugins/)  
Try to download the exact same version from the Trac browser (ex: [https://themes.svn.wordpress.org/matala/](https://plugins.trac.wordpress.org/browser/anual-archive/))  
If you can't find it, try to download the version before and after.
If the plugin can't be found or if it's not free, try to find it from Google or on some illegal website (take care of backdoors).

Once you have everything, open you local dashboard of your local install, and enable all themes and plugins you have downloaded.


## Find an exploit, the easy way

For each themes and plugins found, check on [Exploit Database](https://www.exploit-db.com/) if there is an existing exploit:  
![wordpress exploitdb](/images/wordpress-exploitdb.png)
If yes, try them all.


## Low hanging fruits

Open a terminal and find all files you've got with the themes and the plugins:
```none
find <your_install_path>/wp-content/themes/ -type f
find <your_install_path>/wp-content/plugins/ -type f
```
Save the result in a file and inject in Burp Intruder on your target. Check the response code, this will give you a good idea if you have downloaded the right things or not (you should get a lot of 200).

Check twice the result of PHP files, this could reveal a Full Path Disclosure:
![wordpress full path disclosure](/images/wordpress-fpd.png)

If you don't get anything, retry with all PHP files, even the Wordpress core.
Most of the time WPScan will detect it but it's worth the hit to try!
```none
find <your_install_path> -type f -iname "*.php"
```


## Code analysis

Time to dig deeper! For each themes/plugins, use regexp to locate dangerous functions.
Test everything on your local install before testing your target.

SQL statements:
```none
egrep -ri 'select|insert|update|delete' . | grep -i ".php" | egrep --color -i "select.*FROM|insert.*INTO|update.*SET|delete.*FROM"
egrep -ri '\->prepare|\->get_results' . | grep -i ".php" | egrep --color -i '\->prepare|\->get_results'
egrep -ri '\$_SERVER|\$_REQUEST|\$_GET|\$_POST|\$_COOKIE|\$_ENV|\$_FILES|\$GLOBALS|\$_SESSION' . | grep -i ".php" | egrep --color -i '\$_SERVER\[|\$_REQUEST\[|\$_GET\[|\$_POST\[|\$_COOKIE\[|\$_ENV\[|\$_FILES\[|\$GLOBALS\[|\$_SESSION\['
```

File system interaction:
```none
egrep -ri 'fopen|popen|file_get_contents|file_put_contents|fread|fwrite|fputs|fputcsv|fgetc|fgets|fgetss|fgetcsv|readfile|fpassthru|fscanf|move_uploaded_file|parse_ini_file' . | grep -i ".php" | egrep --color -i "fopen\s*\(|popen\s*\(|file_get_contents\s*\(|file_put_contents\s*\(|fread\s*\(|fwrite\s*\(|fputs\s*\(|fputcsv\s*\(|fgetc\s*\(|fgets\s*\(|fgetss\s*\(|fgetcsv\s*\(|readfile\s*\(|fpassthru\s*\(|fscanf\s*\(|move_uploaded_file\s*\(|parse_ini_file\s*\("
```

Command execution:
```none
egrep -ri 'eval|system|exec|shell_exec|passthru|escapeshell|proc_' . | grep -i ".php" | egrep --color -i "eval\s*\(|system\s*\(|exec\s*\(|shell_exec\s*\(|passthru\s*\(|escapeshell\s*\(|proc_\s*\("
egrep -ri 'assert|create_function|preg_replace|preg_replace_callback' . | grep -i ".php" | egrep --color -i "assert\s*\(|create_function\s*\(|preg_replace\s*\(|preg_replace_callback\s*\("
egrep -ri 'array_map|array_walk|array_walk_recursive|call_user_func|call_user_func_array' . | grep -i ".php" | egrep --color -i "array_map\s*\(|array_walk\s*\(|array_walk_recursive\s*\(|call_user_func\s*\(|call_user_func_array\s*\("
```

File inclusion:
```none
egrep -ri 'include|require' . | grep -i ".php" | egrep --color -i "include\s*\(|include_once\s*\(|require\s*\(|require_once\s*\("
```

Custom headers:
```none
egrep -ri 'header|setcookie|content-disposition' . | grep -i ".php" | egrep --color -i "header\s*\(|setcookie\s*\(|content-disposition"
```


## Boring stuff

Finally, you can use the same technics you use on any other website. Try to find endpoints from Google, Wayback Machine, etc... and test SQLi, XSS and so on...
Far from exhaustive, you also need to check authentication deeper, specially if oauth is part of the game.


## External resources

- [OneLogin authentication bypass on WordPress sites](https://hackerone.com/reports/136169)
- [Image Gallery SQL Injection]({{< ref "/cves/cve-2016-11018" >}})
