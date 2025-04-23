---
date: 2016-01-18
title: Sqlmap
tags:
- sql injection
- sqlmap
see_also:
  -
    link: Sqlmap official website
    url: http://sqlmap.org/
  -
    link: Sqlmap usage on the GitHub project
    url: https://github.com/sqlmapproject/sqlmap/wiki/Usage
---
Written in Python by [Miroslav Stamper](https://twitter.com/stamparm), Sqlmap is probably the best automated tool to detect and exploit SQL Injection.

Sqlmap fully supports many databases as MySQL, Microsoft SQL Server, PostgreSQL, Oracle (and many more) and is able to detect the following injection types : 
Boolean based blind, Error based, Union based, Stacked queries, Time based blind, Inline queries. Depending of the target status, sqlmap is also able to :

- prompt an interactive sql shell
- download/upload files
- prompt a web shell
- crack hashed password using a dictionnary attack
- and a lot more...

Below some examples of the main functions using [bWAPP](/an-extremely-buggy-web-app/)


## Basic usage

![sqlmap basic usage](/images/sqlmap-basic-usage.png)

In this example sqlmap has detected that the GET parameter `title` of the search function is vulnerable to sql injection. 
Well done! Plus it found that 4 different types of injection can be used for exploitation. 
Note that sqlmap has also detected that the parameter is vulnerable to XSS attacks which is unfortunatly very common these days...

To perform test on POST field you should write: `--data="title=sqlitest&action=search"`

In the next example, I'll turn off the verbose mode.

<br>

<!--more-->

## Information gathering

 Who am I ? Where am I ? What can I do ? Once you found an injection point, the next step is to gather as much information as possible about the running environment. 
 This task is very easy with sqlmap which provides a great list of options.

`--hostname`

![sqlmap hostname](/images/sqlmap-hostname.png)

`--current-user`

![sqlmap current user](/images/sqlmap-current-user.png)

`--privileges`

![sqlmap privileges](/images/sqlmap-privileges.png)

And many more... All collected informations are stored in log files in the `~/.sqlmap` directory. 
To get fresh results ignoring the results previously recorded you'll then have to use `--fresh-queries` or `--flush-session` or simply remove the corresponding directory.


## Enumeration

 The next step of the exploitation is to enumerate available databases, tables and columns. Again this is pretty simple with sqlmap.

Databases: `--dbs`

![sqlmap databases](/images/sqlmap-databases.png)

Tables: `-D bwapp --tables`

![sqlmap tables](/images/sqlmap-tables.png)

Columns: `-D bwapp -T users --columns`

![sqlmap columns](/images/sqlmap-columns.png)

Check the official documentation to see the list of all available options.


## Getting datas

Now you know the structure of the tables, you can grab all datas you want. To perform that task, I like to use the interactive sql prompt:

![sqlmap datas](/images/sqlmap-datas.png)

You can also dump a table or a database with the option `--dump` on the command line. If sqlmap detects hashed strings, it will ask you to crack them or not via a dictionnary based attack.
 The default wordlist is +1M length, it's up you to fill it or provide your own list.

![sqlmap crack](/images/sqlmap-crack.png)


## Manage files

Depending of the sql user privileges , the injection type and the filesystem permissions, you maybe could be able to play with the server.

Read file:

![sqlmap file read](/images/sqlmap-file-read.png)

Write file:

![sqlmap file write](/images/sqlmap-file-write.png)

The option `--os-shell` allows you to upload an ASP/JSP/PHP script on the remote server which will render an upload form. 
Then you'll use it to upload whatever file you want, usually a backdoor to perform a POC for your client.


## Usefull

Below my favorite options of sqlmap, the ones I always use in all pentest.

`--threads=10` : to speed up sqlmap

`--tor --tor-type=socks5` : to anonymize the traffic, must be used carefully while performing time based tests

`--random-agent` : a different User-Agent for each session

In this time where security is the heart of our job, day after day more and more network admins like to implements WAF, IPS or IDS. 
Sqlmap has an option to bypass basic configuration of those systems: `--tamper` allows you to trick the requests by applying a filter on the SQL string like random case.


## Going further

Sqlmap is a very very powerful tool. After months I still don't know all options but here are some extra functionnalities who look interesting:

Interaction with Metasploit framework

Smartphone impersonation

Website crawling

Windows registry access


## External resources

- [Sqlmap official website](http://sqlmap.org/)
- [Sqlmap usage on the GitHub project](https://github.com/sqlmapproject/sqlmap/wiki/Usage)
