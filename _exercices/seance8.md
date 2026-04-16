---
layout: default
title: "Analyse statique de sécurité (SAST)"
parent: "Mission : GHOST BEACON Phase 1"
nav_order: 1
has_toc: false
published: true
---

## 1. Analyse statique de sécurité (*SAST*)

### 1.1 Configurer l'outil **Semgrep** 
1. Assurez-vous que l'outil **Semgrep** est correctement installé
    ```bash
    semgrep --version
    ```

1. Vous pouvez maintenant obtenir quelques informations sur la version de **Semgrep** que vous utilisez, par exemple les langages pris en charge:
        ```bash
    semgrep show supported-languages
    ```

1. Il est recommandé (mais pas obligatoire) de se créer un compte à l'adresse [https://semgrep.dev/signup](https://semgrep.dev/signup) afin de bénéficier d'une plus grande couverture (plus de règles incluses). Une fois le compte créé, il suffit de se connecter :
    ```bash
    semgrep login
    ```

### 1.2 Effectuer une analyse SAST
1. Déplacez-vous dans le répertoire racine du projet 
    ```bash
    cd exercice-seances-8-10
    ```

2. Lancez l'analyse **Semgrep**
    - Quelle commande avez-vous utilisée ?
    - Combien de vulnérabilités *différentes* ont été identifiées ?

### 1.3 Analyser les vulnérabilités identifiées
1. Pour chaque problème identifié, expliquez :
    - votre compréhension de la vulnérabilité.
    - une contre-mesure ou un correctif pouvant être mis en place

### 1.4 Corriger les vulnérabilités identifiées
1. Pour chaque problème identifié, implémentez un correctif directement dans le code de **GhostBeacon**

### 1.5 Vulnérabilité non-identifiée
Les versions gratuites des outils *SAST* n'identifient généralement pas toutes les vulnérabilités. C'est un marché très lucratif, et les compagnies préfèrent souvent offrir les fonctionnalités complètes aux clients payants. Ainsi, l'outil **Semgrep** a manqué une vulnérabilité de type **traversée des répertoires** (*Path Traversal*).
1. Renseignez-vous sur ce type de vulnérabilité [ici](https://owasp.org/www-community/attacks/Path_Traversal)
1. Identifiez l'emplacement de la vulnérabilité dans le code de **GhostBeacon**
1. Exploitez la vulnérabilité à l'aide d'un outil comme **Postman** ou **RESTer** (extension Chrome et Firefox)

{: .astuce}
> Tentez d'identifier un endroit où un fichier est lu directement à partir du système de fichiers du serveur...


### 1.6 Ajouter une règle personnalisée

1. En consultant la [documentation officielle de Semgrep](https://semgrep.dev/docs/writing-rules/overview), écrivez une règle qui génère un avertissement à l'utilisation de *print* à la console de type `System.out.println` (on préfère généralement l'utilisation d'un vrai *logger*).
1. Lancez une nouvelle analyse
    - Quelle commande pouvez-vous utiliser pour inclure votre règle personnalisée dans l'analyse ?
    - Quel est le résultat de cette nouvelle analyse ?


### 1.7 BONUS : Raffiner la règle personnalisée
1. Raffinez votre règle pour ne relever que les cas où de l'information sensible est imprimée à la console. Par exemple, on pourrait se fier au nom de la variable pour détecter les cas où une variable appelée `password` ou `secret` est journalisée (*loguée*).