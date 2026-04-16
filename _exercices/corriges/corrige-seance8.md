---
layout: default
parent: "Analyse statique de sécurité (SAST)"
title: "Corrigé - SAST"
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
        ```bash
        semgrep scan --config auto .
        ```
    - Combien de vulnérabilités *différentes* ont été identifiées ?
        - Si vous aviez fait l'étape du *login*, normalement Semgrep aura trouvé 9 vulnérabilités. Sinon, il en aura trouvé 5.

### 1.3 Analyser les vulnérabilités identifiées
1. Pour chaque problème identifié, expliquez :
    - votre compréhension de la vulnérabilité.
    - une contre-mesure ou un correctif pouvant être mis en place

    | Vulnérabilité distincte | Contre-mesure |
    |-------------------------|---------------|
    | Présence d'une clé AES en dur dans le code | Injecter la clé à partir d'une variable d'environnement provenant d'un stockage sécurisé (Vault, etc). |
    | Utilisation du mode ECB de AES | Utiliser plutôt un mode sécuritaire comme GCM |
    | Présence de `e.printStackTrace()` | Enlever ce code et privilégier l'utilisation d'un logger dédié pour afficher les messages d'erreur |
    | Présence d'une classe de test ouvrant un Socket oubliée par un développeur | Supprimer cette classe |

### 1.4 Corriger les vulnérabilités identifiées
1. Pour chaque problème identifié, implémentez un correctif directement dans le code de **GhostBeacon**
    - Voir le code corrigé dans la branche `solution` du dépôt du projet.

### 1.5 Vulnérabilité non-identifiée
Les versions gratuites des outils *SAST* n'identifient généralement pas toutes les vulnérabilités. C'est un marché très lucratif, et les compagnies préfèrent souvent offrir les fonctionnalités complètes aux clients payants. Ainsi, l'outil **Semgrep** a manqué une vulnérabilité de type **traversée des répertoires** (*Path Traversal*).
1. Renseignez-vous sur ce type de vulnérabilité [ici](https://owasp.org/www-community/attacks/Path_Traversal)
1. Identifiez l'emplacement de la vulnérabilité dans le code de **GhostBeacon**
    - La vulnérabilité est présente dans la classe `StatusService` à l'endroit où l'on écrit et lit directement dans un fichier. Le chemin d'accès est construit en concaténant le code de l'agent au chemin, ce qui permet d'injecter des caractères de type `../` afin de se promener dans le système de fichiers.
1. Exploitez la vulnérabilité à l'aide d'un outil comme **Postman** ou **RESTer** (extension Chrome et Firefox)
    - Créez d'abord le fichier `/tmp/secret.txt` contenant le mot `Félicitations`
        ```bash
        echo "Félicitations" > /tmp/secret.txt
        ```
    - À partir de votre navigateur (ou d'un outil comme Postman ou RESTer), lancez la requête suivante :
        ```http
        GET http://localhost:8080/api/v1/status?agent=..%2F..%2Fsecret.txt
        ```


### 1.6 Ajouter une règle personnalisée

1. En consultant la [documentation officielle de Semgrep](https://semgrep.dev/docs/writing-rules/overview), écrivez une règle qui génère un avertissement à l'utilisation de *print* à la console de type `System.out.println` (on préfère généralement l'utilisation d'un vrai *logger*).

    ```yaml
    rules:
      - id: cybermax.logging.system-out
        patterns:
          - pattern: System.out.println("...")
        message: >
            Il est préférable d'utiliser un logger plutot que d'imprimer directement à la console.
            severity: WARNING
        languages:
          - java
    ```


1. Lancez une nouvelle analyse
    - Quelle commande pouvez-vous utiliser pour inclure votre règle personnalisée dans l'analyse ?
        ```bash
        semgrep scan --config auto --config <chemin vers le dossier ou le fichier de règle(s)>
        ```
    - Quel est le résultat de cette nouvelle analyse ?
        - Si un `System.out.println()` est ajouté dans le code, un avertissement sera généré.


### 1.7 BONUS : Raffiner la règle personnalisée
1. Raffinez votre règle pour ne relever que les cas où de l'information sensible est imprimée à la console. Par exemple, on pourrait se fier au nom de la variable pour détecter les cas où une variable appelée `password` ou `secret` est journalisée (*loguée*).

    ```yaml
    rules:
    - id: cybermax.logging.system-out-identifiants
      patterns:
        - pattern: System.out.println($X)
        - metavariable-regex:
            metavariable: $X
            regex: '(?i)\b(password|pass|secret|credentials)\b'
        message: >
            Données sensibles CyberMax écrites sur stdout.
            Toute information liée aux identifiants
            ne doit jamais être journalisée via System.out.
        severity: ERROR
        languages:
          - java
    ```