---
title: "Audit de code source"
slug: "audit-de-code-source"
date: 2026-10-13
description: "Audit de code source par un expert indépendant : revue manuelle, analyse des dépendances et des secrets, trouvailles priorisées. Devis sur demande."
keywords: "audit de code source, revue de code sécurité, audit de code, expert cybersécurité indépendant"
service: true
---

Un [test d'intrusion](/fr/test-intrusion-application-web/) vous dit ce qu'un attaquant peut atteindre aujourd'hui.
Un audit de code source vous dit ce qui est fragile en dessous - y compris les parties qui ne sont pas encore atteignables, mais le seront après la prochaine version.

Les deux répondent à des questions différentes. Cette page traite de la seconde : **où cette base de code casse-t-elle, et pourquoi casse-t-elle toujours de la même façon ?**

## Pourquoi maintenant

Trois chiffres cadrent le problème.

**Votre code est surtout le code des autres.**
Le [rapport Open Source Security and Risk Analysis 2026](https://www.blackduck.com/resources/analyst-reports/open-source-security-risk-analysis.html),
établi sur 947 bases de code auditées entre novembre 2024 et octobre 2025, a trouvé des composants open source dans **98 %** d'entre elles.
**87 %** contenaient des vulnérabilités open source connues, **78 %** à risque élevé et **44 %** à risque critique.
Le nombre moyen de vulnérabilités par base de code atteint **581, contre 280 un an plus tôt** - un bond de 107 %.
Et **93 %** des bases de code embarquaient des composants sans aucune activité de développement depuis plus de deux ans : pas encore vulnérables, simplement non maintenus,
ce qui est le même problème avec un délai.

{{< figure alt="Rapport Open Source Security and Risk Analysis 2026" src="/assets/img/services/2026-open-source-security-and-risk-analysis-report.png" width="500" >}}

**Les failles ne sont pas corrigées.**
Le [State of Software Security 2026](https://www.veracode.com/resources/analyst-reports/state-of-software-security-2026/) de Veracode
situe la dette de sécurité - les failles non résolues plus d'un an après leur découverte - à **82 % des organisations**, contre 74 % l'année précédente et 71 % en 2024.
La dette critique touche désormais **60 %**, contre 50 % l'an dernier. Les failles à haut risque augmentent de 36 % sur un an. Le problème n'est plus la détection, c'est la priorisation.

**Les secrets fuient depuis l'intérieur.**
Le [rapport 2026 de GitGuardian](https://www.gitguardian.com/state-of-secrets-sprawl-report-2026)
a compté environ **29 millions de secrets** détectés sur GitHub public en 2025, soit une hausse de 34 %.
Plus parlant pour un audit : **les dépôts internes sont environ six fois plus susceptibles de contenir des secrets en dur que les dépôts publics**,
et **64 % des secrets valides fuités en 2022 n'étaient toujours pas révoqués en 2026**.
Le même rapport mesure un taux de fuite de secrets de **3,2 % sur les commits assistés par IA, contre 1,5 % de référence**, soit le double.

Ce dernier chiffre compte si votre équipe a adopté des assistants de code IA ces deux dernières années. Le volume a augmenté ; la revue, non.

## Quand lancer un audit de code

- **Avant une version sensible** : un parcours de paiement, une refonte de l'authentification, une fonctionnalité multi-tenant, une API ouverte à des partenaires.
- **Due diligence technique** : vous rachetez, investissez dans ou héritez d'une base de code et devez savoir ce que vous signez,
exposition aux licences comprise. 68 % des bases de code auditées présentaient des conflits de licence dans le jeu de données OSSRA 2026, le taux le plus élevé jamais relevé.
- **Reprise d'un code existant** : nouveau CTO, nouvelle équipe, développement externalisé réinternalisé, auteurs d'origine partis.
- **Après un test d'intrusion** : quand la cause racine d'une trouvaille reste floue, ou quand vous voulez savoir si la même erreur
existe dans les douze endroits que le test n'a pas atteints.
- **Parce que le développement s'est accéléré** : forte assistance IA, hausse marquée des pull requests fusionnées, ou un processus de revue qui a discrètement cessé d'exister.
- **Pour une obligation** : l'[article 32 du RGPD](https://eur-lex.europa.eu/legal-content/FR/TXT/?uri=CELEX%3A32016R0679)
impose de tester régulièrement l'efficacité des mesures techniques ;
NIS2 et DORA renforcent cette exigence pour les entités concernées ; les grands comptes demandent de plus en plus une SBOM et une attestation de revue de code.

## Ce que je revois

**La logique d'authentification et d'autorisation.** Pas de vérifier qu'un login existe, mais que chaque chemin censé contrôler une permission le fait réellement.
Les contrôles manquants se concentrent sur les routes que personne ne démontre : endpoints d'administration, API internes, workers de file, tâches planifiées, fonctions d'import et d'export.
C'est toujours la catégorie A01 de l'[OWASP Top 10:2025](https://top10.owasp.org/2025/) et ça reste le filon le plus riche dans les vraies bases de code.

**Le traitement des entrées et les points d'aboutissement d'injection.** Suivre les données non fiables du point d'entrée jusqu'au sink, plutôt que de chercher des motifs sur des noms de fonctions :
requêtes SQL et NoSQL construites par concaténation, exécution de commandes, rendu de templates, désérialisation, inclusions dynamiques.

**La cryptographie et la gestion des secrets.** Stockage des mots de passe, génération de jetons et entropie, gestion des clés de chiffrement,
vérification de signature qui retourne trop tôt, validation de JWT qui fait confiance à l'en-tête d'algorithme.

**Le traitement des fichiers.** Validation des uploads, emplacement et permissions de stockage, façon dont les fichiers déposés sont resservis, construction des chemins.

**La logique métier.** Séquences d'étapes contournables ou rejouables, traitement des quantités et des montants, machines à états avec des gardes inatteignables,
conditions de course sur des ressources partagées. Aucun outil ne les trouve, et ce sont souvent les failles les plus coûteuses de la base de code.

**Les dépendances et la chaîne d'approvisionnement.** Composants directs et transitifs, vulnérabilités connues, paquets non maintenus, intégrité des lockfiles, risque de substitution à la compilation.
L'OWASP a fait passer les défaillances de la chaîne d'approvisionnement logicielle en A03 dans le Top 10 2025, et le [DBIR 2026](https://www.verizon.com/business/resources/reports/dbir/)
constate l'implication d'un tiers dans 48 % des compromissions, en hausse de 60 % sur un an.

**Les secrets, y compris dans l'historique Git.** Un secret retiré de l'arbre de travail est toujours dans l'historique, toujours valide, et toujours indexable.
Je vérifie les deux. J'ai écrit et publié [l'outillage](/fr/projets/) pour cette partie du travail, et j'ai trouvé des identifiants cloud actifs
dans des dépôts clients à plus d'une reprise.

**La configuration et l'infrastructure as code.** Dockerfiles, définitions de pipelines CI/CD, playbooks Ansible, gestion des environnements.
Les pipelines qui ont un accès en écriture à la production sont une cible, et ils sont rarement revus.

<br>

> PHP et Laravel en profondeur, JavaScript et TypeScript (Node), Python, Bash, SQL, et
l'infrastructure as code (Docker, Ansible). Pour une base de code dominée par Java, .NET, Go ou Rust,
je vous le dis d'emblée et je vous oriente vers quelqu'un de mieux placé - une revue superficielle dans un langage
que je ne pratique pas au quotidien vaut moins que pas de revue du tout.

## Comment ça se déroule

**1. Cadrage** - dépôts concernés, langages, taille approximative, branche ou tag à auditer, méthode d'accès (une clé de déploiement en lecture seule est préférable,
aucune copie ne quitte mon poste chiffré), accord de confidentialité, et un contact technique nommé pour les questions.

**2. Passe d'architecture** - points d'entrée, frontières de confiance, flux de données, le modèle d'autorisation tel qu'il est réellement implémenté plutôt que tel qu'il est documenté.
Cette phase décide où passe le temps restant, c'est pourquoi elle précède tout outillage.

**3. Filet de couverture automatisé** - analyse statique, scan de dépendances et détection de secrets, exécutés pour s'assurer que rien d'évident n'est manqué.
La sortie part en triage, jamais telle quelle dans le rapport. La trouvaille d'un outil est une piste, pas une conclusion.

**4. Revue manuelle des chemins critiques** - le cœur de la mission, guidée par le
[OWASP Code Review Guide](https://owasp.org/www-project-code-review-guide/) pour la méthode,
l'[OWASP ASVS](https://owasp.org/www-project-application-security-verification-standard/) pour le niveau d'exigence,
et le [CWE Top 25](https://cwe.mitre.org/top25/) ainsi que l'OWASP Top 10:2025 pour la couverture des classes de faiblesses.

**5. Vérification** - pour chaque trouvaille sérieuse, une preuve fonctionnelle sur une instance locale quand c'est possible. Je préfère vous remettre une faille démontrée que cinq failles théoriques.

**6. Rapport et restitution** - livrable écrit, puis une séance de travail avec les développeurs, en français ou en anglais.

## Le rapport

- **Une synthèse d'une page** pour la direction : ce qui a été revu, quels sont les risques réels, ce qu'il faut corriger en premier, et un énoncé honnête des limites de couverture.
- **Chaque trouvaille** avec sa localisation (fichier et ligne, référence de commit), la classe CWE, une criticité contextualisée à votre application,
le scénario d'attaque, et une remédiation concrète - dans votre langage et votre framework, pas un lien vers un article générique.
- **Les motifs récurrents**, et c'est là qu'un audit de code justifie son prix. Un contrôle d'autorisation manquant est un bug ; la même omission
dans quatorze contrôleurs est un problème de conception, et la recommandation n'est pas la même.
- **Un inventaire des dépendances** avec ce qu'il faut mettre à jour, dans quel ordre, et ce qui casse quand vous le faites.
- **Les secrets trouvés**, avec une liste de révocation urgente séparée du reste. Cette section vous parvient par canal chiffré avant
la fin du rapport si quelque chose d'actif remonte.

<br>

> Une page d'administration accessible publiquement est une information, pas une vulnérabilité critique.
Gonfler les scores remplit le rapport et vide sa valeur : au troisième audit où
tout est « élevé », plus personne ne le lit, et la seule trouvaille réellement critique
se noie dans le bruit. Acceptez les rapports courts quand l'application est saine.

## Ce qu'un audit de code n'est pas

- **Ce n'est pas un rapport de scanner avec une page de garde.** Si vous voulez une sortie de SAST, vous pouvez l'exécuter vous-même pour moins que mon taux journalier. Ce que je vends, c'est le triage.
- **Ce n'est pas exhaustif.** Un budget borné sur une base de code de taille réelle implique une couverture priorisée.
Le rapport énonce explicitement ce qui a été revu en profondeur, ce qui a été survolé, et ce qui n'a pas été ouvert du tout.
- **Ce n'est pas un remplacement du test d'intrusion.** La revue de code ignore la réalité du déploiement et de la configuration -
la base de données de recette joignable depuis Internet, le sous-domaine oublié. Le [pentest](/fr/test-intrusion-application-web/) ignore
ce que fait le code sur les chemins qu'il n'emprunte jamais. Faites les deux, dans l'ordre de préférence selon la question qui vous empêche de dormir.
- **Ce n'est pas une revue de qualité de code.** Je ne reformaterai pas votre code et je ne débattrai pas d'architecture, sauf là où l'architecture *est* la vulnérabilité.
- **Ça ne corrige rien tout seul.** Rappel du chiffre de 82 % de dette de sécurité plus haut : le goulot n'est pas de trouver les failles, c'est d'agir dessus.

## Pourquoi moi

- **Auditeur pour le ministère des Armées, de 2020 à 2024** : audits de code, audits de configuration selon les recommandations de l'ANSSI,
et tests d'intrusion sur des systèmes Linux et des applications web.
- **Développeur pendant environ 25 ans avant ça**, dans plusieurs sociétés web françaises. Je lis votre code sans traducteur,
et mes recommandations tiennent compte de ce qu'elles coûtent à implémenter, ce qui est précisément la partie que la plupart des rapports de sécurité ratent.
- **Chasseur de vulnérabilités depuis 2016** : plusieurs centaines de rapports à des programmes chez EDF, Decathlon, Payfit, BlaBlaCar, Uber, Yahoo, Zillow et d'autres,
dont des fuites de secrets cloud et des expositions de code source. Des [CVEs](/cves/) publiées sous mon nom, la plupart trouvées en lisant du code.
- **Auteur de l'outillage** : j'ai écrit et publié les [scripts et extensions](/fr/projets/) que j'utilise pour chasser secrets, endpoints et dépôts exposés.
- **Formateur depuis 2022** : une [dizaine de formations](/fr/formations/), dont [sécurité Laravel](/fr/formations/)
et [sécurité des applications web](/fr/formations/) pour développeurs.
Si l'audit montre la même classe de faille qui se répète, le correctif est en amont du code.

Un exemple public, où une fuite de code source était le point d'entrée vers tout le reste :
mon [analyse d'une application médicale française critique](/posts/analysis-of-a-critical-french-medical-application/).

## Questions fréquentes

**Combien de temps ?**
Cinq à quinze jours selon la taille et le langage, plus la rédaction.
Une application Laravel ou Node de taille moyenne tourne autour de huit jours.
Je vous donne une estimation après un coup d'œil aux statistiques du dépôt, pas avant.

**Combien ça coûte ?**
Devis sur demande, au forfait. Les facteurs : lignes de code dans le périmètre, nombre de langages, disponibilité d'une instance en fonctionnement,
et inclusion ou non d'une contre-vérification. Vous obtenez une fourchette dès le premier échange.

**Avez-vous besoin d'un accès à notre dépôt ?**
Oui, en lecture seule, sur la branche ou le tag convenu. Une clé de déploiement en lecture seule ou un jeton d'accès à durée limitée est l'idéal.
Rien n'est copié hors de mon poste chiffré, et tout est détruit après la contre-vérification sauf demande contraire. Accord de confidentialité signé avant l'accès.

**Pouvez-vous travailler à partir d'une archive ?**
Oui, mais vous perdez l'historique Git, et c'est là que sont les secrets.
Si la confidentialité l'interdit, dites-le et nous cadrerons autour.

**Et si la base de code est un chantier ?**
C'est le cas courant, et ce n'est pas une raison de refuser.
Ça change la méthode : plus de temps sur l'architecture et sur la recherche de motifs récurrents, moins sur la couverture exhaustive.

**Et si vous ne trouvez rien de grave ?**
Vous obtenez un rapport qui le dit, avec le détail de couverture qui le rend défendable devant un client ou un auditeur.
C'est le résultat que je préfère, et il est plus rare qu'on ne l'espère.

Enfin, si les trouvailles pointent un manque de compétence plutôt que des erreurs isolées,
une [formation](/fr/formations/) coûte moins cher que d'auditer la même base de code l'année prochaine.

## Parlons-en

Dites-moi ce que fait la base de code, à peu près quelle taille, quels langages, et ce qui déclenche la question.
Je vous réponds avec un périmètre proposé et une fourchette de prix.

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
