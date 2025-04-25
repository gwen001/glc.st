---
date: 2022-08-01
title: "The underlying rules of bug bounty"
tags:
- bug bounty
- management
---
Bug bounty programs come with platforms, scopes, and payouts - but also with a set of unofficial rules.
These invisible norms shape how hackers and companies interact, from communication and trust to report etiquette and rewards.
This post sheds light on the real-world dos and don’ts that every bug hunter and program owner should know.
<!--more-->


# 3 entities, 3 jobs

The bug bounty industry is mainly composed of three big entities with each a specific job:

**Programs: the need.** Companies who want to test their security.  
Besides triage, companies have alot to do.
Create well detailed policies to ensure that hunters will focus on the most interesting issues.
Prepare the internal workflow of all concerned teams to handle the flow of reports.
Estimate the severity of every reports and consequently reward and finally, <u>fix the bugs</u>.

&gt; [5 things to do before running your first bug bounty program](/5-things-to-do-before-running-your-first-bug-bounty-program/)  
&gt; [5 things to avoid in bug bounty](/5-things-to-avoid-in-bug-bounty/)  
&gt; [How to keep hackers motivated in bug bounty](/how-to-keep-hackers-motivated-in-bug-bounty/)

**Hackers: the solution.** Nerds who want to use their skills to get some cash.  
Their work is to find bugs of course, but not only.
Writing a good report is very important to ensure companies understand what it's about.
It's also the job of the hackers to prove the severity of the bugs <u>as well as the business impact</u> which is the main criteria to decide the bounties.
Finally check the fix deployed, which is not mandatory but still appreciated by companies.

&gt; [How to write a Bug Bounty report](/how-to-write-a-bug-bounty-report/)

**Platforms: the glue.**  
The popular bug bounty industry as we know it today, probably won't exist without platforms.
However it doesn't mean they are Kings/Queens, they still have alot to do for the two other entities.
Their job is basically to serve companies AND hackers by creating a trusted relationship between all entities.
<u>They have an important role at preparing companies</u> to the incoming unexpected situations, like a mentor.
And since they are literally selling hackers, it's the job of the platforms to ensure that they are <u>respected and rewarded</u> for their work.

&gt; [List of bug bounty platforms](https://github.com/disclose/bug-bounty-platforms)


# Respect

Yes it's a real rule and one of the most important!  

It's obvious, should be natural but I see too many reports where people don't understand each other and quickly come to rude reactions.
When you come in such situation where you don't understand the decision of the other part, sit on their chair for a minute, it could change your mind.


As a hacker, you should understand that programs have to deal with a huge amount of reports.
They have to prioritize so no need to ask for update every 2 hours, wait at least 1 week.
If the reward is not what you expected, re-read your report, maybe it's not clear enough, maybe you didn't show the real impact of the issue.
From my experience, programs always agree to review the severity (and sometimes the bounty) if you are able to find the good arguments, but **staying professional is a condition**.

<div style="float:left">
    <img src="/images/weekend-update.jpg" alt="weekend update on report" />
</div>
<div style="float:right">
    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">I have a new found respect for triage teams for bug bounties. Oh my gods the stuff they have to put up with.</p>&mdash; Alyssa Herrera🇩🇰 🏳️‍⚧️ (@Alyssa_Herrera_) <a href="https://twitter.com/Alyssa_Herrera_/status/1260656416118849536?ref_src=twsrc%5Etfw">May 13, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</div>
<div style="clear:both;"></div>

As a program, you should understand that hackers spend alot of time trying to find bugs, it's very time consuming.
So it's always a pain to see a report being refused because it's something you were aware of 6 months ago, but didn't fix...
When your dev team is able to escalate a bug from P2 to P1, be honest and adjust the reward accordingly, hackers don't have access to all informations you have.
Also you should strongly consider to <u>reward on triage</u>, kudos don't pay the bills as well as $$$ do.

If none of you is able to handle the situation in a proper way, no need to come to an offensive language or punishment.
Remember that <u>respect works both way</u>, you can't ask for it if you shit on peoples, so keep calm, open a ticket on the platform support.
It's their job to handle such situations, kind of referees, they are (well) paid for that (% of reward + companies subscription), so use them as much as you can, no matter on what side you are.


# Reputation isn't everything

**High reputation doesn't imply good hacker.**
The goal here is not to detail all reputation systems but only demonstrate some pitfalls.

The first point is, only few cases lead to a loss of reputation, which is ok, but that means if you're not a crazy spammer but a normal person - as normal as hackers can be - your reputation will only go up. With time, you'll reach those famous 5k, 10k, 15k...

Then, if we take a look at Hackerone, we can notice that a valid critical report is 57 points while a low report is 17 points.
On YesWeHack, an accepted critical report is 50 points and a low one is 15.
Basically on both of them **3 low reports are worth a single critical** -> 3*CRLF = 1*RCE.

On some platforms like YWH, the variable gain is calculated depending the reward itself and not the severity, but a 500€ reward could be considered as critical on program X and considered as low by program Y, right?

Moreover, a report with a 50€ reward will get the same points than a 500€ report, and a 2000€ report will get the same points than a 10000€ report.
In that way, Hackerone system is a bit better since the severity used in the formula is based on the rewards and relative to the program configuration.

<div style="float:left">
    <a href="/images/hackerone_reputation.jpg"><img src="/images/hackerone_reputation.jpg" alt="hackerone reputation" height="450" /></a>
</div>
<div style="float:right">
    <a href="/images/yeswehack_report_workflow.jpg"><img src="/images/yeswehack_report_workflow.jpg" alt="yeswehack report workflow" height="450" /></a>
</div>
<div style="clear:both;"></div>

Finally, you have to know that reputation is like money, the more you have, the more you'll get.
As soon as as hacker reach a high ranking place, he becomes bankable for platforms.
He is promoted, he gets more private invites, private scopes, interviews, events, which imply more bugs, more reputation, not matter the relevance of his reports.

Don't get me wrong, I'm not saying that top ranked hackers are all farmers, no, but some are.
And what?
It's ok, it's part of game, it's not a problem as soon as everyone is aware about that.
So read carefully the numbers, understand what they mean.

Fortunately some platforms implement other metrics like <u>signal</u>, <u>impact</u>, <u>acceptance rate</u>, those are good quality indicators.
As a program manager, you may prefer to rely on them when it comes to invite hunters.

&gt; [Signal and Impact on Hackerone](https://docs.hackerone.com/hackers/signal-and-impact.html)  
&gt; [How we Measure Researcher Performance on Bugcrowd](https://www.bugcrowd.com/blog/how-we-measure-researcher-performance/)


# CVSS isn't everything

As described on the [official website](https://www.first.org/cvss/calculator/3.1):

> The Common Vulnerability Scoring System (CVSS) is an open framework for communicating the characteristics and severity of software vulnerabilities. CVSS consists of three metric groups: Base, Temporal, and Environmental. The Base group represents the intrinsic qualities of a vulnerability that are constant over time and across user environments, the Temporal group reflects the characteristics of a vulnerability that change over time, and the Environmental group represents the characteristics of a vulnerability that are unique to a user's environment. The Base metrics produce a score ranging from 0 to 10, which can then be modified by scoring the Temporal and Environmental metrics.

Here again I'm not going to explain in details what is CVSS and how it works but only demonstrate some pitfalls that make it pointless in some situations, because yes, I'm telling you loud and clear:  
**CVSS is not the only source of truth.**

Spot the difference:

<div style="float:left">
    <img src="/images/cvss-rce2.jpg" alt="cvss rce" width="420" />
</div>
<div style="float:right">
    <img src="/images/cvss-rce2.jpg" alt="cvss rce" width="420" />
</div>
<div style="clear:both;"></div>

Don't twist your mind, it's the same image.
But imagine for a second that the first screenshot concerns a production web server or a database server, and the second one comes from a test server only hosting the vulnerable script, no data at all, no connection to any network.
CVSS is the same but would it be the same impact ?
Because of that, bounties may vary alot.

In the red corner, we then have vary famous vulnerabilities like <a href="https://www.cvedetails.com/cve/CVE-2016-3714/">ImageTragick</a> and <a href="https://www.cvedetails.com/cve/CVE-2017-5638/">Struts</a>, both have a CVSS score of 10.0 because of the deadly impact.
Those 2 issues have been widely reported and <i>successfully exploited</i> on many systems.  
In the blue corner, we have another famous issue called <a href="https://www.cvedetails.com/cve/CVE-2019-0708/">Bluekeep</a> which also deserves a good 10.0 because of the severity of the <i>potential</i> impact.
It has been reported by many whistleblowers.
But wait!
Problem is, when it was publicly revealed, there was no wild exploitation reported, not even a public exploit.
So does it deserve the same attention?

We can also talk about the SSL/TLS issues all hunters try to monetize at least once: CRIME, PODDLE, BREACH, DROWN...
Very famous, danger is real, but who is going to exploit such vulnerabilities?
It's now considered out of scope by most of the security programs.

Hunters: don't be mad if the bounties don't reflect the <i>potential</i> severity of the bugs you report.
<u>Companies have to consider</u> the real risk, <u>the business impact</u> in case of exploitation against their systems/users.

Companies: it works both way. 3 "low" bugs, individually rated 3.0 could lead to a disaster when chained, scoring a nice 10.0.
For example: login/logout CSRF + open redirect + self XSS.
So don't be narrow.

&gt; [Don’t Be Misled by CVSS Scores](https://securityboulevard.com/2020/03/dont-be-misled-by-cvss-scores/)  
&gt; [Pitfalls of Vulnerability Rating](https://www.troopers.de/media/filer_public/cb/f0/cbf0181b-e9d8-408c-aef3-651bfd114d76/troopers13-pitfalls_of_vulnerability_ratinga_new_approach_called_errs-ernw_rapid_rating_system-michael_thumannmatthias_luft.pdf)


# Duplicates

Companies often encounter a problem when it comes to manage duplicate reports, here
are my recommendations to deal with that.

**Case 1: single hunter, global issue.**  
If it’s a global issue in the target, then only <u>1 report should be payed</u>.
The hacker doesn’t really need to open several reports but if he does then he should get a unique good reward by taking care to explain why.
Example: no CSRF protection, all forms in the website are vulnerable, it’s a global issue.

**Case 2: single hunter, specific issue.**  
The rule: <b>1 fix → 1 bounty</b> applies.
Example: 5 SQL injections found in 5 endpoints, urls are pretty much the same, as well as the vulnerable parameter.
If, in the background the same function is called and so 1 fix in that function solves the problem, then the rule <i>“same root cause”</i> applies, hunter is rewarded only once all other reports closed in a way he will not lose any point.
However if the code need to be patched 5 times, then all 5 reports should be rewarded <u>according their individual severity</u>.

**Case 3: several hunters, same issue**, the most common situation.  
The popular rule in the bug bounty industry applies: <b>first come, first payed</b>.
However if a hunter provides more information than his predecessor(s) (report is well detailed, you learned something new...), then <u>he deserves a bonus</u>.
Example: hunter 1 reports a CSRF in a form, hunter 2 reveals the CSRF in the whole website, then <u>both deserve a reward</u>.

Whatever the situation, a hunter should always be invited to the original report, or at least get the #id, this is the mininum.
Note that, platforms all have different ways to handle dups.
From a bonus related to report status to a reputation loss, everything is possible, when finally, hunters don't have any control on this, it's more a pain than anything else.

<img src="/images/duplicates.jpg" alt="duplicates" />

Now let's talk about the reason why duplicates are so common. There is basically 3 reasons:

- **Poor policies.**
Some issues the company is aware of because of a preceding pentest perhaps.
Some bugs not considered as real bugs, known issues/won't fix/let's see how it goes/it's not that bad/yeah later...
Hackers are kind of magicians, yeah, but they are not mind reader.

- **Overloading.**
A program just starts, all hackers in the community rush the target, or only the invited ones, or their bots.
No matter, in the next days reports will rain.
A company who wasn't well prepared won't be able to handle all of them in an appropriate amount of time.

- **Unfixed bugs.**
The ultimate shame.
What's the purpose of a bug bounty program then?
It's a lack of respect towards hackers who spend their time finding issues in a system.
Hunters: avoid these programs as much as you can.


# Safe Harbor

Depending of the domain (bank, insurance, government...), it can be pretty hard for a company to open a bug bounty program because of the laws that apply to the domain: tracable users, logs saved n months/years, geographic restrictions and so on...
Because of that, platforms put many efforts to provide the needed services: VPN, logs, backups, hackers selection (no it's not only about reputaton)...

The purpose of all of this is of course to follow the laws, but it's also a way for companies to track what have been done and by who.
Imagine a situation where a hacker reports a SQL injection and the developpers notice that some records have been deleted by "someone".
Imagine a customer platform freezed few minutes after the first launch, they may want to know what happened and who is responsible.

So companies have ways and tools to legally protect themself in case of trouble.
But what about hackers ?
Remember that hackers are the fundations of this industry.
No hackers, no bug bounty.
So what about protecting them a little bit?
As bug bounty is growing super fast all over the world, **companies and hackers need to find a way to agree on the legal terms**.
This is what Safe Harbor is about.

> To encourage research and responsible disclosure of security vulnerabilities, we will not pursue civil or criminal action, or send notice to law enforcement for accidental or good faith violations of Microsoft Bug Bounty Terms and Conditions.

As a white hat we all have been in this situation, stressed by the reaction of a company contacted because of a bug found on their website.
From thanks to nothing to legal proceeding, everything is possible, you never know...
Few months ago, [Chloé Messdaghi](https://twitter.com/ChloeMessdaghi) had launch a petition to support ethical hackers.

&gt; https://www.change.org/p/organizations-support-ethical-hackers

Problem is that, even as a part of a bug bounty program, hunters are not always safe and well considered as they should be.
By the end of 2017, [Kevin Finisterre](https://twitter.com/d0tslash) encountered a complicated situation as he was unfairly threaten by DJI after he sent a 30+ pages report.
His story has been mentionned in a presentation by [Amit Elazari](https://twitter.com/amitelazari) at USENIX Enigma 2018.

&gt; [Why I walked away from $30,000 of DJI bounty money](https://regmedia.co.uk/2017/11/16/whyiwalkedfrom3k.pdf)  
&gt; [Hacking the Law: Are Bug Bounties a True Safe Harbor?](https://www.youtube.com/watch?v=riZIFOw0pJA)

Safe Harbor is real help as soon as it's included in the security policies.
In my opinion it should be mandatory in every bug bounty programs on every platforms.
For now, it's still pretty rare.
Hackerone, Bugcrowd and Intigriti are the good examples.

<img src="/images/intigriti-safe-harbour.png" alt="intigriti safe harbour" />

&gt; [Hackerone: offer white hats a safe harbour](https://www.digitalnewsasia.com/digital-economy/hackerone-offer-white-hats-safe-harbour)  
&gt; [GitHub Bug Bounty Program Legal Safe Harbor](https://docs.github.com/en/site-policy/security-policies/github-bug-bounty-program-legal-safe-harbor)  
&gt; [Safe Harbor for Security Bug Bounty Participants by Mozilla](https://blog.mozilla.org/security/2018/08/01/safe-harbor-for-security-bug-bounty-participants/)


# Bounty on triage

Bounty on triage should be the regular way, seriously.

Unless the secteam needs more informations to reproduce the bug, hunter job is done as soon as the report is accepted so <u>he should be immediately rewarded</u>.

Take a minute to think about people who perform bug bounty as a full time job, think about beginnners.
Hunters are not like big companies with a huge cash flow!
They are normal peoples with duties and responsibilities **#paythebills**.


# Disclosure

Disclosure is a good marketing move as it shows companies concern about security.
It also helps to promote programs with the community.
As reports are one of the best resource for hackers to learn, as they get better from them, it can be seen as good investment.

Sharable informations: 
- quantity of reports
- % of accepted reports
- total rewarded
- average bounty
- vulnerability types
- full reports
- …

Disclosure can be community reserved or public.


<style>
    iframe { width: 400px !important; }
</style>
