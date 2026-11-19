---
date: 2022-09-21
slug: "subdomain-takeover-en-pagaille-chez-EDF"
title: "Subdomain Takeover en pagaille chez EDF"
images: [/images/edf-subto.png]
tags:
- tools
- subdomain takeover
---
En raison du Covid, le premier confinement en France a eu lieu en mars 2020.
Durant cette période, j'ai écrit un script Python pour détecter les détournements de sous-domaines ("Subdomain Takeover").
Après avoir obtenu plusieurs succès avec cet outil, l'un d'entre eux s'est avéré particulièrement remarquable.
<!--more-->

<span style="font-size:1.5em;">*L'histoire sur la manière dont j'ai pris le contrôle de +450 sous-domaines du distributeur national d'électricité en France [EDF](https://www.edf.fr/).*</span>

<a href="/images/edf-subto.png" target="_blank"><img src="/images/edf-subto.png" alt="edf subdomain takeover" height="450" /></a>

<br>
<hr>
Je ne vais pas expliquer en quoi consiste le détournement de sous-domaine ; consultez donc les articles suivants si vous souhaitez en savoir plus :

[Guide OWASP](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/02-Configuration_and_Deployment_Management_Testing/10-Test_for_Subdomain_Takeover)  
[Guide Hackerone](https://www.hackerone.com/application-security/guide-subdomain-takeovers)  
[Les bases selon 0xpatrik](https://0xpatrik.com/subdomain-takeover-basics/)  
<hr>

<br>
Comme nous le savons tous, il existe plusieurs manières de prendre le contrôle d'un sous-domaine, mais la méthode la plus simple consiste probablement à acheter un nom de domaine expiré qui est disponible à l'achat.
<br><br><br>

<div style="float:left;width:420px">
    <b>Découverte :</b><br>
    À l'aide de mon outil <a href="https://github.com/gwen001/dnspy">dnspy</a>, j'ai découvert que <code>nucleaire.edf.fr</code> 
    était un alias de <code>edf-linkbynet.com</code>. 
    Une rapide recherche sur Gandi m'a permis de constater que ce dernier était disponible à l'achat pour environ 12 €/an. 
    Sans savoir ce qui allait se passer ensuite, j'ai acheté le nom de domaine et l'ai configuré sur mon serveur pour afficher une page de démo.
</div>
<img hspace="10" src="/images/edf-subto-nucleaire.png" alt="Prise de contrôle de sous-domaine EDF (nucleaire.edf.fr)" height="300" />
<div style="clear:both;"></div>

<br>
<div style="float:left;width:510px">
    <b>Afflux de trafic !</b><br>
    Quelques jours plus tard, j'ai consulté les journaux de mon serveur pour voir si quelqu'un avait effectué des requêtes vers le sous-domaine détourné. 
    Quelle ne fut pas ma surprise de constater une multitude de requêtes ciblant divers sous-domaines dont j'ignorais absolument tout.
</div>
<a href="/images/edf-subto-logs.png" target="_blank"><img hspace="10" src="/images/edf-subto-logs.png" alt="Journaux relatifs au détournement de sous-domaine" height="300" /></a>
<div style="clear:both;"></div>

<br><br>
À l'époque, EDF disposait d'un programme privé de Bug Bounty sur la plateforme française YesWeHack.
N'y ayant pas été invité, j'ai utilisé le formulaire de contact dédié sur leur site web.
Six semaines plus tard, et après plusieurs tentatives pour les joindre, le problème a été corrigé, comme par magie et en toute discrétion.

Au total, plus de 450 sous-domaines étaient redirigés vers mon serveur.
Entre les mains d'une personne malveillante, les conséquences auraient pu être désastreuses : ingénierie sociale, hameçonnage, manipulation de cookies, faux systèmes de paiement, atteinte à la réputation de l'entreprise...
(Un simple *"merci"* aurait été apprécié).

Quant à l'origine du problème, je suppose qu'il s'agit d'un défaut de communication entre les responsables techniques.
L'un des administrateurs a acheté le domaine <code>edf-linkbynet.FR</code>, tandis qu'un autre a configuré tous les enregistrements CNAME pour pointer vers <code>edf-linkbynet.COM</code>, tout simplement !


## The tool

L'outil, baptisé [dnspy](https://github.com/gwen001/dnspy), se compose de 3 modules :

1/ Le __collecteur__ tente de découvrir des sous-domaines en utilisant des outils externes tels que _subfinder, oneforall, github, amass,_ etc. ; des variantes sont ensuite générées à l'aide d'altdns ou de dnsgen.

2/ Le __resolveur__ effectue des requêtes DNS sur l'ensemble des sous-domaines collectés et générés, en utilisant _massdns_, afin de détecter des hôtes inactifs, des enregistrements CNAME, etc.

3/ L'__interpreteur__ analyse les résultats fournis par le resolveur et recherche des possibilités de rachats en s'appuyant sur un fichier d'empreintes.
L'outil s'inspire de _subjack_, tout en proposant des fonctionnalités supplémentaires, comme la prise en charge d'expressions régulières et une liste d'exclusions.

L'ensemble fonctionne sous forme de démon et peut être lancé de manière indépendante. Vous trouverez ci-dessous quelques exemples de résultats intéressants.

<img src="/images/dnspy-acef.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-bred.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-ce-picardie.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-bpce.png" alt="edf nuclaire subdomain takeover" />
<img src="/images/dnspy-jeunesse-gouv.png" alt="edf nuclaire subdomain takeover" height="200" />
<img src="/images/dnspy-hillary.png" alt="edf nuclaire subdomain takeover" height="200" />
