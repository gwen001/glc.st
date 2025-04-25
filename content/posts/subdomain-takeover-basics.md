---
date: 2023-04-01
draft: yes
title: The basics of subdomain takeover
tags:
- bug bounty
- subdomain takeover
---
While subdomain takeover is very popular in the bug hunters community, it looks like some companies or bug bounty triagers have a hard time trying to understand what is it and more important what is the impact of this critical vulnerability.

In this post I will cover the basics of subdomain takeover and explain how a hacker could exploit it.
<!--more-->

The reasons I heard for now:
this subdomains is not in scope, it's out of scope
domains and subdomains are managed by anotehr entity
you use the same IP for testing (???)
it's the same subdomain you reported months ago, but on a different domain...
you can't access any data
we can't use our budget on this / this is not where we want to put the focus / we need to deal with the vulnerabilities that are within the main application first
we know we have problems with DNS records, but we have a very hard time to fix the ones we are aware of
So we are not ready to include this part in our scope.
we already have launched a big campaign targeting this type of issue
that subdomain was not use anymore so it's not important


not talking about NS takeover


## What is it?

This issue became famous in 2014. 


## How to find it?

The main steps are:

1/ find subdomains: brute force, scraping, DNS history, alteration
example of tools: subfinder, amass, OneForAll, SecurityTrails, Web Archive, DNSgen, regulator...

2/ resolve the subdomains found
example of tools: massdns, dig, puredns

3/ analyze the output of the resolver
There, it's up to know

OR

1/ find subdomains: brute force, scraping, DNS history, alteration
example of tools: subfinder, amass, OneForAll, SecurityTrails, Web Archive, DNSgen, regulator...

2/ fingerprint the subdomains alive
example of tools: massdns, dig, puredns

3/ analyze the output of the resolver
There, it's up to know


https://0xpatrik.com/subdomain-takeover-candidates/


## How to fix it?

Remove the affected DNS record — The simplest solution is to remove the affected record from the DNS zone. This step is usually used if the organization determines that the affected source domain name is no longer needed.

Claim the domain name — This means registering the resource in particular cloud provider or a case of a regular Internet domain, repurchasing the expired domain.




## How far can it goes?

aaa

SSL Certificates

https://0xpatrik.com/subdomain-takeover/


## What is the impacts?

aaa

Cookie Stealing

E-mails

