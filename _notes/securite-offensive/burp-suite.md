---
layout: default
title: Burp Suite
parent: Étapes d'une attaque
nav_order: 3
---

# Burp Suite  

**Burp Suite** est un outil d’analyse web qui permet d’intercepter, observer et modifier les communications entre un navigateur et un serveur web. Contrairement aux autres outils présentés jusqu’à présent, Burp ne cherche pas à scanner ou à automatiser : il permet de comprendre ce qui se passe réellement.

Dans une application web, tout repose sur des échanges de requêtes et de réponses HTTP. Ces échanges sont normalement invisibles pour l’utilisateur. Burp Suite rend ces échanges visibles.

---

## Objectif  

L’objectif principal de Burp Suite est de permettre à l’analyste de :

- intercepter les requêtes envoyées au serveur  
- modifier les données avant qu’elles soient envoyées  
- analyser les réponses du serveur  
- tester le comportement de l’application  


{: .highlight}
> Burp ne trouve pas les vulnérabilités automatiquement; il permet de les comprendre et de les manipuler  

---

## Quand l’utiliser ?  

Burp Suite est utilisé très tôt dans le processus d'analyse d'une application web.

Après avoir...

- identifié la cible (ex.: nmap)  
- compris les technologies (ex.: whatweb)  

... on cherche à analyser les requêtes. C'est là où un outil comme Burp Suite est utile. Dans le cycle d’une attaque, Burp correspond à la phase **Interagir avec le système**.

---


## Fonctionnement  

Burp agit comme un **proxy intermédiaire** entre le navigateur et le serveur.

```text
Navigateur → Burp → Serveur
```

Concrètement :

1. le navigateur envoie une requête  
2. Burp l’intercepte  
3. l’analyste peut la modifier  
4. la requête est envoyée au serveur  
5. la réponse passe aussi par Burp  

{: .highlight}
> Cela permet de contrôler complètement les échanges avec le serveur.

### Modules importants
Burp Suite fonctionne en intégrant une série de modules ayant chacun un rôle précis. Il y en a beaucoup, mais voici les plus importants :

- **Intercept** : intercepter et modifier le contenu des requêtes
- **Repeater** : rejouer une requête  
- **Intruder** : automatiser des tests, optionnellement en faisant varier la charge utile (*payload*)

### Module Intercept  

Le module le plus utilisé dans Burp est **Intercept**. Il permet d'observer et de manipuler les échanges avec le serveur qui héberge l'application web. Il permet notamment de :

- intercepter les requêtes  
- les modifier avant envoi  

#### **Exemple de requête interceptée**

```http
POST /rest/user/login
Content-Type: application/json

{
  "email": "test@test.com",
  "password": "test"
}
```

{: .highlight}
> Une fois interceptée, cette requête peut être modifiée avant envoi


#### **Manipulation des requêtes**  

C’est ici que Burp devient extrêmement puissant.


**Test d’injection**

Modifier :

```text
password = test
```

par :

```text
password = ' OR 1=1--
```


**Modification de structure** 

- retirer un champ  
- renommer un paramètre  
- ajouter une valeur supplémentaire  


### Lecture des réponses  

Une fois que la requête modifiée est envoyée au serveur, Burp permet ensuite d’analyser les réponses du serveur, notamment :

- code HTTP  
- contenu retourné  
- messages d’erreur  
- données  

{: .highlight}
> Les réponses permettent de comprendre :
>
>- ce qui est accepté  
>- ce qui est rejeté  
>- ce qui est vulnérable  

---

## Limites  

Burp Suite présente une particularité importante : il ne fait rien par lui-même. Contrairement à plusieurs outils comme ZAP, sqlmap ou hydra, il ne fournit :

- aucune automatisation par défaut (mais des extensions peuvent ajouter quelques fonctionnalités d'automatisation)
- aucune détection automatique  

Il s'agit d'un outil d'analyse, et non d'un *scanner*.  

---

## Comment se protéger 

Burp permet de mettre en lumière le principe suivant : le serveur ne doit jamais faire confiance au client. Pour se protéger des vulnérabilités pouvant être exposées par Burp, les principales techniques consistent à :

- validation côté serveur  
- filtrage des entrées  
- désinfection des données  
- contrôle strict des paramètres  

---

## Liens utiles
- Documentation officielle : [https://portswigger.net/burp/documentation](https://portswigger.net/burp/documentation)
