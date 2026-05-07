---
layout: default  
title: sqlmap  
parent: Étapes d'une attaque  
nav_order: 3
published: false
---

# sqlmap  

L’outil **sqlmap** est un outil d’automatisation spécialisé dans la détection et l’exploitation des vulnérabilités d’injection SQL. Là où une injection SQL peut être testée manuellement, sqlmap permet de systématiser, accélérer et étendre ces tests à grande échelle.

Dans une démarche offensive, sqlmap ne représente pas le point de départ de l’analyse. Il intervient plutôt lorsque l’on suspecte qu’un système est vulnérable et que l’on souhaite confirmer et exploiter cette hypothèse.

---

sqlmap permet ainsi de répondre à une question fondamentale :

> *Peut-on exploiter automatiquement une injection SQL et aller plus loin que ce qui est possible manuellement ?*

---

## Objectif  

L’objectif principal de sqlmap est de :

- détecter automatiquement des injections SQL  
- confirmer leur exploitabilité  
- extraire des informations de la base de données  
- automatiser l’exploitation  

---

Concrètement, sqlmap peut permettre de :

- découvrir les bases de données présentes  
- lister les tables  
- extraire des données sensibles  
- contourner certaines protections  

---

👉 sqlmap transforme une vulnérabilité potentielle en exploitation concrète.

---

## Quand l’utiliser ?  

sqlmap intervient après une phase d’analyse.

Il est utilisé lorsque :

- une requête suspecte est identifiée  
- un comportement anormal est observé  
- une injection SQL est probable  

---

Dans le cycle d’une attaque, sqlmap correspond à la phase :

> **Automatiser**

---

👉 Il ne remplace pas la réflexion.  
👉 Il amplifie ce qui a été découvert.

---

## Fonctionnement  

sqlmap fonctionne à partir d’une requête HTTP.

Cette requête contient généralement des paramètres qui peuvent être manipulés (GET ou POST). sqlmap injecte alors une série de payloads et analyse les réponses du serveur afin de détecter des comportements anormaux.

---

Contrairement à un test manuel, sqlmap :

- teste plusieurs types d’injections  
- ajuste automatiquement ses requêtes  
- recherche des indices subtils dans les réponses  

---

👉 Il explore systématiquement des possibilités qu’un humain testerait difficilement une par une.

---

## Structure de la commande sqlmap  

Comme les autres outils en ligne de commande, sqlmap repose sur une structure claire composée d’options et de paramètres.

---

### Syntaxe générale

```text
sqlmap [OPTIONS]
```

---

### Décomposition de la commande

```text
sqlmap
│
├─ SOURCE
│   ├─ -u            → URL cible avec paramètres
│   └─ -r            → fichier de requête (Burp)
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

---

## Lecture de la structure  

La commande sqlmap se lit comme suit :

- définir la **source** (URL ou requête)  
- ajouter des **options** pour préciser l’action  
- laisser l’outil analyser et exploiter  

---

📌 Exemple :

```bash
sqlmap -r request.txt --dbs
```

---

Lecture :

- `-r request.txt` → utiliser une requête interceptée  
- `--dbs` → lister les bases de données  

---

👉 sqlmap automatise alors toute l’analyse.

---

## Commandes essentielles  

---

### Détection de base

```bash
sqlmap -u "http://site/?id=1"
```

---

### Avec requête Burp

```bash
sqlmap -r request.txt
```

---

### Lister les bases de données

```bash
sqlmap -r request.txt --dbs
```

---

### Lister les tables

```bash
sqlmap -r request.txt --tables
```

---

### Extraire les données

```bash
sqlmap -r request.txt --dump
```

---

## Limites  

Malgré sa puissance, sqlmap présente plusieurs limites importantes.

---

Tout d’abord, il dépend fortement de la qualité de la requête fournie.

👉 Si la requête n’est pas exploitable, sqlmap ne pourra rien faire.

---

Ensuite, certaines protections peuvent limiter son efficacité :

- WAF (Web Application Firewall)  
- validation stricte des entrées  
- filtrage des requêtes  

---

Enfin, sqlmap peut produire :

- des faux positifs  
- des résultats difficiles à interpréter  

---

> *sqlmap automatise l’exploitation.*  
> *Mais il ne remplace pas la compréhension.*

---

## Défense associée  

sqlmap met en évidence l’un des problèmes les plus critiques en sécurité applicative :

> **la confiance excessive dans les entrées utilisateur**

---

Pour se protéger :

- utiliser des requêtes préparées  
- valider et filtrer les entrées  
- éviter l’interprétation directe des données utilisateur  
- utiliser des ORM sécurisés  

---

👉 Une injection SQL correctement traitée rend sqlmap inutile.

---

## Exemple (Juice Shop)  

Dans le laboratoire, sqlmap peut être utilisé sur une requête interceptée avec Burp.

---

### Étape 1  

Capturer une requête dans Burp et l’enregistrer dans un fichier :

```text
request.txt
```

---

### Étape 2  

Lancer sqlmap :

```bash
sqlmap -r request.txt --dbs
```

---

### Résultat  

sqlmap identifie :

- une injection SQL  
- les bases de données disponibles  

---

👉 Cela confirme que la vulnérabilité est exploitable.

---

## Conclusion  

sqlmap est l’un des outils les plus puissants en sécurité offensive, car il permet de transformer une simple hypothèse en exploitation complète.

Cependant, il doit être utilisé avec discernement.

---

> *Ce n’est pas sqlmap qui découvre la faille.*  
> *C’est l’analyste qui la soupçonne.*  

---

## Liens utiles
- Site officiel : [https://sqlmap.org/](https://sqlmap.org/)
- Documentation : [https://github.com/sqlmapproject/sqlmap/wiki](https://github.com/sqlmapproject/sqlmap/wiki)
