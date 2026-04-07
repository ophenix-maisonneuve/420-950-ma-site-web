---
layout: default
title: "Mission : GHOST BEACON - Phase 1"
nav_order: 4
has_toc: false
published: false
---

# Exercice : Mission GHOST BEACON - Phase 1

## Mise en situation
Vous êtes toujours en poste comme analyste principal au quartier général de l'unité technique **CyberMax**, une unité secrète spécialisée dans le support logistique aux opérations clandestines. Il y a quelques semaines, la catastrophe a été évitée de justesse : lors de l’opération **ECHO SILENCIEUX**, Shadow, une agente infiltrée sous couverture, a failli être compromise. Les canaux de communications habituels ont été piratés, et n'eut été de votre capacité à valider l'intégrité, l'authenticité et la confidentialité des messages échangés sur un canal alternatif, Shadow aurait probablement été capturée. Pour éviter qu’un tel scénario ne se reproduise, CyberMax a développé en urgence une nouvelle application de communications. Nom de code : **GhostBeacon**.

Cette application offre les fonctionnalités suivantes pour les agents :
- Chiffrer / déchiffrer des messages (par exemple dans le but de les transmettre ensuite sur un canal non-sécurisé)
- Téléverser (*upload*) des fichiers vers le quartier général
- Envoyer des messages à un autre agent et consulter les messages reçus
- Mettre à jour et consulter le statut courant d'un agent déployé en mission

Sous pression pour livrer l'application le plus rapidement possible, **CyberMax** a confié le développement de l'application **GhostBeacon** à la seule équipe disponible à ce moment : une équipe certes compétente, mais moins expérimentée en sécurité logicielle. Résultat: même si l’application fonctionne, personne n’a encore vérifié si elle est réellement sécurisée. Avant tout déploiement sur le terrain, un audit complet est obligatoire.

On vous confie le mandat de certifier **GhostBeacon** avant son déploiement opérationnel.

Votre mission :
- Utiliser un outil d'analyse statique de sécurité (*Static Analysis Security Testing - SAST*) pour détecter des problèmes avec le code
- Utiliser un outil d'analyse ce composition logicielle (*Software Composition Analysis - SCA*) pour détecter des problèmes avec les librairies tierces
- Générer une nomenclature logicielle (*Software Build of Material - SBOM*) qui fait l'inventaire de toutes les composantes
- Analyser *toutes* les vulnérabilités détectées
- Corriger certaines vulnérabilités et documenter les vulnérabilités restantes

## Objectifs
- Apprendre à manipuler des outils SAST/SCA/SBOM
- Comprendre les résultats de ces outils
- Proposer des correctifs appropriés aux vulnérabilités identifiées
- Corriger certaines vulnérabilités directement dans le code source d'une application

---

## Préparation

### 1. Clonez le dépôt de l'exercice

```bash
git clone git@github.com:ophenix-420-950-ma-24636/exercice-seances-8-10.git
```
ou
```bash
git clone https://github.com/ophenix-420-930-ma-24636/exercice-seances-8-10.git
```

### 2. Lancez le projet Java
```bash
mvn clean package
java -jar target/ghostbeacon-1.0.0.jar
```
ou directement à partir de votre IDE.

En utilisant un générateur de requêtes HTTP (Postman ou extensions Chrome/Firefox telles que RESTer), familiarisez-vous avec les différents services offerts par **GhostBeacon**, notamment:

- `POST /api/v1/crypto/encrypt` et `POST /api/v1/crypto/decrypt`
- `POST /api/v1/file` 
- `POST /api/v1/message` et `GET /api/v1/message/{nom de l'agent}`
- `POST /api/v1/status` et `GET /api/v1/status/{nom de l'agent}`

---

## Analyse statique de sécurité (*SAST*)

### 1. Configurer l'outil **Semgrep** 
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

### 2. Effectuer une analyse SAST
1. Déplacez-vous dans le répertoire racine du projet 
    ```bash
    cd exercice-seances-8-10
    ```

2. Lancez l'analyse **Semgrep**
    - Quelle commande avez-vous utilisée ?
    - Combien de vulnérabilités *différentes* ont été identifiées ?

### 3. Analyser les vulnérabilités identifiées
1. Pour chaque problème identifié, expliquez :
    - votre compréhension de la vulnérabilité.
    - une contre-mesure ou un correctif pouvant être mis en place

### 4. Corriger les vulnérabilités identifiées
1. Pour chaque problème identifié, implémentez un correctif directement dans le code de **GhostBeacon**

### 5. Ajouter une règle personnalisée

1. En consultant la [documentation officielle de Semgrep](https://semgrep.dev/docs/writing-rules/overview), écrivez une règle qui interdit l'utilisation de *print* à la console de type `System.out.println`
1. Lancez une nouvelle analyse
    - Quelle commande pouvez-vous utiliser pour inclure votre règle personnalisée dans l'analyse ?
    - Quel est le résultat de cette nouvelle analyse ?
1. Raffinez votre règle pour ne relever que les cas où de l'information sensible est imprimée à la console (ex.: se fier au nom de la variable).

## Analyse de composition logicielle (*SCA*)

### 1. Configurer l'outil **OWASP Dependency-Check**
1. Demandez une clé NVD à l'adresse suivante: [https://nvd.nist.gov/developers/request-an-api-key](https://nvd.nist.gov/developers/request-an-api-key). Vous pouvez utiliser les informations suivantes:
    - **Organization Name** : College de Maisonneuve
    - **Email Address** : Votre adresse courriel du Collège
    - **Organization Type** : Academia
1. Vérifiez que **OWASP Dependency-Check** est bien installé :
    ```bash
    dependency-check --version
    ```

### 2. Effectuer une analyse SCA
1. Déplacez-vous dans le répertoire racine du projet 
    ```bash
    cd exercice-seances-8-10
    ```
1. Lancez l'analyse de composition **Dependency-Check**
    - Quelle commande avez-vous utilisée
    - Combien de vulnérabilités *différentes* ont été identifiées ?
    - Quelle est la principale différence entre les vulnérabilités identifiées ici et celles qui avaient été identifiées avec **Semgrep** ?

### 3. Analyser les vulnérabilités identifiées
1. Pour chaque problème identifié, expliquez :
    - votre compréhension de la vulnérabilité.
    - les différentes options qui s'offrent à vous pour corriger cette vulnérabilité

### 4. Corriger les vulnérabilités identifiées
1. Pour chaque vulnérabilité identifiée aux questions précédentes :
    - Choisissez le correctif qui semble le plus approprié, et expliquez pourquoi
    - Implémentez le correctif directement dans l'application

## Nomenclature logicielle (*SBOM*)

### 1. Générer un SBOM minimal avec OWASP Dependency-Check
### 2. Configurer l'outil **OWASP CycloneDX**

### 3. Créer la nomenclature logicielle

### 4. Comparer


