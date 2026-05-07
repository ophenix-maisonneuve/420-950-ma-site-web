---
layout: default  
title: gobuster  
parent: Étapes d'une attaque  
nav_order: 4
---

# gobuster  

L’outil **gobuster** est un outil de découverte utilisé pour identifier des ressources cachées sur un serveur web. Contrairement à ce qu’un utilisateur normal peut voir dans une interface, une application contient souvent de nombreuses routes, fichiers ou répertoires non exposés directement.

Un attaquant ne se limite jamais à ce qui est visible. Il cherche au contraire à découvrir ce qui est dissimulé, car on aura généralement tendance à croire (à tort) que ce contenu est mieux protégé.

---

## Objectif  

L’objectif principal de gobuster est d’identifier des **chemins cachés** sur un serveur web. Cela inclut notamment :

- des répertoires non visibles (`/admin`, `/backup`, `/config`)  
- des endpoints d’API (`/api`, `/rest`, `/v1`)  
- des fichiers accessibles (`.git`, `.env`, `.bak`)  

Ces ressources ne sont pas directement accessibles via l’interface utilisateur, mais restent accessibles via des requêtes HTTP.

{: .highlight}
> Un élément non visible n’est pas nécessairement un élément protégé.

---

## Quand l’utiliser ?  

gobuster est utilisé après les premières phases d’analyse.

Une fois que...

- la cible est identifiée (ex.: nmap)
- les technologies sont comprises (ex.: whatweb)
- le comportement est compris (ex.: Burp Suite et/ou navigateur)  

... on cherche à élargir la surface d’attaque. Dans le cycle d’une attaque, gobuster correspond à la phase **Explorer le système**. Sans cette étape, certaines fonctionnalités critiques peuvent rester invisibles.

---

## Fonctionnement  

gobuster fonctionne selon un principe simple : il teste automatiquement une liste de chemins potentiels sur une cible. Ces chemins proviennent d’un fichier appelé **wordlist**. Différents **wordlist**, de tailles variables, peuvent être utilisés avec gobuster selon le degré d'agressivité de l'attaque désiré.

---

## Structure de la commande gobuster  

Comme pour les autres outils en ligne de commande, gobuster utilise une structure claire composée d’options et de paramètres.

### Syntaxe générale

```text
gobuster dir -u <URL> -w <WORDLIST> [OPTIONS]
```

### Synopsis

```text
gobuster
│
├─ MODE
│   └─ dir         → découverte de répertoires web
│
├─ PARAMÈTRES OBLIGATOIRES
│   ├─ -u          → URL cible
│   └─ -w          → wordlist utilisée
│
├─ OPTIONS
│   ├─ -x          → extensions de fichiers
│   ├─ -t          → threads
│   ├─ -s          → filtrer par code HTTP
│   ├─ --exclude-length → ignorer taille
│   └─ --wildcard  → ignorer réponses identiques
│
└─ CIBLE
    └─ URL
```

### Lecture de la structure  

Une commande gobuster suit toujours une logique simple :

- définir un mode
- définir une cible
- fournir une wordlist
- ajouter des options

---

## Liens utiles
- Documentation officielle : [https://github.com/OJ/gobuster](https://github.com/OJ/gobuster)
- Documentation Kali : [https://www.kali.org/tools/gobuster/](https://www.kali.org/tools/gobuster/)
