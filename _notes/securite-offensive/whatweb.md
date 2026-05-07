---
layout: default  
title: whatweb  
parent: Étapes d'une attaque  
nav_order: 2
published: false  
---

# whatweb  

L’outil **whatweb** est un outil de reconnaissance utilisé pour identifier les technologies utilisées par une application web. Contrairement à un scan réseau classique, qui permet de voir quels services sont exposés, whatweb cherche à comprendre **comment l’application est construite**.

Une application web n’est jamais un bloc unique.

Elle repose sur un ensemble de composants :

- serveur web  
- frameworks  
- bibliothèques  
- technologies côté client  

---

👉 Identifier ces éléments permet de mieux comprendre le comportement du système, mais aussi ses faiblesses potentielles.

---

Dans ce contexte, whatweb permet de répondre à une question clé :

> *Sur quoi repose réellement cette application ?*

---

## Objectif  

L’objectif principal de whatweb est d’identifier les **technologies utilisées par une application web**.

---

Cela inclut notamment :

- le serveur web (Apache, Nginx…)  
- le langage ou framework (Node.js, PHP…)  
- les bibliothèques JavaScript  
- les CMS ou plateformes  
- certains éléments de configuration  

---

👉 Ces informations permettent de mieux orienter une analyse.

---

## Quand l’utiliser ?  

whatweb est utilisé très tôt dans une analyse.

Après avoir :

- identifié un service web (avec nmap)  

👉 on cherche à comprendre **ce qu’il y a derrière**

---

Dans le cycle d’une attaque, whatweb correspond à la phase :

> **Observer le système**

---

## Fonctionnement  

whatweb envoie une requête HTTP à la cible et analyse la réponse.

Il examine différents éléments :

- les en-têtes HTTP  
- le contenu HTML  
- les scripts  
- les signatures connues  

---

👉 Il compare ensuite ces éléments à une base de signatures de technologies connues.

---

## Structure de la commande whatweb  

### Syntaxe générale

```text
whatweb [OPTIONS] <CIBLE>
```

---

### Décomposition

```text
whatweb
│
├─ OPTIONS
│   ├─ -v        → mode verbeux
│   ├─ -a        → niveau d’agressivité
│   ├─ --log     → enregistrer résultats
│   └─ -t        → threads
│
└─ CIBLE
    ├─ URL
    ├─ IP
    └─ site distant
```

---

## Lecture de la structure  

La commande est simple :

- définir une **cible web**  
- ajouter éventuellement des options pour plus de précision  

---

📌 Exemple simple :

```bash
whatweb http://192.168.1.10:3000
```

---

📌 Exemple plus détaillé :

```bash
whatweb -a 3 http://192.168.1.10:3000
```

---

Lecture :

- `-a 3` → analyse plus agressive  
- cible → application web  

---

👉 Plus le niveau est élevé, plus l’analyse est approfondie…  
👉 mais elle peut devenir plus lente et plus détectable  

---

## Commandes essentielles  

---

### Scan simple

```bash
whatweb http://<IP>:3000
```

---

### Scan détaillé

```bash
whatweb -a 3 http://<IP>:3000
```

---

### Mode verbeux

```bash
whatweb -v http://<IP>
```

---

## Limites  

whatweb présente plusieurs limites importantes.

---

### 1. Basé sur des signatures  

whatweb identifie uniquement ce qu’il connaît.

👉 Une technologie non reconnue ne sera pas détectée  

---

### 2. Informations partielles  

Les résultats peuvent être incomplets.

👉 certaines technologies ne sont pas visibles directement  

---

### 3. Faux positifs  

Dans certains cas :

- une signature peut être mal interprétée  
- une technologie peut être détectée par erreur  

---

> *whatweb donne des indices.*  
> *Il ne donne pas des certitudes.*

---

## Défense associée  

whatweb met en évidence un principe important :

> **les technologies exposées donnent des informations aux attaquants**

---

Pour limiter cela :

- masquer les en-têtes sensibles  
- éviter de divulguer la version des services  
- utiliser des configurations sécurisées  
- limiter les informations dans le HTML  

---

👉 Réduire l’information visible réduit la surface d’analyse.

---

## Conclusion  

whatweb est un outil simple, mais extrêmement utile pour contextualiser une analyse web. Il permet de transformer une application inconnue en un ensemble de technologies identifiables.

---

> *Un attaquant n’attaque pas un système abstrait.*  
> *Il attaque une technologie concrète.*  

---

## Liens utiles
- Documentation officielle : [https://github.com/urbanadventurer/WhatWeb](https://github.com/urbanadventurer/WhatWeb)
- Documentation Kali : [https://www.kali.org/tools/whatweb/](https://www.kali.org/tools/whatweb/)
