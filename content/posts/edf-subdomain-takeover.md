---
date: 2022-09-21
title: One takeover to rule them all
images: [/images/edf-subto.png]
tags:
- tools
- subdomain takeover
---
Because of Covid, the first quarantaine in France occured in March 2020.
During that time I wrote a Python script to detect Subdomain Takeover.
As I have been successful several times with the tool, one hit was especially beautiful.
<!--more-->

<span style="font-size:1.5em;">*The story of how I have been able to take control of 450+ subdomains of the national french electricity company [EDF](https://www.edf.fr/).*</span>

<a href="/images/edf-subto.png" target="_blank"><img src="/images/edf-subto.png" alt="edf subdomain takeover" height="450" /></a>

<br>
<hr>
I'm not going to explain what is subdomain takeover so take a look at the following articles if you want to know more:

[OWASP test guide](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover)  
[Hackerone guide](https://www.hackerone.com/application-security/guide-subdomain-takeovers)  
[0xpatrik basics](https://0xpatrik.com/subdomain-takeover-basics/)  
<hr>

<br>
As we all know there are several ways to takeover a subdomain but the easiest way is probably to buy an expired domain which is available for purchase.
<br><br><br>

<div style="float:left;width:420px">
    <b>Finding:</b><br>
    Using my tool <a href="https://github.com/gwen001/dnspy">dnspy</a>, I found that <code>nucleaire.edf.fr</code> was an alias of <code>edf-linkbynet.com</code>.  
    A quick search on Gandi and I realized that last one was available for purchase for about 12€/year.  
    Not knowing what would happen next, I bought the domain and configured it on my server to serve a nice PoC page.
</div>
<img hspace="10" src="/images/edf-subto-nucleaire.png" alt="edf nuclaire subdomain takeover" height="300" />
<div style="clear:both;"></div>

<br>
<div style="float:left;width:510px">
    <b>Traffic incoming!</b><br>
    A few days later, I checked my server logs to see if anyone requested the stolen subdomain.
    What was my surprise when I noticed all those requests for different subdomains I didn't know anything about.
</div>
<a href="/images/edf-subto-logs.png" target="_blank"><img hspace="10" src="/images/edf-subto-logs.png" alt="edf subdomain takeover logs" height="300" /></a>
<div style="clear:both;"></div>

<br><br>
At that time EDF had a private bug bounty program on the french platform YesWeHack.
I was not invited so I used the dedicated contact form on their website.
6 weeks later and several tries to get in touch with them, the problem was magically and silently fixed.

At the end more than 450 subdomains were redirected to my server.
Exploited by a malicious user, it could have been devastating: social engineering, phishing, cookies manipulation, fake payment system, company reputation...
(a simple *"thank you"* would have been appreciated).

Regardind the issue itself, my guess is a bad communication between tech team leaders.
One admin bought <code>edf-linkbynet.FR</code> and the other one configured all CNAMEs to point to <code>edf-linkbynet.COM</code>, as simple as that!


# The tool

The tool, called [dnspy](https://github.com/gwen001/dnspy), is composed of 3 modules:

1/ the grabber tries to find subdomains using external tools like subfinder, oneforall, github, amass... then alterations are created using altdns or dnsgen.

2/ the resolver performs DNS queries on all subdomains grabbed and generated using massdns, trying to detect dead hosts, cnames...

3/ the interpreter reads the output of the resolver and looks for possible takeover using the fingerprint file.
It's inspired of subjack with some extra features like regexps support and an ignore list.

The whole thing is daemon based and can be independently launched. Below some good findings.

<img src="/images/dnspy-acef.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-bred.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-ce-picardie.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-bpce.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-jeunesse-gouv.png" alt="edf nuclaire subdomain takeover" height="200" />
<img src="/images/dnspy-hillary.png" alt="edf nuclaire subdomain takeover" height="200" />
