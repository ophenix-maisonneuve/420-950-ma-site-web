---
layout: default
title: whatweb
parent: Étapes d'une attaque
nav_order: 2
---

# whatweb  

L’outil **whatweb** est un outil de reconnaissance utilisé pour identifier les technologies utilisées par une application web. Contrairement à un scan réseau classique, qui permet de voir quels services sont exposés, whatweb cherche à comprendre **comment l’application est construite**.

Une application web n’est jamais un bloc unique. Elle repose sur un ensemble de composants :

- serveur web  
- frameworks  
- bibliothèques  
- technologies côté client
- etc.

Identifier ces éléments permet de mieux comprendre le comportement du système, mais aussi les faiblesses potentielles introduites par ces composants.

---

## Objectif  

L’objectif principal de whatweb est d’identifier les **technologies utilisées par une application web**. Cela inclut :

- le serveur web (Apache, Nginx, etc. )  
- le langage ou framework (Node.js, PHP, etc.)  
- les bibliothèques JavaScript 
- les CMS ou plateformes
- certains éléments de configuration

{: .highlight}
> Ces informations permettent de mieux orienter une analyse.

---

## Quand l’utiliser ?  

whatweb est utilisé très tôt dans une analyse. Après avoir identifié un service web à l'aide d'un outil comme nmap, on cherche à comprendre les composantes utilisées pour construire ce service ou cette application web.

Dans le cycle d’une attaque, whatweb correspond à la phase **Observer le système**.

---

## Fonctionnement  

whatweb envoie une requête HTTP à la cible et analyse la réponse. Il examine différents éléments, tels que :

- les en-têtes HTTP
- le contenu HTML
- les scripts
- les signatures connues

Il compare ensuite ces éléments à une base de signatures de technologies connues afin de les identifier.

---

## Structure de la commande whatweb  

### Syntaxe générale

```text
whatweb [OPTIONS] <CIBLE>
```

### Synopsis

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

### Lecture de la structure  

La commande est relativement simple :

- définir une **cible web**  
- ajouter éventuellement des options pour plus de précision  


**Exemple 1** :

```bash
whatweb http://192.168.1.10:3000
```

**Exemple 2 (plus détaillé)** :

```bash
whatweb -a 3 http://192.168.1.10:3000
```

**Lecture** :

- `-a 3` → analyse plus agressive  
- cible → application web  

{: .highlight}
> Plus le niveau d'agressivité est élevé, plus l’analyse est approfondie, mais elle peut ainsi devenir plus lente et plus détectable.

---

## Commandes utiles  

### Scan simple

```bash
whatweb http://<IP>:3000
```

### Scan détaillé

```bash
whatweb -a 3 http://<IP>:3000
```

### Mode verbeux

```bash
whatweb -v http://<IP>
```

---

## Limites  

Comme tous les outils, whatweb a ses forces et ses faiblesses. Ainsi, il présente les limites suivantes :

- **Basé sur des signatures** : whatweb identifie uniquement ce qu’il connaît. Une technologie non reconnue ne sera pas détectée.
- **Informations partielles** : Les résultats peuvent être incomplets, car certaines technologies ne sont pas visibles directement.
- **Faux positifs** : Dans certains cas, une signature peut être mal intreprétée ou une technologie peut être détectée par erreur.  

{: .highlight}
> Comme plusieurs signatures ou technologies peuvent se ressembler, whatweb donne généralement des indices plutôt que des certitudes.

---

## Comment se protéger

whatweb démontre que les technologies exposées donnent des informations aux attaquants. Pour limiter cela, on peut :

- masquer les en-têtes sensibles  
- éviter de divulguer la version des services  
- utiliser des configurations sécurisées  
- limiter les informations dans le HTML  

{: .highlight}
> Réduire l’information visible réduit la surface d’analyse accessible aux *hackers*.

---

## Liens utiles
- Documentation officielle : [https://github.com/urbanadventurer/WhatWeb](https://github.com/urbanadventurer/WhatWeb)
- Documentation Kali : [https://www.kali.org/tools/whatweb/](https://www.kali.org/tools/whatweb/)
