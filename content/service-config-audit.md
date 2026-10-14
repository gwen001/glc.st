---
title: "Linux configuration and hardening audit"
url: "/linux-configuration-hardening-audit/"
date: 2026-10-13
description: "Linux configuration and hardening audit against ANSSI-BP-028 and CIS benchmarks: exposure, accounts, services, logging, prioritised findings and retesting."
keywords: "configuration audit, Linux hardening audit, ANSSI BP-028, server hardening, independent cybersecurity expert"
service: true
---

A [penetration test](/web-application-penetration-testing/) finds the way in. 
A [code audit](/source-code-security-audit/) finds why the way in exists. A configuration audit answers the question that follows both:
 **once someone is on the machine, how far do they get?**

That is a different question, and most organisations have never asked it. 
They have a server that works, which is not the same as a server built to contain a compromise.

## Why now

**Exploitation of exposed systems is how breaches start.** 
The Verizon [DBIR 2026](https://www.verizon.com/business/resources/reports/dbir/), built on more than 22,000 confirmed breaches across 145 countries, 
puts vulnerability exploitation at **31%** of initial access - now ahead of phishing (16%) and credential abuse (13%). 
In North America the figure reaches 44%. And the gap is widening: **only 26% of critical vulnerabilities were fully remediated** in 2025, 
against 38% the previous year, while the median time to full resolution rose from 32 to **43 days**.

**Misconfiguration moved up the ranking.** 
In the [OWASP Top 10:2025](https://top10.owasp.org/2025/), Security Misconfiguration is now **A02**, second overall. 
Not an exotic flaw class - the default that nobody changed.

**The French picture is specific and unflattering.** 
[ANSSI's Panorama de la cybermenace 2025](https://www.cert.ssi.gouv.fr/uploads/CERTFR-2026-CTI-002.pdf) records **1,366 confirmed incidents**, flat against 2024. 
It describes edge equipment repeatedly compromised through its own vulnerabilities, and industrial control interfaces reachable from 
the internet **with no authentication at all, or with default passwords still in place**. 
Data exfiltration incidents went from 130 to **196** in a year.

{{< figure alt="Verizon bdir" src="/assets/img/services/panorama-cybermenace-2025.png" width="500" >}}

Put those together and the shape of the problem is clear. 
The entry point is usually an exposed service that was not patched or not meant to be exposed. 
What determines whether that becomes an incident or a catastrophe is everything behind it: privilege separation, service isolation, logging, segmentation. 
That is what a configuration audit measures.

## When to run one

- **Before putting a server into production**: the cheapest possible moment, and the rarest in practice.
- **After a penetration test or a code audit**: you know how someone gets in; now find out what they reach afterwards. This is the natural follow-on engagement.
- **After an incident**: to verify that containment was real and that the attacker's path is actually closed, not just the entry point.
- **When inheriting infrastructure**: the admin who built it has left, there is no documentation, and nobody is sure what is running or why.
- **During or after a migration**: new distribution major version, move to cloud, containerisation. Hardening rarely survives a migration intact.
- **For a third-party requirement**: enterprise security questionnaires, insurers, public-sector procurement, or NIS2 and DORA obligations for entities in scope. 
[GDPR article 32](https://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX%3A32016R0679) already requires regularly testing the effectiveness of technical measures.
- **On a cycle, for drift**: this is the one people underestimate. A server hardened eighteen months ago is not a hardened server. 
Packages get installed for a debugging session and stay, firewall rules get opened for a migration and are never closed, a contractor's SSH key is still authorised. 
Configuration drift is quiet and cumulative.

## Reference frameworks

The audit is conducted against a named baseline, not against my personal taste.

[ANSSI-BP-028 v2.0](https://cyber.gouv.fr/sites/default/files/document/linux_configuration-en-v2.pdf), 
the French national cybersecurity agency's configuration recommendations for a GNU/Linux system, 
is the primary reference, and the one I applied for four years auditing systems for the Ministry of Armies. 
It defines four levels of hardening (minimal, intermediary, enhanced, high), and **choosing the target level is part of the scoping conversation**, 
not a default I impose. Auditing a public-facing marketing server against the "high" profile produces a hundred findings nobody will ever act on.

Alongside it: [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks) for distribution-specific and service-specific controls, 
and **OpenSCAP** with the [SCAP Security Guide](https://www.open-scap.org/security-policies/scap-security-guide/), 
which ships ready-made `anssi_bp28_*` profiles for the major distributions - convenient for producing a reproducible, 
formally scored baseline you can re-run yourself after I leave.

## What I audit

**System baseline**: partitioning and mount options, installed package inventory against what is actually needed, enabled services, 
loadable kernel modules, kernel parameters, and mandatory access control (SELinux or AppArmor) status and policy.

**Accounts and privileges**: the sudo policy line by line, which is where I find the most serious issues most often: a single `NOPASSWD` rule 
on a binary with a shell escape undoes everything else on this list. Also password and authentication policy, PAM stack, 
SSH configuration and authorised key inventory, service accounts with shells they do not need.

**Network exposure**: what actually listens, on which interfaces, versus what you believe listens. Firewall rules and their ordering, 
remote access paths, brute-force mitigation, and the exposure of administration interfaces and databases.

**Service hardening**: dedicated unprivileged users, systemd sandboxing directives, web server and reverse proxy configuration, 
TLS configuration and certificate handling, database listeners and authentication.

**Filesystem and integrity**: permissions on sensitive paths, a SUID/SGID inventory with a judgement on each one, ACLs and sticky bits, 
immutable attributes, and file integrity monitoring.

**Logging, audit and detection**: whether anything is actually recorded, whether it is centralised somewhere an attacker cannot erase, 
kernel-level audit rules, retention, and whether anyone would notice. A log nobody reads is a compliance artefact, not a control.

**Secrets and credentials on disk**: environment files, configuration files, deployment scripts, cron jobs, shell history, backup archives.
This overlaps with my [code audit](/source-code-security-audit/) work, and it is consistently productive.

**Containers and orchestration**: image provenance and base image currency, privileged containers, 
mounted sockets, capabilities, resource limits, registry access.

**Automation and backups**: CI/CD runners and their credentials and network reach, backup integrity and restoration, 
and whether your backups are reachable from the machine they protect, which is the detail that turns a ransomware incident into a total loss.

<br>

> Linux systems - Debian and Ubuntu, RHEL and derivatives - plus Docker and
container configuration, and infrastructure as code (Ansible). **Not Windows, not
Active Directory.** If your estate is Microsoft-centred, I will say so in the
first exchange and point you to someone better placed.

## How it runs

**1. Scoping**: inventory of systems in scope, their roles, the target ANSSI level, access method, 
a maintenance window if any intrusive check is needed, and the signed audit agreement.

**2. Collection**: read-only data gathering, by SSH with an unprivileged account plus a precisely scoped sudo allowance, 
or from a configuration export if you prefer. Nothing is modified. I will tell you exactly which commands run before anything runs.

**3. Automated baseline**: Lynis and OpenSCAP and custom tools against the agreed profile, to get reproducible coverage and 
a score you can track over time. Output goes into triage, never straight into the report.

**4. Manual review**: the part that matters. Automated tools check whether a setting matches a value; they cannot tell you 
that this particular sudo rule, on this particular machine, given what this service does, is a path to root. That judgement is the deliverable.

**5. Report and debrief**: written deliverable.

{{< figure alt="Lynis report" src="/assets/img/services/lynis.png" width="500" >}}

## The report

- **A one-page summary** for management: current posture against the target level, the handful of things that genuinely matter, 
and an honest statement of what was and was not covered.
- **Findings by system**, each with the exact setting or rule at fault, its location, the reference control (ANSSI-BP-028 rule identifier, CIS control), 
a contextualised severity, the concrete exploitation path where one exists, and the exact change to make.
- **Remediation ordered by impact against risk of breakage**: because this is the specific trap of hardening work. 
A recommendation that breaks the application gets reverted on Monday and the server ends up less secure than before I came. 
Each item says what might break.
- **A re-runnable baseline**: the OpenSCAP profile and scoring method, so you can measure your own drift at any point without calling me.

<br>

> A publicly reachable admin login page is an informational finding, not a critical vulnerability.
Inflating scores fills the report and empties its value: by the third audit where everything is "high", 
nobody reads it any more, and the one genuinely critical finding drowns in the noise. 
Accept short reports when the application is in good shape.

## What this is not

- **It is not a compliance certification.** I produce an audit report against a published baseline. 
I am not an accredited certification body and this is not a PASSI-qualified engagement. 
If your requirement explicitly demands a qualified provider, you need one, and I will tell you so rather than let you discover it at the review meeting.
- **It is not a scanner report with a cover page.** Automated tools are inputs. You can run them yourself for free, and you should. 
What you are buying is the triage and the exploitation judgement.
- **It is not maximum hardening.** The target level is a decision based on what the system does and what it is worth. 
Hardening that breaks production is not hardening.
- **It is not a penetration test.** This audit is conducted with legitimate access and full visibility. 
It tells you how deep a compromise would go, not whether someone can get in, that is the
[pentest](/web-application-penetration-testing/).
- **It does not stay true.** Recall the drift point above: this is a snapshot. 

## Why me

- **Auditor for the French Ministry of Armies, 2020 to 2024**: configuration audits of Linux systems against ANSSI recommendations, 
alongside penetration testing and code audits. Four years of applying this specific baseline to systems where the consequences of getting 
it wrong were not theoretical.
- **A sysadmin before being an auditor**: I administered Linux servers for years before I started breaking them. 
I know what a recommendation costs to apply at 3am on a system with users on it.
- **Bug hunter since 2016**: several hundred reports to programmes at EDF, Decathlon, Payfit, BlaBlaCar, Swisscom, Uber and others, 
including remote command execution and cloud account takeover obtained from exposed services and leaked credentials. Published [CVEs](/cves/) under my own name.
- **Trainer since 2022**: I teach [Linux hardening](/courses/linux-hardening/) and [Bash scripting](/courses/introduction-to-bash-scripting/), 
which means your team gets explanations rather than a list of settings to copy.

## Frequently asked questions

**How long does it take?**
Two to three days for a single server including the report. For a fleet, it depends on how homogeneous it is: 
ten identical web servers cost barely more than one, ten different roles cost close to ten times as much. 
I will tell you which situation you are in after a look at the inventory.

**What does it cost?**
Quote on request, fixed price. The drivers: number of distinct system roles, target ANSSI level, whether retesting is included.

**What access do you need?**
SSH with an unprivileged account and a scoped sudo allowance for read-only collection. 
No configuration change, no software installed without your explicit agreement. 
I will send you the exact command list in advance for review.

**Can it disrupt production?**
Collection is read-only and safe. The only step carrying any risk is the demonstration of an exploitable finding, 
which happens only with your written agreement, in a window you choose, and on a clone whenever one exists.

**We already run Lynis and our score is good. What do you add?**
Judgement, and the things a score cannot see. A hardening index measures conformity to a checklist; it does not know that your deployment user 
can write to a directory that a root cron job executes. Most of the serious findings I report are compositions of individually acceptable settings.

**Can you apply the fixes rather than just report them?**
I deliver the priority findings and I can walk your team through applying them. I do not take over the 
remediation of production systems I do not operate, that needs someone who will still be there next month.

**What if everything is already in good shape?**
You get a short report that says so, with the detail that makes it defensible in front of a customer or an auditor, 
plus the baseline to track drift. It happens, and it is a good outcome.

## Let's talk

Tell me how many systems, what they run, which distributions, and what is prompting the question. 
I will reply with a proposed scope, a target level and a price range.

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
