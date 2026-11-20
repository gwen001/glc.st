---
date: 2020-01-30
slug: "bugbounty-5-choses-a-eviter-dans-un-programme"
title: "Bug bounty : 5 choses à éviter dans un programme"
related:
  - /courses/bugbounty
  - /posts/bugbounty-management-bad-example
  - /posts/bugbounty-management-hackers-motivation
  - /posts/bugbounty-management-todo-before-launch
  - /posts/bugbounty-management-zomato
  - /posts/bugbounty-unonfficial-rules
tags:
  - bug bounty
  - management
---
Le deuxième article d'une série consacrée à la gestion des programmes de bug bounty.
Celui-ci propose aux entreprises quelques conseils sur les erreurs à éviter afin de préserver de bonnes relations avec les hackers.
<!--more-->


## Bugs non corrigés

Les hackers consacrent du temps à votre programme ; ils auraient pu en choisir un autre, mais ils ont opté pour le vôtre.
Ils passent du temps à chercher des failles dans votre système, ce qui est par définition, l'objectif d'un programme de bug bounty. 
La moindre des choses est donc de corriger les vulnérabilités qu'ils signalent.
Laisser des bugs sans correction, ignorer des rapports ou les mettre de côté "pour plus tard" ne fera qu'entraîner une accumulation 
de signalements répétés concernant les mêmes problèmes.

C'est une perte de temps, tant pour les hackers que pour votre équipe chargée du tri des rapports, et c'est par ailleurs, un manque de respect.
Gardez à l'esprit que, selon la plateforme utilisée, les hackers peuvent voir leur réputation affectée par des signalements jugés en double ou non applicables.
**Cela finira tout simplement par ruiner votre réputation** : les hackers ne travailleront plus pour vous.
Ce qui devait constituer une amélioration de votre sécurité se transformera en une perte financière, 
et ce qui aurait pu être une excellente opération marketing deviendra, au contraire, une très mauvaise publicité. 
En deux mots : un désastre total.


## Absence de réponse

Se sentir ignoré est ce qu'il y a de pire. Vraiment.

Qu'on le veuille ou non, il est de votre devoir de traiter chaque signalement.
Par traiter, j'entends répondre. Et par répondre, j'entends une réponse personnalisée, rédigée manuellement.
Un message automatique envoyé par un bot que vous avez configuré ne saurait constituer un véritable traitement du signalement.
Cela confirme simplement la bonne réception du rapport, mais ne garantit en rien que votre équipe est travaille sur le sujet.

Les hackers consacrent du temps à dénicher des failles et à rédiger des rapports, 
il est donc tout à fait normal de leur donner des nouvelles de l'avancement de leur signalement.
Chaque étape doit faire l'objet d'un commentaire (changement de statut, attribution d'une récompense, divulgation publique, etc.),
 l en va de même si après un certain temps aucune action n'a été entreprise.

Par ailleurs, si un hacker vous sollicite pour obtenir des informations, vous êtes tenu de lui répondre.
Il peut s'agir, par exemple, d'une simple confirmation que le ticket a bien été examiné, que votre équipe est sur le coup, 
ou que vous avez besoin de temps pour déterminer le montant de la prime...
Peu importe la teneur exacte : **La communication est primordiale.**

Bien entendu, le délai de réponse constitue également un facteur déterminant.
C'est l'indicateur principal de la réactivité et de l'efficacité de votre programme.


## Manque de cohérence

Ce n'est manifestement pas l'aspect le plus simple. 
Bien que cela puisse paraître évident, veillez à évaluer correctement la gravité des vulnérabilités ainsi que les récompenses associées,
surtout si vous avez pour habitude de rendre les rapports publics.

Qu'est-ce que cela signifie ?
Accorder une prime de 300 $ pour une fuite de chemin absolu est très généreux, mais cela deviendra problématique si 
vous ne proposez que 100$ pour une faille XSS.
Une telle décision serait incompréhensible.

Pour vous aider dans cette démarche, certaines plateformes proposent une grille d'évaluation des vulnérabilités, 
classant les problèmes selon leur degré de gravité.
Une autre approche consiste à utiliser le [score CVSS](https://www.first.org/cvss/calculator/3.0), en l'associant à une politique de primes bien définie.
Quelle que soit la méthode choisie, gardez à l'esprit que le contexte et l'impact sont toujours des paramètres essentiels à prendre en compte.
Par exemple, une exécution de code à distance sur un serveur oublié, isolé de votre réseau et ne contenant aucune donnée, 
n'est pas aussi critique qu'une exécution de code sur le serveur de production de votre site principal.

<img src="/images/bugcrowd-vrt.jpg" alt="bugcrowd vrt" />


## Le manque de souplesse

Quel que soit le périmètre que vous avez défini, vous recevrez inévitablement des rapports concernant des éléments situés en dehors de celui-ci,
cela ne signifie pas pour autant qu'ils ne méritent pas votre attention.
Il vous appartient de décider si vous acceptez ou non un bug découvert sur un actif qui n'est pas inclus dans 
le périmètre (voire qui en est explicitement exclu). 
Toutefois, si l'impact de cette vulnérabilité concerne un actif essentiel qui, lui, fait bien partie du périmètre, 
alors vous devriez **y réfléchir à deux fois et reconsidérer votre position**.

Faire preuve d'une <u>trop grande rigidité</u> vis-à-vis du périmètre en ignorant ou en refusant de récompenser des bugs critiques 
qui affectent votre service ou votre produit <u>ne fera qu'irriter les hackers</u>, qui ne comprendront pas votre décision.
Regardez ce [contre-exemple flagrant chez Twitter](https://hackerone.com/reports/273698) : un chercheur y a découvert une faille permettant 
de lire des tweets privés via `niche.co`, mais le rapport a été classé comme <i>non éligible à une récompense. Merci de nous aider à sécuriser Twitter !</i>

Voici comment je vois les choses :  
<img src="/images/bugbounty-scopes.jpg" alt="bug bounty scopes" width="600" />


## Propos inappropriés / sanctions

Choisissez votre équipe de triage avec soin.

Comme indiqué dans le paragraphe précédent, la communication est LA clé.
Un bon responsable du triage doit posséder plusieurs compétences : patience, sens de la communication, 
maîtrise de l'anglais, gestion de communauté, connaissances techniques...
Les personnes chargées du triage constituent l'interface de l'entreprise, elles incarnent votre marque, et votre image dépend d'elles.

**Il faut absolument éviter de perdre patience, de s'emporter contre les hackers, d'utiliser des termes inappropriés ou de proférer des menaces.**
Encore une fois, si vos responsables du triage ne respectent pas le travail acharné des hackers, votre réputation en pâtira gravement.

Les bannissements sont assez rares, mais ils peuvent parfois survenir.
Si vous vous retrouvez dans une telle situation, prenez un moment pour comprendre pourquoi les choses sont devenues si complexes.
Gardez à l'esprit que certains bug hunters exercent cette activité à plein temps, c'est un travail très exigeant, 
où les revenus et la réputation sont au coeur de l'activité, ce qui peut mettre les nerfs à rude épreuve.


## Ressources externes

- [Système de classification des vulnérabilités selon Bugcrowd](https://bugcrowd.com/vulnerability-rating-taxonomy)
- [Grille tarifaire des vulnérabilités de Zerocopter](https://www.zerocopter.com/vulnerability-price-list)
- [Formule de calcul des primes par EdOverflow](https://github.com/EdOverflow/bounty-formula)
