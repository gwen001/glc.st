---
date: 2020-04-04
slug: "bugbounty-comment-garder-les-hackers-motives"
title: "Bug bounty : comment garder les hackers motivés"
related:
  - /courses/bugbounty
  - /posts/interview-bugbounty-program
  - /posts/bugbounty-management-bad-example
  - /posts/bugbounty-management-todo-before-launch
  - /posts/bugbounty-management-what-to-avoid
  - /posts/bugbounty-management-zomato
  - /posts/bugbounty-unonfficial-rules
  - /posts/platform-tournament
tags:
  - bug bounty
  - management
  - hackers
---
Aujourd'hui, il existe des milliers de programmes répartis sur diverses plateformes. comment attirer les hackers vers le vôtre ? 
Pourquoi choisirais-je votre programme plutôt qu'un autre ?
Pourquoi consacrerais-je du temps à tester votre système ?
Considérez les hackers comme des clients ou des investisseurs : vous devez les convaincre de collaborer avec vous et promouvoir 
votre programme comme vous le feriez pour vos produits.
<!--more-->

Une différence majeure par rapport aux tests d'intrusion réside dans le fait qu'un programme de bug bounty s'inscrit dans la durée. 
Dans cet article, je vais vous donner quelques conseils pour séduire les bug hunters et les fidéliser.


## Des récompenses attractives

Évidemment.

Soyons honnêtes : la plupart des hackers dans le secteur du bug bounty travaillent pour l'argent. 
Certes, ce n'est pas leur unique source de motivation, comme vous le découvrirez plus tard, mais cela reste un facteur déterminant.
**La récompense principale est importante, mais elle ne fait pas tout.**

N'hésitez pas à accorder un petit bonus ponctuel : lorsqu'un rapport est particulièrement bien détaillé, lorsqu'un hacker fait preuve 
d'une grande créativité en découvrant une faille totalement inattendue, ou encore lorsque la vulnérabilité signalée est non seulement critique, 
mais tout bonnement stupéfiante.
En cas de doute, demandez-vous quelles seraient les pertes si un utilisateur malveillant exploitait cette faille ; 
cela vous aidera probablement à prendre la bonne décision.

Certaines entreprises organisent également des opérations promotionnelles, augmentant temporairement le montant des primes.
Si vous optez pour cette stratégie, veillez à en informer votre communauté, sinon la démarche perd tout son sens.

Cependant, <u>l'argent n'est pas le seul moyen de récompenser les chasseurs de failles</u>.
T-shirts, casquettes, invitations à des conférences, autocollants... 
nous apprécions tous ces petits objets dérivés qui apportent un peu de couleur à notre quotidien :)

Enfin, la meilleure récompense, et la plus simple, consiste à témoigner aux hackers le respect que vous leur portez.
N'hésitez pas à employer des mots bienveillants, comme vous le feriez dans la vie réelle face à une réalisation impressionnante ; 
sachez exprimer votre reconnaissance.
Valorisez leurs belles découvertes !


## Des bonnes SLA

&gt; Si vous n'êtes pas familier avec les SLA, consultez cet [article sur HackerOne](https://www.hackerone.com/blog/Healthy-programs-make-happy-hackers-Introducing-response-SLAs).

Comme je l'ai déjà mentionné dans l'article précédent [5 choses à éviter dans les programmes de bug bounty](/posts/5-things-to-avoid-in-bug-bounty/), 
une bonne communication est indispensable, tout comme la mise en place d'accords de niveau de service (SLA) adaptés.
On peut essentiellement retenir <u>quatre indicateurs, tous d'une importance capitale</u> pour les différentes parties prenantes :

- délai avant la première réponse : informer le hacker que le rapport a bien été reçu
- délai avant le premier triage : indiquer au hacker si le rapport est jugé valide ou non. Si c'est le cas, il est alors intégré à votre flux de travail interne
- délai avant le versement de la première prime : faire savoir au hacker que son travail est apprécié
- délai avant la première correction : le moment où la vulnérabilité est corrigée

Il va sans dire que **plus ces délais sont courts, mieux c'est pour tout le monde.**
Les hackers ne travailleront pas pour une entreprise qui met six mois à trier un rapport ; nous souhaitons être rémunérés le plus rapidement possible.
<u>Le versement de la prime dès le triage devrait être la norme.</u>
Par ailleurs, vous ne souhaitez pas recevoir sans cesse des signalements pour une même vulnérabilité, les correctifs doivent être déployés rapidement.
Notez qu'à chaque étape, de la première réponse jusqu'à la correction finale, le hacker est en droit d'être tenu informé de l'avancement du processus.

<img src="/images/hackerone_sla.png" />


## Évolution du programme

Optimisez vos processus, renforcez votre sécurité pour éviter les pièges classiques, développez votre communauté de 
chercheurs et affinez votre politique de récompenses...
Faites évoluer votre programme, tout en le gardant stimulant !

**Élargissez le périmètre.**
Ajoutez régulièrement de nouveaux assets pour étendre progressivement la portée de votre programme.
Vous pouvez également en intégrer certains pour une durée limitée, cela permet de maintenir l'intérêt et le côté ludique :)

**Créez un programme privé.**
Certaines entreprises choisissent de mettre en place un programme secondaire, confidentiel, pour tester de nouvelles fonctionnalités avant leur mise en production.
Vous pouvez y inviter certains des membres les plus appréciés de votre communauté.
Pour un chercheur en sécurité, bénéficier d'un tel privilège est particulièrement gratifiant.

**Augmentez les récompenses.**
À mesure que la sécurité de votre système s'améliore, la découverte de vulnérabilités devient plus complexe.
Il est donc logique d'augmenter les montants des récompenses pour maintenir l'engagement des chercheurs.
Pensez également aux opérations promotionnelles, elles sont très appréciées.

**Publiez les rapports.**
Comme je l'expliquerai dans le prochain chapitre, la divulgation responsable est essentielle pour la communauté.
Partagez régulièrement des rapports, les chercheurs en sécurité apprécieront de pouvoir les consulter.
Vous pourriez, par exemple, publier chaque mois les rapports concernant des vulnérabilités corrigées depuis au moins six mois.


## Divulgation responsable

**La publication de rapports constitue l'une des meilleures sources d'apprentissage.**
Les partager s'apparente à un investissement pour l'avenir, car cela permet aux hackers d'enrichir leurs connaissances.
Et si les hackers gagnent en compétence, ils seront en mesure de découvrir davantage de vulnérabilités.
C'est simple et logique.

C'est également une excellente initiative en matière de communication : vous montrez à la communauté que vous vous souciez de la sécurité.
Il est important pour nous de constater que les entreprises s'impliquent réellement dans le secteur de la sécurité.

Enfin, et ce n'est pas le moins important, c'est toujours une grande satisfaction pour un hacker de voir l'un de ses rapports rendu public.
Nous en tirons de la fierté, et cela contribue à nous faire reconnaître par nos pairs.


## Impliquez-vous

Comme indiqué dans le paragraphe précédent, être proche de la communauté constitue un réel avantage,
mais la divulgation responsable n'est pas le meilleur moyen d'y parvenir.
Jetez un oeil à l'équipe de sécurité de Verizon Media :

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">We are back in our &quot;home office hours&quot; this morning to take questions from hackers and <a href="https://twitter.com/Hacker0x01?ref_src=twsrc%5Etfw">@hacker0x01</a>, and are also working on the next batch of event bounties! See the coins fly at <a href="https://t.co/gZGqtk8yPI">https://t.co/gZGqtk8yPI</a> <a href="https://twitter.com/hashtag/h12004?src=hash&amp;ref_src=twsrc%5Etfw">#h12004</a></p>&mdash; The Paranoids (@TheParanoids) <a href="https://twitter.com/TheParanoids/status/1245709577699479553?ref_src=twsrc%5Etfw">April 2, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

C'est l'exemple parfait de la manière d'entretenir une bonne relation avec la communauté.
Ils utilisent Twitter comme canal de communication avec les bug hunters, sont présents lors de nombreux événements dédiés à la sécurité, 
et parrainent des initiatives de hackers dans le domaine de la cybersécurité.
Ils sont réellement **impliqués dans notre univers**, et mieux encore, **ils en font partie**. 
C'est pourquoi la communauté leur en est reconnaissante et reste fidèle à leur programme de bug bounty.

C'est une excellente source d'inspiration pour toute équipe de sécurité.
