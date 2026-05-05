---
layout: default  
title: Étapes d’une attaque
parent: Sécurité offensive
nav_order: 2
has_toc: true  

---

# Étapes d’une attaque  

Les attaques informatiques se déroulent rarement, voire jamais, comme dans les films, où quelques commandes dans un terminal suffisent à un *hacker* pour prendre le contrôle de l'arsenal nucléaire d'un pays entier. L'instant précis où une intrusion est réussie n'est en fait qu'une petite partie d'un processus plus large.

En réalité, une attaque est rarement instantanée. Elle est le résultat d’un processus progressif, souvent assez long et composé de plusieurs étapes logiques, où chaque action dépend des informations obtenues à l’étape précédente.

Un attaquant ne commence pas par exploiter une vulnérabilité; cela serait impossible sans savoir quoi exploiter. Il suit plutôt, formellement ou non, une série d’étapes se résumant généralement à :

1. Observer le système  
2. Interagir avec ses interfaces  
3. Explorer sa structure  
4. Tester ses limites  
5. Automatiser certaines attaques  
6. Exploiter les vulnérabilités  

---

Ces étapes ne sont pas toujours linéaires. Un attaquant peut revenir en arrière, ajuster sa stratégie ou changer d’approche en fonction des résultats obtenus.

Ce qui est constant, en revanche, c’est le raisonnement qui sous-tend chacune de ces actions.

---

## Le mindset d’un attaquant  

Avant de s’intéresser aux outils ou aux techniques, il est essentiel de comprendre la manière dont un attaquant aborde un système.

Contrairement à un utilisateur classique, qui cherche à accomplir une tâche de manière efficace et conforme aux attentes, un attaquant adopte une posture différente.

Il cherche à comprendre :

- ce qu’il peut contrôler  
- ce que le système suppose  
- ce qui n’est pas vérifié  
- ce qui peut être détourné  

---

Un attaquant ne suit pas un scénario prévu par les développeurs.  
Il explore les scénarios imprévus.

---

Cette approche repose sur une série de questions simples, mais puissantes :

- Que puis-je modifier ici ?  
- Cette donnée est-elle réellement validée ?  
- Que se passe-t-il si j’envoie quelque chose d’inattendu ?  
- Le système réagit-il toujours comme prévu ?  

---

Ces questions ne sont pas liées à un outil particulier. Elles constituent un cadre mental, une façon d’analyser un système.

C’est ce que l’on appelle le **mindset hacker**.

---

## 1. Observer le système  

La première étape consiste à comprendre ce qui est visible.

Avant toute interaction, un attaquant cherche à cartographier la surface d’attaque : quels services sont accessibles, quelles technologies sont utilisées, quelles interfaces sont exposées.

---

Cette étape permet de répondre à des questions comme :

- Quels ports sont ouverts ?  
- Quels services sont disponibles ?  
- Quelle technologie est utilisée ?  

---

### Outils associés  

- **nmap** : identification des ports et services  
- **whatweb** : identification des technologies web  

---

👉 À ce stade, aucune attaque n’est lancée.  
On collecte simplement de l’information.

---

## 2. Interagir avec le système  

Une fois la surface d’attaque identifiée, l’étape suivante consiste à observer les interactions entre l’utilisateur et le système.

Plutôt que de se limiter à l’interface graphique, un attaquant s’intéresse aux échanges réels : requêtes HTTP, paramètres, données transmises.

---

L’objectif est de comprendre comment le système fonctionne réellement.

---

### Questions importantes  

- Quelles données sont envoyées au serveur ?  
- Ces données sont-elles modifiables ?  
- Le serveur vérifie-t-il ces données ?  

---

### Outils associés  

- **Burp Suite** : interception et modification des requêtes  

---

👉 Cette étape marque une transition importante :  
on passe de l’observation passive à l’interaction active.

---

## 3. Explorer le système  

Une application ne montre généralement qu’une partie de ses fonctionnalités.

Un attaquant cherche à découvrir ce qui n’est pas visible dans l’interface : endpoints cachés, routes internes, fonctionnalités non documentées.

---

Cette exploration permet d’élargir la surface d’attaque.

---

### Questions importantes  

- Existe-t-il des ressources non exposées ?  
- Certaines pages sont-elles accessibles directement ?  
- Le système cache-t-il des fonctionnalités ?  

---

### Outils associés  

- **gobuster** : découverte de répertoires et endpoints  

---

👉 Cette étape repose souvent sur l’automatisation, mais nécessite une interprétation des résultats.

---

## 4. Tester les hypothèses  

Une fois le système exploré, l’attaquant commence à tester ses hypothèses.

C’est à ce moment que les vulnérabilités peuvent apparaître.

---

L’idée est simple :  
si le système fait une hypothèse… elle peut être fausse.

---

Exemples d’hypothèses implicites :

- l’utilisateur saisit des données valides  
- les entrées ne contiennent pas de code malveillant  
- les paramètres ne seront pas modifiés  

---

### Outils et techniques  

- injections SQL  
- Cross-Site Scripting (XSS)  
- manipulation de paramètres  

---

👉 Cette étape est souvent réalisée manuellement, car elle demande de comprendre le comportement du système.

---

## 5. Automatiser  

Une fois une vulnérabilité ou un comportement intéressant identifié, un attaquant peut chercher à automatiser l’attaque.

Pourquoi tester une seule entrée… quand on peut en tester des centaines ou des milliers ?

---

L’automatisation permet :

- d’accélérer les tests  
- d’explorer davantage de cas  
- d’augmenter les chances de succès  

---

### Outils associés  

- **sqlmap** : exploitation automatisée des injections SQL  
- **hydra** : attaques par brute force  

---

👉 À ce stade, la vitesse devient un facteur clé.

---

## 6. Exploiter  

La dernière étape consiste à exploiter une vulnérabilité identifiée afin d’obtenir un accès, des données ou un contrôle sur le système.

---

Cela peut prendre différentes formes :

- accéder à un compte utilisateur  
- extraire des données  
- exécuter du code  
- obtenir un shell  

---

### Outils associés  

- **Metasploit** : framework d’exploitation  

---

👉 Cette étape est souvent spectaculaire… mais elle dépend entièrement des étapes précédentes.

---

## Une progression logique  

Chacune de ces étapes repose sur la précédente.

Sans observation, pas d’interaction.  
Sans interaction, pas d’exploration.  
Sans exploration, pas de test.  
Sans test, pas d’exploitation.

---

> *Une attaque réussie n’est pas une question de chance.*  
> *C’est une question de compréhension.*

---

## Limites des outils  

Il est important de comprendre que les outils utilisés dans ces différentes étapes ne sont pas infaillibles.

Ils reposent sur des hypothèses, des règles ou des signatures, et leur efficacité dépend fortement du contexte.

---

Un outil peut :

- produire des faux positifs  
- manquer des vulnérabilités  
- interpréter incorrectement une réponse  

---

Un exemple classique est celui d’un serveur qui retourne toujours le même code HTTP, même pour des pages inexistantes. Dans ce cas, un outil automatisé peut avoir de la difficulté à distinguer les ressources valides des ressources invalides.

---

👉 C’est pourquoi l’interprétation humaine est essentielle.

---

> *Un outil peut exécuter des tests.*  
> *Mais seul un analyste peut comprendre les résultats.*

---

## Conclusion  

Comprendre les étapes d’une attaque permet de dépasser l’utilisation mécanique des outils.

Il ne s’agit pas simplement d’appliquer des commandes, mais de comprendre pourquoi elles sont utilisées, à quel moment, et dans quel objectif.

---

Cette compréhension constitue le fondement de la sécurité offensive.

---

> *Un attaquant ne suit pas une liste d’outils.*  
> *Il suit une logique.*
