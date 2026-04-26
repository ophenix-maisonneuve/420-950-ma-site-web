---
layout: default
title: DAST - Analyse dynamique de sécurité
parent: Analyse dynamique
nav_order: 1
has_toc: false
---


# DAST - Analyse dynamique de sécurité (*Dynamic Application Security Testing*)

Le ***Dynamic Application Security Testing* (DAST)** désigne un ensemble de techniques et d’outils visant à évaluer la sécurité d’une application **lorsqu’elle est en cours d’exécution**, en interagissant avec elle exactement comme le ferait un utilisateur ou un attaquant externe. Contrairement aux approches basées sur l’analyse du code source ou des binaires, le DAST observe uniquement ce que l’application *expose réellement* à travers ses interfaces publiques.

Concrètement, un outil DAST communique avec l’application via les protocoles qu’elle supporte — principalement HTTP/HTTPS pour les applications web et les API REST, mais également WebSocket, GraphQL ou SOAP. Le DAST se place ainsi dans une posture **boîte noire**, ou parfois **boîte grise** lorsqu’un minimum de contexte est fourni (comptes utilisateurs, rôles, documentation API).

Cette approche permet de répondre à une question fondamentale en sécurité applicative :  
> *Que peut réellement exploiter un attaquant contre une application déployée et opérationnelle ?*

---

## Historique

Les origines du DAST remontent aux premiers scanners de vulnérabilités web apparus à la fin des années 1990, époque où les applications web étaient encore relativement simples, mais déjà exposées à Internet. Ces premiers outils se concentraient surtout sur la détection de scripts CGI vulnérables et de configurations dangereuses.

Au début des années 2000, avec la montée en complexité des applications dynamiques (PHP, ASP, Java EE), les limites des simples scanners réseau deviennent évidentes. Des outils spécialisés apparaissent pour analyser le comportement applicatif, comme **Nikto** ou **Paros Proxy**, qui introduisent la notion d’interception et de modification du trafic HTTP.

Un tournant majeur survient avec la création des projets **OWASP** et la standardisation des vulnérabilités web (OWASP Top 10). Dans ce contexte, **OWASP ZAP** voit le jour à la fin des années 2000 comme fork communautaire de Paros, avec un objectif clair : démocratiser les tests DAST et les rendre accessibles aux développeurs.

Depuis les années 2010, le DAST s’intègre progressivement dans les pratiques **DevSecOps**, avec :
- l’automatisation des scans,
- l’analyse des API,
- l’exécution en CI/CD,
- l’utilisation de conteneurs et d’images Docker.

Aujourd’hui, le DAST n’est plus un outil ponctuel de pentest, mais un **composant continu du cycle de vie logiciel**.

---

## Objectifs

L’objectif principal du DAST est d’identifier des **vulnérabilités exploitables dans des conditions réelles**. Là où d’autres approches tentent de prédire des failles potentielles, le DAST cherche à démontrer des comportements dangereux observables à l’exécution.

En simulant les actions d’un attaquant, le DAST permet notamment :
- de vérifier l’efficacité des mécanismes de validation d’entrées,
- de tester les contrôles d’accès effectifs (et non ceux supposés),
- d’évaluer la robustesse de l’authentification et de la gestion de session,
- d’observer les fuites d’information involontaires.

Un élément clé du DAST est son **orientation résultat** : lorsqu’une vulnérabilité est détectée, elle est généralement accompagnée d’une preuve concrète (requête, réponse, payload), ce qui réduit considérablement le nombre de faux positifs par rapport à d’autres approches.

---

## Types de vulnérabilités détectées par le DAST

Le DAST est particulièrement efficace pour détecter des vulnérabilités liées au **traitement dynamique des entrées utilisateur**. Ces vulnérabilités n’existent souvent qu’à l’exécution, lorsque des données externes traversent les différentes couches applicatives.

Parmi les vulnérabilités les plus couramment détectées, on retrouve notamment les injections (SQL, NoSQL, commandes système), les failles de type **Cross‑Site Scripting (XSS)**, les problèmes de contrôle d’accès, ainsi que les mauvaises configurations HTTP.

Le DAST excelle également dans la détection de problèmes dits « transversaux », tels que :
- l’absence de headers de sécurité,
- l’usage incorrect des cookies de session,
- la divulgation d’informations sensibles dans les réponses.

Ces failles sont souvent invisibles dans le code source seul, car elles dépendent du comportement réel de l’application et de son environnement d’exécution.

---

## Déroulement typique d’un test DAST

Un test DAST suit généralement une série d’étapes bien définies, qui visent à maximiser la couverture tout en contrôlant les risques.

### 1. Définition du périmètre (Scope)

Avant toute action, le périmètre du test doit être précisément défini. Cette étape est cruciale d’un point de vue légal et opérationnel. Elle détermine quelles URL, quels services et quels environnements sont autorisés à être testés, ainsi que les types de tests permis (passifs vs actifs).

Sans périmètre clair, un test DAST peut facilement provoquer des incidents ou sortir du cadre contractuel.


### 2. Découverte de la surface d’attaque

La découverte de la surface d’attaque consiste à identifier toutes les ressources accessibles : pages web, routes sur les API, paramètres, formulaires, etc. Cette phase repose généralement sur des techniques de découverte comme le *crawling* ou l'utilisation d'un *spider* automatisé, parfois complétées par une exploration manuelle.

La qualité de cette étape détermine directement l’efficacité du DAST : une fonctionnalité non découverte est une fonctionnalité non testée.


### 3. Analyse / attaque

**Analyse passive**

L’analyse passive observe les requêtes et réponses sans modifier le trafic. Elle permet de détecter rapidement des failles de configuration ou de conception, tout en étant généralement sans danger pour les environnements sensibles, y compris la production.

Cette phase est souvent utilisée comme première ligne de défense dans un pipeline CI/CD.


**Analyse active**

L’analyse active consiste à injecter des contenus malveillants dans les paramètres identifiés lors de la découverte. Cette étape est plus intrusive, mais également plus puissante, car elle permet de détecter des vulnérabilités exploitables comme les injections ou les failles logiques.

Elle doit être exécutée avec prudence, idéalement dans des environnements de test ou de pré‑production dédiés à cet effet.

{: .warning}
> Cette étape doit absolument être bien planifiée et coordonnée, car elle a un réel potentiel destructeur. Ainsi, lancer une analyse active sur un environnement critique (ex.: un environnement nécessaire aux développeurs, un serveur de tests interne ou pire, un serveur de prodution...) peut avoir des conséquences catastrophiques, car l'environnement peut en ressortir passablement démoli.


### 4. Traitement des réponses

Suite à l'analyse, l'outil DAST traite chacune des réponses de l'application afin d'y découvrir des comportements vulnérables : erreurs inhabituelles, divulgation de données ou tout autre symptôme que l'une des techniques utilisées lors de l'analyse a connu un certain succès. Pour chaque vulnérabilité potentielle, l'outil produira généralement une alerte qui devra ensuite être révisée.


### 5. Rapport

La dernière étape consiste à analyser les alertes produites, éliminer les faux positifs, confirmer les vulnérabilités et en évaluer l’impact réel. Cette phase nécessite presque toujours une validation humaine.

Un bon DAST ne se limite pas à produire des alertes, mais à fournir des **éléments exploitables** et, idéalement, des **pistes de correction**. Ces éléments seront consignés dans un rapport qui servira ensuite d'outil aux équipes de développement afin de corriger les vulnérabilités confirmées. 

---

## Limites du DAST

Malgré sa puissance, le DAST présente des limites importantes. Il ne peut analyser que ce qui est accessible à l’exécution, et sa couverture dépend fortement de la qualité de la phase de découverte.

Les applications très dynamiques (SPA, logique métier complexe, flows multi‑étapes) nécessitent une configuration avancée, voire une intervention manuelle, pour être correctement testées.

Ces limites expliquent pourquoi le DAST ne doit jamais être utilisé seul.

---

## Le DAST en DevSecOps

Dans une démarche DevSecOps moderne, le DAST est intégré de manière continue :
- scans passifs automatiques à chaque build,
- scans plus agressifs sur les branches de release,
- tests spécifiques sur les API exposées.

L’objectif n’est plus de trouver toutes les failles, mais de **réduire continuellement la surface de risque**.

---

## Conclusion

Le DAST est un outil essentiel pour comprendre la sécurité réelle d’une application déployée. Il permet de confronter les hypothèses de conception à la réalité du terrain, et d’identifier des vulnérabilités exploitables dans des conditions proches de celles d’un attaquant.

Utilisé intelligemment et combiné à d’autres approches, le DAST constitue l’un des piliers de la sécurité applicative moderne.
