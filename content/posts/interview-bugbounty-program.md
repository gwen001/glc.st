---
date: 2018-09-10
title: Interview with a Bug Bounty program
tags:
- bug bounty
---
One month ago I started to chase on a new private program.
Since we were in touch by mail and since their office is pretty close to my place, I proposed to meet.
They immediatly accepted.
We talked for an hour, about security, my job, their program.
That was so interesting, question after question, we learned alot from each other.
I though it would be nice to share this experience, some people probably have some of these questions in mind, so in this article I tried to resume our interview.
<!--more-->


## About the company

Since it's a private program, I cannot reveal too much details about the company, I will call it: Monka.  
Monka is a french company, based in Paris. Created in 2016, they are now more than 100 employees.
Monka has developed a SaaS about accounting stuff.
Datas privacy is really important in this domain so in June 2018 they decided to open a bug bounty program on [Hackerone](https://hackerone.com/) in order to improve their security.
Needless to say that they have been very surprised by how things went.


## Me questions

__Q: How did you heard about bug bounty and Hackerone?__
  
This is something that have been recommended to us by a friend.
He works in a company that also have a program on Hackerone and he seems to be very happy about it.
<br/><br/>
__Q: What is your internal organization regarding security and how do you manage your bug bounty program?__  
  
For now we don't have a person dedicated to security.
We have selected 3 people from our tech team to manage our Hackerone account.
Every day a new person take a look at the tickets and assign the new ones to the concerned team.
<br/><br/>
__Q: Do you receive many reports?__  
  
Yes! We are overloaded, we didn't expect that.
It takes much more time than we expected and requires several qualities: development, security, community management...  
A security engineer will join us full time in 5 days and he will manage our H1 account.
Hopefully this will help to flush the queue of reports and triage faster in the near future.
<br/><br/>
__Q: What kind of issue do you receive?__  
  
We mainly have IDORs, some of them are really critical.
But honestly we also receive alot of low reports, what we call "noise".
<br/><br/>
__Q: Do you use an internal bug tracker?__  
  
Depending of the team we use Trello, Github issues and ZenHub extension.
But the single source of truth still Hackerone issues tracker.
We know that it's possible to integrate Jira with Hackerone, we are thinking about it.
<br/><br/>
__Q: What about the money, do you have a limit?__  
  
No, we don't have a fixed budget, we want to continue as far as we receive valid reports and see how it will go. 
It's good for us, we are very happy that hackers find issues.
It's easy to understand that it's much better for everyone that an honest security researcher find the holes in our system instead of a black hacker with bad intentions.
It's worse the money.
<br/><br/>
__Q: How many private invitation did you send?__  
  
We send 2 invitations by day.
We launched the program 3 months ago, so about 200 invitations now.
But at the end only few hackers work on our program (~10 thanks).
Many hackers refuse the invitation because of the language of our application (it's not english) or because of the specific domain we are evolving.
<br/><br/>
__Q: Have you been in trouble with any hackers?__  
  
Not so much, but managing the program require a good communication skill.
We had some problems because of the delay to reward, or because we didn't agree with the criticity of the bug.
We also "lost" some tickets when we stopped the report managment by the platform itself, it didn't fit to us.
Sometimes we spend more time on invalid reports than valid ones, trying to make everyone happy.
<br/><br/>
__Q: Do you communicate with your customers about your bug bounty program?__  
  
Not yet but this is something we have in mind for marketing purpose.
We will probably create a security page on our corporate website.
But we are afraid that hackers bypass H1 and use direct contact.
We couldn't accept bugs or reward them if the official program is paused.


## The company questions

__Q: How long do you work in the security industry?__  
  
I started security in 2015, I passed OSCP in September/October.
<br/><br/>
__Q: Do you perform bug bounty full time? When did you start?__  
  
Yes, I started bug bounty in Sebruary 2016 half time, and I perform full time since June 2017.
<br/><br/>
__Q: Does it pay well?__  
  
Depends, some months I find a lot of bugs, the company is very effective, fast to triage/fix/reward, and so nice bounties, and sometimes it sucks xD
Slow triage, low bounties, many duplicates...  
It's definitely not something linear.
<br/><br/>
__Q: How many programs do you work on at the same time?__  
  
I try to keep 4/5 programs in my pool (reality is more 3), plus the quick recon I always perform when I receive a new invitation.
<br/><br/>
__Q: Do you receive many private invitations? How does it work?__  
  
As far as I know, to receive private invites you need to be active (submit valid reports) and a minimum reputation.
That's why it's so important for hackers, otherwise they have to work on public programs and it's more difficult to find bugs.  
I usually receive 3/4 invites a week but then I have to filter according my own criterias.
<br/><br/>
__Q: Which are?__  
  
Technologies:
Since I'm a web developper, I mostly target webapps.
I also use mobile apps to find new informations like endpoints or S3 buckets, but I don't test the app itself.
No way for me to play with Python or C librairies :)  

Hacktivity: 
Quickly check if the program is dead  or alive. Who are my challengers and how many reputation points they got on this program.

Rewards:
Because I do it for living.  

And the most important SLA/response time:
I prefer to earn 1k$ in a week than 5k$ in a year #monthlybills
<br/><br/>
__Q: Do you play on managed program?__  
  
Honestly, as a hacker I try to avoid that...
This is something that researchers don't really enjoy.  
We never know if the triager will send the report to the company or not.
The point is that only the company is able to say if the bug is "by design", if they really own this bucket or not, if a P4 is more a P2...
Unfortunately there are some examples on Twitter where people complain about that :/  
For me it's also a nice part of the job, I mean in my previous developer life, I would never imagine that I would be able to talk with the tech team of such big companies like Yahoo, Uber, Deliveroo...
It's so interesting, I really love that.
<br/><br/>
__Q: Do you think we should change the scope to *.domain.com?__  
  
It's up to you. Are you ready to handle more reports?
Will you be able to fix all bugs found?  
Your `www` is managed by a third company service, will they fix the bugs?
If not you will just make angry hackers and kill your reputation.
<br/><br/>
__Q: Do you think we should award swag?__  
  
Definitely!
Researchers love swag, a little bonus because of the quality of the report, hoodies, stickers, whatever!  
You should take a look at the famous Dutch gouvernment :)
<br/><br/>
__Q: Are you in touch with other hackers?__  
  
Sure, I already talked with some hackers that also work on your program :)


## More

I have been to several meetings organized by bug bounty platforms where they try to convince guests to create a program (understand convert them as customers), and there is one question that always come back over and over.
<br/><br/>
__Q: What would prevent a hacker to use the issues for bad pruposes or publish/sell the datas on Internet?__  
Short answer: nothing.  

But what would prevent a hacker to do it right now, without bug bounty?
Maybe it's already done but you just don't know about it?
There is no more reason to attack your platform, and by attack I mean "perform bad things", with or without bug bounty.
In fact, it's exactly the opposite, hackers are more prone to help you if you have a security program.
They get money, they get reputation, they get recognition from the community, they can talk about their findings, they can find a job... 
So many pros to be a white hat instead of black.

But like every relationships, it works both way, as long as you act fairly the researchers have no reason to hate you, so keep cool and eat penguins :)
