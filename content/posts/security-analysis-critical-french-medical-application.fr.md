---
date: 2025-04-21
slug: "analyse-d-une-application-medicale-francaise-critique"
title: "Analyse d'une application médicale française critique"
images: [/images/optimed-preview.png]
related:
  - /posts/github-tools-collection
  - /posts/vulnerability-in-wordpress-video-gallery
  - /services/pentest
  - /services/code-audit
tags:
  - pii
  - leak
  - git
  - sql injection
  - rce
  - file upload
  - php
---
Dans le cadre de mes activités habituelles en tant que bug hunter et auditeur de sécurité pour des institutions sensibles, j'ai découvert une application dédiée au secteur de la santé, baptisée __OptiMed__ (rédigé), développée par __Synaptek Digital__ (rédigé) et utilisée dans de nombreux centres d'imagerie médicale en France. Malheureusement, ce logiciel présente un niveau de sécurité catastrophique, exposant ainsi __des millions de dossiers médicaux sensibles__.
<!--more-->


## Introducing OptiMed

Selon le site de l'éditeur :
> Synaptek Digital propose une solution globale entièrement intégrée, couvrant l'ensemble des besoins d'un service d'imagerie médicale.

_OptiMed_ prend en charge les examens médicaux tels que les scanners et les IRMs, et prévoit différents rôles utilisateurs : patients, médecins, personnel administratif, administrateurs système, etc.

Toujours selon leur site :
> Synaptek Digital est expert en matière d'interopérabilité et de sécurité des systèmes d'information de santé depuis plus de 10 ans.

Pourtant, dans la réalité, la mise en œuvre constitue un véritable __cauchemar en matière de sécurité__.


## Vulnérabilités critiques

### Fuite de code source

<div style="float:left;width:550px;">
    Premier problème critique : le répertoire <code>.git</code> était accessible publiquement à la racine du portail web :
    <code>
        https://www.irm-redacted.fr/.git/
    </code>
    <p>
        Le souci, dans ce genre de situation, est qu'il est généralement possible de récupérer le code source de l'application. 
        À l'aide de quelques manipulations Git et d'outils, j'ai rapidement pu récupérer le tout.
    </p>
    <p>
        Répertoires et sous-répertoires, fichiers PHP, fichiers cachés, fichiers de configuration... rien ne manquait, et le code source était parfaitement intact. 
    </p>
    <p>
        Au vu du désordre régnant dans le répertoire racine, je me doutais déjà que la situation serait grave. 
        Il était temps de procéder à un audit de code "sérieux". 
    </p>
</div>

<img src="/images/optimed-ls.png" alt="OptiMed file listing" width="300" style="float:right;" />

<div style="clear:both;"></div>

### Secrets

<div style="float:left;width:550px;">
    Comme je réalise souvent des audits de code, je dispose d'expressions régulières personnalisées qui me permettent de détecter des éléments sensibles : 
    <i>noms d'utilisateur</i>, <i>mots de passe</i>, <i>jetons</i>, <i>clés d'API</i>...
    <p>
        En somme, un simple <code>grep</code> lancé depuis le répertoire racine suffit à faire ressortir ces informations sensibles. 
    </p>
    <p>
        Le résultat a été inattendu (enfin...) : j'ai mis la main sur des éléments plutôt compromettants, notamment :
        des identifiants FTP, SMTP ou de base de données, des accès administrateur, et bien plus encore. 
    </p>
</div>

<img src="/images/optimed-secrets-1.png" alt="OptiMed secrets" width="300" style="float:right;" vspace="3" />
<img src="/images/optimed-secrets-2.png" alt="OptiMed secrets" width="300" style="float:right;" vspace="3" />
<img src="/images/optimed-secrets-4.png" alt="OptiMed secrets" width="300" style="float:right;" vspace="3" />

<div style="clear:both;"></div>

### Injections SQL

En ouvrant des fichiers PHP au hasard pour en examiner la qualité du code, je suis tombé sur de véritables abbérations.
La plupart des pages contenaient des requêtes SQL non filtrées, manifestement dépourvues de toute validation des données en entrée.
```php
$lastname		= isset($_POST['nom']) ? $_POST['nom'] : "";
$firstname		= isset($_POST['prenom']) ? $_POST['prenom'] : "";
$email			= isset($_POST['email']) ? $_POST['email'] : "";

$request = "SELECT * FROM radio ";
$request.= " WHERE lastname='$lastname' ";
$request.= " AND firstname='$firstname' ";
$request.= " AND email='$email' ";

$result = $mysql->query($request);
```
Il s'agit de vulnérabilités classiques mais critiques de type injection SQL qui peuvent permettre :
- L'extraction de données
- Le contournement de l'authentification
- L'accès administrateur

### Mots de passe

En exploitant l'une des nombreuses injection SQL, j'ai pu examiner la structure de la base de données.
J'ai utilisé [SQLMap](https://sqlmap.org/) pour recueillir des informations sur cette structure, lorsque la table `utilisateur` a attiré mon attention.

![OptiMed utilisateurs](/images/optimed-utilisateurs.png)

Les mots de passe étaient manifestement stockés à l'aide d'un algorithme de hachage faible. 
Grâce à [hashes.com](https://hashes.com/en/decrypt/hash), j'ai pu déchiffrer certains mots de passe en un clin d'oeil :

![OptiMed passwords](/images/optimed-passwords.png)

Mot de passe _administrateur_ récupéré : accès au backoffice d'administration débloqué !

### Téléchargement de fichiers arbitraires

Une vulnérabilité liée au téléchargement de fichiers arbitraires survient lorsqu'une application permet aux utilisateurs 
d'uploader n'importe quel type de fichier sans les contrôles adéquats. 
Un attaquant peut exploiter cette faille pour importer des fichiers malveillants, tels que des _prompts web_, ce qui peut conduire à 
l'exécution de code à distance, au vol de données ou à la compromission du serveur.

Plusieurs scripts de téléchargement de fichiers non protégés, comme celui-ci, étaient présents dans l'application.  
Une situation extrêmement dangereuse.
```php
$uploadDir = "./";
if(isset($_FILES['FileToImport']))
{
	$fichier = $uploadDir.basename($_FILES['FileToImport']['name']);
	if(move_uploaded_file($_FILES['FileToImport']['tmp_name'], $fichier)) {
		echo 'Upload effectué avec succés !<br>';
	}
	else {
		echo 'Echec de l\'upload !<br>';
		return;
	}
```

### Interaction système

Dans le pire des cas, les vulnérabilités par injection de commandes permettent l'exécution arbitraire de commandes système à partir 
de données fournies par l'utilisateur :
```php
$PID = $_POST['PID'];
exec("taskkill /F /PID $PID" );
```
Un attaquant pourrait alors injecter n'importe quelle commande pour neutraliser le système.

Certains outils d'administration critiques étaient également accessibles au public, sans aucun système d'authentification.
Inutile de préciser ce qui se passerait si un utilisateur malveillant venait à découvrir cela :
![OptiMed services](/images/optimed-services.png)


## Une surface d'attaque gigantesque

Combien de centres médicaux utilisaient _OptiMed_, combien d'instances ont été déployées et combien d'utilisateurs auraient été affectés par une fuite de données ?
En effectuant quelques recherches sur Shodan, notamment la classique technique du [hachage de favicon](https://github.com/gwen001/favicon-hashtrick), 
j'ai identifié environ __190 centres médicaux en France__ (ainsi que quelques-uns en Suisse) qui utilisaient cette application.

![OptiMed Shodan](/images/optimed-shodan.png)

<div style="float:left;width:450px;">
    En utilisant une injection SQL sur l'ensemble d'entre eux, j'ai pu me faire une idée approximative du nombre d'utilisateurs exposés, hôte par hôte.
    Les chiffres étaient effrayants, je ne m'attendais absolument pas à cela.
</div>

<img src="/images/optimed-count.png" alt="OptiMed count" width="400" style="float:right;" />

<div style="clear:both;"></div>

Au total, cela représente environ __28 millions de patients__ et __77 millions de résultats d'examens médicaux__.
C'était colossale !


## Ce que cela révèle

En tant que développeur web "à l'ancienne" devenu expert en sécurité, je comprends aisément l'origine du problème :
- Absence de validation des données en entrée
- Absence de séparation des privilèges
- Des années de mauvaises pratiques accumulées
- Absence de revue de code et de tests de sécurité
- Un nombre incalculable de développeurs dépourvus de connaissances en matière de sécurité

À ce stade, on peut affirmer que l'éditeur du logiciel ne se sent pas concerné, car __aucun effort n'a été consacré à la sécurité__.


## Et ensuite ?

En 2022, j'ai signalé la situation à l'[ANSSI](https://cyber.gouv.fr/) - l'agence nationale française de la sécurité des systèmes d'information - en respectant les principes de divulgation responsable.

Je n'ai accédé à aucune donnée personnelle, et je n'ai ni modifié, ni supprimé, ni altéré le moindre élément du système. 
Mon unique objectif était d'éviter une catastrophe.

En 2025, je constate que de nombreux centres médicaux ont changé de logiciel. 
Pour ceux qui continuent de l'utiliser, je reste à la disposition des autorités et de l'éditeur pour aider à résoudre ce problème.


## Conclusion

La sécurité dans les applications médicales ne saurait être facultative.
Elle doit être intégrée dès le premier jour, ancrée dans les pratiques de développement et faire l'objet d'audits réguliers.

Dans un monde où les données de santé constituent une cible critique à haute valeur, _OptiMed_ représente l'exemple type de ce qu'il ne faut pas faire.

