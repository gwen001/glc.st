---
title: "Security audits and penetration testing"
slug: "services"
date: 2026-10-29
lastmod: 2026-10-29
description: "Three security services: web application penetration testing, source code audit, Linux configuration audit. Which one you need, and why."
services_hub: true
---

Three services, three different questions. Most people who contact me know they
need _"something security-related"_ without knowing which of the three answers
their actual problem. This page is here to settle that in five minutes.

| Service | The question it answers | Duration |
|---|---|---|
| [Penetration testing](/web-application-penetration-testing/) | Where do they get in? | 3 to 10 days |
| [Source code audit](/source-code-security-audit/) | Why does that door exist? | 5 to 15 days |
| [Configuration audit](/linux-configuration-hardening-audit/) | How far do they get once inside? | 2 to 3 days per server |

## Web application penetration testing

I attack your application the way someone with bad intentions would, within an
agreed scope and schedule, and hand you the list of what is actually exploitable
— not the list of what could theoretically be.

For you if: your application is in production and nobody has ever tested it
seriously, or a client is asking you for evidence.

**[See the details →](/web-application-penetration-testing/)**

## Source code audit

I read your code, your dependencies and your Git history to find what is fragile
on the inside — including the paths a penetration test never walks, and the
secrets you believe you deleted.

For you if: you are taking over a codebase, preparing due diligence, or you want
to know whether a flaw found in one place also exists in the twelve others where
the same omission could have happened.

**[See the details →](/source-code-security-audit/)**

## Linux configuration and hardening audit

I measure the gap between your servers and a named baseline — the ANSSI-BP-028
guide, at a level we choose together — and deliver the fixes as replayable
Ansible tasks.

For you if: your servers have never been hardened, you are coming out of an
incident and want to verify that containment is real, or you are being asked to
demonstrate conformity with ANSSI recommendations.

**[See the details →](/linux-configuration-hardening-audit/)**

## Which one do you need

In the order I hear them most often:

**"Our app is live and we've never done anything about security."** → Penetration testing.  
It is the default starting point: it tells you where you stand before you invest anywhere else.

**"A client is asking us for a security report."** → Penetration testing.  
It is the deliverable that presents best to a third party, because it describes concrete attack scenarios rather than a list of settings.

**"We've inherited a codebase and the original authors are gone."** → Code audit.  
A penetration test will tell you nothing about the debt sleeping in the branches nobody executes yet.

**"We're acquiring a company, or raising money."** → Code audit.  
For the technical risk and the licence exposure.

**"We got breached last month."** → Configuration audit first, to verify the attacker has no path left, then a
penetration test to confirm the way in is closed.

**"The pentest found a SQL injection and it worries us."** → Code audit.  
One flaw found is a bug; the same flaw in fourteen places is a design problem, and that is a different conversation.

**"We need to demonstrate conformity with ANSSI recommendations."** → Configuration audit.  
With the target level of the guide fixed during scoping.

**"We keep finding the same categories of flaw at every audit."** → None of the three.  
What you need is [training](/courses/): paying to have the same mistakes pointed out every year costs more than learning not to write them.

**"We don't know, and nobody is asking us for anything."** → Then possibly nothing, for now.  
Write to me anyway, in two lines, and I will tell you straight — a service sold to someone who did not need it only pays once.

## The combinations that make sense

Penetration testing and the configuration audit complement each other well: the
first finds the entrance, the second determines how far it leads. It is the
sequence I recommend most often, and the second costs considerably less than the
first.

The code audit earns its place mainly **after** a penetration test, when you want
to understand the cause rather than the symptom — or **before** a major release,
when there is nothing to attack yet.

I do not sell a bundle of all three. If you run them in sequence, it is because
the result of the previous one justified it, not because it was in the quote.

## What I don't do

Better said before you write.

I work alone. For a very large scope, a multi-week team engagement, or work
requiring formal accreditation — a PASSI-qualified provider, a DORA threat-led
penetration test — I will say so in the first exchange and point you elsewhere.
My reports are audit reports, not certifications.

On systems, I cover Linux and containers. Not Windows, not Active Directory.

And I don't inflate severities. A publicly reachable admin login page is an
informational finding, not a critical vulnerability. By the third report where
everything is "high", nobody reads any more, and it is the real flaw that drowns.

## Let's talk

Describe your situation in a few lines — what the application or system does, who
uses it, what is prompting the question — and I will reply with the service that
fits, a proposed scope and a price range. If the answer is _"none of the three"_, I
will tell you that too.

**[contact@glc.st](mailto:contact@glc.st)**
