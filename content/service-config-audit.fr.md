---
title: "Audit de configuration et durcissement Linux"
slug: "audit-configuration-durcissement-linux"
date: 2026-10-13
description: "Audit de configuration et durcissement Linux selon ANSSI-BP-028 et les CIS Benchmarks : exposition, comptes, services, journalisation, trouvailles priorisées."
keywords: "audit de configuration, audit durcissement Linux, ANSSI BP-028, durcissement serveur, expert cybersécurité indépendant"
service: true
---

Un [test d'intrusion](/fr/test-intrusion-application-web/) trouve la porte d'entrée.
Un [audit de code](/fr/audit-de-code-source/) trouve pourquoi cette porte existe. Un audit de configuration répond à la question qui suit les deux :
 **une fois quelqu'un sur la machine, jusqu'où va-t-il ?**

C'est une question différente, et la plupart des organisations ne se la sont jamais posée.
Elles ont un serveur qui fonctionne, ce qui n'est pas la même chose qu'un serveur conçu pour contenir une compromission.

## Pourquoi maintenant

**L'exploitation de systèmes exposés est le point de départ des compromissions.**
Le [DBIR 2026](https://www.verizon.com/business/resources/reports/dbir/) de Verizon, établi sur plus de 22 000 compromissions confirmées dans 145 pays,
situe l'exploitation de vulnérabilités à **31 %** des accès initiaux - désormais devant le phishing (16 %) et l'abus d'identifiants (13 %).
En Amérique du Nord, le chiffre atteint 44 %. Et l'écart se creuse : **seules 26 % des vulnérabilités critiques ont été entièrement corrigées** en 2025,
contre 38 % l'année précédente, tandis que le délai médian de résolution complète passait de 32 à **43 jours**.

**La mauvaise configuration a gagné des places.**
Dans l'[OWASP Top 10:2025](https://top10.owasp.org/2025/), Security Misconfiguration est désormais en **A02**, deuxième au classement général.
Pas une classe de faille exotique - le réglage par défaut que personne n'a changé.

**Le tableau français est spécifique et peu flatteur.**
Le [Panorama de la cybermenace 2025 de l'ANSSI](https://www.cert.ssi.gouv.fr/uploads/CERTFR-2026-CTI-002.pdf) recense **1 366 incidents confirmés**, stable par rapport à 2024.
Il décrit des équipements de bordure compromis à répétition par leurs propres vulnérabilités, et des interfaces de contrôle industriel joignables depuis
Internet **sans aucune authentification, ou avec les mots de passe par défaut toujours en place**.
Les incidents d'exfiltration de données passent de 130 à **196** en un an.

{{< figure alt="Panorama de la cybermenace 2025 de l'ANSSI" src="/assets/img/services/panorama-cybermenace-2025.png" width="500" >}}

Mis bout à bout, la forme du problème est claire.
Le point d'entrée est généralement un service exposé qui n'a pas été corrigé, ou qui n'était pas censé être exposé.
Ce qui détermine si ça devient un incident ou une catastrophe, c'est tout ce qui se trouve derrière : séparation des privilèges, isolation des services, journalisation, segmentation.
C'est ce que mesure un audit de configuration.

## Quand en lancer un

- **Avant la mise en production d'un serveur** : le moment le moins cher possible, et le plus rare en pratique.
- **Après un test d'intrusion ou un audit de code** : vous savez comment on entre ; il s'agit maintenant de savoir ce qu'on atteint ensuite. C'est la mission qui enchaîne naturellement.
- **Après un incident** : pour vérifier que le confinement était réel et que le chemin de l'attaquant est bien fermé, pas seulement le point d'entrée.
- **En reprenant une infrastructure** : l'administrateur qui l'a construite est parti, il n'y a pas de documentation, et personne n'est sûr de ce qui tourne ni pourquoi.
- **Pendant ou après une migration** : nouvelle version majeure de distribution, passage au cloud, conteneurisation. Le durcissement survit rarement intact à une migration.
- **Pour une exigence externe** : questionnaires sécurité de grands comptes, assureurs, marchés publics, ou obligations NIS2 et DORA pour les entités concernées.
L'[article 32 du RGPD](https://eur-lex.europa.eu/legal-content/FR/TXT/?uri=CELEX%3A32016R0679) impose déjà de tester régulièrement l'efficacité des mesures techniques.
- **En cycle, pour la dérive** : c'est celle qu'on sous-estime. Un serveur durci il y a dix-huit mois n'est plus un serveur durci.
Des paquets installés pour une séance de débogage y restent, des règles de pare-feu ouvertes pour une migration ne sont jamais refermées, la clé SSH d'un prestataire est toujours autorisée.
La dérive de configuration est silencieuse et cumulative.

## Référentiels

L'audit est conduit face à un référentiel nommé, pas face à mon goût personnel.

L'[ANSSI-BP-028 v2.0](https://cyber.gouv.fr/sites/default/files/document/linux_configuration-en-v2.pdf),
les recommandations de configuration d'un système GNU/Linux de l'agence nationale,
est la référence principale, et celle que j'ai appliquée pendant quatre ans en auditant des systèmes pour le ministère des Armées.
Il définit quatre niveaux de durcissement (minimal, intermédiaire, renforcé, élevé), et **le choix du niveau cible fait partie de la conversation de cadrage**,
pas d'un défaut que j'impose. Auditer un serveur vitrine public selon le profil « élevé » produit cent recommandations sur lesquelles personne n'agira jamais.

À côté : les [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks) pour les contrôles propres à une distribution ou à un service,
et **OpenSCAP** avec le [SCAP Security Guide](https://www.open-scap.org/security-policies/scap-security-guide/),
qui livre des profils `anssi_bp28_*` prêts à l'emploi pour les principales distributions - pratique pour produire une base de référence reproductible et
formellement notée, que vous pourrez rejouer vous-même après mon départ.

## Ce que j'audite

**Socle système** : partitionnement et options de montage, inventaire des paquets installés face à ce qui est réellement nécessaire, services activés,
modules noyau chargeables, paramètres du noyau, et état et politique du contrôle d'accès obligatoire (SELinux ou AppArmor).

**Comptes et privilèges** : la politique sudo ligne par ligne, et c'est là que je trouve le plus souvent les problèmes les plus graves : une seule règle `NOPASSWD`
sur un binaire qui permet une évasion de shell annule tout le reste de cette liste. Également la politique de mots de passe et d'authentification, la pile PAM,
la configuration SSH et l'inventaire des clés autorisées, les comptes de service dotés d'un shell dont ils n'ont pas besoin.

**Exposition réseau** : ce qui écoute réellement, sur quelles interfaces, face à ce que vous croyez qui écoute. Règles de pare-feu et leur ordre,
chemins d'accès distant, atténuation du force brute, et exposition des interfaces d'administration et des bases de données.

**Durcissement des services** : utilisateurs dédiés non privilégiés, directives de sandboxing systemd, configuration du serveur web et du reverse proxy,
configuration TLS et gestion des certificats, écouteurs et authentification des bases de données.

**Système de fichiers et intégrité** : permissions sur les chemins sensibles, inventaire SUID/SGID avec un jugement sur chaque entrée, ACLs et sticky bits,
attributs d'immuabilité, et surveillance de l'intégrité des fichiers.

**Journalisation, audit et détection** : si quelque chose est effectivement enregistré, si c'est centralisé là où un attaquant ne peut pas l'effacer,
règles d'audit au niveau du noyau, rétention, et si quiconque s'en apercevrait. Un journal que personne ne lit est un artefact de conformité, pas un contrôle.

**Secrets et identifiants sur disque** : fichiers d'environnement, fichiers de configuration, scripts de déploiement, tâches cron, historique de shell, archives de sauvegarde.
Ça recoupe mon travail d'[audit de code](/fr/audit-de-code-source/), et c'est systématiquement productif.

**Conteneurs et orchestration** : provenance des images et fraîcheur des images de base, conteneurs privilégiés,
sockets montées, capabilities, limites de ressources, accès au registre.

**Automatisation et sauvegardes** : runners CI/CD avec leurs identifiants et leur portée réseau, intégrité et restauration des sauvegardes,
et si vos sauvegardes sont joignables depuis la machine qu'elles protègent - le détail qui transforme un incident de rançongiciel en perte totale.

<br>

> Systèmes Linux - Debian et Ubuntu, RHEL et dérivés - plus Docker et la
configuration de conteneurs, et l'infrastructure as code (Ansible). **Pas Windows, pas
Active Directory.** Si votre parc est centré sur Microsoft, je vous le dis dès le
premier échange et je vous oriente vers quelqu'un de mieux placé.

## Comment ça se déroule

**1. Cadrage** : inventaire des systèmes concernés, leurs rôles, le niveau ANSSI cible, la méthode d'accès,
une fenêtre de maintenance si une vérification intrusive est nécessaire, et la convention d'audit signée.

**2. Collecte** : récupération de données en lecture seule, par SSH avec un compte non privilégié et une autorisation sudo précisément délimitée,
ou depuis un export de configuration si vous préférez. Rien n'est modifié. Je vous dis exactement quelles commandes s'exécutent avant que quoi que ce soit s'exécute.

**3. Base de référence automatisée** : Lynis, OpenSCAP et des outils maison face au profil convenu, pour obtenir une couverture reproductible et
un score que vous pourrez suivre dans le temps. La sortie part en triage, jamais directement dans le rapport.

**4. Revue manuelle** : la partie qui compte. Les outils automatisés vérifient qu'un réglage correspond à une valeur ; ils ne peuvent pas vous dire
que cette règle sudo précise, sur cette machine précise, compte tenu de ce que fait ce service, est un chemin vers root. Ce jugement est le livrable.

**5. Rapport et restitution** : livrable écrit.

{{< figure alt="Rapport Lynis" src="/assets/img/services/lynis.png" width="500" >}}

## Le rapport

- **Une synthèse d'une page** pour la direction : posture actuelle face au niveau cible, la poignée de choses qui comptent vraiment,
et un énoncé honnête de ce qui a été couvert et de ce qui ne l'a pas été.
- **Les trouvailles par système**, chacune avec le réglage ou la règle exacte en cause, sa localisation, le contrôle de référence (identifiant de règle ANSSI-BP-028, contrôle CIS),
une criticité contextualisée, le chemin d'exploitation concret là où il en existe un, et la modification exacte à apporter.
- **Une remédiation ordonnée par impact rapporté au risque de casse** : parce que c'est le piège propre au travail de durcissement.
Une recommandation qui casse l'application est annulée le lundi et le serveur finit moins sûr qu'avant mon passage.
Chaque point indique ce qui pourrait casser.
- **Une base de référence rejouable** : le profil OpenSCAP et la méthode de notation, pour que vous puissiez mesurer votre propre dérive à tout moment sans m'appeler.

<br>

> Une page d'administration accessible publiquement est une information, pas une vulnérabilité critique.
Gonfler les scores remplit le rapport et vide sa valeur : au troisième audit où tout est « élevé »,
plus personne ne le lit, et la seule trouvaille réellement critique se noie dans le bruit.
Acceptez les rapports courts quand le système est bien construit.

## Ce que ce n'est pas

- **Ce n'est pas une certification de conformité.** Je produis un rapport d'audit face à un référentiel publié.
Je ne suis pas un organisme de certification accrédité et ce n'est pas une prestation qualifiée PASSI.
Si votre exigence impose explicitement un prestataire qualifié, il vous en faut un, et je vous le dis plutôt que de vous laisser le découvrir en réunion de restitution.
- **Ce n'est pas un rapport de scanner avec une page de garde.** Les outils automatisés sont des entrées. Vous pouvez les exécuter vous-même gratuitement, et vous devriez.
Ce que vous achetez, c'est le triage et le jugement d'exploitabilité.
- **Ce n'est pas du durcissement maximal.** Le niveau cible est une décision fondée sur ce que fait le système et sur ce qu'il vaut.
Un durcissement qui casse la production n'est pas du durcissement.
- **Ce n'est pas un test d'intrusion.** Cet audit est conduit avec un accès légitime et une visibilité complète.
Il vous dit jusqu'où irait une compromission, pas si quelqu'un peut entrer - ça, c'est le
[pentest](/fr/test-intrusion-application-web/).
- **Ça ne reste pas vrai.** Rappel du point sur la dérive plus haut : c'est une photographie.

## Pourquoi moi

- **Auditeur pour le ministère des Armées, de 2020 à 2024** : audits de configuration de systèmes Linux selon les recommandations de l'ANSSI,
en parallèle de tests d'intrusion et d'audits de code. Quatre ans à appliquer ce référentiel précis à des systèmes où les conséquences d'une erreur
n'étaient pas théoriques.
- **Administrateur système avant d'être auditeur** : j'ai administré des serveurs Linux pendant des années avant de commencer à les casser.
Je sais ce que coûte une recommandation à appliquer à 3 h du matin sur un système avec des utilisateurs dessus.
- **Chasseur de vulnérabilités depuis 2016** : plusieurs centaines de rapports à des programmes chez EDF, Decathlon, Payfit, BlaBlaCar, Swisscom, Uber et d'autres,
dont des exécutions de commandes à distance et des prises de contrôle de comptes cloud obtenues depuis des services exposés et des identifiants fuités. Des [CVEs](/cves/) publiées sous mon nom.
- **Formateur depuis 2022** : j'enseigne le [durcissement Linux](/fr/formations/) et le [scripting Bash](/fr/formations/),
ce qui veut dire que votre équipe reçoit des explications plutôt qu'une liste de réglages à recopier.

## Questions fréquentes

**Combien de temps ?**
Deux à trois jours pour un serveur, rapport compris. Pour un parc, ça dépend de son homogénéité :
dix serveurs web identiques coûtent à peine plus qu'un seul, dix rôles différents coûtent près de dix fois plus.
Je vous dis dans quel cas vous êtes après un coup d'œil à l'inventaire.

**Combien ça coûte ?**
Devis sur demande, au forfait. Les facteurs : nombre de rôles systèmes distincts, niveau ANSSI cible, inclusion ou non d'une contre-vérification.

**Quel accès vous faut-il ?**
SSH avec un compte non privilégié et une autorisation sudo délimitée, pour une collecte en lecture seule.
Aucune modification de configuration, aucun logiciel installé sans votre accord explicite.
Je vous envoie la liste exacte des commandes à l'avance, pour relecture.

**Est-ce que ça peut perturber la production ?**
La collecte est en lecture seule et sans risque. La seule étape qui en comporte est la démonstration d'une trouvaille exploitable,
qui n'a lieu qu'avec votre accord écrit, dans une fenêtre que vous choisissez, et sur un clone dès qu'il en existe un.

**On fait déjà tourner Lynis et notre score est bon. Qu'apportez-vous ?**
Le jugement, et ce qu'un score ne peut pas voir. Un indice de durcissement mesure la conformité à une liste de contrôles ; il ne sait pas que votre utilisateur de déploiement
peut écrire dans un répertoire qu'une tâche cron root exécute. La plupart des trouvailles sérieuses que je remonte sont des compositions de réglages individuellement acceptables.

**Pouvez-vous appliquer les correctifs au lieu de seulement les signaler ?**
Je livre les trouvailles prioritaires et je peux accompagner votre équipe dans leur application. Je ne reprends pas la
remédiation de systèmes de production que je n'exploite pas, ça demande quelqu'un qui sera encore là le mois prochain.

**Et si tout est déjà en bon état ?**
Vous obtenez un rapport court qui le dit, avec le détail qui le rend défendable devant un client ou un auditeur,
plus la base de référence pour suivre la dérive. Ça arrive, et c'est un bon résultat.

## Parlons-en

Dites-moi combien de systèmes, ce qu'ils font tourner, quelles distributions, et ce qui déclenche la question.
Je vous réponds avec un périmètre proposé, un niveau cible et une fourchette de prix.

<br><br>

---
Gwendal Le Coguic - <a href="mailto:contact@glc.st" target="_blank">contact@glc.st</a> - devis sur demande - SIRET 79778302400038

<style>
    .content figure {
        border: 0px solid #000;
        margin-bottom: 20px;
        margin-top: 20px;
        padding: 0px;
        text-align: center;
    }

    blockquote {
        font-size: 0.9em;
        text-align: justify;
        background-color: #fef6e1;
        border: 1px solid #f9b915;
        font-style: normal;
        padding: 2rem;
    }
    blockquote p {
        margin: 1.0rem 0 1.0rem 0;
    }
</style>
