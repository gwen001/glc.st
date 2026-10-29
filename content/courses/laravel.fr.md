---
date: 2025-04-22
slug: securite-laravel
title: Sécurité Laravel
images: [/assets/img/courses/laravel.png]
tags:
    - courses
    - cybersecurity
    - laravel
workload: "P1D"
description: "Formation d'une journée sur la sécurité dans Laravel, destinée aux développeurs PHP : configuration sécurisée, authentification et politiques d'accès, *mass assignment* (affectation de masse), injections SQL, XSS, CSRF, CORS et gestion des téléversements de fichiers."
prerequisites:
    - "Solide expérience de PHP et du framework Laravel"
    - "Compréhension des concepts de sécurité des applications web"
    - "Connaissance pratique des vulnérabilités web courantes (XSS, CSRF, injections SQL)"
audience:
    - "Développeurs Laravel"
    - "Professionnels de la sécurité"
    - "Ingénieurs Dev(Sec)Ops"
teaches:
    - "Configuration globale sécurisée et gestion de la clé d'application"
    - "Validation des données d'entrée"
    - "Journalisation et gestion des erreurs"
    - "Mises à jour et sauvegardes"
    - "Cookies, sessions et fixation de session"
    - "Chiffrement et déchiffrement des données"
    - "Validation des mots de passe et procédures de réinitialisation"
    - "Laravel Passport et les packages d'authentification officiels"
    - "*Gates*, politiques d'accès, rôles et permissions"
    - "Limitation du taux de requêtes (*rate limiting*) sur les routes et les actions"
    - "Téléversement et stockage sécurisés de fichiers"
    - "*Mass assignment* (affectation de masse)"
    - "Exposition excessive de données"
    - "Injections SQL, requêtes préparées et liaison de paramètres"
    - "Cross-Site Scripting (XSS) dans les templates Blade"
    - "Jetons CSRF et middleware"
    - "Configuration CORS"
---

Cette formation d'une journée est destinée aux __développeurs Laravel__ qui souhaitent
créer des applications sécurisées de A à Z. Vous apprendrez à __identifier les
vulnérabilités__ courantes, à __appliquer les protections intégrées de Laravel__ et à
__adopter les meilleures pratiques__ pour sécuriser l'__authentification__, l'__autorisation__,
la __validation des entrées__, et bien plus encore.
<!--more-->

<br>
<center>
    <img src="/assets/img/courses/laravel.png" alt="Sécurité Laravel" width="500" />
    <br /><a href="/assets/formations/SÉCURITÉ LARAVEL - FR.pdf" target="_blank">Télécharger le programme</a> (pdf)
</center>


## Public cible:
- Développeurs Laravel
- Professionnels de la sécurité
- Ingénieurs Dev(Sec)Ops


## Objectifs pédagogiques:
- Reconnaître les vulnérabilités web courantes et les protections de Laravel
- Implémenter une authentification et une autorisation sécurisées avec les outils Laravel
- Configurer correctement les protections CORS et CSRF
- Valider et nettoyer les entrées utilisateur de manière sécurisée
- Gérer les uploads et le stockage de fichiers en toute sécurité


## Pré-requis:
- Good experience with PHP and Laravel framework
- Bonne expérience avec PHP et le framework Laravel
- Compréhension des concepts de sécurité des applications web
- Familiarité avec les vulnérabilités web de base (XSS, CSRF, SQLi…), [voir la formation cybersécurité débutant](/fr/formations/hacking-et-securite-application-web/)


## Programme:
###### Configuration globale
- Sécurité PHP de base
- Configuration globale
- Sessions
- _APP_KEY_

###### Les bases
- PHP brut dans les vues
- Validation des entrées
- Journalisation et gestion des erreurs
- Mises à jour et sauvegardes
- Tromper les robots

###### Cookies et sessions
- Configuration globale
- Configuration spécifique à Laravel
- Fixation de session

###### Mots de passe et chiffrement
- Chiffrer/déchiffrer les données
- Validation des mots de passe
- Réinitialiser les mots de passe

###### Authentification et permissions
- Packages officiels
- Laravel Passeport
- Gates et Policies
- Utilisateurs / rôles / permissions

###### Rate limiting
- Utilisation de base
- Limiter les actions / routes

###### Téléchargement et stockage de fichiers
- Qu'est-ce que c'est ?
- Règles de validation et nettoyage
- Configuration et utilisation des stockages
- Autorisations / permissions / visibilité

###### Affectation de masse
- Qu'est-ce que c'est ?
- Attentes vs. réalité
- Comment se protéger

###### Exposition excessive des données
- Qu'est-ce que c'est ?CommenterSurligner
- Comment se protéger
- Ce qu’il faut retenir

###### Injection SQL (SQLi)
- Qu'est-ce que c'est ?
- Requêtes préparées
- Paramètres liés
- Les dangers
- Ce qu’il faut retenir

###### Cross-Site scripting (XSS)
- Qu'est-ce que c'est ?
- Contrôleurs
- Modèles
- Nettoyage des entrées
- Aides externes

###### Cross-Site Request Forgery (CSRF)
- Qu'est-ce que c'est ?
- Jeton anti-CSRF
- Middleware
- Formulaires
- _X-csrf-token_ et _X-xsrf-token_

###### Cross origin resources sharing (CORS)
- Qu'est-ce que c'est ?
- Configuration
- _allowed_origins_ et _supports_credentials_

<br><br>

---
Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - devis sur demande - SIRET 79778302400038
