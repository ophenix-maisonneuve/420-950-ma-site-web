---
layout: default  
title: Étapes d’une attaque
parent: Sécurité offensive
nav_order: 1
has_toc: true  
---

# Étapes d’une attaque  

Les attaques informatiques se déroulent rarement, voire jamais, comme dans les films, où quelques commandes dans un terminal suffisent à un *hacker* pour prendre le contrôle de l'arsenal nucléaire d'un pays entier. L'instant précis où une intrusion est réussie n'est en fait qu'une petite partie d'un processus plus large.

En réalité, une attaque est rarement instantanée. Elle est le résultat d’un processus progressif, souvent assez long et composé de plusieurs étapes logiques, où chaque action dépend des informations obtenues à l’étape précédente.

Un attaquant ne commence pas par exploiter une vulnérabilité; cela serait impossible sans savoir quoi exploiter. Il suit plutôt, formellement ou non, une série d’étapes se résumant généralement à :

1. Observer le système  
1. Interagir avec ses interfaces  
1. Explorer sa structure  
1. Tester ses limites  
1. Automatiser certaines attaques  
1. Exploiter les vulnérabilités  

Ces étapes ne sont pas toujours linéaires. Un attaquant peut revenir en arrière, ajuster sa stratégie ou changer d’approche en fonction des résultats obtenus. Ce qui est constant, toutefois, c’est le raisonnement qui mène à chacune de ces actions.

{: .highlight-title}
> La mentalité d'un attaquant  
>
> Avant de s’intéresser aux outils ou aux techniques, il est essentiel de comprendre la manière dont un attaquant aborde un système. Contrairement à un utilisateur classique, qui cherche à accomplir une tâche de manière efficace et conforme aux attentes, un attaquant adopte une posture différente.
>
>Il cherche à comprendre :
>
>- ce qu’il peut contrôler  
>- ce que le système suppose  
>- ce qui n’est pas vérifié  
>- ce qui peut être détourné  
>
> Un attaquant ne suit pas un scénario prévu par les développeurs. Il explore les scénarios imprévus. Cette approche repose sur une série de questions simples qu'un *hacker* se pose toujours :
>
>- Que puis-je modifier ici ?  
>- Cette donnée est-elle réellement validée ?  
>- Que se passe-t-il si j’envoie quelque chose d’inattendu ?  
>- Le système réagit-il toujours comme prévu ?  
>
> Ces questions ne sont pas liées à un outil particulier. Elles constituent un cadre mental, une façon d’analyser un système. C’est ce que l’on pourrait qualifier de la **mentalité d'un *hacker***.


## 1. Observer le système  

La première étape consiste à comprendre ce qui est visible.

Avant toute interaction, un attaquant cherche à cartographier la surface d’attaque : quels services sont accessibles, quelles technologies sont utilisées, quelles interfaces sont exposées.

Cette étape permet de répondre à des questions comme :

- Quels ports sont ouverts ?  
- Quels services sont disponibles ?  
- Quelle technologie est utilisée ?  


### Outils associés

- **nmap** : identification des ports et services  
- **whatweb** : identification des technologies web  

{: .highlight}
> À ce stade, aucune attaque n'est tentée. On recueille simplement de l'information.

---

## 2. Interagir avec le système  

Une fois la surface d’attaque identifiée, l’étape suivante consiste à observer les interactions entre l’utilisateur et le système. Plutôt que de se limiter à l’interface graphique, un attaquant s’intéresse aux échanges réels : requêtes HTTP, paramètres, données transmises, etc.

L’objectif est de comprendre comment le système fonctionne réellement.

### Questions importantes  

- Quelles données sont envoyées au serveur ?  
- Ces données sont-elles modifiables ?  
- Le serveur vérifie-t-il ces données ?  

### Outil(s) associé(s)

- **Burp Suite** : interception et modification des requêtes  

{: .highlight}
> Il s'agit d'un étape de transition : on passe de l’observation passive à l’interaction active.

---

## 3. Explorer le système  

Une application ne montre généralement qu’une partie de ses fonctionnalités. Un attaquant cherche à découvrir ce qui n’est pas visible dans l’interface : *endpoints* cachés, routes internes, fonctionnalités non documentées ou oubliées par un développeur voulant effectuer des tests.

Cette exploration permet d’élargir la surface d’attaque.

### Questions importantes  

- Existe-t-il des ressources non exposées ?  
- Certaines pages sont-elles accessibles directement ?  
- Le système cache-t-il des fonctionnalités ?  


### Outil(s) associés

- **gobuster** : découverte de répertoires et endpoints  

{: .highlight}
> Cette étape repose souvent sur l’automatisation, mais nécessite une interprétation des résultats.

---

## 4. Tester les hypothèses  

Une fois le système exploré, l’attaquant commence à tester ses hypothèses. C’est à ce moment que les vulnérabilités peuvent apparaître. L'attaquant cherche à comprendre si le système assumes certains faits ou se base sur des hypothèses pouvant être fausses.

Exemples d’hypothèses formelles ou, plus souvent, implicites :

- l’utilisateur saisit des données valides  
- les entrées ne contiennent pas de code malveillant  
- les paramètres ne seront pas modifiés  

### Outils et/ou techniques  

- injections SQL  
- Cross-Site Scripting (XSS)  
- manipulation de paramètres  

{: .highlight}
> Cette étape est souvent réalisée manuellement, car elle demande de comprendre le comportement du système.

---

## 5. Automatiser  

Une fois une vulnérabilité ou un comportement intéressant identifié, un attaquant peut chercher à automatiser l’attaque. L’automatisation permet :

- d’accélérer les tests  
- d’explorer davantage de cas  
- d’augmenter les chances de succès  


### Outils associés  

- **sqlmap** : exploitation automatisée des injections SQL  
- **hydra** : attaques par brute force  


{: .highlight}
> À ce stade, la vitesse devient un facteur clé. On cherche à exécuter une opération (ou une série d'opérations) un grand nombre de fois afin d'explorer un maximum de possibilités (par exemple, tester des entrées différentes).

---

## 6. Exploiter  

La dernière étape consiste à exploiter une vulnérabilité identifiée afin d’obtenir un accès, des données ou un contrôle sur le système. Cela peut prendre différentes formes :

- accéder à un compte utilisateur
- extraire des données
- exécuter du code
- obtenir un shell sur un système
- etc.

### Outils associés  

- **Metasploit** : Cadriciel (*framework*) d’exploitation  

{: .highlight}
> C'est l'étape qu'on voit dans les films. Elle est souvent spectaculaire… mais elle dépend entièrement des étapes précédentes.

---

## Conclusion  

Avant de réussir une attaque, un *hacker* passe beaucoup de temps à analyser l'application et à interagir, explorer et manipuler le système. Même si elle sont moins spectaculaires, ces étapes sont nécessaires afin de découvrir des vulnérabilités exploitables dans un système.
