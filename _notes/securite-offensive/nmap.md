---
layout: default  
title: nmap  
parent: Étapes d'une attaque  
nav_order: 1
published: false
---

# nmap  

L’outil **nmap** (Network Mapper) est l’un des outils les plus fondamentaux en sécurité offensive. Il est utilisé pour découvrir les systèmes présents sur un réseau, identifier les services qu’ils exposent et obtenir des informations techniques sur leur configuration.

Avant même d’interagir avec une application, un attaquant doit comprendre ce qui est accessible. Cette étape, souvent appelée reconnaissance, constitue la base de toute démarche offensive.

nmap permet précisément de répondre à cette première question essentielle :

> *Qu’est-ce qui est exposé sur ce système ?*

---

## Objectif  

L’objectif principal de nmap est d’identifier la **surface d’attaque réseau** d’une cible.

Concrètement, cela implique de déterminer :

- quels ports sont ouverts  
- quels services sont accessibles  
- quelles versions de logiciels sont utilisées  
- quels systèmes d’exploitation sont potentiellement en place  

{: .highlight}
> Chaque port ouvert représente une porte potentielle vers le système. Même si cette porte est protégée, sa simple existence fournit une information précieuse à un attaquant.

---

## Quand l’utiliser ?  

nmap est utilisé **en tout début d’une analyse**.

Avant de tester une application, d’intercepter du trafic ou de rechercher des vulnérabilités, il est essentiel de savoir :

- où se trouve la cible  
- quels services elle expose  
- sur quels ports ces services sont accessibles  

Dans le contexte d’une attaque, nmap correspond à la phase : **Observer le système**. Sans cette étape, un attaquant travaille à l’aveugle.

---

## Fonctionnement  

nmap fonctionne en envoyant différents types de paquets réseau vers une cible et en analysant les réponses obtenues.

En fonction de ces réponses, il est capable de déterminer si un port est :

- ouvert  
- fermé  
- filtré (par un pare-feu)  

---

## Structure de la commande nmap  

Comme pour la plupart des outils en ligne de commande en sécurité, nmap repose sur une structure composée de plusieurs éléments distincts, chacun ayant un rôle précis.

Une commande nmap peut être représentée comme suit :

```text
nmap [OPTIONS] <CIBLE>
```

---

### Décomposition de la commande

```text
nmap
│
├─ OPTIONS
│   ├─ -sV        → détection des versions de services
│   ├─ -p-        → scan de tous les ports
│   ├─ -F         → scan rapide (ports courants)
│   ├─ -O         → détection du système d’exploitation
│   ├─ -Pn        → ignorer le ping (considérer la cible active)
│   └─ -A         → scan agressif (combinaison de techniques)
│
└─ CIBLE
    ├─ adresse IP (ex: 192.168.1.10)
    ├─ nom de domaine (ex: example.com)
    └─ plage d’adresses (ex: 192.168.1.0/24)
```

---

## Lecture de la structure  

La commande se lit de gauche à droite :

- on commence par l’outil (`nmap`)  
- on ajoute des **options** pour préciser le type d’analyse  
- on termine par la **cible**  

---

📌 Exemple :

```bash
nmap -sV -p- 192.168.1.10
```

---

Lecture :

- `-sV` → identifier les versions  
- `-p-` → scanner tous les ports  
- `192.168.1.10` → cible  

---

Plus on ajoute d’options, plus l’analyse est complète, mais aussi plus longue et plus visible.

---

## Commandes essentielles  

### Scan de base

```bash
nmap <IP>
```

### Scan avec détection de version

```bash
nmap -sV <IP>
```

### Scan complet

```bash
nmap -p- -sV <IP>
```

---

## Limites  

nmap ne détecte pas directement les vulnérabilités. Il identifie des services, mais ne dit pas s’ils sont sécurisés.

Les résultats peuvent être influencés par :

- les pare-feu  
- les systèmes de détection  
- les filtres réseau  

---

> *nmap montre la surface d’attaque.*  
> *Il ne montre pas encore les vulnérabilités.*

---

## Défense associée  

L’utilisation de nmap met en évidence un principe fondamental :

> **Tout ce qui est exposé peut être analysé.**

---

Contre-mesures :

- fermer les ports inutiles  
- limiter les services exposés  
- filtrer avec un pare-feu    

---

## Conclusion  

nmap permet de transformer un système inconnu en un ensemble de points d’entrée concrets.

---

> *Un attaquant ne peut exploiter que ce qu’il voit.*  
> *nmap permet de voir.*

---

## Liens utiles
- Documentation officielle de nmap : [https://nmap.org/book/man.html](https://nmap.org/book/man.html)
- Livre officiel *Nmap Network Scanning* : [https://nmap.org/book/](https://nmap.org/book/)
- Introduction interactive à nmap : [https://tryhackme.com/module/nmap](https://tryhackme.com/module/nmap)
