---
slug: "audit-de-configuration-linux-demarche-outillage-livrables"
title: "Audit de configuration Linux : démarche, outillage et livrables"
date: 2026-11-12
description: "Cadre contractuel, interlocuteurs, référentiels ANSSI, scripts de collecte et d'analyse, rapport et livrables : la démarche complète d'un audit de configuration de serveurs GNU/Linux."
related:
  - /courses/linux-hardening
  - /services/config-audit
tags: ["audit", "ANSSI", "BP-028", "PASSI", "Linux", "OpenSSH", "durcissement"]
---

## Qu'est-ce qu'un audit de configuration ?

L'audit de configuration confronte la configuration réelle d'un système à un référentiel reconnu, recommandation par recommandation, preuves à l'appui. 
[Le référentiel PASSI de l'ANSSI](https://cyber.gouv.fr/actualites/lanssi-met-%C3%A0-jour-les-referentiels-passi-version-22-et-pris-version-30/) 
le définit comme l'évaluation de la conformité ou de la sécurité de la configuration des dispositifs matériels et logiciels d'un système d'information.

C'est l'une des 5 activités couvertes par ce référentiel :
- [l'audit de configuration](/fr/prestations/audit-configuration-durcissement-linux/)
- l'audit d'architecture
- [l'audit de code source](/fr/prestations/audit-de-code-source/)
- [le test d'intrusion](/fr/prestations/test-intrusion-application-web/)
- l'audit organisationnel et physique

Cet article décrit la démarche appliquée aux serveurs GNU/Linux audités notamment au regard des guides de références de l'ANSSI, et l'outillage qui la soutient.

Là où le test d'intrusion cherche un chemin d'attaque, l'audit de configuration dresse un état des lieux exhaustif. 
Il fait apparaître des faiblesses latentes : un paramètre noyau oublié, un compte _root_ joignable en SSH, une clé privée sans phrase de passe. 
Pris isolément, ces écarts sont rarement exploitables mais cumulés ils retirent des couches à la défense en profondeur.

L'exercice a des limites qu'il faut annoncer au client :
- c'est une photographie à la date de la collecte
- il ne démontre pas l'exploitabilité d'un écart
- il vaut ce que vaut le référentiel retenu

Enfin, l'outillage ne remplace pas l'auditeur. 
Le référentiel PASSI précise qu'une activité réalisée de manière entièrement automatisée ne constitue pas une activité d'audit : 
__les scripts collectent__ et trient, __l'auditeur qualifie__.


## Le cadre contractuel

Aucune commande ne s’exécute sur un serveur avant la signature de ces documents : 
- la proposition commerciale
- la fiche de cadrage 
- l’autorisation d’exécution

Pour un auditeur indépendant, ils tiennent en deux ou trois pages. 
Ce qui compte, c’est qu’ils soient écrits et signés avant la collecte.
En pratique, un seul document suffit : le devis, la fiche de cadrage en annexe et l’autorisation en dernière page, signés en une fois.


#### La proposition signée

Un devis détaillé accompagné de conditions générales, signé par le client, tient lieu de contrat. Il couvre l’essentiel :

- le périmètre : nombre de serveurs, référentiel et niveau de durcissement visé
- les livrables, le calendrier et le prix
- la propriété : les scripts restent à l’auditeur, le rapport appartient au client
- la confidentialité, et la destruction des données collectées à la fin de la mission
- la limite de responsabilité, adossée à une assurance responsabilité civile professionnelle

#### La fiche de cadrage

Une page, en annexe du devis ou dans un courriel validé par le client, remplace la note de cadrage et le plan d’audit. 
Elle liste les serveurs et leur rôle, nomme l’interlocuteur côté client, et fixe la fenêtre de collecte et le canal de transfert. 
Si l’audit porte sur un échantillon, elle dit lequel et pourquoi.

#### L’autorisation d’exécution

C’est le document à ne jamais sauter, même pour une petite mission. 
Le Code Pénal réprime l’accès ou le maintien frauduleux dans un système de traitement automatisé de données, 
une autorisation écrite protège l’auditeur autant que le client.

Elle tient en quelques lignes : les serveurs visés, la fenêtre d’exécution, l’empreinte _SHA-256_ du script remis et 
la signature d’une personne qui a autorité sur ces systèmes. Si les serveurs sont hébergés ou infogérés par un tiers, son accord s’y ajoute.

Quand la mission passe par un intermédiaire, ESN ou plateforme, l’autorisation doit venir du propriétaire des serveurs, pas de l’intermédiaire.

## Les interlocuteurs privilégiés

Chaque rôle est nommé dans la note de cadrage avant le lancement, avec ses coordonnées. 
Le plus important est le correspondant de la prestation, il est le point de contact unique, chargé de faire respecter la convention et la note de cadrage.

| Rôle | Côté | Ce qu'on attend de lui |
| --- | --- | --- |
| Correspondant de la prestation      | Client   | Interlocuteur unique ; tient la note de cadrage à jour, arbitre et répond vite |
| RSSI                                | Client   | Valide le périmètre, le niveau de durcissement visé et le marquage du rapport |
| Administrateurs système             | Client   | Relisent puis exécutent le script, transmettent les archives, répondent aux questions techniques |
| Délégué à la protection des données | Client   | Consulté dès que les journaux ou les répertoires personnels entrent dans la collecte |
| Direction                           | Client   | Signe la convention, reçoit la synthèse, assiste à la réunion de clôture |
| Hébergeur ou infogérant             | Tiers    | Donne son accord quand les serveurs ne sont pas au client ; exécute parfois la collecte |
| Responsable d'équipe d'audit        | Auditeur | Rédige la note de cadrage et le plan d'audit, valide le rapport |
| Auditeurs                           | Auditeur | Analysent les archives, qualifient les écarts, rédigent les constats |

Les canaux se fixent eux aussi d'avance : une adresse de contact, un numéro joignable pendant la fenêtre de collecte et un canal chiffré pour les fichiers. 
L'empreinte d'une archive se confirme toujours par un canal différent de celui qui l'a transportée.


## Les pièces jointes et prérequis

L'audit démarre sur deux dossiers : sans le premier, les écarts ne peuvent pas être qualifiés ; sans le second, rien n'est collecté.

__Ce que le client fournit pour cadrer l'analyse :__

| Document | À quoi il sert |
| --- | --- |
| Inventaire des serveurs : nom, adresse IP, distribution, rôle, environnement, criticité | Délimite le périmètre et fonde l'échantillonnage |
| Schéma d'architecture et matrice de flux                                                | Situe chaque serveur ; un routeur n'attend pas les mêmes paramètres réseau qu'un serveur d'application |
| PSSI, standard de durcissement interne, dérogations accordées                           | Ajoute des critères et signale les écarts déjà acceptés |
| Rapports d'audit précédents                                                             | Permet de suivre les écarts récurrents |
| Liste des contacts et astreinte                                                         | Alimente la note de cadrage |

__Et ce que l'auditeur remet pour la collecte :__

| Document | À quoi il sert |
| --- | --- |
| Convention de service, note de cadrage, plan d'audit       | Fixent le cadre, à signer et valider avant la collecte |
| Script de collecte, empreinte SHA-256, notice d'exécution  | Permettent au client de relire le script avant de l'exécuter |
| Clé publique de chiffrement (age ou GPG)                   | Sert à chiffrer les archives avant leur envoi |
| Attestation de qualification PASSI, le cas échéant         | Annexée à la convention pour une prestation qualifiée |

Le script est remis en clair, accompagné de son empreinte. Demander au client d'exécuter du code en _root_ suppose de lui laisser le lire. 
En retour, les archives produites rejoignent le dossier d'audit sans jamais circuler en clair.


## Les référentiels

Les critères de sécurité principaux sont issues des guides de l'ANSSI, entre autre :
- [Guide relatif à la maturité SSI](https://messervices.cyber.gouv.fr/guides/guide-relatif-la-maturite-ssi)
- [ANSSI-PA-022: Recommandations relatives à l’administration sécurisée des SI](https://messervices.cyber.gouv.fr/guides/recommandations-relatives-ladministration-securisee-des-si)
- [ANSSI-PA-066: Recommandations relatives à l’interconnexion d’un SI à Internet](https://messervices.cyber.gouv.fr/guides/recommandations-relatives-linterconnexion-dun-si-internet)
- [ANSSI-BP-028: Recommandations de configuration d'un système GNU/Linux](https://messervices.cyber.gouv.fr/guides/recommandations-de-securite-relatives-un-systeme-gnulinux)
- [DAT-NT-007: Usage sécurisé d'(Open)SSH](https://messervices.cyber.gouv.fr/guides/usage-securise-dopenssh)
- [ANSSI-PA-009: Sécuriser un site web](https://messervices.cyber.gouv.fr/guides/securiser-un-site-web)
- [Recommandations de sécurité relatives à TLS](https://messervices.cyber.gouv.fr/guides/recommandations-de-securite-relatives-tls)

Cependant certaines recommandations dépendent du rôle du serveur. Les paramètres réseau de la recommandation _ANSSI-BP-028-R12_, par exemple, 
sont donnés pour un serveur qui ne fait pas de routage. 
Sur un routeur, plusieurs valeurs diffèrent volontairement.
Les recommandations concernées sont donc notées comme *"non applicable"* dans le rapport final.

A noter qu'une extrapolation, même pertinente, est présentée comme telle et jamais sous le numéro d'une règle qui ne la dit pas.


Au-delà des référentiels, tout comme en test d’intrusion, l’expérience de l’auditeur compte autant que la liste de contrôles. 
Il sait où se cachent le plus souvent les écarts critiques, y compris là où les guides ne les formulent pas, ou pas aussi directement. 

Quelques contrôles hors référentiel se révèlent régulièrement fructueux :
- des mots de passe systèmes trop faibles ou présents dans les historiques de shell
- des secrets en clair dans des logs
- des sauvegardes de configuration et des dumps SQL sous la racine d’un serveur web
- des scripts lancés par la crontab de _root_ mais modifiables par un autre utilisateur
- des confirgurations web peu sécurisées

Ces constats suivent la règle de citation ci-dessus : ils figurent dans une section dédiée, sans numéro de recommandation, 
et sont cotés selon leur risque réel, avec la même prudence que les autres. La proposition l’annonce d’emblée, 
pour que le client ne les découvre pas dans le rapport.


## La démarche, étape par étape

Le client exécute, l'auditeur analyse : à aucun moment l'auditeur ne se connecte aux serveurs audités. 
C'est ce que recommande l'ANSSI : quand une prestation exige d'exécuter un outil sur le système audité, le commanditaire le fait lui-même.

#### Préparation
L'auditeur vérifie d'abord qu'il peut réaliser l'ensemble de la prestation, puis établit la convention, la note de cadrage et le plan d'audit. 
La réunion de lancement réunit le correspondant, le RSSI et les administrateurs. 
On y parcourt le script, puis on arrête les options d'exclusion, le canal de transfert et la fenêtre de collecte.

<br>

<a href="/images/audit-configuration-demarche.jpg" target="_blank"><img src="/images/audit-configuration-demarche.jpg" alt="Démarche d'audit : du besoin initial à la destruction des données" width="400" style="float:left;" /></a>

<div style="float:right;width:450px;">
    <h4 id="collecte" style="margin:0">Collecte</h4>
    <br>Un administrateur du client relit le script, vérifie son empreinte et l'exécute sur chaque serveur.
    L'ANSSI recommande de mener les audits de configuration sur l'environnement de production : c'est sa configuration réelle qu'on évalue. 
    Sur un gros volume, le parcours du système de fichiers peut prendre quelques minutes, l'exploitation doit en être prévenue.
    <br><br>
    <h4 id="analyse" style="margin:0">Analyse</h4>
    <br>L'auditeur contrôle l'empreinte, extrait l'archive et l'analyse.
    Commence alors le vrai travail : relire chaque constat à la lumière du rôle du serveur, des dérogations et du niveau de durcissement visé. 
    Les contrôles indéterminés et les écarts ambigus deviennent des questions au client.
</div>

<div style="clear:both;"></div>

#### Restitution

Les constats les plus graves sont signalés sans attendre le rapport. Le rapport provisoire passe en relecture factuelle, 
le rapport définitif est présenté en réunion de clôture, puis les données collectées sont détruites.


## L'outillage, partie 1 : le script de collecte

`collect.sh` est exécuté par le client, en _root_, sur chaque serveur audité. 
Il ne modifie rien, n'ouvre aucune connexion réseau et ne tente aucune authentification ; il produit une archive `.tar.gz` à renvoyer à l'auditeur.

Écrit en _Shell POSIX_, il fonctionne sous la plupart des Shell Linux. 
Il ne s'appuie que sur des packages de base, chaque commande absente est journalisée puis ignorée. 
Une distribution minimale donne une collecte partielle, jamais un échec.

{{< figure alt="Audit de configuration, collecte" src="/images/audit-configuration-collecte.png" width="600">}}

#### Ce que contient l'archive

**Contexte de la collecte :** distribution, noyau, virtualisation, système d'init, privilèges, options utilisées.  

**Sortie brute de plus de 80 commandes en lecture seule :** `sysctl`, `ss`, `iptables`, `netstat`, `nft`, `systemctl`, `auditctl`, paquets installés, etc.  

**Inventaire du système de fichiers :** type, droits, propriétaire, groupe, taille, date et chemin de chaque entrée.  

**Listes tirées de l'inventaire :** _setuid_ et _setgid_, fichiers sans propriétaire, éléments inscriptibles par tous, archives suspectes, 
index des fichiers copiés.  

**Fichiers de configuration, sauvegardes, fichiers sensibles** et journaux repérés, chemins absolus conservés.  

**`/etc` complet**, puis `/home` et `/root` sans les caches, droits préservés.  

**Journal horodaté** de la collecte et copie du script effectivement exécuté.  

#### Les choix de conception

**Une hygiène de fichiers stricte.** Un répertoire de travail temporaire est créé et supprimé une fois l'exécution de la collecte terminée.

**Un seul parcours du système de fichiers.** Toutes les listes dérivées sortent de l'inventaire. 
Les pseudo-systèmes `/proc`, `/sys`, `/dev` et `/run` sont exclus.  

**Des garde-fous de volume.** Un fichier copié est tronqué à 256 Ko pour éviter les surcharges. 
Tout dépassement est consigné dans l'archive. Les copies conservent droits et dates.  

**La collecte ne masque, ne pseudonymise ni ne modifie aucune donnée :** Les fichiers sont copiés tels quels, avec leurs droits et leurs dates, 
car une donnée altérée fausse l’analyse. 
Une empreinte de mot de passe masquée ne dit plus quel algorithme la protège, une clé privée caviardée ne dit plus si elle est chiffrée.

**La transparence.** Les fichiers de logs listent exactement les commandes exécutées et ce qui quitte la machine, le client peut le vérifier.  

En fin d'exécution, le script affiche le chemin de l'archive, sa taille et son empreinte SHA-256. Le client transmet cette empreinte par un canal distinct ; l'auditeur la recalcule avant toute analyse.

## L'outillage, partie 2 : le script d'analyse

`analyse.sh` tourne sur le poste de l'auditeur, sur l'archive extraite. 
Il confronte chaque élément collecté à la recommandation qui le vise et produit un rapport texte où chaque verdict cite sa source.

{{< figure alt="Audit de configuration, analyse" src="/images/audit-configuration-analyse.png" width="600">}}

#### Cinq statuts, pas deux

| Statut | Signification |
| --- | --- |
| `COMPLIANT`       | La recommandation est respectée |
| `NON_COMPLIANT`   | Elle ne l'est pas ; la sévérité (_CRITICAL, MAJOR, MINOR_) est déclarée par la règle |
| `UNDETERMINED`    | La preuve nécessaire est absente ou illisible |
| `NOT_APPLICABLE`  | La règle ne s'applique pas à ce serveur |
| `INFO`            | Information sans verdict |

Chaque directive testée reçoit en plus une étiquette : 
- _OK_ si la valeur est correcte et fixée dans la configuration
- _DEFAULT_ si elle est correcte mais héritée d'une valeur par défaut
- _FAIL_ si elle s'écarte de l'attendu
- _UNKNOWN_ si elle est illisible
 
La distinction entre _OK_ et _DEFAULT_ compte : une valeur que rien ne fige peut changer à la prochaine mise à jour.

#### Les règles de rigueur

- **Indéterminé n'est pas non conforme.** Une preuve manquante ne vaut ni réussite ni échec, et la confondre avec un écart décrédibilise un rapport.
- **L'exécution et la persistance sont distinguées.** Un paramètre correct à l'exécution mais déclaré nulle part est signalé, car un redémarrage peut le ramener à sa valeur par défaut.
- **Le propriétaire d'un fichier se lit dans l'inventaire, jamais dans la copie extraite.** `tar` ne restaure les propriétaires que s'il tourne en _root_.
- **L'interprétation et l'extrapolation restent séparées.** `--tips` ajoute des explications sans changer aucun verdict. `--extra` lance des contrôles qui prolongent une règle au-delà de son texte ; leurs résultats sont comptés à part.

#### La sortie

La source vient avant le verdict : le lecteur sait ce qui a été lu avant d'apprendre ce que cela signifie.
Le terminal est coloré, le fichier de rapport jamais : il reste en texte brut, exploitable avec `grep` et comparable d'un audit à l'autre avec `diff`. 

Le code de sortie résume le résultat :
```text
============================================================================
 Summary
============================================================================
 checks run      : 29
 not run         : 1 extrapolated check(s), available with --extra

 COMPLIANT       : 7
 NON_COMPLIANT   : 13
 UNDETERMINED    : 6
 NOT_APPLICABLE  : 3
 INFO            : 0

 severity of the non-compliant findings:
   CRITICAL      : 0
   MAJOR         : 2
   MINOR         : 11
```

## Le rendu : rapport, cotation et matrice

Le rapport transforme des sorties de script en décisions : chaque écart y devient un constat prouvé, coté et assorti d'une mesure. 
La sortie brute n'est qu'une annexe.

<br>

<a href="/images/audit-configuration-fiche.png" target="_blank"><img src="/images/audit-configuration-fiche.png" alt="Démarche d'audit : douze étapes réparties entre le client et l'auditeur, du besoin initial à la destruction des données" width="400" style="float:right;" /></a>

<div style="float:left;width:450px;">
    <h4 id="la-structure-du-rapport" style="margin:0">La structure du rapport</h4>
    <br>1. <b>Le cadre :</b> objectifs, critères d'audit, périmètre, dates, équipe d'audit et modalités particulières.
    <br>2. <b>La synthèse managériale :</b> lisible sans expertise : appréciation générale, risques principaux et recommandations÷. 
    <br>3. <b>Les résultats :</b> une fiche par non-conformité, puis la synthèse par serveur.
    <br>4. <b>Les annexes :</b> note de cadrage, plan d'audit, matrice de conformité et sorties brutes de l'analyse.
    <br>
    <h4 id="la-fiche-de-constat">La fiche de constat</h4>
    Chaque non-conformité porte un identifiant unique, l'exigence non satisfaite, la preuve, une gravité, une mesure et une priorité. 
    Exemple tiré de l'extrait ci-dessus →
</div>

<div style="clear:both;"></div>

#### Les échelles de cotation

La gravité d'un constat est la combinaison de son impact et de sa difficulté d'exploitation.
Sa priorité de correction combine cette gravité et l'effort de remise en conformité. 

**L'impact** mesure ce que la vulnérabilité procure à un attaquant qui l'exploite avec succès, et rien d'autre. 
Ni la probabilité que cela arrive, ni la difficulté d'y parvenir.

**La difficulté d'exploitation** mesure l'effort total à fournir pour exploiter la vulnérabilité, de la position de départ jusqu'au résultat. 
Il agrège deux composantes distinctes :
- les prérequis : position réseau, authentification, privilèges, interaction d'un tiers, fenêtre temporelle
- le geste technique : outillage nécessaire, fiabilité de l'exploitation, expertise requise

Ces deux composantes divergent souvent, et c'est le cas le plus exposé à la contestation en réunion de restitution. 
Le niveau retenu est celui des prérequis, et la fiche mentionne les deux.

{{< figure alt="Audit de configuration, matrice" src="/images/audit-configuration-matrix.png" width="600">}}

#### La matrice de conformité

La matrice est un tableur : une ligne par recommandation et par serveur, avec le statut, la gravité, la preuve, la mesure et la priorité. Elle sert au client pour piloter la remédiation, puis à l'auditeur comme base de l'audit de contrôle. Les lignes indéterminées y figurent, avec leur raison.


## Les livrables et la restitution

À la clôture, le client dispose d'un rapport définitif, d'une matrice de conformité exploitable et d'un procès-verbal de destruction. 
Les autres livrables jalonnent la prestation.

| Livrable | Contenu |
| --- | --- |
| Note de cadrage et plan d'audit | Modalités, interlocuteurs, calendrier, méthode, échantillonnage<br>Remis avant la collecte |
| Kit de collecte | Script, empreinte SHA-256, notice d'exécution, clé publique de chiffrement<br>Remis a la réunion de lancement |
| Restitution à chaud | Constats les plus graves et premières conclusions<br>Remis dès la fin de l'analyse |
| Rapport provisoire | Rapport complet, soumis à relecture factuelle<br>Remis après la qualification des écarts |
| Rapport définitif | Rapport corrigé et marqué selon la note de cadrage<br>Remis après la relecture |
| Matrice de conformité | Tableur, une ligne par recommandation et par serveur<br>Remis avec le rapport définitif |
| Sorties brutes de l'analyse | Un fichier texte par serveur<br>Remis en annexe du rapport |
| Procès-verbal de destruction | Éléments détruits, date et mode de destruction<br>Remis à la clôture |

La relecture du rapport provisoire porte sur les faits : un serveur mal identifié, une dérogation oubliée, une preuve mal lue. 
La cotation ne se négocie pas ; si le client conteste une gravité, son argument figure dans le rapport à côté de celle de l'auditeur.

La réunion de clôture réunit la direction, le RSSI, le correspondant et les administrateurs. 
Elle s'adresse à deux publics : la synthèse pour la direction, les fiches de constat pour les équipes techniques.

## Conclusion

Un audit de configuration vaut par sa rigueur plus que par son outillage. 
Le script de collecte rend la démarche reproductible et transparente pour le client.
Le script d'analyse rend chaque verdict traçable jusqu'à sa preuve.

Le reste demeure le travail de l'auditeur : cadrer, qualifier chaque écart dans son contexte, coter au plus juste et proposer 
des mesures que l'exploitation pourra réellement appliquer.

<style>
    .content figure {
        text-align: center;
    }
</style>

