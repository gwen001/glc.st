---
date: 2020-01-16
title: "5 things to do before running your first bug bounty program"
tags:
- bug bounty
- management
---
Welcome 2020!
I will start this year with a set of articles about program management.
In this one, I will try to explain some important things all companies should do BEFORE running a first bug bounty program.
Since there is no official rules in bug bounty, we miss that, at the end you can always do whatever you want.
So those articles are more about best practices/good tips.


# Do pentest/audits

Test before the test.  

You have issues, yes you have, a lot, you just don't know yet.
Some of them can easily be detected during a pentest or code audit, the basics can also be found by most of the automated scanners used by security companies.
Once they have been identified, you basically have two options:

- fix them
- add them to the OOS/know issues/won't fix section of your program policy

If you don't do anything, keeping them "for later", hackers will find them, report, and you will have to pay for this, at least the first times, to avoid hackers anger.
It's not acceptable to mark those reports as N/A, acting like "yeah yeah we already know that" and not pay, it will kill your reputation.
You also don't want to waste your time again and again to manage reports about the vulnerabilities you're already aware of.

<u>Jumping into bug bounty without a proper security background will literally ruin everything.</u>


# Prepare your technical infrastructure

Hackers hack, be prepared!

No matter if you mention to refrain the use of automated scanner, hackers use tools, they generate alot of traffic.
You probably want to avoid DDOS as well as you don't want to pause your program because your system is not able to handle so much traffic.
Doing that would stop hackers in their run, you will lose their precious motivation.
It's frustrating as hell to be forced to stop hacking a target when you're already on something interesting.

Plus, there is no point to open a bug bounty program if your hackers are banned after an hour by your firewall and you don't have any easy way to bring them back in the game.
You don't want to manually manage a huge blacklist of IP addresses.

![ie service unavailable](/images/ie_service_unavailable.png)


# Think your internal workflow

Hackers hack more, be more prepared!

As soon as your program is open, no matter if you choose to stay private or public, you will receive the first reports in less than an hour (~15/30mn).
It's very important to think your internal workflow **BEFORE** you open the program.
There is a strong chance that you get more reports in the next days.
First, you don't want to be overloaded, second, it's not acceptable to pause the program just because you thought you were not (so) vulnerable.

<div style="float:left;width:620px">
    Really, don't underestimate hackers.
    I saw too many programs "paused" because they were not able to handle the flow of reports constantly growing up.
    Because they though that they could manage all this shit only one day/week, because the security team was not ready, because the triagers didn't speak english, because the manager had to ask the permission for every single bounty, because they don't know how to forward the bugs to the dev team etc...
    <br/><br/>
    On the right side is an example of the workflow of a report I made to explain how it should be handled <u>on the platform</u>.
    Then you have to create your own internal process.
    <br/><br/>
    Most of the platforms allow the integration of external tools like bug trackers, Jira, Slack, Zendesk, GitLab and so on...
    Use this feature as much as you need to make your report management more efficient.
</div>
<a href="/images/report-flow.png" title="Report flow" target="_blank"><img src="/images/report-flow.png" alt="Report flow" width="200" style="float:right;" /></a>
<div style="clear:both;"></div>


# Study others programs

You're not the first one so learn from other programs, their mistakes, their successes.

- Read their policy or simply copy/paste/update to create your own.  
- Quickly check the bounty table to know what rewards they offer (money, swag...), even if it all depends of your own budget of course.  
- See what kind of scope they test: subdomain, wildcard, api, mobile...  
- Read carefully the out of scope section to avoid the common pitfalls, really, you don't want to spend your whole yearly budget for missing SPF or SSL certificates errors.  
- Read their disclosed reports, how they reply to hackers, the automated responses, the delay, their communication skills (appreciation, greetings, closing formula...).
(good examples: [Hackerone](https://hackerone.com/security/hacktivity), [Valve](https://hackerone.com/valve/hacktivity), poor example: [Twitter](https://hackerone.com/twitter/hacktivity))


# Join the community

If not done yet, joining the community on [Twitter](https://twitter.com/search?q=bugbounty) could be your best move ever.  

Start by following the platforms and some popular hackers in the bug bounty industry.
From here you will be able to better understand what are their expectations, their motivations, what they are looking for.
More, your security team will learn a lot from them, the current popular vulnerabilities, the tools they use etc...

Time after time you will follow more hackers, YOUR hackers.
You will be immediately warned if someone talk about your programs, good words or complaints are always important to improve yourself.


# Good reads

Information Security Assessment Types by Daniel Miessler:  
https://danielmiessler.com/study/security-assessment-types/#vulnerability

The hacker powered security report 2018 by Hackerone:  
https://www.hackerone.com/sites/default/files/2018-07/The%20Hacker-Powered%20Security%20Report%202018.pdf

Report workflow by YesWeHack:  
https://blog.yeswehack.com/2019/07/24/a-quick-update-on-our-point-system/
