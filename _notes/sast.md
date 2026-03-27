---
layout: default
title: SAST - Analyse statique de sécurité
nav_order: 9
has_children: true
has_toc: true
published: false
---

# SAST - Analyse statique de sécurité (*Static Application Security Testing*)  

Le **SAST (Static Application Security Testing)** désigne l'ensemble des techniques permettant d’analyser la sécurité d’une application **sans l’exécuter**, en observant exclusivement son **code source**, ses **binaires** ou ses **bytecodes**. Il s’agit d’une approche dite *white-box* : l’outil voit tout ce que verrait un développeur.

L’objectif principal du SAST est d’identifier dès que possible les failles introduites lors du développement. En examinant la structure interne du code, il devient possible de détecter :
- des vulnérabilités logiques, 
- des mauvaises pratiques de validation d’entrées,
- des erreurs cryptographiques,
- des appels d’API sensibles,
- des risques d’injections,
- des fuites potentielles d’informations.

Le SAST se distingue par sa capacité à explorer **tous les chemins d’exécution possibles**, y compris ceux difficiles à atteindre en situation réelle. Cela en fait une méthode particulièrement adaptée pour intervenir tôt dans le cycle de développement, avant même la compilation ou le déploiement. Contrairement au DAST (Dynamic Application Security Testing), qui teste l’application en fonctionnement, le SAST fournit une vision interne, profonde et exhaustive du comportement potentiel du code.

En pratique, le SAST s’intègre parfaitement dans une démarche DevSecOps moderne, notamment grâce à son exécution rapide, son automatisation dans les pipelines et sa capacité à guider les développeurs vers des correctifs précis. Il contribue ainsi à instaurer une culture de sécurité continue et proactive.

---

## À quoi ça sert ?
Le SAST permet d’identifier des problèmes **très tôt** dans le cycle de développement.  
Plus précisément, il sert à :

### Détecter des vulnérabilités dès la phase de développement
Il est beaucoup moins coûteux de corriger une faille repérée au moment de coder que lorsqu’elle se retrouve en production.

### Favoriser une culture DevSecOps
Les outils SAST sont souvent intégrés dans :
- des IDE (VSCode, IntelliJ, Pycharm…),  
- des pipelines CI/CD (GitHub Actions, GitLab CI, Azure DevOps…),  
- des hooks de pre-commit.

### Documenter et normaliser les pratiques de sécurité
Les résultats s’intègrent dans des tableaux de bord, des politiques internes et des workflows d’équipe.

### Réduire les risques de sécurité avant compilation
Le SAST protège les organisations contre :
- des fuites de données,  
- des injections de code,  
- des failles logiques,  
- des problèmes de conformité (PCI-DSS, ISO 27001…).

---

## Outils SAST
Voici une sélection des outils les plus utilisés :

| Outil | Description rapide | Open source ? |
|-------|--------------------|----------------|
| **Semgrep** | Rapide, multi‑langages, règles personnalisables | ✅ Oui |
| **SonarQube** | Analyse qualité + sécurité, très populaire en entreprise | Version Community |
| **Checkmarx** | Solution SAST complète orientée entreprises | ❌ |
| **Fortify** | Solution commerciale mature avec intégration large | ❌ |
| **Bandit (Python)** | Analyse statique spécialisée Python | ✅ |
| **ESLint + plugins sécurité** | Pour analyser JS/TS | Dépend des plugins |
