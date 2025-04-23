---
date: 2018-06-15
title: Cons of Bug Bounty
tags:
- bug bounty
see_also:
  -
    link: Why Bug Bounty
    url: /why-bugbounty/
  -
    link: Why bug bounty hunters love the thrill of the chase
    url: https://www.csoonline.com/article/2846873/data-protection/why-bug-bounty-hunters-love-the-thrill-of-the-chase.html
  -
    link: Bug bounty is a joke
    url: https://medium.com/@phwd/bug-bounty-is-a-joke-91bc1875c890
  -
    link: Bug bounties, a broken system
    url: https://hackernoon.com/bug-bounties-a-broken-system-7781fe0bf7d
---
This article is the following of the previous one (obviously...) about [why I love bug bounty]({{< ref "/posts/why-bugbounty" >}}).
However I realized that that one sounds like everything is perfect in this job, but since the world is not whole pinky full of rainbows, and butterflies, and unicorns, I have to say the truth.
Long time resolution, loneliness, deception, companies, there are also bad points (or maybe I'm just frustrated to get so much duplicates these days xD).
<!--more-->


## Private invite

You think it's fictional? Mystical? Maybe Spiritual.

We all know that private invitations are very interesting for hackers, less competition, but it can be a terrible painful loooooonnnnnnnggggg way to get some.
Reputation, impact, signal, my statistics are the magic that decide if I receive private invitations or not. 
Before that, I had to work on public programs, those programs have been tested again and again by many great hackers.
Of course it's much more complicated when you are a beginner in bug bounty or in the all security industry.
Like everyone, you firstly try the basic stuff, that way you will have a hard time to find valid bugs in order to improve your stats.  
![My Hakerone statistics](/images/bb-cons-mystats.png)


## Duplicates

You have it or you don't that's a fallacy.

Because yes, I'm not alone in this world, I'm not the only hacker, I have opponents.
They also have skills, knowledge and imagination.
If I can find a bug they can do it too.
The competition is tough and I have to be the first one to report the bug if I want the money/reputation/swag/tee-shirt/pogs.
Hackers play from all around the world.
When I'm sleeping at 4am, there is someone on the other side of this planet that is already reconning the new public programs.
And when I'll wake up tomorrow morning, he will probably already have reported some subdomain takeover or vulnerable buckets.

Plus, some companies are late to fix the bugs, it's still possible to get duplicates of reports that are 6 month old...

![low SLA metrics](/images/bb-cons-myreports-status.png)


## Bounty deception

I ain't happy, I'm feeling glad.

I find a RCE, the company promises 5000$ for that kind of issue but I only get 500$.
What the Hell §@#&$£!!!!
The server is outdated since ages, there is nothing interesting/sensitive on it, it's outside the current architecture and maintained by an external group blablabla...
I'm fucked. Yes it happens, sometimes.

I find 5 XSS, I open 5 reports but 4 of them are closed as duplicate of the first one.
How could this happen??
if a single patch fix them all, that means they share the same root cause and same root cause implies a single bounty.
That's the rule in bug bounty.
Unfair? Your opinion.

The bug highly impact the main in-scope target but through an out-of-scope asset. Once again, it's up to the company to decide if it rewards the report or not.
Time for a nice "Bounty plz". Check this fantastic example of unfair decision by Twitter: [https://hackerone.com/reports/273698](https://hackerone.com/reports/273698)


## Companies

Pick and choose, Sit and lose.

There are many various reasons for why a company opens a bug bounty program.
The problem is that sometimes it underestimates the hard work and the responsabilities that this involves.
It desn't have enough people in their security team to absorb the flow of reports and most of the time those peoples already have a daily job, they are not dedicated to bug bounty.

What are the consequences?
[SLA metrics](https://www.hackerone.com/blog/Healthy-programs-make-happy-hackers-Introducing-response-SLAs) tumble down, the program becomes less and less reponsive, bugs are not solved, more and more duplicates, big delays in payments, frustration, angry hackers.  

![low SLA metrics](/images/bb-cons-sla.png)


## Burn your mind

No squealing, remember that it's all in your head.

So so much things to do. I want to do everything, I want to play with all programs, I want to recon all domains, I want to test all tools, I want to learn Python/Ruby/Go, I want to pratice CTF, my todo list is growing faster than Sharknado chapters.
But you know what? Sadly, there is only 24 hours a day :/

Even if I exchange alot with my similars, even if I'm active on Twitter and Slack, I mainly hunt alone.
Even if I train on dedicated platforms, even if I pratice CTF, I mainly learn alone.
That's it.

Sometimes I don't earn a single dollar for weeks while I see other people tweeting about the big rewards they got because of internal website with login/pass:admin/admin, then I question my whole life: WHY NOT ME §§§§ Why I don't find bugs like this, why I choosed this job, why it's raining today, why GoT is not on Netflix and why the hell my washing machine keeps eating my socks!!!!


## Conclusion

Now you shouldn't be scared.

As I previously said in the previous article I previously wrote, the best way to make your own opinion is to give it a try, also take care to read the stories linked at the very bottom of this page.


## External resources

- [Why Bug Bounty]({{< ref "/posts/why-bugbounty" >}})
- [Why bug bounty hunters love the thrill of the chase](https://www.csoonline.com/article/2846873/data-protection/why-bug-bounty-hunters-love-the-thrill-of-the-chase.html)
- [Bug bounty is a joke](https://medium.com/@phwd/bug-bounty-is-a-joke-91bc1875c890)
- [Bug bounties, a broken system](https://hackernoon.com/bug-bounties-a-broken-system-7781fe0bf7d)
