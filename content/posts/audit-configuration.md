---
slug: "linux-configuration-audit-approach-tools-and-deliverables"
title: "Linux configuration audit: approach, tools and deliverables"
images : [/images/audit-configuration-analyse.png]
date: 2026-11-12
description: "Contractual framework, stakeholders, ANSSI reference data, collection and analysis scripts, report and deliverables: the complete approach to a GNU/Linux server configuration audit."
related:
  - /courses/linux-hardening
  - /services/config-audit
tags: ["audit", "ANSSI", "BP-028", "PASSI", "Linux", "OpenSSH", "durcissement"]
---

## What is a configuration audit?

A configuration audit compares the actual configuration of a system to a recognized standard, recommendation by recommendation, with supporting evidence.
[The ANSSI PASSI standard](https://cyber.gouv.fr/actualites/lanssi-met-%C3%A0-jour-les-referentiels-passi-version-22-et-pris-version-30/)
defines it as the evaluation of the conformity or security of the configuration of the hardware and software components of an information system.

This is one of the 5 activities covered by this framework:
- [configuration audit](/services/linux-configuration-hardening-audit/)
- architecture audit
- [source code audit](/services/source-code-security-audit/)
- [penetration testing](/services/web-application-penetration-testing/)
- organizational and physical audit

This article describes the approach applied to audited GNU/Linux servers, particularly with regard to the ANSSI reference guides, and the tools that support it.
While penetration testing looks for an attack path, configuration auditing provides a comprehensive overview.
It reveals latent weaknesses: a forgotten kernel parameter, a _root_ account accessible via SSH, a private key without a passphrase.
Taken individually, these discrepancies are rarely exploitable, but cumulatively they weaken the defense-in-depth.

The exercise has limitations that must be communicated to the client:
- it provides a snapshot at the time of data collection
- it does not demonstrate the usability of a discrepancy
- its value is limited by the chosen framework

Finally, the tool does not replace the auditor.
The PASSI framework specifies that an activity performed entirely automatically does not constitute an audit activity:
__scripts collect__ and sort, __the auditor qualifies__.


## The Contractual framework

No order is executed on a server before these documents are signed:
- the commercial proposal
- the scoping document
- the authorization to execute

For an independent auditor, these documents are only two or three pages long.
What matters is that they are written and signed before data collection.
In practice, a single document is sufficient: the quote, the scoping document as an appendix, and the authorization on the last page, all signed at once.

#### The signed proposal

A detailed quote accompanied by general terms and conditions, signed by the client, serves as the contract. It covers the essentials:

- Scope: number of servers, repository, and targeted level of security
- Deliverables, schedule, and price
- Ownership: the scripts remain with the auditor, the report belongs to the client
- Confidentiality and destruction of collected data at the end of the engagement
- Liability limits, backed by professional liability insurance

#### The scoping document

A single page, attached to the quote or in an email approved by the client, replaces the scoping document and audit plan.
It lists the servers and their role, names the client contact person, and defines the data collection window and transfer channel.
If the audit involves a sample, it specifies which sample and why.

#### The authorization to execute

This is the document that should never be skipped, even for a small engagement. The Penal Code punishes unauthorized access to 
or remaining in an automated data processing system.
Written authorization protects both the auditor and the client.

It consists of a few lines: the servers in question, the execution window, the SHA-256 hash of the submitted script, and
the signature of a person with authority over these systems. If the servers are hosted or managed by a third party, their agreement is also required.

When the assignment is handled through an intermediary, such as an IT services company or platform, the authorization must come from the server owner, not the intermediary.


## Key contacts

Each role is named in the project charter before launch, along with their contact information.
The most important is the service representative; they are the single point of contact, 
responsible for ensuring compliance with the agreement and the project charter.

| Role | Side | Expectations |
| --- | --- | --- |
| Service Representative | Client | Single point of contact; keeps the project charter up to date, makes decisions, and responds promptly |
| CISO | Client | Validates the scope, the targeted level of security, and the report marking |
| System Administrators | Client | Review and then execute the script, forward the archives, and answer technical questions |
| Data Protection Officer | Client | Consulted as soon as logs or personal directories are included in the data collection |
| Management | Client | Signs the agreement, receives the summary, and attends the closing meeting |
| Hosting Provider or Managed Services Provider | Third Party | Gives approval when the servers are not at the client's premises; sometimes performs the data collection |
| Audit Team Leader | Auditor | Drafts the scoping document and audit plan, approves the report |
| Auditors | Auditor | Analyze the archives, identify discrepancies, and write the findings |

The channels are also predetermined: a contact email address, a phone number reachable during the data collection window, and an encrypted channel for the files.
The digital fingerprint of an archive is always confirmed by a channel different from the one that transmitted it.


## Attachments and prerequisites

The audit begins with two files: without the first, discrepancies cannot be qualified; without the second, no data is collected.

__What the client provides to frame the analysis:__

| Document | Purpose |
| --- | --- |
| Server inventory: name, IP address, distribution, role, environment, criticality | Defines the scope and establishes the sampling |
| Architecture diagram and flow matrix | Locates each server; a router does not expect the same network parameters as an application server |
| Information Security Policy (ISSP), internal hardening standard, granted exceptions | Adds criteria and indicates discrepancies already accepted |
| Previous audit reports | Allows tracking of recurring discrepancies |
| Contact list and on-call schedule | Provides input for the scoping document |

__And what the auditor submits for data collection:__

| Document | Purpose |
| --- | --- |
| Service agreement, scoping document, audit plan | These define the framework and must be signed and validated before data collection |
| Data collection script, SHA-256 hash, execution instructions | These allow the client to review the script before running it |
| Public encryption key (AGE or GPG) | Used to encrypt the archives before sending them |
| PASSI qualification certificate, if applicable | Attached to the agreement for a qualified service |

The script is delivered in plain text, along with its hash. Asking the client to run code as _root_ implies allowing them to read it.
In return, the generated archives are added to the audit file and are never transmitted in plain text.


## Reference frameworks

The main security criteria are drawn from ANSSI guides, including:
- [Guide to Information Systems Security Maturity](https://messervices.cyber.gouv.fr/guides/guide-relatif-la-maturite-ssi)
- [ANSSI-PA-022: Recommendations for the Secure Administration of Information Systems](https://messervices.cyber.gouv.fr/guides/recommandations-relatives-ladministration-securisee-des-si)
- [ANSSI-PA-066: Recommendations for Interconnecting an Information System to the Internet](https://messervices.cyber.gouv.fr/guides/recommandations-relatives-linterconnexion-dun-si-internet)
- [ANSSI-BP-028: System Configuration Recommendations GNU/Linux](https://messervices.cyber.gouv.fr/guides/recommandations-de-securite-relatives-un-systeme-gnulinux)
- [DAT-NT-007: Secure Use of (Open)SSH](https://messervices.cyber.gouv.fr/guides/usage-securise-dopenssh)
- [ANSSI-PA-009: Securing a Website](https://messervices.cyber.gouv.fr/guides/securiser-un-site-web)
- [Security Recommendations Relating to TLS](https://messervices.cyber.gouv.fr/guides/recommandations-de-securite-relatives-tls)

However, some recommendations depend on the server's role. For example, the network parameters in recommendation _ANSSI-BP-028-R12_
are given for a server that does not perform routing.
On a router, several values ​​are intentionally different.
The relevant recommendations are therefore marked as *"not applicable"* in the final report.

Note that an extrapolation, even a relevant one, is presented as such and never under the number of a rule that doesn't explicitly state it.

Beyond the standards, just as in penetration testing, the auditor's experience is as important as the checklist.
They know where critical discrepancies most often lie, including where the guidelines don't explicitly state them, or not as directly.

Some non-standard checks regularly prove fruitful:
- weak system passwords or passwords present in shell logs
- secrets in plain text in logs
- configuration backups and SQL dumps located in the web server root directory
- scripts launched by _root_'s crontab but modifiable by another user
- insecure web configurations

These findings follow the citation rule above: they appear in a dedicated section, without a recommendation number, 
and are rated according to their actual risk, with the same caution as the others. 
The proposal states this upfront so that the client doesn't discover them in the report.


## The approach step by step

The client executes, the auditor analyzes: at no point does the auditor connect to the audited servers.
This is what ANSSI recommends: when a service requires running a tool on the audited system, the client commissions it.

#### Preparation
The auditor first verifies that they can perform the entire service, then establishes the agreement, the scoping document, and the audit plan.
The kick-off meeting brings together the liaison, the CISO, and the administrators.
The script is reviewed, and then the exclusion options, the transfer channel, and the data collection window are finalized.

<br>

<a href="/images/audit-configuration-demarche.jpg" target="_blank"><img src="/images/audit-configuration-demarche.jpg" alt="Audit process: from initial need to data destruction" width="400" style="float:left;" /></a>


<div style="float:right;width:450px;">
    <h4 id="collecte" style="margin:0">Collection</h4>
    <br>A client administrator reviews the script, verifies its fingerprint, and executes it on each server.
    ANSSI recommends conducting configuration audits in the production environment: it's the actual configuration that is being evaluated.
    With a large volume of data, scanning the file system can take several minutes; operations must be notified.
    <br><br>
    <h4 id="analyse" style="margin:0">Analysis</h4>
    <br>The auditor checks the fingerprint, extracts the archive, and analyzes it.
    The real work then begins: reviewing each finding in light of the server's role, any exceptions, and the targeted level of security.
    Undefined controls and ambiguous discrepancies become questions for the client.
</div>

<div style="clear:both;"></div>

#### Reporting

The most serious findings are reported immediately, without waiting for the report. 
The draft report undergoes a factual review, the final report is presented at a closing meeting, and then the collected data is destroyed.

## Tools, part 1: the data collection script

`collect.sh` is executed by the client, as root, on each audited server.
It does not modify anything, does not open any network connections, and does not attempt any authentication; it produces a `.tar.gz` archive to be sent back to the auditor.

Written in _POSIX Shell_ script, it runs under most Linux shells.
It relies only on basic packages; any missing command is logged and then ignored.
A minimal distribution results in partial data collection, never a complete failure.

{{< figure alt="Configuration audit, collect" src="/images/audit-configuration-collecte.png" width="600">}}

#### What the archive contains

**Collection context:** distribution, kernel, virtualization, init system, privileges, options used.

**Raw output of over 80 read-only commands:** `sysctl`, `ss`, `iptables`, `netstat`, `nft`, `systemctl`, `auditctl`, installed packages, etc.

**File system inventory:** type, permissions, owner, group, size, date, and path of each entry.

**Lists extracted from the inventory:** _setuid_ and _setgid_, files without an owner, writable by everyone, suspicious archives, index of copied files.

**Configuration files, backups, sensitive files** and logs identified, absolute paths preserved.

**`/etc` complete**, then `/home` and `/root` without caches, preserving permissions.

**Timestamped log** of collection and copy of the script actually executed.

#### Design choices

**Strict file hygiene.** A temporary working directory is created and deleted once the collection process is complete.

**A single scan of the file system.** All derived lists are removed from the inventory.
The pseudo-systems `/proc`, `/sys`, `/dev`, and `/run` are excluded.

**Volume safeguards.** A copied file is truncated to 256 KB to prevent overflow.
Any excess is logged in the archive. Copies retain permissions and dates.

**The collection does not mask, pseudonymize, or modify any data:** Files are copied as is, with their permissions and dates,
because altered data skews the analysis.
A masked password hash no longer reveals which algorithm protects it, and a redacted private key no longer indicates whether it is encrypted.

**Transparency.** The log files list exactly which commands are executed and what leaves the machine; the client can verify this.

At the end of execution, the script displays the archive's path, size, and SHA-256 hash. The client transmits this hash through a separate channel; the auditor recalculates it before any analysis.

## The tools, part 2: the analysis script

`analyse.sh` runs on the auditor's machine, on the extracted archive. It compares each collected element against the relevant 
recommendation and produces a text report where each verdict cites its source.

{{< figure alt="Configuration audit, analysis" src="/images/audit-configuration-analyse.png" width="600">}}

#### Five statuses, not two

| Status | Meaning |
| --- | --- |
| `COMPLIANT` | The recommendation is followed |
| `NON_COMPLIANT` | It is not followed; the severity (_CRITICAL, MAJOR, MINOR_) is declared by the rule |
| `UNDETERMINED` | The necessary evidence is missing or illegible |
| `NOT_APPLICABLE` | The rule does not apply to this server |
| `INFO` | Information without a verdict |

Each tested directive also receives a label:
- _OK_ if the value is correct and fixed in the configuration
- _DEFAULT_ if it is correct but inherited from a default value
- _FAIL_ if it deviates from the expected value
- _UNKNOWN_ if it is unreadable

The distinction between _OK_ and _DEFAULT_ is important: a value that is not fixed can change at the next update.

#### Rules of strictness

- **Undetermined is not non-compliant.** Missing evidence is neither a success nor a failure, and mistaking it for a discrepancy discredits a report.
- **Execution and persistence are distinguished.** A parameter that is correct at runtime but not declared anywhere is flagged, because a reboot can revert it to its default value.
- **A file's owner is read from the inventory, never from the extracted copy.** `tar` only restores owners if it runs as _root_.
- **Interpretation and extrapolation remain separate.** `--tips` adds explanations without changing any verdicts. `--extra` runs checks that extend a rule beyond its text; their results are counted separately.

#### The output

The source code comes before the verdict: the reader knows what has been read before learning what it means.
The terminal is color-coded, the report file never is: it remains plain text, usable with `grep` and comparable from one audit to another with `diff`.

The exit code summarizes the result:
```text
============================================================================
 Summary
============================================================================
 checks run      : 29
 not run         : 1 extrapolated check(s), available with --extra

 COMPLIANT       : 7
 NON_COMPLIANT   : 13
 UNDETERMINED    : 6
 NOT_APPLICABLE  : 3
 INFO            : 0

 severity of the non-compliant findings:
   CRITICAL      : 0
   MAJOR         : 2
   MINOR         : 11
```

## The deliverable: report, rating, and matrix

The report transforms script output into decisions: each discrepancy becomes a verified observation, rated and accompanied by a measurement.
The raw output is only an appendix.

<br>

<a href="/images/audit-configuration-fiche.png" target="_blank"><img src="/images/audit-configuration-fiche.png" alt="Audit approach: results sheet" width="400" style="float:right;" /></a>

<div style="float:left;width:450px;">
    <h4 id="la-structure-du-rapport" style="margin:0">Report Structure</h4>
    <br>1. <b>The framework:</b> objectives, audit criteria, scope, dates, audit team, and specific procedures.
    <br>2. <b>The management summary:</b> readable without expertise: general assessment, main risks, and recommendations.
    <br>3. <b>Results:</b> One record per nonconformity, then a summary per server.
    <br>4. <b>Appendices:</b> Scoping note, audit plan, compliance matrix, and raw analysis outputs.
    <br>
    <h4 id="la-fiche-de-constat">The Findings Record</h4>
    Each nonconformity has a unique identifier, the unmet requirement, the evidence, a severity level, a measure, and a priority.
    Example taken from the excerpt above →
</div>

<div style="clear:both;"></div>

#### Rating scales

The severity of a vulnerability is a combination of its impact and the difficulty of exploiting it.
Its priority for remediation combines this severity with the effort required to achieve compliance.

**Impact** measures what the vulnerability provides to an attacker who successfully exploits it, and nothing else. 
Neither the probability of this happening, nor the difficulty of achieving it.

**Difficulty of Exploitation** measures the total effort required to exploit the vulnerability, from the initial setup to the result.
It combines two distinct components:
- Prerequisites: network setup, authentication, privileges, third-party interaction, time window
- Technical steps: necessary tools, reliability of the exploit, required expertise

These two components often diverge, and this is the case most likely to be contested during feedback meetings.
The level used is that of the prerequisites, and the report mentions both.

{{< figure alt="Configuration audit, matrix" src="/images/audit-configuration-matrix.png" width="600">}}

#### The compliance matrix

The matrix is ​​a spreadsheet: one row per recommendation and per server, showing the status, severity, evidence, measure, and priority. 
It is used by the client to manage remediation and then by the auditor as the basis for the control audit. 
Undetermined rows are included, along with their explanation.

## Deliverables and Feedback

Upon completion, the client receives a final report, a usable compliance matrix, and a destruction report.
Other deliverables are provided throughout the service.

| Deliverable | Content |
| --- | --- |
| Scoping Note and Audit Plan | Terms, stakeholders, schedule, methodology, sampling<br>Submitted before data collection |
| Data Collection Kit | Script, SHA-256 hash, execution instructions, public encryption key<br>Submitted at the kick-off meeting |
| Immediate Feedback | Most Serious Findings and Initial Conclusions<br>Submitted upon completion of the analysis |
| Draft Report | Full Report, subject to factual review<br>Submitted after the discrepancies have been qualified |
| Final Report | Corrected and marked report according to the scoping document<br>Submitted after review |
| Compliance Matrix | Spreadsheet, one row per recommendation and per server<br>Submitted with the final report |
| Raw Analysis Outputs | One text file per server<br>Submitted as an appendix to the report |
| Destruction Record | Items destroyed, date and method of destruction<br>Submitted upon completion |

The review of the draft report focuses on the facts: an incorrectly identified server, a forgotten exception, a misinterpreted piece of evidence.
The rating is non-negotiable; if the client disputes a serious issue, their argument is included in the report alongside the auditor's.

The closing meeting brings together management, the CISO, the liaison, and the administrators.
It is intended for two audiences: a summary for management and findings reports for the technical teams.

## Conclusion

A configuration audit is more valuable for its rigor than for its tools.
The data collection script makes the process reproducible and transparent for the client.
The analysis script makes each finding traceable to its supporting evidence.

The rest remains the auditor's job: to define the scope, qualify each discrepancy within its context, assign the most accurate assessment possible, 
and propose measures that operations can actually implement.

<style>
    .content figure {
        text-align: center;
    }
</style>

