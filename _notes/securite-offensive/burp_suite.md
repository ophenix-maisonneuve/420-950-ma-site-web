---
layout: default  
title: burp suite  
parent: Étapes d'une attaque  
nav_order: 5
published: false
---

# Burp Suite  

**Burp Suite** est un outil d’analyse web qui permet d’intercepter, observer et modifier les communications entre un navigateur et un serveur web. Contrairement aux autres outils présentés jusqu’à présent, Burp ne cherche pas à scanner ou à automatiser : il permet de comprendre ce qui se passe réellement.

Dans une application web, tout repose sur des échanges de requêtes et de réponses HTTP. Ces échanges sont normalement invisibles pour l’utilisateur.

Burp Suite rend ces échanges visibles.

---

👉 C’est un changement fondamental :

> *On ne se contente plus d’utiliser une application.*  
> *On observe comment elle fonctionne réellement.*

---

## Objectif  

L’objectif principal de Burp Suite est de permettre à l’analyste de :

- intercepter les requêtes envoyées au serveur  
- modifier les données avant qu’elles soient envoyées  
- analyser les réponses du serveur  
- tester le comportement de l’application  

---

👉 Burp ne trouve pas les vulnérabilités automatiquement  
👉 Il permet de les comprendre et de les manipuler  

---

## Quand l’utiliser ?  

Burp Suite est utilisé très tôt dans l’analyse web.

Après avoir :

- identifié la cible (nmap)  
- compris les technologies (whatweb)  

👉 on cherche à analyser les requêtes  

---

Dans le cycle d’une attaque, Burp correspond à la phase :

> **Interagir avec le système**

---

👉 C’est l’outil qui permet de passer de l’observation à l’action

---

## Fonctionnement  

Burp agit comme un **proxy intermédiaire** entre le navigateur et le serveur.

---

### Schéma simplifié

```text
Navigateur → Burp → Serveur
```

---

Concrètement :

1. le navigateur envoie une requête  
2. Burp l’intercepte  
3. l’analyste peut la modifier  
4. la requête est envoyée au serveur  
5. la réponse passe aussi par Burp  

---

👉 Cela permet un contrôle complet sur les échanges

---

## Composant principal : Intercept  

Le module le plus utilisé dans Burp est **Intercept**.

---

### Fonction  

- intercepter les requêtes  
- les modifier avant envoi  

---

### Exemple de requête interceptée

```http
POST /rest/user/login
Content-Type: application/json

{
  "email": "test@test.com",
  "password": "test"
}
```

---

👉 Cette requête peut être modifiée avant envoi

---

## Manipulation des requêtes  

C’est ici que Burp devient extrêmement puissant.

---

### Exemple : test d’injection

Modifier :

```text
password = test
```

par :

```text
password = ' OR 1=1--
```

---

👉 Puis observer la réponse

---

### Exemple : modification de structure  

- retirer un champ  
- renommer un paramètre  
- ajouter une valeur supplémentaire  

---

👉 Objectif : tester la validation du serveur  

---

## Lecture des réponses  

Burp permet également d’analyser les réponses du serveur :

- code HTTP  
- contenu retourné  
- messages d’erreur  
- données  

---

👉 Les réponses permettent de comprendre :

- ce qui est accepté  
- ce qui est rejeté  
- ce qui est vulnérable  

---

## Modules importants  

### Intercept  
- intercepter et modifier  

### Repeater  
- rejouer une requête  

### Intruder  
- automatiser des tests  

---

## Limites  

Burp Suite présente une particularité importante :

👉 il ne fait rien tout seul  

---

Contrairement à sqlmap ou hydra :

- aucune automatisation par défaut  
- aucune détection automatique  

---

👉 C’est un outil d’analyse, pas un scanner  

---

## Défense associée  

Burp met en évidence un principe crucial :

> **le serveur ne doit jamais faire confiance au client**

---

Pour se protéger :

- validation côté serveur  
- filtrage des entrées  
- désinfection des données  
- contrôle strict des paramètres  

---

## Exemple (Juice Shop)  

Burp est utilisé pour intercepter :

```http
POST /rest/user/login
```

---

Puis :

- modifier les champs  
- tester des injections  
- observer les réponses  

---

👉 On découvre des comportements inattendus  

---

## Conclusion  

Burp Suite est l’outil central de l’analyse web, car il permet de comprendre en profondeur le fonctionnement d’une application.

---

> *Un outil lance des attaques.*  
> *Burp permet de comprendre pourquoi elles fonctionnent.*

## Liens utiles
- Documentation officielle : [https://portswigger.net/burp/documentation](https://portswigger.net/burp/documentation)
