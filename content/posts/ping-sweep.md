---
date: 2015-03-13
title: Ping sweep
tags:
- pentest
- information gathering
see_also:
  -
    link: Introduction to pentesting
    url: /introduction-to-pentesting/
---
In the second part of the first phase of a penetration test, active information gathering,it's important to mapas accurate as possible the network of you target.
To find live hosts there isa technique called _ping sweep_.
Different tools exist to perform that task.

## Ping

A single `ping` can give you the ip address of the target, the ttl and the time. However since the methodis based on ICMP requests, the target could be configured to block or trash them... With a simple bash script, you can loop throught a range of ip address to test them all:

```bash
#!/bin/bash

for ip in $(seq 200 220); do
  ping -c 1 192.168.11.$ip | grep "bytes from" | cut -d " " -f 4 | cut -d ":" -f1 &
done
```

The `-c` option is configuredto only send 1 request. The grep is usedto display onlythe host who respond to the request (and supposed alive) and finally cut is used for a nice display. Notice the `&` at the end of the line, it's very usefull pto paralellize tasks and make the script faster. Here is the result:

![ping sweep ping](/images/ping-sweep-ping.png)

<!--more-->

## Netcat

The bad point of this method is that you mustchoose a port to test.
I usually use the HTTP port (80) but you'll have toadapt this option for your target.
Below a small bashscript:

```bash
#!/bin/bash

for ip in $(seq 200 210); do
  nc -n -v -z -w 1 192.168.11.$ip 80 2>&1 |grep open
done
```

`-n` : no DNS resolution, ip address must be provided
`-v` : verbose mode
`-z` : don't not prompt and wait anything
`-w` : timeout in seconde
`2>&1` : used to make the ouput grappable

The result:

![ping sweep netcat](/images/ping-sweep-netcat.png)

## Nmap

Nmap is so powerful that you don't needto write a script, a simple command can do the trick:

![ping sweep nmap](/images/ping-sweep-nmap.png)

`-sn` : ping scan, disable port scan

The grep and cut command are only usedfor a nice display ;)


## External resources

- [Introduction to pentesting](/introduction-to-pentesting/)
