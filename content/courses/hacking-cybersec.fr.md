---
date: 2025-04-22
slug: hacking-et-securite-application-web
title: Hacking & securité des applications web
images: [/assets/img/courses/hacking-websec.png]
tags:
    - courses
    - cybersecurity
workload: "P3D"
description: "Formation intensive et pratique de 3 jours destinée aux développeurs et aux administrateurs système : apprenez à identifier, exploiter et corriger les vulnérabilités les plus courantes des applications web à l'aide de Burp Suite et SQLmap."
prerequisites:
    - "Connaissances de base en développement web (HTML, CSS, JavaScript)"
    - "Familiarité avec les protocoles web (HTTP, HTTPS)"
    - "Expérience de l'utilisation d'outils en ligne de commande"
audience:
    - "Développeurs web"
    - "Administrateurs système"
    - "Ingénieurs Dev(Sec)Ops"
    - "Passionnés de cybersécurité"
teaches:
    - "OWASP, CWE, CVE et CVSS"
    - "Analyse du protocole HTTP avec Burp Suite"
    - "Divulgation d'informations sensibles"
    - "Validation des entrées"
    - "Cross-Site Scripting (XSS)"
    - "Injections SQL (SQLi) et utilisation de SQLmap"
    - "Content Security Policy (CSP)"
    - "Injections de code et de commandes système"
    - "Téléversement arbitraire de fichiers et traversée de répertoires (path traversal)"
    - "Authentification et sécurité des mots de passe"
    - "Attributs des cookies et détournement de session (session hijacking)"
    - "Cross-Site Request Forgery (CSRF)"
    - "Clickjacking"
    - "Contrôle d'accès défaillant et IDOR (Insecure Direct Object Reference)"
    - "Cross-Origin Resource Sharing (CORS)"
---

Cette formation intensive de 3 jours est destinée aux __développeurs__ et
__administrateurs système__ souhaitant renforcer la sécurité de leurs applications web.
Combinant __connaissances théorique__ et __exercices pratiques__, elle aborde les
__vulnérabilités courantes__, l'utilisation des __outils essentiels__ et les __bonnes
pratiques__ en matière de développement web sécurisé. Les participants apprendront
à __identifier__, __exploiter__ et __corriger__ les failles de sécurité, acquérant ainsi une
expérience pratique pour se défendre proactivement contre les menaces réelles.
​<!--more-->

<br>
<center>
    <img src="/assets/img/courses/hacking-websec.png" alt="Hacking & securité des applications web" width="500" />
    <br /><a href="/assets/formations/HACKING & SÉCURITÉ DES APPLICATIONS WEB - FR.pdf" target="_blank">Télécharger le programme</a> (pdf)
</center>


## Public cible:
- Développeurs web
- Administrateurs système
- Ingénieurs Dev(Sec)Ops
- Passionnés de cybersécurité


## Objectifs pédagogiques:
- Comprendre les principales vulnérabilités des applications web
- Apprendre à détecter et exploiter les failles de sécurité courantes
- Utiliser efficacement des outils comme _Burp Suite_ et _SQLmap_
- Appliquer les bonnes pratiques de codage sécurisé pour prévenir les attaques
- Améliorer la résilience globale des applications face aux menaces


## Pré-requis:
- Connaissances de base en développement web (HTML, CSS, JavaScript)
- Familiarité avec les protocoles web (HTTP, HTTPS)
- Expérience avec les outils en ligne de commande


## Programme:
#### Jour 1 - Les bases de la sécurité web
###### Le paysage de l'insécurité web
- La sécurité web : un vaste sujet
- Architecture type d'une application web
- Projet de sécurité des applications web ouvertes (OWASP)
- CWE, CVE et CVSS
- Attaquants externes et internes
- Les différentes étapes d'une attaque
- La boîte à outils d'un attaquant
- Idées reçues
###### Place de la sécurité dans le cycle de développement
- L'importance de l'inventaire
- Shadow IT
- Biais de production et incompatibilité avec les approches agiles
- Le concept de _DevSecOps_
- L'importance des tests et des revues de code
- Analyse statique et dynamique
###### Aide externe
- Audits de sécurité et _Bug Bounty_
- Politique de divulgation des vulnérabilités
###### Protocole HTTP
- Rappel sur le protocole HTTP(s)
- Anatomie des requêtes et réponses HTTP
- Sémantique et codes de retour
- Introduction aux _Dev Tools_
- Introduction à _Burp Suite_
###### URLs
- Anatomie d'une URL
- Exploitation des redirections ouvertes
###### Divulgation d'informations sensibles Informations
- ID séquentiels
- Messages d'erreur détaillés
- Liste des répertoires
- Journalisation en clair des informations sensibles
- Secrets côté client
- Exposition excessive des données API
- Dépôts et historique _Git_ publiques
- Répertoire _Git_ déployé

#### Jour 2 - Failles de saisie utilisateur
###### Validation des saisies
- Côté client, côté serveur ou les deux ?
- Pollution des paramètres HTTP
- Assignation de masse
###### Cross-Site Scripting (XSS)
- Les XSS sont omniprésentes
- Types de XSS
- Quel impact ?
- Comment se protéger
###### Injections SQL (SQLi)
- De quoi s'agit-il ?
- Quel impact ?
- Comment se protéger
- Présentation de _SQLMap_
- Injection _NoSQL_
###### Content Security Policy (CSP)
- Définition
- Directives les plus courantes
- Configuration possible
- Comment se protéger
###### Autres formes d'injections
- Injection HTML
- Injection de code
- Injection de commandes système
###### Fichiers
- Problèmes courants liés aux fichiers
- Téléchargement de fichiers arbitraires
- Traversée de chemin
- Inclusion de fichiers locaux et distants

#### Jour 3 - Vulnérabilités des utilisateurs authentifiés
###### Authentification et vulnérabilités
- Authentification
- Enumération des utilisateurs
- Questions de sécurité et Rate Limit
- Authentification multifacteur
###### Mots de passe
- Cryptographie en bref : codage, hachage, chiffrement
- Politique de sécurité des mots de passe
- Fuites de mots de passe
- Stockage des mots de passe
- Outils de cassage
###### Cookies
- Attributs des cookies : _secure, httponly, domain, samesite_...
- Détournement de session
###### Cross-Site Request Forgery (CSRF)
- Qu'est-ce que c'est ?
- Quel impact ?
- Comment se protéger
###### Détournement de clic
- Qu'est-ce que c'est ?
- Quel impact ?
- Comment se protéger
###### Autorisation et contrôle d'accès
- Authentification vs. autorisation
- Rôles et permissions
- Référence directe d'objet non sécurisée
###### Cross-Origin resource sharing (CORS)
- Rappel à propos de _Same-Origin Policy_
- Erreurs de configuration possibles
- Quel impact ?
- Comment se protéger

<br><br>

---
Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - devis sur demande - SIRET 79778302400038
