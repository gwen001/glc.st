---
date: 2026-07-14
slug: Linux hardening
title: Linux hardening
images: [/assets/img/courses/linux-hardening.png]
tags:
- courses
- cybersecurity
- linux
workload: "P1D"
description: "Intensive 1-day hands-on course on hardening a Linux server: attack surface reduction, sudo and account policy, firewalling, auditing with Lynis and OpenSCAP."
prerequisites:
  - "Comfortable working on the command line"
  - "Familiarity with basic Linux administration"
audience:
  - "System administrators"
  - "Dev(Sec)Ops engineers"
teaches:
  - "Attack surface analysis and defense in depth"
  - "Minimal install and package reduction"
  - "Partitioning strategy"
  - "Kernel hardening and unused kernel modules"
  - "Mandatory Access Control (MAC)"
  - "Account and sudo hardening"
  - "Password and authentication policy"
  - "Firewalling and network exposure"
  - "Brute-force mitigation"
  - "Service sandboxing and dedicated users"
  - "SUID/SGID auditing, ACLs and immutable attributes"
  - "File integrity monitoring"
  - "Centralized logging and intrusion detection"
  - "Compliance scoring with Lynis and OpenSCAP"
  - "Hardening-as-code with Ansible"
---

This intensive 1‑day training teaches you how to harden a Linux server from the ground up.
You’ll learn to minimize the __system footprint__, lock down __user privileges__, secure __network exposure__, and reinforce __critical services__.
Through hands‑on exercises, you’ll apply real‑world hardening techniques used by security professionals.
<!--more-->

<br>
<center>
    <img src="/assets/img/courses/linux-hardening.png" alt="Laravel security" width="500" />
</center>


## Targeted audience:

- System administrators
- Dev(Sec)Ops engineers


## Educational goals:
- Identify common entry points, privilege‑escalation paths, and misconfigurations
- Minimize installed software and apply kernel and filesystem protections
- Configure strong authentication, secure sudo policies, and controlled user environments
- Secure network interfaces, firewall rules, and remote access pathways
- Deploy logging, auditing, and basic intrusion‑detection mechanisms


## Prerequisites:
- Being comfortable with CLI mode
- Familiarity with basic Linux administration


## Program:
###### Introduction
- Why hardening matters?
- Understanding the attack surface
- Defense in depth
- Principle of least privilege
- Minimal install philosophy
- Hardening is role-specific
- Reference frameworks
- Baseline vs. hardening vs. compliance

###### System baseline
- Partitioning strategy
- Unnecessary packages & services
- Unused kernel modules
- Hardening the Kernel

###### Account hardening
- Mandatory Access Control (MAC)
- Account and _sudo_ hardening
- Password and authentication policy

###### Network hardening
- Network exposure
- Firewalling
- Brute-force mitigation

###### Services hardening
- Dedicated users
- File permissions
- Service sandboxing

###### Filesystem, permissions & integrity
- _SUID/SGID_ auditing
- Sticky bit and ACLs
- Immutable attributes
- File integrity monitoring

###### Logging, auditing & monitoring
- Kernel-level security event
- Centralized logging
- Intrusion detection concepts

###### Automation & compliance validation
- Scoring with _Lynis_
- Formal compliance scoring with _OpenSCAP_
- Hardening-as-code with _Ansible_

###### Resources
- Books
- Interactive platforms
- Courses & videos
- Tools

<br><br>

Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - quotes on request - SIRET 79778302400038
