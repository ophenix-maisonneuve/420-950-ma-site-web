---
layout: default
title: "Analyse de composition locicielle (SCA)"
parent: "Mission : GHOST BEACON Phase 1"
nav_order: 2
has_toc: false
published: true
---

## 2. Analyse de composition logicielle (*SCA*)

### 2.1 Configurer l'outil **OWASP Dependency-Check**
1. Demandez une clé NVD à l'adresse suivante: [https://nvd.nist.gov/developers/request-an-api-key](https://nvd.nist.gov/developers/request-an-api-key). Vous pouvez utiliser les informations suivantes:
    - **Organization Name** : College de Maisonneuve
    - **Email Address** : Votre adresse courriel du Collège
    - **Organization Type** : Academia


1. Vérifiez que **OWASP Dependency-Check** est bien installé :
    ```bash
    dependency-check --version
    ```

### 2.2 Effectuer une analyse SCA
1. Déplacez-vous dans le répertoire racine du projet 
    ```bash
    cd exercice-seances-8-10
    ```
1. Lancez l'analyse de composition **Dependency-Check**
    - Quelle commande avez-vous utilisée
    - Combien de vulnérabilités *différentes* ont été identifiées ?
    - Quelle est la principale différence entre les vulnérabilités identifiées ici et celles qui avaient été identifiées avec **Semgrep** ?

### 2.3 Analyser les vulnérabilités identifiées
1. Pour chaque problème identifié, expliquez :
    - votre compréhension de la vulnérabilité.
    - les différentes options qui s'offrent à vous pour corriger cette vulnérabilité

### 2.4 Corriger les vulnérabilités identifiées
1. Pour chaque vulnérabilité identifiée aux questions précédentes :
    - Choisissez le correctif qui semble le plus approprié, et expliquez pourquoi
    - Implémentez le correctif directement dans l'application