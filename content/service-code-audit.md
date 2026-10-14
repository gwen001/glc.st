---
title: "Source code security audit"
url: "/source-code-security-audit/"
date: 2026-10-13
description: "Source code security audit by an independent expert: manual review, dependency and secrets analysis, prioritised findings and retesting. Quote on request."
keywords: "source code security audit, secure code review, code audit, independent cybersecurity expert"
service: true
---

A [penetration test](/web-application-penetration-testing/) tells you what an attacker can reach today. 
A source code audit tells you what is fragile underneath - including the parts that are not reachable yet, but will be after the next release.

The two answer different questions. This page is about the second one: **where does this codebase break, and why does it keep breaking the same way?**

## Why now

Three numbers frame the problem.

**Your code is mostly other people's code.** 
The [2026 Open Source Security and Risk Analysis Report](https://www.blackduck.com/resources/analyst-reports/open-source-security-risk-analysis.html), 
built on 947 audited codebases between November 2024 and October 2025, found open source components in **98%** of them. 
**87%** contained known open source vulnerabilities, **78%** at high risk and **44%** at critical risk. 
The mean number of vulnerabilities per codebase reached **581, up from 280 a year earlier** - a 107% jump. 
And **93%** of codebases carried components with no development activity in over two years: not vulnerable yet, simply unmaintained, 
which is the same problem with a delay.

{{< figure alt="Verizon bdir" src="/assets/img/services/2026-open-source-security-and-risk-analysis-report.png" width="500" >}}

**Flaws are not being fixed.** 
Veracode's [2026 State of Software Security](https://www.veracode.com/resources/analyst-reports/state-of-software-security-2026/) 
puts security debt - flaws left unresolved more than a year after discovery - at **82% of organisations**, up from 74% the year before and 71% in 2024. 
Critical security debt now affects **60%**, against 50% last year. High-risk flaws are up 36% year over year. The problem is no longer detection; it is prioritisation.

**Secrets leak from the inside.** 
[GitGuardian's 2026 report](https://www.gitguardian.com/state-of-secrets-sprawl-report-2026) 
counted around **29 million secrets** detected on public GitHub in 2025, a 34% increase. 
More to the point for an audit: **internal repositories are roughly six times more likely to contain hardcoded secrets than public ones**, 
and **64% of valid secrets leaked in 2022 were still not revoked in 2026**. 
The same report measured AI-assisted commits leaking secrets at **3.2% against a 1.5% baseline**, twice the rate.

That last figure matters if your team has adopted AI coding assistants in the past two years. Volume went up; review did not.

## When to run a code audit

- **Before a security-sensitive release**: a payment flow, an authentication rewrite, a multi-tenant feature, an API opened to partners.
- **Technical due diligence**: you are acquiring, investing in, or inheriting a codebase and need to know what you are signing for, 
including licence exposure. 68% of audited codebases had licence conflicts in the OSSRA 2026 dataset, the highest rate it has ever recorded.
- **Taking over a legacy codebase**: new CTO, new team, outsourced development brought back in house, original authors gone.
- **After a penetration test**: when a finding's root cause is unclear, or when you want to know whether the same mistake 
exists in the twelve places the test did not reach.
- **Because development got faster**: heavy AI assistance, a sharp increase in merged pull requests, or a review process that quietly stopped happening.
- **For an obligation**: [GDPR article 32](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679) 
requires regularly testing the effectiveness of technical measures; 
NIS2 and DORA reinforce that for entities in scope; enterprise customers increasingly ask for an SBOM and a code review attestation.

## What I review

**Authentication and authorisation logic.** Not whether a login exists, but whether every path that should check a permission actually does. 
Missing checks cluster on the routes nobody demonstrates: admin endpoints, internal APIs, queue workers, scheduled jobs, export and import features. 
This is still OWASP's A01 category in the [Top 10:2025](https://top10.owasp.org/2025/) and it remains the richest vein in real codebases.

**Input handling and injection sinks.** Tracing untrusted data from entry point to sink, rather than pattern-matching on function names: 
SQL and NoSQL queries built by concatenation, command execution, template rendering, deserialisation, dynamic includes.

**Cryptography and secrets handling.** Password storage, token generation and entropy, encryption key management, 
signature verification that returns early, JWT validation that trusts the algorithm header.

**File handling.** Upload validation, storage location and permissions, how uploaded files are served back, path construction.

**Business logic.** Step sequences that can be skipped or replayed, quantity and amount handling, state machines with unreachable guards, 
race conditions on shared resources. No tool finds these, and they are often the most expensive flaws in the codebase.

**Dependencies and supply chain.** Direct and transitive components, known vulnerabilities, unmaintained packages, lockfile integrity, build-time substitution risk.
OWASP moved software supply chain failures to A03 in the 2025 Top 10, and the [DBIR 2026](https://www.verizon.com/business/resources/reports/dbir/) 
found third-party involvement in 48% of breaches, a 60% increase year over year.

**Secrets, including in git history.** A secret removed from the working tree is still in the history, still valid, and still indexable.
I check both. I have written and published the [tooling](/projects/) for this side of the work, and found live cloud 
credentials in client repositories more than once.

**Configuration and infrastructure as code.** Dockerfiles, CI/CD pipeline definitions, Ansible playbooks, environment handling. 
Pipelines with write access to production are a target, and they are rarely reviewed.

<br>

> PHP and Laravel in depth, JavaScript and TypeScript (Node), Python, Bash, SQL, and
infrastructure as code (Docker, Ansible). For a codebase dominated by Java, .NET, Go or Rust, 
I will tell you up front and point you to someone better placed - a shallow review in a language
I do not live in is worth less than no review.

## How it runs

**1. Scoping** - repositories in scope, languages, approximate size, branch or tag to audit, access method (a read-only deploy key is preferred, 
no copy leaves my encrypted workstation), non-disclosure agreement, and a named technical contact for questions.

**2. Architecture pass** - entry points, trust boundaries, data flows, the authorisation model as it is actually implemented rather than as documented. 
This phase decides where the remaining time is spent, which is why it comes before any tooling.

**3. Automated coverage net** - static analysis, dependency scanning and secret detection, run to make sure nothing obvious is missed. 
Output goes into triage, never into the report as-is. A tool's finding is a lead, not a conclusion.

**4. Manual review of critical paths** - the core of the engagement, guided by the 
[OWASP Code Review Guide](https://owasp.org/www-project-code-review-guide/) for method, 
[OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/) for the level of rigour, 
and the [CWE Top 25](https://cwe.mitre.org/top25/) and OWASP Top 10:2025 for coverage of weakness classes.

**5. Verification** - for each serious finding, a working proof on a local instance where possible. I would rather hand you one demonstrated flaw than five theoretical ones.

**6. Report and debrief** - written deliverable, then a working session with the developers, in French or English.

## The report

- **A one-page summary** for management: what was reviewed, what the real risks are, what to fix first, and an honest statement of coverage limits.
- **Each finding** with its location (file and line, commit reference), the CWE class, a severity contextualised to your application, 
the attack scenario, and a concrete remediation - in your language and your framework, not a link to a generic article.
- **Recurring patterns**, which is where a code audit earns its price. One missing authorisation check is a bug; the same omission 
in fourteen controllers is a design problem, and the recommendation is different.
- **A dependency inventory** with what to upgrade, in what order, and what breaks when you do.
- **Secrets found**, with an urgent revocation list separated from the rest. That section goes to you by encrypted channel before
the report is finished if anything live turns up.

<br>

> A publicly reachable admin login page is an informational finding, not a critical vulnerability.
Inflating scores fills the report and empties its value: by the third audit where
everything is "high", nobody reads it any more, and the one genuinely critical
finding drowns in the noise. Accept short reports when the application is in good shape.

## What a code audit is not

- **It is not a scanner report with a cover page.** If you wanted SAST output you can run it yourself for less than my day rate. What I sell is the triage.
- **It is not exhaustive.** A bounded budget over a codebase of any real size means prioritised coverage. 
The report states explicitly what was reviewed in depth, what was reviewed superficially, and what was not opened at all.
- **It is not a replacement for a penetration test.** Code review misses deployment and configuration reality - 
the staging database reachable from the internet, the forgotten subdomain. The [pentest](/web-application-penetration-testing/) misses 
what the code does on paths it never walks. Run both, in that order of preference depending on which question is keeping you awake.
- **It is not a code quality review.** I will not reformat your code or argue about architecture, except where the architecture *is* the vulnerability.
- **It fixes nothing by itself.** Recall the 82% security debt figure above: the bottleneck is not finding flaws, it is acting on them. 

## Why me

- **Auditor for the French Ministry of Armies, 2020 to 2024**: code audits, configuration audits against ANSSI recommendations, 
and penetration testing on Linux systems and web applications.
- **A developer for about 25 years before that**, across several French web companies. I read your code without a translator, 
and my recommendations take into account what they cost to implement, which is the part most security reports get wrong.
- **Bug hunter since 2016**: several hundred reports to programmes at EDF, Decathlon, Payfit, BlaBlaCar, Uber, Yahoo, Zillow and others, 
including cloud secret leaks and source code exposure. Published [CVEs](/cves/) under my own name, most of them found by reading code.
- **Author of the tooling**: I wrote and published the [scripts and extensions](/projects/) I use to hunt secrets, endpoints and exposed repositories.
- **Trainer since 2022**: [dozen of courses](/courses/), including [Laravel security](/courses/laravel-security/) 
and [web application security](/courses/hacking-and-web-application-security/) for developers. 
If the audit shows the same class of flaw repeating, the fix is upstream of the code.

A public example, where a source code leak was the entry point to everything else: 
my [analysis of a critical French medical application](/posts/analysis-of-a-critical-french-medical-application/).

## Frequently asked questions

**How long does it take?**
Five to fifteen days depending on size and language, plus reporting. 
A mid-sized Laravel or Node application is usually around eight days. 
I give you an estimate after a look at the repository statistics, not before.

**What does it cost?**
Quote on request, fixed price. The drivers: lines of code in scope, number of languages, availability of a running instance, 
and whether retesting is included. You get a range in our first exchange.

**Do you need access to our repository?**
Yes, read-only, on the branch or tag we agree on. A read-only deploy key or a time-limited access token is ideal. 
Nothing is copied outside my encrypted workstation, and everything is destroyed after the retest unless you ask otherwise. NDA signed before access.

**Can you work from an archive instead?**
Yes, but you lose the git history and that is where the secrets are. 
If confidentiality rules it out, say so and we scope around it.

**What if the codebase is a mess?**
That is the common case, and it is not a reason to decline. 
It changes the method: more time on architecture and on finding recurring patterns, less on exhaustive coverage.

**What if you find nothing serious?**
You get a report that says so, with the coverage detail that makes it defensible in front of a customer or an auditor. 
It is the outcome I prefer, and it is rarer than you would hope.

Finally, if the findings point at a skills gap rather than isolated mistakes, 
[training](/courses/) is cheaper than auditing the same codebase again next year.

## Let's talk

Tell me what the codebase does, roughly how big it is, which languages, and what is prompting the question. 
I will reply with a proposed scope and a price range.

<br><br>

---
Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - quotes on request - SIRET 79778302400038

<style>
    .content figure {
        border: 0px solid #000;
        margin-bottom: 20px;
        margin-top: 20px;
        padding: 0px;
        text-align: center;
    }

    blockquote {
        font-size: 0.9em;
        text-align: justify;
        background-color: #fef6e1;
        border: 1px solid #f9b915;
        font-style: normal;
        padding: 2rem;
    }
    blockquote p {
        margin: 1.0rem 0 1.0rem 0;
    }
</style>

