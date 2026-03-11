---
layout: default
title: "Cycle de développement sécurisé (SDLC)"
nav_order: 2
published: true
has_toc: true
---

# Introduction cycle de développement sécurisé (SDLC)

Historiquement, le développement logiciel se déroulait sur deux voies séparées :

- **Les fonctionnalités** : les *product owners* définissent les besoins fonctionnels, puis les développeurs implémentent les fonctionnalités.
- **La sécurité** : les équipes de sécurité interviennent tardivement, souvent en fin de développement.

Ce fonctionnement en parallèle amène un problème majeur : la sécurité arrive **trop tard**. Les analyses réalisées en fin de cycle révèlent parfois des vulnérabilités structurelles, ce qui impose des **correctifs coûteux** ou des retards de livraison.

Le **cycle de développement sécurisé** (en anglais *Secure Development Lifecycle* ou *SDLC)* est une approche qui consiste à intégrer la sécurité dans **toutes les étapes** du cycle de développement logiciel, plutôt que de la traiter comme une vérification finale. Cela réduit les retours en arrière, les coûts de correction et améliore la qualité globale du logiciel. Cette approche est soutenue par des cadres reconnus comme le **Microsoft SDL**, le **NIST SSDF**, ou encore le **OWASP SAMM**. 

Il n'existe pas *SDLC* unique ou universel. En pratique, les entreprises partent généralement d'une base reconnue (**NIST SSDF**, **OWASP SAMM**, ou même **Microsoft SDL**) puis l’adaptent à leur réalité : leurs risques, leurs outils, leur culture et leur maturité. Un *SDLC* est donc un processus vivant, appelé à évoluer et à s’améliorer continuellement au fil des projets et des apprentissages.

---

## Pourquoi un cycle de développement sécurisé ?

### Les attaques sont plus nombreuses et plus coûteuses
IBM rapporte que les attaques contre la chaîne d'approvisionnement logicielle ont augmenté de **1300 %** en trois ans. [Source IBM](https://www.ibm.com/think/topics/secure-software-development-life-cycle)

### Les vulnérabilités coûtent bien plus cher à corriger en production
Corriger une vulnérabilité en production peut coûter jusqu’à **100 fois plus cher** qu’en développement. [Source Aikido Security](https://www.aikido.dev/blog/secure-sdlc)

### La conformité réglementaire l'exige
De nombreuses normes telles que **GDPR** ou **HIPAA** exigent l’intégration de la sécurité dès la conception. [Source IBM](https://www.ibm.com/think/topics/secure-software-development-life-cycle)

### La sécurité fait partie d'une culture organisationnelle durable
Le Microsoft SDL illustre cette intégration via ses pratiques couvrant formation, exigences, design sécurisé et réponse aux incidents. [Source Microsoft SDL](https://learn.microsoft.com/en-us/compliance/assurance/assurance-microsoft-security-development-lifecycle)

---

## Étapes-clés
Dans presque tous les 

### 1. **Requis**
Définition des exigences de sécurité et de confidentialité dès le début.

### 2. **Design**
Conception sécurisée, modélisation STRIDE, revue d'architecture.

### 3. **Implémentation**
Usage de frameworks sûrs, revues de code, gestion des fournisseurs (librairies, logiciels tiers, etc)

### 4. **Vérification**
Tests SAST, DAST, SCA, audits, etc.

### 5. **Déploiement et opérations**
Durcissement, monitoring, gestion des incidents.

---

# Références
- IBM — https://www.ibm.com/think/topics/secure-software-development-life-cycle
- GeeksforGeeks — https://www.geeksforgeeks.org/ethical-hacking/what-is-secure-software-development-life-cycle-ssdlc/
- Microsoft SDL — https://learn.microsoft.com/en-us/compliance/assurance/assurance-microsoft-security-development-lifecycle
- Aikido Security — https://www.aikido.dev/blog/secure-sdlc
- Codific — https://codific.com/owasp-sdlc-owasp-samm/
- GlobalDots — https://www.globaldots.com/resources/blog/application-security-frameworks/
