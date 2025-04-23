---
date: 2019-03-01
title: How to write a Bug Bounty report
images: [/images/report-h1-empty.png]
tags:
- bug bounty
- writing
- report
---
One of the first thing I learned when I started security, is that the report is just as important as the pentest itself.
Some bug bounty platforms give reputation points according the quality.
While there is no official rules to write a good report, there are some good practices to know and some bad ones to avoid.
Mines are probably not the best but I never had any problem with any company, it's also pretty rare that the secteam asks for more informations since I try to detail as much as I can in the initial report.
Let me give you some tips and the global pattern of my templates.
If you like it, use it, if not, then create your own :)
<!--more-->


## General rules

<span style="color:#4BAE50">**Be polite!**</span>
Yes there are humans behind computers and they are not your enemies. 
Be nice, say _hi/hello_, _please_, _sorry_, _thank you_, _bye/best regards_ etc...
Use the basic words as you would do in real life. At the end, they are supposed to give you some money, plus you can be totally wrong at any point, so this is the minimum you can do.

<span style="color:#4BAE50">**Use markdown formatting.**</span>
Many platforms use [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) as a text formatter. Use his easy syntax to make your report clear.
- create paragraph and skip lines to break the text
- use bold to highlight **important things**
- use inline code for `parameters names` and `values`
- use block code to take advantage of the language colorizer

<span style="color:#4BAE50">**Oops!**</span>
If you notice afterward that you forgot to mention an important detail or if you finally discover that the bug is not a bug at all but an intentional behaviour, then feel free to add a comment to explain the situation.
The team will thanks you million times for all extra infos you provide saving their precious time.

<span style="color:#EE3B3B">**Don't be "too much".**</span>
Being _friendly_ doesn't mean that they really are your friends, avoid things like "_Sup mate!_".
Remain professional!  

<span style="color:#EE3B3B">**Avoid multiple issues in a single report**</span>
It's confusing for you and the triager and could possibly leads to smaller rewards than separated reports.
Unless you can chain several vulnerabilities of course (for example: OR + CSRF + XSS) and then submit a report with a bigger impact (and so increase the bounty).

Note that all informations are important, however the first things that triagers see are: <u>date</u>, <u>title</u>, <u>status</u> and <u>criticity</u>. Since you don't have any control regarding the date and the status, title and criticity deserve your attention.


## Rating

Take time to rate the issue, in an **<u>obvious</u>** way.
Too low, there is a chance that the secteam pass over it, but you could be happy if finally the bounty is higher than your expectations.
Too high, the secteam could think that you overrated in order to increase the bounty, they will notice, lower the rating, lower the bounty and you will be disappointed.

<img src="/images/report-cvss.png" alt="report cvss" width="220" style="float:left;" />

<div style="float:right;width:600px;">
    Sometimes optional, sometimes mandatory, if available, try to fill the <a href="https://www.first.org/cvss/calculator/3.0">CVSS</a> score.
    Even if it's not perfect and context dependent, it gives a good idea of the criticity of the issue <u>in a technical point of view</u>.
    Note that some platforms award bonus points for that.
</div>

<div style="clear:both;"></div>


## Title

It has to be simple but clear, explain what about is the report in one single line.
It should contain the type of the vulnerability, the potential impact and what asset is concerned. If the program is big enough and many assets are in scope, consider to add the name of the asset as a prefix, this will help the team to sort the reports.

<span style="color:#4BAE50">**Good:**</span>  
Open redirect + Stored XSS in profile lead to account takeover on www.example.com  
[192.168.1.1] Public Jenkins instance leads to RCE

<span style="color:#EE852F">**So so:**</span>  
XSS on www.example.com  
PHP errors reveal webapp full path  

<span style="color:#EE3B3B">**Forget it:**</span>  
XSS  
local file inclusion   
critical bug on www.example.com  


## Introduction

The introduction is basically a reminder of the title a little bit more verbose, but no technical details at all.
You can also write a quick explanation of the class of the vulnerabilty.
Yeah some vulnerabilites are not so famous, and sometimes triagers are not infosec people.
Perhaps they don't know about Appcache and never heard about that ImageTragick bug.


## Description

In a nutshell, the full explanation of the vulnerability.
Name the variables, their values, provide endpoints and all conditions required to trigger the issue: what, when, where, who etc...
The whole everything.


## Steps to reproduce

The goal here is to help the team to reproduce the bug in an easy way.
Give them the whole process step by step using an ordered list so you could reference any step at any moment.  

1/ Connect to your account: [https://www.example.com/login](https://www.example.com/login)  
2/ Click on the _"profile"_ tab  
3/ Enter value `payload` in the input `input`  
4/ repeat step 2  
...    

If you use a local proxy like Burp Suite, you can provide the request in a `http` block code.
It's very easy to reproduce the issue that way, you simply need to copy/paste it back to the software, update the cookie or any auth token and that's it, simple and efficient.

{{< highlight http >}}
GET /directory_traversal_1.php?page=/etc/issue HTTP/1.1
Host: bwapp.test.net
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:56.0) Gecko/20100101 Firefox/56.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Cookie: _ga=GA1.2.1344627302.1544974513; _gid=GA1.2.395150311.1544974513; PHPSESSID=cfpt3iskot4sfbjhjvf192je2f; security_level=0
Connection: close
{{< / highlight >}}

Providing the response is also a good thing to show the team the difference between a legit result and an unexpected behavior.


## PoC

As we often say, a picture speaks a thousand words. Provide everything that can prove the bug.
One of my previous article about [Colorize your hunt](/colorize-your-hunt/) can help to visualize things.
Also, keep in mind that the report can be publicly disclosed in the futur, so take care of hidding personal informations you want to keep private.

**Videos** that replays the whole drama that leads you to this great report.

**Screenhosts** that you can quickly modify with an image editor in order to highlight payloads and datas extracted.
No need to be a great designer here.

<div  style="width:400px;float:left;margin-right:20px;">
  <a href="/images/report-poc-xss.png" title="poc example xss" target="_blank" ><img src="/images/report-poc-xss.png" alt="poc example xss" /></a>
</div>
<div  style="width:400px;float:left;">
  <a href="/images/report-poc-rce.png" title="poc example rce" target="_blank"><img src="/images/report-poc-rce.png" alt="poc example rce" /></a>
</div>
<div style="clear:both;"></div>


## Mitigation

Trying to stay obvious and honest, if you think that some technical details make the issue very hard to exploit then it's important to let the team know about it. For instance a RCE that can only be triggered in January, between 12h and 2am at full moon night (don't laugh this is how I was imagining bug bounty when I started :x).


## Remediation

Do you have any idea on how to solve the problem ?

This is greatly appreciated by companies, they will be happy to read your tips/recommendations.
Remember that bug bounty is also about learning (for both parts).


## Impact

It's the job of the hacker to prove the criticity of the vulnerabilities he finds.
Try to create a possible scenario showing the potential risks of the issue.
But take care to not fall to the "Hollywood syndrom", there is only a few chance that a Full Path Disclosure leads to an earthquake followed by a tsunami, stamping out the human race.
Seriously, it's not gonna happen.  

![2012 movie](/images/2012-movie.jpg)


## Additional notes

Sometimes you have to provide small details that can be helpful to the team to better understand the issue, why it works most of the time but fails in a specific case.
Some weeks ago I reported a P1 to a program regarding their user permissions.
The developper was not able to reproduce the issue.
I simply added a tiny detail in my report about a minor change that occured some days before and they immediately understood the bug.  

The faster they reproduce the issue, the faster your report will be triaged, the faster you will be payed :)


## References

This is where I put links to external resources: owasp, blog articles, CVE, disclosed reports, real study case or whatsoever that can support my reports.
The goal is to help the team to understand and fix the issue but also show her the criticity.


## What's next?

<div style="float:left;width:550px;">
    <span style="color:#EE3B3B"><b>Do not harass.</b></span>
    Do not ask for update every single day.
    You're probably not the only hacker working on this program, they probably receive alot of reports.
    They have to prioritize the tasks.
    Acting like a kid does not only discredit yourself but the whole community.  
    <br /><br />

    <span style="color:#EE3B3B"><b>Do not harass more!</b></span>
    Do not spam the company over and over with the same report closed as informative or N/A in order to change their mind about it.
    They won't unless you give important additional informations.
    If you really feel that something is going wrong with this report/program then consider to request an intervention from the platform itself.
</div>

<img src="/images/yoda-medit.jpg" alt="Yoda mediting" width="250" style="float:right;" />

<div style="clear:both;"></div>

<span style="color:#4BAE50">**Be patient :)**</span>
I personally ping every 2 weeks when no news.
One of my report has been fixed and rewarded 2500$ after 1 year...
Patience is a vertue in bug bounty.



## Conclusion

As I said in the intro, the report is just as important as the pentest itself. You better to spend time on it trying to show the real severity of your bugs in order to get bigger bounties. Remember that non accepted reports (oos, duplicate...) can be rewarded if you are able to improve the security of the company whatever the way (by telling them something they don't know for example).

Consider to create templates to save time with the basics (introduction, courtesy, references...), then you could think about automation. Frans Rosén developped awesome tools to perform that kind of task, as [template-generator](https://github.com/fransr/template-generator) and [bountyplz](https://github.com/fransr/bountyplz).

Finally, let's quote one the biggest actor in the bug bounty industry:
<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/BugbountyProTip?src=hash&amp;ref_src=twsrc%5Etfw">#BugbountyProTip</a> The technical part is only 50% submission success. The other 50% is the write-up &amp; talking about impact (without doing it).  Spend time describing the vuln &amp; what exactly the worst case scenarios are with it. Speak to the developer, not the security engineer.</p>&mdash; Jason Haddix (@Jhaddix) <a href="https://twitter.com/Jhaddix/status/1021470944265531392?ref_src=twsrc%5Etfw">July 23, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

