---
layout: default
title: sqlmap
parent: Étapes d'une attaque
nav_order: 5
---

# sqlmap  

L’outil **sqlmap** est un outil d’automatisation spécialisé dans la détection et l’exploitation des vulnérabilités d’injection SQL. Là où une injection SQL peut être testée manuellement, sqlmap permet de systématiser, accélérer et étendre ces tests à grande échelle.

Dans une démarche offensive, sqlmap ne représente pas le point de départ de l’analyse. Il intervient plutôt lorsque l’on suspecte qu’un système est vulnérable et que l’on souhaite confirmer et exploiter cette hypothèse.

sqlmap permet ainsi de vérifier si une injection SQL est possible et si cette dernière permet d'aller plus loin que ce qui était prévu par le système.

---

## Objectif  

L’objectif principal de sqlmap est de :

- détecter automatiquement des injections SQL  
- confirmer leur exploitabilité  
- extraire des informations de la base de données  
- automatiser l’exploitation  

Concrètement, sqlmap peut permettre de :

- découvrir les bases de données présentes  
- lister les tables  
- extraire des données sensibles  
- contourner certaines protections  

---

{: .highlight}
> sqlmap transforme une vulnérabilité potentielle en exploitation concrète.

---

## Quand l’utiliser ?  

sqlmap intervient après une phase d’analyse. Il est utilisé lorsque :

- une requête suspecte est identifiée
- un comportement anormal est observé  
- une injection SQL est probable  

Dans le cycle d’une attaque, sqlmap correspond à la phase **Automatiser**.

{: .highlight}
> sqlmap ne remplace pas l'analyse requise pour découvrir les requêtes potentiellement vulnérables; il amplifie ce qui a été découvert.

---

## Fonctionnement  

sqlmap fonctionne à partir d’une requête HTTP que l'on soupçonne vulnérable aux attaques par injection SQL.

Cette requête contient généralement des paramètres qui peuvent être manipulés (GET ou POST). sqlmap injecte alors une série de payloads et analyse les réponses du serveur afin de détecter des comportements anormaux. Contrairement à un test manuel, sqlmap :

- teste plusieurs types d’injections  
- ajuste automatiquement ses requêtes  
- recherche des indices subtils dans les réponses  

{: .highlight}
> sqlmap explore systématiquement des possibilités qu’un humain testerait difficilement une par une.

---

## Structure de la commande sqlmap  

Comme les autres outils en ligne de commande, sqlmap repose sur une structure claire composée d’options et de paramètres.

### Syntaxe générale

```text
sqlmap [OPTIONS]
```

### Synopsis

```text
sqlmap
│
├─ SOURCE
│   ├─ -u            → URL cible avec paramètres
│   └─ -r            → fichier de requête (ex.: extraite de Burp Suite)
│
├─ OPTIONS DE TEST
│   ├─ --dbs         → lister bases de données
│   ├─ --tables      → lister tables
│   ├─ --dump        → extraire données
│   ├─ --batch       → mode automatique
│   └─ --level       → profondeur des tests
│
└─ OPTIONS AVANCÉES
    ├─ --risk        → niveau de risque
    ├─ --threads     → parallélisation
    └─ --method      → GET/POST
```

### Lecture de la structure  

La commande sqlmap se lit comme suit :

- définir la source (URL ou requête)  
- ajouter des options pour préciser l’action  
- laisser l’outil analyser et exploiter  

**Exemple** :

```bash
sqlmap -u "http://mon-serveur:45000/rest/api" --dbs
```

- `-u` → utiliser l'URL spécifiée  
- `--dbs` → lister les bases de données  

{: .highlight}
> sqlmap automatise alors toute l’analyse.

---

## Commandes utiles

### Détection automatique de base

```bash
sqlmap -u "http://site/?id=1"
```

### Avec requête sauvegardée

```bash
sqlmap -r request.txt
```


### Lister les bases de données

```bash
sqlmap -u "http://mon-serveur:45000/rest/api" --dbs
```


### Lister les tables

```bash
sqlmap -r request.txt --tables
```

### Extraire les données

```bash
sqlmap -u "http://mon-serveur:45000/rest/api" --dump
```

---

## Limites  

Malgré sa puissance, sqlmap présente plusieurs limites importantes. Tout d’abord, il dépend fortement de la qualité de la requête fournie : si la requête n’est pas exploitable, sqlmap ne pourra rien faire.

Ensuite, certaines protections peuvent limiter son efficacité :

- WAF (Web Application Firewall)
- validation stricte des entrées
- filtrage des requêtes

Enfin, sqlmap peut produire :

- des faux positifs  
- des résultats difficiles à interpréter  


{: .highlight}
> sqlmap automatise l’exploitation, mais il ne remplace pas la compréhension et l'analyse.

---

## Comment se protéger

sqlmap exploite l'un des problèmes les plus courants dans les applications web : la confiance excessive dans les entrées utilisateur. Pour se protéger, on utilise donc généralement les techniques suivantes :

- utiliser des requêtes préparées  
- valider et filtrer les entrées  
- éviter l’interprétation directe des données utilisateur  
- utiliser des ORM sécurisés  

{: .highlight}
> Une tentative d'injection SQL correctement traitée rend sqlmap inutile.

---

## Liens utiles
- Site officiel : [https://sqlmap.org/](https://sqlmap.org/)
- Documentation : [https://github.com/sqlmapproject/sqlmap/wiki](https://github.com/sqlmapproject/sqlmap/wiki)
