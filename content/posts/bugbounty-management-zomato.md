---
date: 2019-11-19
title: "Bug bounty management, a great example: Zomato"
tags:
- bug bounty
- management
---
Since I decided to work on my advisor position in the bug bounty industry, I wanted to know more details about what makes a good program, what are the important keys and what to avoid.

I contacted Prateek Tiwari, lead security engineering at [Zomato](https://twitter.com/Zomato), he was nice enough to answer all my questions.
Let's discover how Zomato became one of the best program, not only on Hackerone but in the whole bug bounty industry.
<!--more-->

<a href="https://hackerone.com/zomato" title="Zomato on Hackerone"><img src="/images/zomato.png" title="Zomato on Hackerone" alt="Zomato on Hackerone" /></a>


__Can you introduce yourself please?__  
I'm Prateek Tiwari from India and I go with my handle [@prateek_0490](https://twitter.com/prateek_0490) on all platforms.
Been into hacking for about 2.5 years now. I started working with Zomato from Mar'18.
Here, I'm responsible to lead the security engineering team to develop a common solution and infrastructure.


__Can you shortly describe Zomato?__  
Launched in Delhi 11 years ago, Zomato has grown from a home project to one of the largest food aggregators in the world.
We are present in 24 countries and 10000+ cities globally, enabling our vision of better food for more people.
We not only connect people to food in every context but work closely with restaurants to enable a sustainable ecosystem.


__How many people are you in the security team?__  
We're still a small team, we're a team of 6.


__What kind of audit do you perform?__  
We've contracted with agencies to perform VAPT every quarter.
Moreover, we perform internal audits which include white/black box, red team exercise.


__Can you share some stats about the bug bounty program?__  
At the beginning of every month, we publish all these details on Twitter, SLAs are also available on [our policy page on Hackerone](https://hackerone.com/zomato).

<div style="float:left;width:550px">
    Valid Submissions for 2019: Q1:28 | Q2:16 | Q3:28 | Q4:15  
    <br/>Average bounty/quarter in 2019: around 13~15K$  
    <br/>Biggest bounty in 2019: <a href="https://hackerone.com/reports/512968">4500$ for a business logic error</a>  
    <br/>Average time to triage: 2 hours  
    <br/>Average time to bounty: 1 day  
</div>

<img src="/images/zomato-stats.png" alt="Zomato stats" width="270" style="float:right;" />
<div style="clear:both;"></div>


__What kind of major changes occured in the program since 3 years?__  
Internal process is where we focused more initially.  
It was important to work across the aisle of departments within the organization to get everyone on the same page and to make them understand about the security risks/threats.
We wanted to ensure that both our employees and vendors are working within the framework of a policy we've laid out.
Once we started getting everything in place, we changed the bounty structure and increased the scope.  

We also ran a couple of promotions and rewarded researchers with bonuses which are important to attract top researchers and make them focus on your program.
On that note, another important thing is shorter turnaround times - this was another area where we have done a lot of homework.
Still, a lot of changes need to be done and we're on it.


__As far as I can see the program is not managed by Hackerone, any reason for that?__  
We use to be a managed program but then we decided to move to non-managed because of a couple of reasons:

- The number of reports we receive was quite manageable by our internal team.
- Since we have our own team, it's easy for us to understand the reports/risks and plan on the mitigation steps.


__How do you to rate the issues?__  
We see how many users will get affected, if it is a direct exploit, if there are any pre-requisites to exploit the issue, etc.
Like, an SQLi is straight P1, an IDOR leaking users PIIs is P1, and so on and so forth.  

We've defined our rating policy on [our page on Hackerone](https://hackerone.com/zomato) as well as the bounty table.

![Zomato](/images/zomato-bounty-table.png)


__How do you handle the bugs internally?__  
Proactively and effectively :)  
From report to Triage, probably it takes less than 1 hour.
If it's a P1 we fix it within mins to an hour - it all depends on severity and workaround.
We also use Jira for tracking bugs internally.


__What kind of bugs do you receive the most?__  
I'll be honest, the most we receive are NA's.  
Currently, the valid reports we receive have decreased drastically, so there is nothing like "most kind of bugs".
But on average what we received the most is like CSRF and XSS.


__Any changes planned in the near future?__  
Yes, increase scope, increase rewards and a live event (should happen soon).


__Do you have any advice for companies who run or want to run a bug bounty program?__  
For companies who already run or want to run a bug bounty program, here are few things which you should consider:  

- Quick Response Time.
- "Fix your bugs", researchers will lose interest if you're not fixing your bugs.
- Bounty on Triage, this will really attract a lot of researchers, the reason being researcher has given you the report which means he has vested his time/efforts, don't make them wait for months for their reward.
- Clear scope/policy + bounty table.
- Take regular feedback from hackers and retain your hackers, making sure they don't lose out interest from your program.


__Any words for hackers?__  
Thanks to all the hackers that have participated in the program to-date.  
Our bug bounty program with HackerOne will continue to evolve as we strive to strengthen our relationship with the hacker community with every report.


## Conclusion

After some hard years in the bug bounty industry, tables have turned, Zomato greatly improved their internal process to manage the reports.
SLAs talk by themself, ATT:2hours/ATB:1day is kind of a dream for everyone who plays bug bounty, companies and hunters all included.
They also changed their way to communicate with hackers, and hackers are thankful for that.

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr">17 hours to triage, resolve, and reward! That’s how fast <a href="https://twitter.com/Zomato">@Zomato</a>’s program on <a href="https://twitter.com/Hacker0x01">@Hacker0x01</a> is.

I’d like to also give a shoutout to <a href="https://twitter.com/EdOverflow">@EdOverflow</a>.
In that narrow timeframe, he managed to mentor me!</p>&mdash; Karim Rahal (@KarimPwnz) <a href="https://twitter.com/KarimPwnz/status/1190585460072038405">July 23, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

Remember that the more hackers like your program, the more they will work on it, meaning more bugs found -> enhanced security!

<br />
Thanks to [Prateek Tiwari](https://twitter.com/prateek_0490).
