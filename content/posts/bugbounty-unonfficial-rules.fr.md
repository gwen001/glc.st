---
date: 2022-08-01
slug: "les-regles-sous-jacentes-du-bug-bounty"
title: "Les règles sous-jacentes du bug bounty"
related:
  - /courses/bugbounty
  - /posts/bugbounty-management-bad-example
  - /posts/bugbounty-management-hackers-motivation
  - /posts/bugbounty-management-todo-before-launch
  - /posts/bugbounty-management-what-to-avoid
  - /posts/bugbounty-management-zomato
  - /posts/platform-tournament
  - /posts/swag-store
tags:
  - bug bounty
  - management
---
Les programmes de bug bounty s'accompagnent de plateformes, de périmètres et de rémunérations, mais aussi d'un ensemble de règles officieuses.
Ces normes invisibles façonnent les interactions entre les hackers et les entreprises, qu'il s'agisse de communication, de confiance, 
des usages en matière de signalement de vulnérabilités ou des récompenses.
Cet article met en lumière les bonnes et les mauvaises pratiques, les « à faire » et les « à éviter », que tout chasseur de vulnérabilités et 
tout responsable de programme se doit de connaître.
<!--more-->


## 3 entités, 3 rôles

Le milieu du bug bounty se compose principalement de 3 grandes entités avec chacune un rôle spécifique :

**Programmes : le besoin.** Les entreprises souhaitant tester leur sécurité.  
Au-delà du tri des remontées, les entreprises ont fort à faire.
Établir des règles détaillées pour orienter les chercheurs vers les vulnérabilités les plus critiques.
Préparer les processus internes des équipes concernées pour gérer le flux de rapports.
Évaluer la sévérité de chaque remontée, verser les récompenses correspondantes et, enfin, <u>corriger les failles</u>.

&gt; [5 points à aborder avant de lancer votre premier programme de bug bounty](/posts/5-things-to-do-before-running-your-first-bug-bounty-program/)  
&gt; [5 erreurs à éviter dans le cadre d'un programme de bug bounty](/posts/5-things-to-avoid-in-bug-bounty/)  
&gt; [Comment maintenir la motivation des hackers dans un programme de bug bounty](/posts/how-to-keep-hackers-motivated-in-bug-bounty/)

**Hackers : la solution.** Des passionnés qui souhaitent mettre leurs compétences à profit.  
Leur mission consiste, bien évidemment, à découvrir des failles, mais pas seulement.
La rédaction d'un rapport de qualité est essentielle pour permettre aux entreprises de bien comprendre la nature du problème.
Il incombe également aux hackers de démontrer la gravité des failles, <u>ainsi que leur impact sur l'activité</u> de l'entreprise, 
un critère déterminant pour estimer au mieux le montant des récompenses.
Enfin, ils peuvent vérifier la correction déployée ; une démarche facultative, mais très appréciée par les entreprises.

&gt; [Comment rédiger un rapport dans le cadre d'un programme de bug bounty](/posts/how-to-write-a-bug-bounty-report/)

**Plateformes : le lien manquant.**  
Le secteur du bug bounty, tel que nous le connaissons aujourd'hui, n'existerait probablement pas sans ces plateformes.
Cela ne signifie pas pour autant qu'elles occupent une position dominante ; elles ont un rôle crucial à jouer auprès des deux autres parties prenantes.
Leur mission consiste essentiellement à accompagner les entreprises ET les hackers, en instaurant une relation de confiance entre tous les acteurs.
<u>Elles jouent un rôle essentiel dans la préparation des entreprises</u> face à des situations imprévues, agissant en quelque sorte comme des mentors. 
Et puisqu'elles vendent littéralement les services de hackers, il incombe aux plateformes de veiller à ce que ces derniers soient 
<u>respectés et récompensés</u> pour leur travail.

&gt; [Liste de plateformes de programmes de bug bounty](https://github.com/disclose/bug-bounty-platforms)


## Le respect

Oui, c'est une règle bien réelle, et même l'une des plus importantes !

Cela peut paraître évident, voire naturel, mais je vois trop de signalements où les parties ne se comprennent pas, 
ce qui débouche rapidement sur des réactions discourtoises.
Si vous vous retrouvez dans une situation où vous ne comprenez pas la décision prise par l'autre partie, essayez de vous mettre 
à sa place pendant un instant ; cela pourrait changer votre point de vue.

En tant que hacker, vous devez comprendre que les programmes doivent gérer un volume considérable de remontées.
Ils sont contraints d'établir des priorités ; inutile donc de demander des nouvelles toutes les deux heures, attendez au moins une semaine.
Si la récompense n'est pas à la hauteur de vos attentes, relisez votre signalement : peut-être manque-t-il de clarté, 
ou peut-être n'avez-vous pas suffisamment démontré l'impact réel du problème.
D'après mon expérience, les programmes acceptent généralement de réexaminer la sévérité (et parfois le montant de la prime) si vous parvenez à 
avancer des arguments pertinents ; toutefois, **rester professionnel est une condition indispensable**.

<div style="float:left">
    <img src="/images/weekend-update.jpg" alt="weekend update on report" />
</div>
<div style="float:right">
    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">I have a new found respect for triage teams for bug bounties. Oh my gods the stuff they have to put up with.</p>&mdash; Alyssa Herrera🇩🇰 🏳️‍⚧️ (@Alyssa_Herrera_) <a href="https://twitter.com/Alyssa_Herrera_/status/1260656416118849536?ref_src=twsrc%5Etfw">May 13, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</div>
<div style="clear:both;"></div>

En tant que gestionnaire de programme, vous devez comprendre que les hackers consacrent beaucoup de temps à la recherche de failles ; 
c'est une activité très chronophage.
Il est donc toujours frustrant de voir un rapport rejeté au motif que le problème était connu depuis six mois, sans pour autant avoir été corrigé...
Si votre équipe de développement est en mesure d'escalader une faille d'un niveau moyenne à critique, 
faites preuve d'honnêteté et ajustez la récompense en conséquence ; les hackers n'ont pas accès à toutes les informations dont vous disposez.
Par ailleurs, vous devriez sérieusement envisager de **rémunérer dès l'étape du triage** : les remerciements _"kudos"_ ne payent pas 
les factures aussi bien que de l'argent.

Si aucune des parties ne parvient à gérer la situation de manière appropriée, inutile de recourir à des propos offensants ou à des sanctions.
N'oubliez pas que **le respect est une voie à double sens** : vous ne pouvez pas l'exiger si vous traitez les autres avec mépris. 
Gardez donc votre calme et ouvrez un ticket auprès du support de la plateforme.
C'est leur rôle de gérer ce type de situation, ils agissent en quelque sorte comme des arbitres, et ils sont (très bien) rémunérés pour cela (% des récompenses et les abonnements des entreprises). 
N'hésitez donc pas à faire appel à eux, quel que soit le camp auquel vous appartenez.


## La réputation ne fait pas tout

**Une réputation élevée n'est pas synonyme de bon hacker.**
L'objectif ici n'est pas de détailler tous les systèmes de réputation, mais simplement de mettre en évidence certains de leurs écueils.

Tout d'abord, il faut noter que rares sont les situations entraînant une perte de réputation, ce qui est en soi une bonne chose.
Cela implique que, si vous n'êtes pas un spammeur invétéré mais plutôt un hacker "normal" - autant qu'un hacker puisse l'être - votre réputation ne fera qu'augmenter. 
Avec le temps, vous finirez par atteindre ces fameux paliers de 5k, 10k, 15k...

Par ailleurs, si l'on examine la plateforme HackerOne, on constate qu'un rapport jugé critique rapporte 57 points, contre 17 points 
pour un rapport de gravité faible.
Sur YesWeHack, un rapport critique accepté rapporte 50 points, tandis qu'un rapport faible en rapporte 15.
En somme, sur ces deux plateformes, **3 rapports de criticité faible rivalisent avec un seul rapport critique** -> 3*CRLF = 1*RCE.

Sur certaines plateformes comme Yogosha, le gain de points est également calculé en fonction du montant de la prime plutôt qu'en fonction de la sévérité.
Or, une prime de 500€ peut être considérée comme critique sur un programme donné, mais comme faible sur un autre, n'est-ce pas ?

De plus, un rapport récompensé par 50€ rapporte autant de points qu'un rapport récompensé par 500€, et un rapport primé à 2000€ en 
rapporte autant qu'un autre primé à 10000€.
À cet égard, le système de HackerOne est mieux conçu, car la gravité utilisée dans le calcul est déterminée en fonction des primes, 
tout en restant relative à la configuration propre à chaque programme.

<div style="float:left">
    <a href="/images/hackerone_reputation.jpg"><img src="/images/hackerone_reputation.jpg" alt="hackerone reputation" height="450" /></a>
</div>
<div style="float:right">
    <a href="/images/yeswehack_report_workflow.jpg"><img src="/images/yeswehack_report_workflow.jpg" alt="yeswehack report workflow" height="450" /></a>
</div>
<div style="clear:both;"></div>

Enfin, il faut savoir que la réputation, c'est comme l'argent : plus on en a, plus on en reçoit.
Dès qu'un hacker atteint un rang élevé, il devient une valeur sûre pour les plateformes.
Il est promu, reçoit davantage d'invitations privées, d'accès à des ressources privées, d'entretiens et d'événements, ce qui se traduit par 
plus de bugs et une meilleure réputation, indépendamment de la pertinence de ses rapports.

Attention, je ne dis pas que tous les hackers les mieux classés sont des "farmeurs", loin de là, mais certains le sont.
Et alors ?
Ce n'est pas grave, ça fait partie du jeu. Ce n'est pas un problème à partir du moment ou tout le monde en est conscient.
Alors, analysez attentivement les chiffres et comprenez leur signification.

Heureusement, certaines plateformes utilisent d'autres indicateurs comme le **signal**, l'**impact** et le **taux d'acceptation**, 
qui sont de bons indicateurs de qualité.
En tant que responsable de programme, vous préférerez peut-être vous fier à ces indicateurs pour la sélection de vos hunters.

&gt; [Signal et impact sur Hackerone](https://docs.hackerone.com/hackers/signal-and-impact.html)  
&gt; [Comment nous mesurons la performance des chercheurs sur Bugcrowd](https://www.bugcrowd.com/blog/how-we-measure-researcher-performance/)


## Le CVSS n'est pas tout

Comme indiqué sur le [site officiel](https://www.first.org/cvss/calculator/3.1) :

> Le Common Vulnerability Scoring System (CVSS) est un cadre ouvert permettant de communiquer les caractéristiques et la gravité des vulnérabilités logicielles. CVSS se compose de trois groupes de métriques : Base, Temporelles et Environnementales. Le groupe Base représente les qualités intrinsèques d'une vulnérabilité, constantes dans le temps et quel que soit l'environnement utilisateur. Le groupe Temporelles reflète les caractéristiques d'une vulnérabilité qui évoluent avec le temps. Enfin, le groupe Environnemental représente les caractéristiques d'une vulnérabilité propres à l'environnement de l'utilisateur. Les métriques Base produisent un score compris entre 0 et 10, qui peut ensuite être ajusté en fonction des scores des métriques Temporelles et Environnementales.

Là encore, je ne vais pas expliquer en détail ce qu'est le CVSS et comment il fonctionne, mais seulement illustrer certains pièges qui le rendent inutile dans certaines situations. Car oui, je vous le dis haut et fort :
**le CVSS n'est pas la seule source de vérité.**

Le jeu des 7 différences :

<div style="float:left">
    <img src="/images/cvss-rce2.jpg" alt="cvss rce" width="420" />
</div>
<div style="float:right">
    <img src="/images/cvss-rce2.jpg" alt="cvss rce" width="420" />
</div>
<div style="clear:both;"></div>

Ne vous cassez pas la tête, il s'agit de la même image.
Imaginez un instant que la première capture d'écran concerne un serveur web ou un serveur de base de données en production, tandis que la seconde 
provient d'un serveur de test hébergeant uniquement le script malveillant, sans aucune donnée ni connexion réseau.
Le score CVSS est identique, mais l'impact serait-il le même ?
De ce fait, les primes peuvent varier considérablement.

Dans le panel des vulnérabilités critiques, on trouve des failles très connues comme [ImageTragick](https://www.cvedetails.com/cve/CVE-2016-3714/) 
et [Struts](https://www.cvedetails.com/cve/CVE-2017-5638/), toutes deux avec un score CVSS de 10.0 en raison de leur impact dévastateur.
Ces deux failles ont été largement signalées et exploitées avec succès sur de nombreux systèmes.  
D'un autre côté, on retrouve une autre faille célèbre appelée [Bluekeep](https://www.cvedetails.com/cve/CVE-2019-0708/), 
qui mérite elle aussi un bon 10.0 en raison de la gravité de son impact potentiel.
Elle a été signalée par de nombreux lanceurs d'alerte.
Le problème, c'est que lors de sa révélation publique, aucune exploitation à grande échelle n'a été signalée, pas même une exploitation publique !
Mérite-t-elle alors la même attention ?

On peut également évoquer les failles SSL/TLS que tous les bug hunters tentent de remonter au moins une fois : _CRIME, PODDLE, BREACH, DROWN_.
Très connues, le danger est bien réel, mais qui va exploiter de telles vulnérabilités ?
Elles sont désormais considérées comme hors de leur champ d'action par la plupart des programmes de sécurité.

Hunters : ne vous énervez pas si les primes ne reflètent pas la sévérité potentielle des bugs que vous signalez. 
**Les entreprises doivent prendre en compte le risque réel et l'impact commercial** en cas d'exploitation de leurs systèmes ou de leurs utilisateurs.

Attention : cela fonctionne dans les deux sens. 
Trois failles mineures, notées individuellement à 3.0, peuvent s'avérer catastrophiques lorsqu'elles sont combinées, atteignant un score de 10.0.
Par exemple : une faille CSRF lors de la connexion/déconnexion, redirection ouverte et XSS réfléchie.
Il est donc essentiel d'avoir une vision globale.

&gt; [Ne vous laissez pas tromper par les scores CVSS](https://securityboulevard.com/2020/03/dont-be-misled-by-cvss-scores/)  
&gt; [Pièges liés à l'évaluation de vulnérabilité](https://www.troopers.de/media/filer_public/cb/f0/cbf0181b-e9d8-408c-aef3-651bfd114d76/troopers13-pitfalls_of_vulnerability_ratinga_new_approach_called_errs-ernw_rapid_rating_system-michael_thumannmatthias_luft.pdf)


## Doublons

Les entreprises rencontrent souvent des difficultés pour gérer les rapports en double.
Voici mes recommandations pour y remédier.

**Cas 1 : Un seul chercheur, problème global.**  
S'il s'agit d'un problème global au sein de la cible, <u>un seul rapport doit être rémunéré</u>.
Le chercheur n'a pas besoin d'ouvrir plusieurs rapports, mais s'il le fait, il doit recevoir une récompense conséquente en 
expliquant clairement ses motivations.
Exemple : absence de protection CSRF, tous les formulaires du site sont vulnérables, il s'agit d'un problème global.

**Cas 2 : Un seul bug hunter, problème spécifique.**  
La règle : **1 correction → 1 prime** s'applique.
Exemple : 5 injections SQL découvertes dans 5 points de terminaison, les URLs sont quasiment identiques, tout comme le paramètre vulnérable. 
Si, en arrière-plan, la même fonction est appelée et qu'une correction dans cette fonction résout le problème, la règle *"même cause racine"* s'applique : 
le hacker est récompensé uniquement lorsque tous les autres rapports sont clos de manière à ce qu'il ne perde aucun point.
Cependant, si le code nécessite cinq corrections, les cinq rapports doivent être récompensés en fonction de leur sévérité respective.

**Cas 3 : plusieurs bug hunters, même problème**, la situation la plus courante.  
La règle classique **premier arrivé, premier servi** s'applique.
Toutefois, si un hunter fournit plus d'informations que ses prédécesseurs (rapport détaillé, informations additionnelles...), il mérite une prime.
Exemple : le chasseur 1 signale une vulnérabilité CSRF dans un formulaire, le chasseur 2 la révèle sur l'ensemble du site web alors 
<u>les deux méritent une récompense</u>.

Quelle que soit la situation, un chercher devrait toujours être invité à consulter le rapport initial, ou au moins obtenir l'identifiant, c'est le minimum.
Notez que chaque plateforme gère les doublons différemment.
D'un bonus lié au statut du signalement à une perte de réputation, tout est possible. 
Et, au final, les chercheurs n'ont aucun contrôle sur cette problèmatique, c'est plus un inconvénient qu'autre chose pour eux.

<img src="/images/duplicates.jpg" alt="duplicates" width="700" />

Parlons maintenant des raisons pour lesquelles les doublons sont si fréquents.  Il y en a essentiellement 3 raisons :

- **Règles inadéquates.**
L'entreprise a peut-être connaissance de certains problèmes suite à un test d'intrusion.
Certains bugs ne sont pas considérés comme de véritables bugs : problèmes connus, non corrigés, "on verra bien", "ce n'est pas si grave", 
"on verra plus tard"...
Les hackers sont certes des magiciens, mais ils ne lisent pas dans les pensées et ils n'ont pas accès à vos rapports d'audits.

- **Surcharge.** Dès le lancement d'un programme, tous les hackers de la communauté se ruent sur la cible, ou seulement les invités, ou encore leurs bots.
Quoi qu'il en soit, les signalements affluent dans les jours qui suivent.
Une entreprise mal préparée ne pourra pas tous les traiter dans un délai raisonnable.

- **Bugs non corrigés.** La honte absolue.
À quoi sert alors un programme de bug bounty ?
C'est un manque de respect envers les hackers qui consacrent leur temps à trouver des failles dans un système. 
Hunters : évitez ces programmes autant que possible.


## Safe Harbor

Selon le secteur d'activité (banque, assurance, gouvernement...), il peut être très difficile pour une entreprise de mettre en place un programme de bug bounty en raison des réglementations applicables : traçabilité des utilisateurs, conservation des fichiers de logs pendant des mois/années, restrictions géographiques, etc.
C'est pourquoi les plateformes déploient des efforts considérables pour fournir les services nécessaires : VPN, logs, sauvegardes, sélection des hackers (qui ne repose pas uniquement sur la réputation).

L'objectif est bien sûr de respecter la loi, mais c'est aussi un moyen pour les entreprises de suivre les tests réalisés et par qui.
Imaginez qu'un hacker signale une injection SQL et que les développeurs constatent la suppression de certains enregistrements.
Imaginez également qu'une plateforme client se bloque quelques minutes après son lancement : le client voudra sans doute savoir ce qui s'est passé et qui est responsable.

Les entreprises disposent donc de moyens et d'outils pour se protéger légalement en cas de problème.
Mais qu'en est-il des hackers ?
N'oublions pas que les hackers sont les piliers de ce secteur.
Sans hackers, pas de bug bounty.
Alors, comment les protéger un minimum ?
Alors que le domaine du bug bounty connait une croissance fulgurante à travers le monde, **entreprises et hackers doivent trouver un terrain d'entente sur les conditions juridiques**.
C'est précisément l'objectif du Safe Harbor.

> Afin d'encourager la recherche et la divulgation responsable des failles de sécurité, nous n'engagerons aucune action civile ou pénale et n'enverrons aucune notification aux forces de l'ordre en cas de violation accidentelle ou de bonne foi des conditions générales du programme de primes aux bogues de Microsoft.

En tant que hacker éthique, nous avons tous déjà vécu cette situation stressante : la réaction d'une entreprise contactée suite à 
la découverte d'un bug sur son site.
Des remerciements à l'indifférence, en passant par des poursuites judiciaires, tout est possible, on ne sait jamais...
Il y a quelques mois, [Chloé Messdaghi](https://twitter.com/ChloeMessdaghi) a lancé une pétition pour soutenir les hackers éthiques.

&gt; https://www.change.org/p/organizations-support-ethical-hackers

Le problème, c'est que même dans le cadre d'un programme de bug bounty, les hunters ne sont pas toujours aussi bien protégés qu'ils le devraient.

Fin 2017, [Kevin Finisterre](https://twitter.com/d0tslash) a vécu une situation compliquée : il a été injustement menacé par DJI après avoir envoyé un rapport de plus de 30 pages. Son histoire a été mentionnée dans une présentation d'[Amit Elazari](https://twitter.com/amitelazari) à USENIX Enigma 2018.

&gt; [Pourquoi j'ai renoncé à une prine de 30k$](https://regmedia.co.uk/2017/11/16/whyiwalkedfrom3k.pdf)  
&gt; [Piratage juridique : le bug bounty est-il un véritable refuge ?](https://www.youtube.com/watch?v=riZIFOw0pJA)

Le Safe Harbor est d'une aide précieuse dès son intégration aux politiques de sécurité.
Il devrait être obligatoire dans tous les programmes de bug bounty, sur toutes les plateformes.
Pour l'instant, il reste assez rare.
HackerOne, Bugcrowd et Intigriti en sont de bons exemples sur ce sujet.

<img src="/images/intigriti-safe-harbour.png" alt="intigriti safe harbour" />

&gt; [Hackerone: offrir un refuge sûr aux hackers éthiques](https://www.digitalnewsasia.com/digital-economy/hackerone-offer-white-hats-safe-harbour)  
&gt; [Clause légale du programme de GitHub](https://docs.github.com/en/site-policy/security-policies/github-bug-bounty-program-legal-safe-harbor)  
&gt; [Safe Harbor pour les participants au programme de Mozilla](https://blog.mozilla.org/security/2018/08/01/safe-harbor-for-security-bug-bounty-participants/)


## Paiement au triage

Le paiement au moment du triage devrait être la norme.

À moins que l'équipe de sécurité ait besoin de plus d'informations pour reproduire le bug, le travail du hunter est terminé à partir du moment ou le rapport est 
accepté <u>il devrait donc être récompensé immédiatement</u>.

Imaginez un instant les chercheurs qui font du bug bounty leur activité principale, et notamment les débutants.
Les hackers ne sont pas comme les grandes entreprises aux revenus colossaux.
Ce sont des personnes comme les autres, avec des devoirs et des responsabilités qui leur permettent de gagner leur vie **#payetesfactures**.


## Politique de divulgation responsable

La divulgation responsable est une stratégie marketing efficace car elle témoigne de l'engagement des entreprises en matière de sécurité.
Elle contribue également à promouvoir les programmes auprès de la communauté.
Les rapports étant une ressource précieuse pour les hackers souhaitant progresser, leur utilisation peut être considérée comme un investissement judicieux.

Informations partageables :
- nombre de rapports
- pourcentage de rapports acceptés
- montant total des récompenses
- prime moyenne
- types de vulnérabilités
- rapports complets
- ...

La divulgation reponsable peut être réservée à la communauté ou publique.

