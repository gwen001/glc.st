---
date: 2019-11-19
slug: "bugbounty-un-tres-exemple-bon-de-gestion-zomato"
title: "Bug bounty : un très exemple bon de gestion : Zomato"
related:
  - /courses/bugbounty
  - /posts/interview-bugbounty-program
  - /posts/bugbounty-management-bad-example
  - /posts/bugbounty-management-hackers-motivation
  - /posts/bugbounty-management-todo-before-launch
  - /posts/bugbounty-management-what-to-avoid
  - /posts/bugbounty-unonfficial-rules
tags:
  - bug bounty
  - management
---
Ayant décidé de me consacrer à mon rôle de conseiller dans le secteur du bug bounty, je souhaitais en savoir plus sur les caractéristiques 
d'un bon programme, ses points clés et les pièges à éviter.

J'ai contacté Prateek Tiwari, responsable cyber chez [Zomato](https://twitter.com/Zomato), qui a eu la gentillesse de répondre à toutes mes questions.
Découvrons comment Zomato est devenu l'un des meilleurs programmes, non seulement sur Hackerone, mais dans tout le secteur du bug bounty.
<!--more-->

<a href="https://hackerone.com/zomato" title="Zomato sur Hackerone"><img src="/images/zomato.png" title="Zomato sur Hackerone" alt="Zomato sur Hackerone" /></a>


__Pouvez-vous vous présenter ?__  
Je suis Prateek Tiwari, originaire d'Inde, et vous pouvez me suivre sur sous le pseudo [@prateek_0490](https://twitter.com/prateek_0490) sur toutes les plateformes.
Je pratique le hacking depuis environ 2 ans et demi. J'ai rejoint Zomato en mars 2018.
J'y suis responsable de l'équipe cyber, chargé de développer une solution et une infrastructure commune.


__Pouvez-vous décrire brièvement Zomato ?__  
Lancé à Delhi il y a 11 ans, Zomato est passé d'un projet personnel à l'un des plus grands agrégateurs de repas au monde.
Présents dans 24 pays et plus de 10000 villes à travers le monde, nous concrétisons notre vision : une meilleure alimentation pour tous.
Nous mettons en relation les consommateurs et les restaurants, et nous collaborons étroitement avec eux pour créer un écosystème durable.


__Combien de personnes composent votre équipe de sécurité ?__  
Nous sommes encore une petite équipe de 6 personnes.


__Quel type d'audit réalisez-vous ?__  
Nous faisons appel à des agences spécialisées pour réaliser des audits (VAPT) chaque trimestre.
De plus, nous effectuons des audits internes comprenant des tests boîte blanche/boîte noire et des exercices de type red team.


__Pouvez-vous partager des statistiques sur le programme de bug bounty ?__  
Au début de chaque mois, nous publions tous ces détails sur Twitter.
Nos SLA sont également disponibles sur [notre page de politique sur HackerOne](https://hackerone.com/zomato).

<div style="float:left;width:550px">
    Remontées valides pour 2019: Q1:28 | Q2:16 | Q3:28 | Q4:15  
    <br/>Prime moyenne/trimestre en 2019: environ 13~15K$  
    <br/>Plus grosse prime en 2019: <a href="https://hackerone.com/reports/512968">4500$ pour une erreur de logique métier</a>  
    <br/>Temps moyen avant triage: 2 heures  
    <br/>Temps moyen avant prime: 1 jour  
</div>

<img src="/images/zomato-stats.png" alt="Zomato stats" width="270" style="float:right;" />
<div style="clear:both;"></div>


__Quels changements majeurs sont survenus dans le programme ces 3 dernières années ?__  
Au départ, nous nous sommes davantage concentrés sur les processus internes.
Il était essentiel de collaborer avec tous les services de l'organisation afin d'harmoniser les pratiques et 
de sensibiliser chacun aux risques et menaces en matière de sécurité.
Nous souhaitions garantir que nos employés et nos fournisseurs respectent le règlement que nous avons défini.
Une fois le dispositif en place, nous avons modifié le système de primes et élargi son périmètre.

Nous avons également procédé à des promotions et récompensé les chercheurs par des bonus, 
éléments importants pour attirer les meilleurs talents et les inciter à s'investir dans le programme.
Par ailleurs, la réduction des délais de traitement est un autre point crucial sur lequel nous avons mené une étude approfondie.
De nombreux changements restent à apporter et nous y travaillons activement.


__D'après ce que je vois, le programme n'est pas géré par Hackerone. Y a-t-il une raison particulière à cela ?__  
Nous étions auparavant gérés par Hackerone, mais nous avons décidé de passer à un programme non géré pour plusieurs raisons :

- Le nombre de signalements que nous recevions était facilement gérable par notre équipe interne.
- Disposant de notre propre équipe, il nous est facile de comprendre les signalements et les risques, et de planifier les mesures d'atténuation.


__Comment évaluez-vous les vulnérabilités ?__  
Nous analysons le nombre d'utilisateurs susceptibles d'être affectés, s'il s'agit d'une exploitation directe, 
s'il existe des prérequis pour exploiter la faille, etc.
Par exemple, une injection SQL est classée P1, une fuite de données personnelles (IDOR) est également classée P1, et ainsi de suite.

Notre politique d'évaluation est définie sur [notre page HackerOne](https://hackerone.com/zomato) ainsi que dans le tableau des primes.

![Zomato](/images/zomato-bounty-table.png)


__Comment gérez-vous les bugs en interne ?__
De manière proactive et efficace :)
Du signalement au triage, cela prend probablement moins d'une heure.
S'il s'agit d'un bug de tyrpe P1, nous le corrigeons en quelques minutes à une heure, selon sa gravité et la solution de contournement disponible.
Nous utilisons également Jira pour le suivi interne des bugs.

__Quels types de bugs rencontrez-vous le plus souvent ?__
Honnêtement, la plupart des bugs que nous rencontrons sont non applicables NA.
Actuellement, le nombre de rapports valides que nous recevons a considérablement diminué, il n'y a donc pas vraiment de "type de bug prédominant".
Mais en moyenne, les bugs les plus fréquents sont les CSRF et les XSS.

__Des changements sont-ils prévus prochainement ?__
Oui, nous prévoyons d'élargir le périmètre d'intervention, d'augmenter les récompenses et d'organiser un 
événement en direct (qui devrait avoir lieu prochainement).

__Avez-vous des conseils pour les entreprises qui gèrent ou souhaitent gérer un programme de bug bounty ?__
Pour les entreprises qui gèrent déjà ou souhaitent gérer un programme de bug bounty, voici quelques points à prendre en compte :

- Réponse rapide.
- "Corrigez vos bugs", les hunters se désintéresseront si vous ne corrigez pas.
- Prime au triage : cela attirera de nombreux chercheurs, car le chercheur vous aura transmis son rapport, 
ce qui signifie qu'il a investi du temps et des efforts. Ne le faites pas attendre des mois pour sa récompense.
- Périmètre et règlement clairs, ainsi qu'un tableau des primes.
- Recueillez régulièrement les commentaires des hackers et fidélisez-les en veillant à ce qu'ils restent motivés par votre programme.

__Un mot pour les hackers ?__
Merci à tous les hackers qui ont participé au programme jusqu'à présent.
Notre programme de bug bounty avec Hackerone continuera d'évoluer, car nous nous efforçons de renforcer notre relation avec 
la communauté des hackers grâce à chaque rapport.


## Conclusion

Après des années difficiles dans le secteur du bug bounty, la situation a radicalement changé : 
Zomato a considérablement amélioré son processus interne de gestion des signalements.
Les SLA parlent d'eux-mêmes : un délai de réponse de 2 heures et un délai de résolution d'1 jour représentent un véritable rêve pour 
tous les acteurs, entreprises et hunters confondus.
L'entreprise a également revu sa communication avec les hackers, ce qui est très apprécié.

<blockquote class="twitter-tweet" data-partner="tweetdeck"><p lang="en" dir="ltr">17 hours to triage, resolve, and reward! That’s how fast <a href="https://twitter.com/Zomato">@Zomato</a>’s program on <a href="https://twitter.com/Hacker0x01">@Hacker0x01</a> is.

I’d like to also give a shoutout to <a href="https://twitter.com/EdOverflow">@EdOverflow</a>.
In that narrow timeframe, he managed to mentor me!</p>&mdash; Karim Rahal (@KarimPwnz) <a href="https://twitter.com/KarimPwnz/status/1190585460072038405">July 23, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

N'oubliez pas que plus votre programme plaît aux hackers, plus ils travailleront dessus, 
ce qui signifie plus de bugs découverts et donc une sécurité renforcée !

<br />

Un grand merci à [Prateek Tiwari](https://twitter.com/prateek_0490).
