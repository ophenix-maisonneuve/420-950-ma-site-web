---
layout: default
title: "La brocante à Joseph"
nav_order: 8
has_toc: true
published: false
---

# Exercice : La Brocante à Joseph
## Test d'intrusion

*À la Brocante à Joseph, on achète vos vieilleries, on les renippe, on les revend, on les rachète, pis on ferme au bout de deux semaines.*
*À la Brocante à Joseph, tous se vend, tout revient et tourlou!*

---

## Mise en situation

Vous travaillez pour *La Brocante à Joseph*, une entreprise spécialisée dans l'achat et la revente d'items d'occasion. Pour ses activités en ligne, l'entreprise a récemment adopté la plateforme de e-commerce Juice Shop, qui gagne en popularité. En ce jeudi matin ensoleillé alors que vous rêvassiez à une victoire des Canadiens contre les Sabres de Buffalo, un mystérieux message apparaît dans votre boîte de courriels.

```
From : petitcanetonpolisson3432343@gmail.com
To : mona@brocanteajoseph.com

Bonjour. 

Le collectif SPECTRE vous informe que la plateforme Juice Shop utilisée par votre entreprise contient une vulnérabilité critique permettant à quiconque de se connecter en tant qu'administrateur, même sans connaître le mot de passe. Il s'agit d'une attaque par Injection SQL. 

Veuillez prendre les mesures nécessaires pour corriger la situation.

Bisous,

Le Petit Caneton Polisson
```

Affolé(e) à l'idée de devoir travailler ce soir au lieu de regarder le match, vous vous rappelez que vous maîtrisez déjà les outils de sécurité offensive, ce qui vous permettrait d'effectuer un test d'intrusion afin de valider les dires de ce Petit Caneton Polisson (c'est étrange, ce nom évoque un vague souvenir de jeunesse... Ce doit être un pseudonyme.) 

---

## Objectifs

- Comprendre le lien entre la sécurité offensive et les tests d'intrusion
- Appliquer les étapes d'un plan de test d'intrusion
- Rédiger un court rapport de test d'intrusion

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

## 1. Reconnaissance

Afin de bien cerner la plateforme, utilisez un outil comme **whatweb** pour énumérer les différentes technologies utilisées.

### Étapes
1. Utilisez les connaissance acquises lors des derniers cours pour lancer une analyse de Juice Shop avec l'outil **whatweb**

## Questions de réflexion
- Est-ce que **whatweb** pourrait normalement vous fournir des informations utiles quant à la faille d'injection SQL ?
- Pourquoi n'est-ce pas le cas ici ?

---

## 2. Énumération

Tentez maintenant de découvrir au maximum la surface d'attaque en exposant tous les points d'entrée avec l'outil des outils comme **gobuster** et un **spider** de OWASP ZAP.

### Étapes

1. Lancez une analyse de Juice Shop avec l'outil **gobuster**
1. Visitez l'une des routes ayant retourné un code 500.
- Quelle information supplémentaire cette route fournit-elle à un *hacker* ?
1. Lancez maintenant un *spider* OWASP ZAP sur 

### Questions de réflexion
- Lorsque vous avez visité une route ayant causé une erreur avec **gobuster**, quelle information utile par rapport à la faille d'injection SQL vous a été révélée ?
- Quel outil (**gobuster** ou **ZAP**) vous a donné les meilleurs résultats ? Pourquoi ?
- Quels cas sont plus appropriés pour chacun des outils:
    - gobuster?
    - ZAP ?

---

## 3. Analyse

Confirmez maintenant que la route découverte à l'étape précédente est bien celle utilisée par l'authentification dans Juice Shop.

## Étapes

1. À l'aide de l'outil Burp Suite, interceptez une requête d'authentification
1. Sous l'onglet **HTTP Traffic**, faites un clic droit sur la requête d'authentification, puis sélectionnez **Save item**
    - Sauvegardez la requête dans un fichier `requete.txt`

## Questions de réflexion

- Dans quelle(s) circonstance(s) un outil comme Burp Suite excelle-t-il ?
- Quelles sont ses avantages par rapport à un outil d'énumération comme **gobuster** ou **ZAP**?
- Quels sont ses inconvénients par rapport à un outil d'énumération comme **gobuster** ou **ZAP**?

---

## 4. Exploitation

Vérifiez que la route d'authentification est bel et bien vulnérable aux injections SQL.

## Étapes

1. À l'aide de l'outil **sqlmap**, validez que la route d'authentification est vulnérable à l'injection SQL.

{: .astuce}
> Plutôt que d'utiliser une URL, il sera plus efficace ici d'utiliser la requête vulnérable sauvegardée à l'étape précédente. Une requête du format suivant serait appropriée :
>
>```bash
> sqlmap -r <requête> --batch --level=5 -p <paramètre vulnérable>
>```

## Questions de réflexion
- **sqlmap** a-t-il été en mesure de détecter la faille d'injection SQL ? Pourquoi ?
- **sqlmap** a-t-il été en mesure d'exploiter directement la faille d'injection SQL en devenant administrateur ? Pourquoi ?


## 5. Post-exploitation

On suppose maintenant que l'obtention de l'accès `admin@juice-sh.op` vous a permis d'obtenir une combinaison usager/mot de passe qui est également utilisée comme compte directement sur le serveur. Pour simuler ce cas, on utilisera notre compte `dev/dev`, mais cela fonctionnerait avec tout compte ayant un accès SSH au serveur sur lequel s'exécute Juice Shop.

## Étapes
1. Utiliser le module ssh de 

