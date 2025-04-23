---
date: 2019-12-16
title: "Bug bounty management, a bad example"
tags:
- bug bounty
- management
---
As I lately released an article about a [good bug bounty program example](/bug-bounty-management-a-great-example-zomato/), I also wanted to write something about what kind of behaviours companies should avoid to keep the community motivated.

In 2017, I worked on a great program, so great that I published an article about them called "[The Bug Bounty program that changed my life](/the-bug-bounty-program-that-changed-my-life/)".
Since I was very successful on that program, some weeks ago I decided to give another try.
I quickly checked the policy on their Hackerone page and noticed some changes in the rules.
I quickly reported some small bugs and also noticed some changes in the reports management process.

Unfortunately all those changes now make the program **terrible**.
Here is my analysis.
<!--more-->

## History

First of the all, I'm going to talk about a private program, since I don't want to violate Hackerone privacy policy (and even if you can list all programs through the sitemap #bugbountytips), I won't use the real company name here.
So I will call it: **Willoz** (be creative).

According to [Crunchbase](https://www.crunchbase.com/), Willoz is a group with 12 acquisitions.
As far as I know, this group currently has 11 programs on Hackerone, one per acquisition, all private, all created between July 2016 and February 2017 and all following the same rules.
However 5 of them are "taking a break" which means that you're not supposed to submit a report unless you find a critical vulnerability (the report form is still available) or direct contact the company through the emergency email they provide: bugbounty@willozgroup.com.

As I previously said, my personal experience with this company has always been great.
My reports always handled in a week, very quickly rewarded, most of time within 2 days because of the "payment on triage" rule they tend to apply.
Since I was good enough, they created a private Slack channel with the head of brand security, the application security lead and myself.
Here we shared our thought and they gave me some private scopes to play with, at that time no mentionned on Hackerone, where I was very successful again.
I now have these people listed on my LinkedIn.

Let's try to figure out what happened.


## Rewards

First thing I immediately noticed was the changes regarding the bounties.  
Since August 2019: **_$4,500, $2,250, $1,125, $450_**  
Prior that date: _$1,000, $500, $250, $150_ || _$1,500, $750, $375, $250_  

Wow! Such an increase after 3 years!  
It means that they greatly improved their security so they're not scared to pay more for good bugs and even the smallest.
Hackerone minimum is 500$ as well as Facebook, so 450$ is very nice.
They might be very confident, no low bugs anymore (at least what they think) or very rich.

Anyway, this is the normal evolution of a bug bounty program.
It usually starts with a linear approach and once that most critical bugs have been found <u>and fixed</u>, **you can increase the top bounties to put a stronger accent on high severity bugs**.

![bounty policy](/images/willoz-bounty-policy.png)

So where is the problem?  
The problem is not the amounts.
There is simply no point to raise the bounties so much if at the end your goal is to avoid payment as much as you can, using tons of excuses or crazy policy.
Another possibility is that they don't get so much reports so they need to motivate hackers to work on their programs.


## Management

Right after I sent my first report I got a message from _"HackerOne staff"_ saying my report has been validated.
Ok so the **programs are now managed** by Hackerone.
How bad is it?

It's not necessarily bad but it's far from good to me.
It means that the reports will be handled very quickly, but not necessarily in a proper way.
Triagers are not aware about all details about the security of the company, they have to learn/ask what to do with that kind of bug, should it be rewarded or not, how much...
When they take decision by themself it often leads to big mistakes.
I think I'm safe to say that hackers usually don't like (understand hate) managed programs.

![triager complaint](/images/willoz-triager-complaint.png)

Second point is that **it breaks the link between the security team and hackers**.
This is one of the reason I like bug bounty: being in touch with the employees of companies all over the world and help them to fix their bugs, talk with them, share.
In this case, I lost that trusted relationship built with time in a second.


## Targets

Every acquisition has her own website and that domain is the main scope of the program as well as the mobile app if any.

In the good old times it was possible to report an issue regarding any websites belonging to the group using the main program.
I already reported critical bugs on domains not listed in the program and been rewarded as it should.
Unfortunately it looks like they decided to review this position and made a step back.
I reported a subdomain takeover which was surprisingly declined while it was accepted 2 years ago.  

Don't get me wrong, I am not blaming the company because they follow their own rules, that would be stupid.
The domain is not in scope, I should not expect anything, ok, fair enough.
But being very permissive, allowing something and suddently change your mind without a proper commuication first is not a good idea, specially with people who trust you, and more, when you already rewarded those peoples in the same situation.

When you step in the bug bounty area, you should **create your first program with a limited target list and increase the scope with time**, not downgrade it.


## Scope and Out of scope

I'm not going to reveal the whole history of the policies but here are the most important steps.

The programs started with the basic exclusions:

+ DoS
+ Version disclosure
+ Reports from automated tools or scans
+ Social engineering
+ Physical attempts
+ Content spoofing / text injection
+ Login / logout CSRF
+ Clickjacking
+ ...

At the end of 2018 they added:

+ Punycode attacks
+ Rate limit
+ URL injection where HTML injection is not possible
+ Password reset token leaked to 3rd party services

In 2019, major changes occured:

+ Staging, Test, and Demo environments are out of scope
+ Cross-Site Scripting (XSS)
+ Cross-Site Request Forgery (CSRF)

While some companies accept all kind of bugs, it's pretty common that issues like clickjacking, text injection or login/logout CSRF are considered out of scope when not leading to critical findings.
I don't see any problem there.
However it's pretty unusual to exclude XSS, CSRF, especially <u>after 3 years</u>!

Months after months, years after years, a bug bounty program evolves, so it's absolutely normal to also see the out of scope section evolving as well.
Willoz group isn't an exception, they improved that part of the program, but a bit too much, it should go the other way.
As they already fixed the major bugs and probably a big part of the mediums (XSS and CSRF in scope for 3 years), the program is supposed to be more and more permissive, they are supposed to **accept a bigger variety of bugs**.

Moreover they also decided to exclude several environments, it suddently widely reduces the scope of the tests.
Again **the scope is supposed to grow up** as the program is more and more mature and the security team get the experience.


## Unresponsive

Since the programs are managed by the platform, the reports are first handled by the platform triagers.
According to Hackerone, when a program is managed, the security team of the company still have access to all reports and so can step in at any moment to talk with the hunters.

As a good hunter, when I don't get any answer after a few days/weeks, I throw a ping in the report to catch up the attention of the triagers.
I can say that **being ignored is the most frustrating feeling so far**.
Not only the security team but also the platforms triagers are now unresponsive.

Obviously companies has priorities, an open redirect is nothing compared to a RCE, but that's it, it's part of the game, every reports should be managed the same way.
And again after 3 years, that company is supposed to have a very good internal worklow, **the internal process should be very well oiled**.
Ignoring a 2 month report with 3 dunning notices is a big mess, you're not gonna earn the community confidence with that kind of behaviour.

Side note: when you send a message to the emergency email provided by the company in their rules, you get the following response:
![automatic reply](/images/willoz-autoreply.png)

Two words: DEAD END.  
What's the point to set some stupid requirements based on meaningless numbers to finally say to send a message to the exact same email???
It's a waste of time for everyone.


## Punishment

It was a pure accident that I was contacted by their top hacker at that exact same moment I was struggling with Hackerone mediation.
He is a very well known hunter in the community performing bug bounty full time as far as I know, and he was hacking on Willoz's programs since the beginning (2016) so we can say that he also <s>has</s> had a good connection with them.

As specified in the rules:

> Any unauthorized attempts that bypass the HackerOne platform and trying to reach out directly to previous / current employees of Willoz Group in regards to any bug-bounty-related discussion could result in termination from our Bug Bounty Program. Please keep all communications in the HackerOne platform.


<div style="float:left;width:450px">
    Because he couldn't see his reports solved, the hacker tried to contact the company by email.
    What should happened finally happened, he has been banned.
    <br/><br/>
    What a mistake from the company.
</div>

<img src="/images/willoz-unhappy-hacker.png" alt="unhappy hacker" width="350" style="float:right;" />
<div style="clear:both;"></div>

**Ban should be considered as the very last option** and a hacker with 3 years of experience on the programs who tried "direct contact" should be a big red alert for the security team, it means alot.


## Conclusion

According to all those changes, for me there is only one possible conclusion: the group hired a new security engineer in charge of the bug bounty stuff, he probably tries his best to manage all of this but <u>this is a big fail</u>. Bug bounty best practices are not met anymore when they perfectly were in the past:

- less targets, smaller scope: should be the very opposite after such a long time/strong experience.
- reward increase leads to reward escape: pointless.
- dead relationships with hackers

**Communication is the main key in bug bounty.**
No matter if a report is accepted or closed, payed or not and how much, the company must _always_ leave a word to inform the hacker about the status of his/her ticket.
And <u>this is the job of the company managers, not the platform triagers</u>.

When I start hunting on a new program I always report some "minor" bugs to check how responsive is the team and see if it's worth digging.
In this case, I reported 1 subdomain takeover, 4 XSS and 4 open redirect, only 2 of them have been accepted.
Next...
