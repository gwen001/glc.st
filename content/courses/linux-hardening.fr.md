---
date: 2026-07-14
slug: durcissement-linux
title: Durcissement Linux
images: [/assets/img/courses/linux-hardening.png]
tags:
    - courses
    - cybersecurity
    - linux
workload: "P1D"
description: "Formation pratique intensive d'une journée sur le durcissement (hardening) d'un serveur Linux : réduction de la surface d'attaque, gestion des comptes et de `sudo`, pare-feu, et audit à l'aide de Lynis et OpenSCAP."
prerequisites:
    - "Être à l'aise avec l'utilisation de la ligne de commande"
    - "Connaissances de base en administration système Linux"
audience:
    - "Administrateurs système"
    - "Ingénieurs Dev(Sec)Ops"
teaches:
    - "Analyse de la surface d'attaque et défense en profondeur"
    - "Installation minimale et réduction du nombre de paquets"
    - "Stratégie de partitionnement"
    - "Durcissement du noyau (kernel) et gestion des modules inutilisés"
    - "Contrôle d'accès obligatoire (MAC - Mandatory Access Control)"
    - "Durcissement des comptes et de sudo"
    - "Politiques de mots de passe et d'authentification"
    - "Mise en place de pare-feu et gestion de l'exposition réseau"
    - "Atténuation des attaques par force brute"
    - "Isolation des services (sandboxing) et utilisation de comptes dédiés"
    - "Audit des permissions SUID/SGID, des ACL et des attributs immuables"
    - "Surveillance de l'intégrité des fichiers"
    - "Centralisation des journaux (logs) et détection d'intrusions"
    - "Évaluation de la conformité avec Lynis et OpenSCAP"
    - "Approche Hardening-as-code avec Ansible"
---

Cette formation intensive d'une journée vous apprend à durcir un serveur Linux de
fond en comble. Vous apprendrez à __réduire l'empreinte du système__, à __restreindre
les privilèges des utilisateurs__, à __sécuriser l'exposition réseau__ et à __renforcer les
services critiques__. Grâce à des exercices pratiques, vous mettrez en œuvre des
__techniques de durcissement concrètes__ utilisées par les professionnels de la sécurité.
<!--more-->

<br>
<center>
    <img src="/assets/img/courses/linux-hardening.png" alt="Durcissement Linux" width="500" />
    <br /><a href="/assets/formations/DURCISSEMENT LINUX - FR.pdf" target="_blank">Télécharger le programme</a> (pdf)
</center>


## Public cible:
- Administrateurs systèmes
- Ingénieurs Dev(Sec)Ops


## Objectifs pédagogiques:
- Identifier les points d'entrée courants, les vecteurs d'élévation de privilèges et les erreurs de configuration
- Réduire le nombre de logiciels installés et mettre en œuvre des protections au niveau du noyau et du système de fichiers
- Configurer une authentification forte, des politiques sudo sécurisées et des environnements utilisateur contrôlés
- Sécuriser les interfaces réseau, les règles de pare-feu et les voies d'accès à distance
- Déployer des mécanismes de journalisation, d'audit et de détection d'intrusion de base


## Pré-requis:
- Connaissance des bases de l'administration Linux
- Être à l'aise avec le mode CLI


## Programme:
###### Introduction
- Pourquoi le durcissement est-il important ?
- Comprendre la surface d'attaque
- Défense en profondeur
- Principe du moindre privilège
- Philosophie d'installation minimale
- Le durcissement dépend du rôle
- Cadres de référence
- Configuration de base, durcissement et conformité

###### Configuration de base du système
- Stratégie de partitionnement
- Paquets et services inutiles
- Modules du noyau inutilisés
- Durcissement du noyau

###### Durcissement des comptes
- Contrôle d'accès obligatoire (MAC)
- Durcissement des comptes et de _sudo_
- Politique de mots de passe et d'authentification

###### Durcissement du réseau
- Exposition réseau
- Filtrage réseau (pare-feu)
- Atténuation des attaques par force brute

###### Durcissement des services
- Utilisateurs dédiés
- Permissions de fichiers
- Isolation (sandboxing) des services

###### Système de fichiers, permissions et intégrité
- Audit _SUID/SGID_
- Sticky bit et ACLs
- Attributs immuables
- Surveillance de l'intégrité des fichiers

###### Journalisation, audit et surveillance
- Événements de sécurité au niveau du noyau
- Journalisation centralisée
- Concepts de détection d'intrusion

###### Automatisation et validation de la conformité
- Évaluation du score avec _Lynis_
- Évaluation formelle de conformité avec _OpenSCAP_
- Durcissement as-code avec _Ansible_

###### Ressources
- Livres
- Plateformes interactives
- Cours & vidéos
- Outils

<br><br>

---
Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - devis sur demande - SIRET 79778302400038
