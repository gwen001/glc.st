---
date: 2020-01-16
slug: "bugbounty-5-choses-a-faire-avant-de-lancer-votre-programme"
title: "Bug Bounty: 5 choses à faire avant de lancer votre programme"
related:
  - /courses/bugbounty
  - /posts/interview-bugbounty-program
  - /posts/bugbounty-management-bad-example
  - /posts/bugbounty-management-hackers-motivation
  - /posts/bugbounty-management-what-to-avoid
  - /posts/bugbounty-management-zomato
  - /posts/bugbounty-unonfficial-rules
tags:
  - bug bounty
  - management
---
Bienvenue en 2020 !
J'entame cette année avec une série d'articles consacrés à la gestion de programmes.
Dans celui-ci, je vais tenter d'expliquer certains points essentiels que toute entreprise devrait aborder AVANT de lancer 
son premier programme de bug bounty.
Comme il n'existe pas de règles officielles en la matière, chacun reste libre de procéder comme il l'entend.
Ces articles relèvent donc davantage des bonnes pratiques et des conseils utiles.
<!--more-->


## Réalisez des tests d'intrusion et des audits

Testez avant les tests.

Vous avez des vulnérabilités, oui, assurément, et en grand nombre. 
Vous ne le savez tout simplement pas encore.
Certaines peuvent être facilement détectées lors d'un test d'intrusion ou d'un audit de code, 
les failles les plus courantes peuvent également être identifiées par la plupart des outils d'analyse automatisés utilisés par les entreprises de cybersécurité.
Une fois ces vulnérabilités identifiées, deux options s'offrent à vous :

- les corriger
- les ajouter à la section hors périmètre/problèmes connus/non corrigés du règlement de votre programme

Si vous ne faites rien, en vous disant que vous les traiterez "plus tard", des hackers finiront par les découvrir et vous les signaler. 
Vous devrez alors les rémunérer (du moins au début) pour éviter de susciter leur mécontentement.
Il est inacceptable de marquer ces rapports comme N/A en adoptant une attitude du type "oui, oui, nous sommes déjà au courant" 
tout en refusant de verser une récompense, une telle pratique détruirait votre réputation.
Par ailleurs, vous ne voulez certainement pas perdre votre temps, encore et encore, à gérer des rapports concernant des 
vulnérabilités dont vous avez déjà connaissance.

<u>Se lancer dans un programme de bug bounty sans disposer au préalable de bases solides en matière de sécurité risque, littéralement, de tout compromettre.</u>


## Préparez votre infrastructure technique

Les hackers sont à l'oeuvre : soyez prêts !

Même si vous demandez aux participants de ne pas utiliser de scanners automatisés, 
sachez que les hackers ont recours à des outils qui génèrent un volume de trafic important.
Vous voulez probablement éviter les attaques par déni de service et, par la même occasion, éviter d'avoir à suspendre votre programme parce que 
votre système n'est pas capable d'absorber la charge.
Une telle interruption freinerait les hackers dans leur élan et leur ferait perdre une motivation précieuse.
Il est extrêmement frustrant de devoir cesser ses recherches sur une cible alors que l'on a justement mis le doigt sur quelque chose d'intéressant.

Par ailleurs, quel intérêt y a-t-il à lancer un programme de bug bounty si vos hackers se retrouvent bannis par votre pare-feu au bout d'une heure, 
sans qu'il existe un moyen simple de les réintégrer ?
Vous ne voulez certainement pas avoir à gérer manuellement une liste noire interminable d'adresses IP.

![service unavailable](/images/ie_service_unavailable.png)


## Préparez votre process interne

Les hackers sont de plus en plus actifs ; soyez mieux préparés !

Dès l'ouverture de votre programme, qu'il soit privé ou public, vous recevrez vos premiers rapports en moins d'une heure (~15/30mn).
Il est crucial de réfléchir à vos flux internes **AVANT** de lancer le programme.
Il est fort probable que le volume de rapports augmente dans les jours qui suivent.
D'une part, vous ne voulez pas vous retrouver submergés, d'autre part il est inacceptable de devoir suspendre le programme simplement parce que vous pensiez,
à tort, ne pas être (trop) vulnérable.

<div style="float:left;width:620px">
    Ne sous-estimez surtout pas les hackers. 
    J'ai vu bien trop de programmes être "mis en pause" parce qu'ils n'étaient pas en mesure de gérer un flux de rapports en constante augmentation. 
    Cela arrive parce que certains pensent pouvoir gérer tout ce volume en une seule journée par semaine, parce que l'équipe de sécurité n'est pas prête, 
    parce que les personnes chargées du tri ne maîtrisent pas l'anglais, parce que le responsable du programme doit demander une autorisation pour 
    chaque récompense, ou encore parce que personne ne sait comment transmettre les bugs à l'équipe de développement, etc.
    <br/><br/>
    A droite, un exemple de flux de traitement d'un rapport, illustrant la manière dont il devrait être géré <u>sur la plateforme</u>. 
    À vous ensuite de définir votre propre processus interne. 
    <br/><br/>
    La plupart des plateformes permettent l'intégration d'outils externes, tels que des systèmes de suivi de bugs, Jira, Slack, Zendesk, GitLab, etc.
    N'hésitez pas à exploiter ces fonctionnalités autant que nécessaire pour optimiser la gestion de vos rapports.
</div>
<a href="/images/report-flow.png" title="Flux de traitement des rapports" target="_blank"><img src="/images/report-flow.png" alt="Flux de traitement des rapports" width="200" style="float:right;" /></a>
<div style="clear:both;"></div>


## Étudiez les autres programmes

Vous n'êtes pas le premier à vous lancer, apprenez donc des programmes existants, de leurs erreurs comme de leurs réussites.

- Consultez leur règlement ou inspirez-vous-en, voire copiez/collez et adaptez pour rédiger le vôtre.
- Examinez leur barème de primes pour connaître les récompenses proposées (argent, produits dérivés, etc.), tout en gardant à 
l'esprit que cela dépendra, bien entendu, de votre propre budget.
- Observez le périmètre (couvert par leurs tests : sous-domaines, domaines génériques, API, applications mobiles, etc.
- Lisez attentivement la section relative aux éléments exclus du périmètre afin d'éviter les pièges classiques,
vous ne voudriez certainement pas épuiser votre budget annuel pour des signalements mineurs, 
comme une configuration SPF manquante ou des erreurs liées aux certificats SSL.
- Analysez les rapports rendus publics : comment les responsables du programme répondent aux chercheurs en sécurité, les délais de réponse, s'ils utilisent des messages automatisés, la qualité de leur communication (formules de politesse, remerciements, ton employé, etc.).
(Exemples à suivre : [Hackerone](https://hackerone.com/security/hacktivity), [Valve](https://hackerone.com/valve/hacktivity) ; exemple à éviter : [Twitter](https://hackerone.com/twitter/hacktivity))


## Rejoignez la communauté

Si ce n'est pas déjà fait, rejoindre la communauté sur [Twitter](https://twitter.com/search?q=bugbounty) pourrait bien être l'une de vos meilleures initiatives.

Commencez par suivre les plateformes ainsi que certains hackers reconnus dans le milieu.
Vous comprendrez ainsi mieux leurs attentes, leurs motivations et ce qu'ils recherchent.
Par ailleurs, votre équipe de sécurité pourra beaucoup apprendre à leur contact : quelles sont les vulnérabilités en vogue, quels outils sont utilisés, etc.

Au fil du temps, vous serez amené à suivre davantage de hackers, VOS hackers.
Vous serez immédiatement informé si quelqu'un évoque vos programmes, qu'il s'agisse d'éloges ou de critiques, ces retours sont toujours précieux pour progresser.


## Join the community

If not done yet, joining the community on [Twitter](https://twitter.com/search?q=bugbounty) could be your best move ever.  

Start by following the platforms and some popular hackers in the bug bounty industry.
From here you will be able to better understand what are their expectations, their motivations, what they are looking for.
More, your security team will learn a lot from them, the current popular vulnerabilities, the tools they use etc...

Time after time you will follow more hackers, YOUR hackers.
You will be immediately warned if someone talk about your programs, good words or complaints are always important to improve yourself.


## External resources

[Les différents types d'audits par Daniel Miessler](https://danielmiessler.com/study/security-assessment-types/#vulnerability)  
[Rapport 2018 sur la sécurité axée sur les hackers par Hackerone](https://www.hackerone.com/sites/default/files/2018-07/The%20Hacker-Powered%20Security%20Report%202018.pdf)  
[Flux de traitement des rapports par YesWeHack](https://blog.yeswehack.com/2019/07/24/a-quick-update-on-our-point-system/)  
