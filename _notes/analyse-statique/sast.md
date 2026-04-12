---
layout: default
title: SAST - Analyse statique de sécurité
parent: Analyse statique
nav_order: 1
has_children: true
has_toc: true
published: true
---

# SAST - Analyse statique de sécurité (*Static Application Security Testing*)  

Le ***SAST* (*Static Application Security Testing*)** désigne l'ensemble des techniques permettant d’analyser la sécurité d’une application **sans l’exécuter**, en observant exclusivement son **code source**, ses **binaires** ou son ***bytecode***. Il s’agit d’une approche dite boîte blanche (*white-box*) : l’outil voit tout ce que verrait un développeur.

L’objectif principal du *SAST* est d’identifier dès que possible les failles introduites lors du développement. En examinant la structure interne du code, il devient possible de détecter :
- des vulnérabilités logiques
- des mauvaises pratiques de validation d’entrées
- des erreurs cryptographiques
- des appels d’API sensibles
- des risques d’injections
- des fuites potentielles d’informations

Le *SAST* se distingue par sa capacité à explorer **tous les chemins d’exécution possibles**, y compris ceux difficiles à atteindre en situation réelle. Cela en fait une méthode particulièrement adaptée pour intervenir tôt dans le cycle de développement, avant même la compilation ou le déploiement. Contrairement à l'**analyse dynamique** (*DAST - Dynamic Application Security Testing*), qui teste l’application en fonctionnement, le *SAST* fournit une vision interne et exhaustive du comportement potentiel du code.

En pratique, le *SAST* s’intègre parfaitement dans une démarche *DevSecOps* moderne, notamment grâce à son exécution rapide, son automatisation dans les pipelines CI/CD et sa capacité à guider les développeurs vers des correctifs précis. Il contribue ainsi à instaurer une pratique de sécurité en continu.

---

## Objectif

Le *SAST* permet d’identifier des problèmes **très tôt** dans le cycle de développement. Plus précisément, il sert à :

### Détecter des vulnérabilités dès la phase de développement
Il est beaucoup moins coûteux de corriger une faille identifiée au moment de coder que lorsqu’elle se retrouve en production.

### Favoriser une culture DevSecOps
Les outils *SAST* sont souvent intégrés dans :
- des IDE (Visual Studio Code, IntelliJ, Pycharm, etc)
- des pipelines CI/CD (GitHub Actions, GitLab CI, Azure DevOps, etc)
- des hooks de pre-commit sur git

### Documenter et normaliser les pratiques de sécurité
Les résultats s’intègrent bien dans des tableaux de bord et des revues internes. Les rapports *SAST* font généralement partie des artefacts qui seront demandés lors d'un audit de sécurité, qu'il soit interne ou externe.

### Réduire les risques de sécurité avant compilation
Le *SAST* aide à protèger les organisations contre :
- des fuites de données
- des injections de code  
- des failles logiques 
- des problèmes de conformité

---

## Outils SAST
Voici une liste d'outils *SAST* parmi les plus utilisés dans l'industrie :

| Outil | Description rapide | Open source ? |
|-------|--------------------|----------------|
| **Semgrep** | Rapide, multi‑langages, règles personnalisables | Oui |
| **SonarQube** | Analyse qualité + sécurité, très populaire en entreprise | Version Community seulement|
| **Checkmarx** | Solution SAST complète orientée entreprises | Non |
| **Fortify** | Solution commerciale mature avec intégration large | Non |
| **Bandit (Python)** | Analyse statique spécialisée Python | Oui |
| **ESLint + plugins sécurité** | Pour analyser Javascript ou Typescript | Dépend des plugins |
