---
title: "Audits et tests d'intrusion"
slug: "prestations"
date: 2026-10-29
lastmod: 2026-10-29
description: "Trois prestations de sécurité : test d'intrusion d'application web, audit de code source, audit de configuration Linux. Laquelle vous correspond, et pourquoi."
services_hub: true
---

Trois prestations, trois questions différentes. La plupart des gens qui me contactent savent qu'ils ont besoin de  
_"quelque chose en sécurité"_ sans savoir laquelle des trois répond à leur problème. Cette page est là pour trancher en cinq minutes.

| Prestation | La question à laquelle elle répond | Durée |
|---|---|---|
| [Test d'intrusion](/fr/test-intrusion-application-web/) | Par où entre-t-on ? | 3 à 10 jours |
| [Audit de code source](/fr/audit-de-code-source/) | Pourquoi cette porte existe-t-elle ? | 5 à 15 jours |
| [Audit de configuration](/fr/audit-configuration-durcissement-linux/) | Jusqu'où va-t-on une fois entré ? | 2 à 3 jours par serveur |

## Test d'intrusion d'application web

J'attaque votre application comme le ferait quelqu'un de mal intentionné, dans un
périmètre et un calendrier convenus, et je vous remets la liste de ce qui est
réellement exploitable — pas la liste de ce qui pourrait théoriquement l'être.

Pour vous si : votre application est en production et personne ne l'a jamais
testée sérieusement, ou un client vous demande une preuve.

**[Voir le détail →](/fr/test-intrusion-application-web/)**

## Audit de code source

Je lis votre code, vos dépendances et votre historique Git pour trouver ce qui
est fragile à l'intérieur, y compris sur les chemins qu'un test d'intrusion
n'emprunte jamais, et les secrets que vous croyez avoir supprimés.

Pour vous si : vous reprenez une base de code, vous préparez une due diligence,
ou vous voulez savoir si une faille trouvée ailleurs existe aussi dans les douze
autres endroits où le même oubli a pu se produire.

**[Voir le détail →](/fr/audit-de-code-source/)**

## Audit de configuration et durcissement Linux

Je mesure l'écart entre vos serveurs et un référentiel nommé: le guide
ANSSI-BP-028, au niveau que nous choisissons ensemble — et je vous livre les
correctifs sous forme de tâches Ansible rejouables.

Pour vous si : vos serveurs n'ont jamais été durcis, vous sortez d'un incident et
voulez vérifier que le confinement est réel, ou on vous demande une conformité
aux recommandations de l'ANSSI.

**[Voir le détail →](/fr/audit-configuration-durcissement-linux/)**

## Laquelle vous faut-il

Dans l'ordre de ce que j'entends le plus souvent :

**« Notre appli est en ligne, on n'a jamais rien fait. »** → Test d'intrusion.  
C'est le point de départ par défaut : il vous dit où vous en êtes avant d'investir ailleurs.

**« Un client nous demande un rapport de sécurité. »** → Test d'intrusion.  
C'est le livrable qui se présente le mieux à un tiers, parce qu'il décrit des scénarios d'attaque concrets plutôt qu'une liste de réglages.

**« On hérite d'un code dont les auteurs sont partis. »** → Audit de code.  
Un test d'intrusion ne vous dira rien de la dette qui dort dans les branches que personne n'exécute encore.

**« On rachète une société / on lève des fonds. »** → Audit de code.  
Pour le risque technique et l'exposition aux licences.

**« On s'est fait attaquer le mois dernier. »** → Audit de configuration d'abord, pour vérifier que l'attaquant n'a plus de chemin,
puis test d'intrusion pour confirmer que la porte d'entrée est fermée.

**« Le pentest a trouvé une injection SQL et ça nous inquiète. »** → Audit de code.  
Une faille trouvée est un bug ; la même faille à quatorze endroits est un problème de conception, et c'est une autre conversation.

**« On nous demande d'être conformes à l'ANSSI. »** → Audit de configuration.  
Avec le niveau du guide fixé au cadrage.

**« On retrouve les mêmes failles à chaque audit. »** → Aucune des trois.  
C'est une [formation](/fr/formations/) qu'il vous faut : payer pour faire constater les mêmes catégories d'erreurs 
tous les ans coûte plus cher que d'apprendre à ne plus les écrire.

**« On ne sait pas, personne ne nous demande rien. »** → Alors peut-être rien, pour l'instant.  
Écrivez-moi quand même en deux lignes, je vous le dirai franchement, 
une prestation vendue à quelqu'un qui n'en avait pas besoin ne me rapporte qu'une fois.

## Les combinaisons qui ont du sens

Le test d'intrusion et l'audit de configuration se complètent bien : le premier
trouve l'entrée, le second détermine jusqu'où elle mène. C'est l'enchaînement que
je recommande le plus souvent, et le second coûte beaucoup moins cher que le
premier.

L'audit de code se justifie surtout **après** un test d'intrusion, quand vous
voulez comprendre la cause plutôt que le symptôme — ou **avant** une mise en
production majeure, quand il n'y a encore rien à attaquer.

Je ne vends pas de « pack des trois ». Si vous les enchaînez, c'est parce que le
résultat du précédent le justifie, pas parce que c'était dans le devis.

## Ce que je ne fais pas

Autant le dire avant que vous écriviez.

Je travaille seul. Pour un périmètre très large, une mission en équipe sur
plusieurs semaines, ou une prestation exigeant une qualification formelle — un
prestataire PASSI, un test TLPT au titre de DORA — je vous le dis au premier
échange et je vous oriente ailleurs. Mes rapports sont des rapports d'audit, pas
des certifications.

Côté systèmes, je couvre Linux et les conteneurs. Pas Windows, pas Active
Directory.

Et je ne gonfle pas les criticités. Une page d'administration accessible
publiquement est une information, pas une faille critique. Au troisième rapport
où tout est « élevé », plus personne ne lit, et c'est la vraie faille qui se noie.

## Parlons-en

Décrivez-moi votre situation en quelques lignes, ce que fait l'application ou le
système, qui l'utilise, ce qui déclenche la question, et je vous réponds avec la
prestation qui correspond, un périmètre proposé et une fourchette de prix. Si la
réponse est _"aucune des trois"_, je vous le dirai aussi.

<br><br>

---
Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - devis sur demande - SIRET 79778302400038
