---
date: 2020-01-30
title: "5 things to avoid in bug bounty"
tags:
- bug bounty
- management
---
The second article of a serie about bug bounty management.
This one give some tips to companies about what kind of mistakes they should avoid to keep good relationship with hackers.
<!--more-->


## Unfixed bugs

Hackers work on your program, they could choose another one but they chose you.
They spend time trying to find issues in your system, which is the purpose of a bug bounty program obviously, the minimum you can do is to fix what they report.
Leaving bugs not fixed, reports not solved, keeping them "for later" will only lead to more reports about the same bugs over and over.  

It's a waste of time for hackers, a waste of time for your triage team, it's a lack of respect.
Remember that depending the platform, hackers can lost reputation for duplicate, N/A.
**It will simply ruin you reputation**, hackers won't work for you anymore.
What was supposed to be a security improvement will become a loss of money and the good marketing move will become a bad ads campaign for you, in 2 words: total mess.


## Being quiet/unresponsive

Feeling ignored is the worst. Really.

Like it or not, it's your job to handle every single report.
By handling I mean answer, and by answering I mean manually reply.
The automatic message sent by the a bot you configured cannot be considered as report management.
It just confirm that the report has sent/received but not that the team is working on it.

Hackers spend time to find bugs, they take time to write reports, so it's normal to take some time to tell them about their evolution.
Every action must be commented (status change, reward, public disclosure...) as well as no action at all after a while.

Moreover, when hackers send a ping to know about it, you're also suppose to reply.
Even a single confirmation that the ticket has been reviewed, your team is working on it, you need time to decide the bounty, whatever...
**Communication is the most important.**

Of course the delay of response is also an important key.
This is the main sign of responsivity/efficiency of your program.


## Being inconsistent

Clearly not the easiest part and even if it's seems pretty obvious, take care to correctly evaluate the severity of the issues as well as the rewards, especially if you're prone to publicly disclose the reports.

What does that mean?
Rewarding 300$ for a full path disclosure is very nice but then you'll be in trouble if you reward 100$ for a XSS.
No one would understand that decision.

To help you in this task, some platforms provide a table of vulnerability rating, kind of listing where issues are ordered by severity.
Another way is to use the [CVSS score](https://www.first.org/cvss/calculator/3.0) combined with a good bounty policy.
No matter what solution you choose to use, remember that the context and the impact are always two important parameters to consider.
For instance, a RCE on a forgotten server, isolated from your network, without any data stored isn't as critical as a RCE on the production server of your main site.

<img src="/images/bugcrowd-vrt.jpg" alt="bugcrowd vrt" />


## Being too strict regarding the scope

No matter what scope you configured, you'll get out of scope reports for sure, but it doesn't mean that they don't deserve your attention.
It's your decision to accept or not a bug in an asset which is clearly not included in the scope (or even excluded) BUT if the impact of the issue is one of the main asset in scope, then you better **think twice and reconsider your position**.

<u>Being too strict</u> regarding the scope, ignoring or not rewarding critical bugs that affect your service/product <u>will make hackers angry</u>, they will not understand your decision.
Take a look at this [horrible example by Twitter](https://hackerone.com/reports/273698) where the hunter found a bug that allows to read private tweets through `niche.co` which was marked as <i>not eligible for bounty. Thanks for helping keep Twitter secure!</i>

This is how I see the things:  
<img src="/images/bugbounty-scopes.jpg" alt="bug bounty scopes" width="600" />


## Bad words / punishment

Wisely choose your triage team.

As said in the previous paragraph: communication is THE key.
Being a good triager requires several skills: patience, communication, good english, patience, community management (IT), technical knowledges, patience...
Triagers are the interface of the company, they represent your name, your image depends on them.

**Losing patience, being mad at the hackers, saying bad words, threats must be avoided.**
Again it would kill your reputation if your triagers don't respect the hard work of the hackers.

Bans are pretty rare, but sometimes it happens.
If you're close to reach that situation, take a moment to understand why things are so complicated.
Remember that some hunters perform bug bounty full time, it's a very tedious job where money and reputation are the heart of this job, nerves can be affected.


## External resources

- [Bugcrowd’s Vulnerability Rating Taxonomy](https://bugcrowd.com/vulnerability-rating-taxonomy)
- [Zerocopter's Vulnerability Price List](https://www.zerocopter.com/vulnerability-price-list)
- [Bounty formula by EdOverflow](https://github.com/EdOverflow/bounty-formula)
