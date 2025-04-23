---
date: 2019-03-24
title: The Hunter Games
images: [/images/platforms-tournament-calendar.jpg]
tags:
- bug bounty
- ctf
- hunter
- game
- project
---
What if bug bounty platforms had their own contest?
Imagine a tournament where bug hunters would be promoted and sponsored by platforms like in every (e-)sports.
Imagine an event where they could fight on a dedicated scope as it would be in a CTF event but with bounties or a Hackerone event but with a huge competition in the background.
This is THE HUNTER GAMES.
<!--more-->

<a href="javascript;:" id="lnk-goal"></a>
## Goal

Have fun.  
Promote the industry.  
Have fun!  
Earn money.  
Have fun!!  
Secure the customers.  
And have fun?


<a href="javascript;:" id="lnk-elligibility"></a>
## Elligibility

To participate, a platform must:

- be able to host a physical event. [See "The play"](#lnk-the-play).
- be able to sponsor a team. [See "Players"](#lnk-players).
- be able to provide a scope. [See "Scopes"](#lnk-scopes).
- be able to triage the reports regarding the scopes she provides. [See "Reports"](#lnk-reports).


<a href="javascript;:" id="lnk-players"></a>
## Players

Each team consists 5 players.
One of them must be an internal employee of the platform, to be easily noticeable he wears the number `00`.
The fourth others are hackers of the community selected by platforms themself using their own process.

<div  style="width:150px;float:left;margin-right:5px;">
  <img src="/images/teeshirt-bugcrowd.png" alt="platform tournament bugcrowd" />
</div>
<div  style="width:150px;float:left;margin-right:5px;">
  <img src="/images/teeshirt-intigriti.png" alt="platform tournament intigriti" />
</div>
<div  style="width:150px;float:left;margin-right:5px;">
  <img src="/images/teeshirt-hackerone.png" alt="platform tournament hackerone" />
</div>
<div  style="width:150px;float:left;margin-right:5px;">
  <img src="/images/teeshirt-yeswehack.png" alt="platform tournament yeswehack" />
</div>
<div  style="width:150px;float:left;margin-right:5px;">
  <img src="/images/teeshirt-yogosha.png" alt="platform tournament yogosha" />
</div>
<div style="clear:both;"></div>


<a href="javascript;:" id="lnk-random-draw"></a>
## Random draw

At the beginning of the season, a random draw is performed in order to create the calendar.
Teams are grouped two by two to decide the matches.

Game after game, the calendar is filled with the results, showing the progress of all teams.

<a href="/images/platforms-tournament-calendar.jpg" title="platform tournament calendar" target="_blank"><img src="/images/platforms-tournament-calendar.jpg" alt="platform tournament calendar" /></a>


<a href="javascript;:" id="lnk-the-play"></a>
## The play

A match is a set of 2 games.

The first game is hosted by the platform who has been choosen first during the random draw.
The platform who has been choosen second will host the second encounter.

It's the responsability of the host to prepare all technical details for both teams: place, network, electricity...

Points from both games are summed, the teams with the bigger score pass the round and is qualified for the next step.


<a href="javascript;:" id="lnk-game"></a>
## Game

A month before the game, a random draw is performed to choose a scope among the pool of scopes brought by platforms who do not participate that game.
Then this scope is removed from the pool.

Two accounts with the teams name are created on the platform who belong the randomly choosen scope.

A game is height hours long, no break.
Only issues reported during this period are used to calculate the points of the two fighting teams. [See "Scoring"](#lnk-scoring).
Any extra reports after the end of time are considered as null.

{{< figure src="/images/platforms-tournament-scoreboard.jpg" alt="platform tournament scoreboard" width="600" >}}


<a href="javascript;:" id="lnk-scopes"></a>
## Scopes

Accepted scopes are:

- Host IP: provide the CIDR
- API: provide the url
- Mobile app: provide the package name from Android Play Store and/or IOS App Store
- Web app: provide the domains and subdomains
- Source code: provide the url where it can be downloaded
- IoT: provide the product
- Car: provide the car

It's the responsability of the platform owner of the scope to provide all necessary credentials for 2 teams.  
It's the responsability of the platform owner of the scope to communicate with the customer the details regarding the game (date/time, ips...).


<a href="javascript;:" id="lnk-reports"></a>
## Reports

Reports are handled during the game by a triager employee of the platform owner of the randomly choosen scope, he is the referee.
This implies that he must be aware of the date of the game, as well as the customer.
It's the responsability of the platform owner of the scope to take care about the technical problems that could occur during the game.

While bounties can be awarded after the game, the severity of all reports is properly estimated a maximum of 1 hour after the end of the game.


<a href="javascript;:" id="lnk-confidentiality"></a>
## Confidentiality

It's the decision of the platforms with the agreement of their customers to disclose informations regarding their own scopes/reports.
Players are not allowed to reveal any details about the customers/scopes/reports outside the games.


<a href="javascript;:" id="lnk-scoring"></a>
## Scoring

Every accepted report is awarded with points according his criticity.
The official [Bugcrowd rating system](https://bugcrowd.com/vulnerability-rating-taxonomy) is used as a base reference to estimate severities.

P1 (critical): 100 points.  
P2 (high): 50 points.  
P3 (medium): 20 points.  
P4 (low): 10 points.  
P5 (informative): 1 points.  


<a href="javascript;:" id="lnk-winning"></a>
## Winning the game

The winner is the team who get the most points in a game.
In case of equality, the fastest report is awarded with a bonus of 100 points which is enough to get a winner.
