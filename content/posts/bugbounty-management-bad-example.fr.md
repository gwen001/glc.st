---
date: 2019-12-16
slug: "bugbounty-un-mauvais-exemple-de-gestion"
title: "Bug bounty : un mauvais exemple de gestion"
related:
  - /courses/bugbounty
  - /posts/interview-bugbounty-program
  - /posts/bugbounty-management-zomato
  - /posts/bugbounty-management-hackers-motivation
  - /posts/bugbounty-management-todo-before-launch
  - /posts/bugbounty-management-what-to-avoid
  - /posts/bugbounty-unonfficial-rules
tags:
  - bug bounty
  - management
---
J'ai récemment publié un article sur un [bon exemple de programme de bug bounty](/posts/bug-bounty-management-a-great-example-zomato/), 
j'ai donc également souhaité aborder les comportements que les entreprises doivent éviter afin de maintenir la motivation de leur communauté.

En 2017, j'ai participé à un excellent programme, tellement excellent que j'y ai consacré un article intitulé "[Le programme de bug bounty qui a changé ma vie](/posts/the-bug-bounty-program-that-changed-my-life/)".
Comme j'avais obtenu de très bons résultats dans le cadre de ce programme, j'ai décidé, il y a quelques semaines, de retenter l'expérience.
J'ai consulté la page Hackerone de ce programme et j'y ai constaté des modifications dans le règlement.
J'ai rapidement signalé quelques bugs mineurs et remarqué par la même occasion, des changements dans le processus de gestion des rapports.

Malheureusement, tous ces changements rendent désormais le programme **déplorable**.
Voici mon analyse.
<!--more-->

## Historique

Tout d'abord, je vais évoquer un programme privé, comme je ne souhaite pas enfreindre la politique de confidentialité de HackerOne 
(et même s'il est possible de lister tous les programmes via le sitemap #bugbountytips), je ne mentionnerai pas le nom réel de l'entreprise.
Je l'appellerai donc : **Willoz** (un peu de créativité).

D'après [Crunchbase](https://www.crunchbase.com/), Willoz est un groupe ayant réalisé 12 acquisitions.
À ma connaissance, ce groupe gère actuellement 11 programmes sur HackerOne - un par acquisition. 
Tous sont privés, ont été créés entre juillet 2016 et février 2017, et suivent les mêmes règles.
Cependant, 5 d'entre eux sont "en pause" : cela signifie qu'il est déconseillé de soumettre un rapport, 
sauf en cas de découverte d'une vulnérabilité critique (le formulaire de soumission reste accessible),
 ou bien il convient de contacter directement l'entreprise via l'adresse e-mail d'urgence fournie : bugbounty@willozgroup.com.

Comme je l'ai mentionné précédemment, mon expérience personnelle avec cette entreprise a toujours été excellente.
Mes rapports ont systématiquement été traités en une semaine et récompensés très rapidement, souvent en moins de 48 heures, 
grâce à leur politique de "paiement au triage".
Compte tenu de la qualité de mon travail, ils ont créé un canal Slack privé réunissant le responsable de la sécurité de la marque, 
le responsable de la sécurité et moi-même.
Nous y avons échangé nos réflexions, et ils m'ont donné accès à des périmètres privés, non répertoriés sur Hackerone à l'époque, 
sur lesquels j'ai également obtenu de très bons résultats.
Ces personnes figurent désormais dans mes contacts sur LinkedIn.

Analysons ensemble ce qui s'est passé.


## Récompenses

La première chose que j'ai remarquée, ce sont les changements apportés aux primes.  
Depuis août 2019 : **_4500$, 2250$, 1125$, 450$_**  
Avant cette date : _1000$, 500$, 250$, 150$_ || _1500$, 750$, 375$, 250$_

Waouh ! Une belle augmentation après 3 ans !  
Cela signifie qu'ils ont considérablement renforcé leur sécurité, ils ne craignent donc plus de payer davantage pour des vulnérabilités pertinentes, 
même les plus mineures.
Le montant minimum sur Hackerone est de 500$, tout comme chez Facebook, 450$ est donc une somme très intéressante.
Peut-être sont-ils très confiants, estimant qu'il n'y a plus de failles mineures ou tout simplement un gros budget.

Quoi qu'il en soit, il s'agit d'une évolution classique pour un programme de bug bounty.
On commence généralement par une approche linéaire et, une fois que la plupart des vulnérabilités critiques ont été découvertes <u>et corrigées</u>, 
**il est possible d'augmenter les primes maximales pour mettre davantage l'accent sur les failles à haute criticité**.

![politique de primes](/images/willoz-bounty-policy.png)

Alors, où est le problème ?  
Le problème ne réside pas dans les montants.
Il est tout simplement inutile d'augmenter autant les primes si, au final, l'objectif est d'éviter autant que possible de payer, 
en invoquant une multitude d'excuses ou en appliquant une politique absurde.
Une autre explication possible est qu'ils reçoivent peu de rapports et cherchent donc à motiver les hackers à travailler sur leurs programmes.


## Gestion

Juste après avoir envoyé mon premier rapport, j'ai reçu un message de la part de l'"équipe Hackerone" m'informant que mon rapport avait été validé.
Bon, les **programmes sont désormais gérés par Hackerone**.
Est-ce une mauvaise chose ?

Ce n'est pas forcément mauvais, mais, à mon avis, c'est loin d'être idéal.
Cela signifie que les rapports seront traités très rapidement, mais pas nécessairement de manière appropriée.
Les personnes chargées du triage ne connaissent pas tous les détails liés à la sécurité de l'entreprise, 
elles doivent se renseigner concernant la marche à suivre pour chaque type de vulnérabilité : 
faut-il accorder une récompense, et si oui, quel en doit être le montant...
Lorsqu'elles prennent des décisions par elles-mêmes, cela conduit souvent à des erreurs importantes.
Je peux affirmer sans me tromper que les hackers n'apprécient généralement pas (pour ne pas dire qu'ils détestent) les programmes gérés par des tiers.

![Plainte concernant le tri](/images/willoz-triager-complaint.png)

Deuxième point : cela **rompt le lien entre l'équipe de sécurité et les hackers**.
C'est précisément l'une des raisons pour lesquelles j'aime le bug bounty : pouvoir échanger avec des employés d'entreprises aux quatre coins du monde, 
les aider à corriger leurs vulnérabilités, discuter avec eux et partager des connaissances.
Dans ce cas précis, cette relation de confiance, bâtie au fil du temps, a été anéantie en un instant.


## Cibles

Chaque entité acquise dispose de son propre site web, ce domaine constitue, avec l'éventuelle application mobile associée, 
le périmètre principal du programme.

Auparavant, il était possible de signaler une vulnérabilité sur n'importe quel site web appartenant au groupe, dans le cadre du programme principal.
Il m'est déjà arrivé de signaler des failles critiques sur des domaines qui ne figuraient pas explicitement dans le périmètre, 
et j'ai été récompensé comme il se doit.
Malheureusement, il semble que l'entreprise ait décidé de revoir sa position et de faire marche arrière.
J'ai signalé un Subdomain Takeover, ce signalement a été rejeté, ce qui est surprenant, étant donné qu'un cas similaire avait été accepté il y a deux ans.

Ne vous méprenez pas : je ne blâme pas l'entreprise, car elle ne fait qu'appliquer ses propres règles, ce serait absurde de ma part.
Le domaine ne fait pas partie du périmètre, je ne devrais donc rien attendre en retour, soit, c'est tout à fait compréhensible.
Cependant, adopter une approche très permissive, en autorisant certains signalements, pour ensuite changer d'avis soudainement, 
sans communication préalable, n'est pas une bonne stratégie. 
Cela est particulièrement vrai vis-à-vis de personnes qui vous font confiance, surtout lorsque vous avez déjà récompensé ces mêmes personnes 
par le passé dans des situations identiques.

Lorsque vous vous lancez dans le bug bounty, il est préférable de **définir un programme initial avec un périmètre restreint, puis de l'élargir progressivement**, plutôt que de le restreindre par la suite.


## Périmètre et exclusions

Je ne vais pas retracer toute l'historique du règlement, mais voici les étapes les plus marquantes.

Au départ, les programmes comportaient des exclusions classiques :

+ Déni de service (DoS)
+ Divulgation de version
+ Rapports générés par des outils automatisés ou des scans
+ Ingénierie sociale
+ Tentatives d'accès physique
+ Usurpation de contenu / injection de texte
+ CSRF lors de la connexion ou de la déconnexion
+ Clickjacking
+ ...

Fin 2018, les éléments suivants ont été ajoutés :

+ Attaques Punycode
+ Problèmes liés à la limitation du taux de requêtes (rate limiting)
+ Injection d'URL (lorsqu'une injection HTML n'est pas possible)
+ Fuite de token de réinitialisation de mot de passe vers des services tiers

En 2019, des changements majeurs sont intervenus :

+ Les environnements de préproduction, de test et de démo sont désormais exclus du périmètre
+ Cross-Site Scripting (XSS)
+ Cross-Site Request Forgery (CSRF)

Si certaines entreprises acceptent tous types de vulnérabilités, il est assez courant que des problèmes comme le clickjacking, 
l'injection de texte ou la CSRF lors de la connexion/déconnexion soient exclus lorsqu'ils ne débouchent pas sur des failles critiques.
Cela ne pose aucun problème en soi.
Cependant, il est assez inhabituel d'exclure totalement les XSS et les CSRF, surtout <u>après trois ans</u> !

Un programme de bug bounty évolue au fil des mois et des années, il est donc tout à fait normal que la section dédiée aux exclusions évolue également.
Le groupe Willoz ne fait pas exception : ils ont fait évoluer cette partie du programme, mais peut-être un peu trop, 
la tendance devrait aller dans le sens inverse.
Comme ils ont déjà corrigé les vulnérabilités majeures, ainsi qu'une grande partie des failles de gravité moyenne 
(les XSS et CSRF ayant été incluses pendant 3 ans), le programme devrait, au contraire, devenir plus permissif 
et **accepter une plus grande variété de vulnérabilités**.

Par ailleurs, ils ont décidé d'exclure plusieurs environnements, ce qui réduit considérablement le périmètre des tests.
Or, en principe, **le périmètre devrait s'élargir** à mesure que le programme gagne en maturité et que l'équipe de sécurité acquiert davantage d'expérience.


## Absence de réponse

Comme les programmes sont gérés par la plateforme, les rapports sont d'abord pris en charge par les équipes de tri de celle-ci.
Selon HackerOne, même lorsqu'un programme est géré par une plateforme, l'équipe de sécurité de l'entreprise conserve un accès à 
tous les rapports et peut donc intervenir à tout moment pour échanger avec les chercheurs.

En tant que hunter, lorsque je n'obtiens aucune réponse après quelques jours ou semaines, je lance _ping_ sur le rapport pour attirer 
l'attention des équipes de triage.
Je peux dire qu'**être ignoré est, de loin, le sentiment le plus frustrant**.
Non seulement l'équipe de sécurité, mais aussi les équipes de triage de la plateforme, restent silencieuses.

Certes, les entreprises ont des priorités, une redirection ouverte n'a rien à voir avec une exécution de code à distance, mais cela fait partie du jeu : 
chaque rapport devrait être traité avec la même rigueur.
Par ailleurs, après 3 ans d'existence, cette entreprise est censée disposer d'un workflow interne bien établi, 
*les processus internes devraient être parfaitement rodés**.
Ignorer un rapport pendant 2 mois, malgré 3 relances, est inacceptable. 
Un tel comportement ne permet certainement pas de gagner la confiance de la communauté.

Petite précision : lorsque vous envoyez un message à l'adresse e-mail d'urgence fournie par l'entreprise dans ses règles, 
vous recevez la réponse suivante :  
![réponse automatique](/images/willoz-autoreply.png)

En deux mots : une impasse.
Quel est l'intérêt d'imposer des <u>exigences absurdes</u>, fondées sur des chiffres dénués de sens, pour finalement vous renvoyer 
vers cette même adresse e-mail ?
C'est une perte de temps pour tout le monde.


## Sanction

C’est totalement par pur hasard que j’ai été contacté par leur meilleur hacker, au moment précis où je rencontrais des difficultés avec la médiation de Hackerone.
Il s’agit d’un bug hunter très connu au sein de la communauté, qui pratique à plein temps, pour autant que je sache. 
Comme il participe aux programmes de Willoz depuis leurs débuts (en 2016), on peut dire qu’il entretenait également de bonnes relations avec eux.

Comme l’indique le règlement :

> Toute tentative non autorisée visant à contourner la plateforme HackerOne pour contacter directement des employés (actuels ou anciens) du groupe Willoz, dans le cadre de discussions liées au programme de bug bounty, peut entraîner une exclusion de notre programme. Veuillez maintenir tous vos échanges au sein de la plateforme HackerOne.


<div style="float:left;width:450px">
    Ne voyant pas ses rapports avancés, ce hacker a tenté de contacter l’entreprise par e-mail. 
    Ce qui devait arriver arriva : il a été banni. 
    <br/><br/>
    Quelle erreur de la part de l’entreprise !
</div>

<img src="/images/willoz-unhappy-hacker.png" alt="hacker mécontent" width="350" style="float:right;" />
<div style="clear:both;"></div>

**Le bannissement ne devrait être envisagé qu’en tout dernier recours.** et le fait qu’un hacker avec 3 années d’expérience sur le programme, 
en vienne à tenter un "contact direct", devrait constituer un signal d’alerte majeur pour l’équipe de sécurité, c’est un indicateur très révélateur.


## Conclusion

Au vu de tous ces changements, une seule conclusion s'impose : le groupe a embauché un nouvel ingénieur sécurité en charge du programme de bug bounty. 
Il fait sans doute de son mieux, mais c'est un échec cuisant. 
Les bonnes pratiques en la matière ne sont plus respectées, contrairement à avant :

- un périmètre plus restreint, moins de cibles : après une si longue expérience, c'est tout le contraire
- l'augmentation des récompenses incite esquiver les paiements : inutile
- relations rompues avec les hackers

**La communication est essentielle en matière de bug bountys.**
Que le rapport soit accepté ou non, rémunéré ou non, et quel que soit le montant, 
l'entreprise doit _toujours_ tenir le hacker informé de l'état de sa demande.

Et <u>c'est le rôle des responsables de l'entreprise, pas de l'équipe de triage de la plateforme</u>.

Lorsque je commence à chasser sur un nouveau programme, je signale toujours quelques bogues "mineurs" pour tester la réactivité 
de l'équipe et évaluer l'intérêt de creuser davantage. 
Dans ce cas précis, j'ai signalé 1 prise de contrôle de sous-domaine, 4 attaques XSS et 4 redirections ouvertes, seules deux ont été acceptées.

Ciao...
