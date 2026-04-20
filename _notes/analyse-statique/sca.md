---
layout: default
title: SCA - Analyse de composition logicielle
parent: Analyse statique
nav_order: 2
has_children: true
has_toc: true
published: true
---

# SCA - Analyse de composition logicielle (*Software Composition Analysis*)

L'**analyse de composition logicielle** (*Software Composition Analysis - SCA*) est un ensemble de techniques dédiées à l’analyse des composants logiciels externes utilisés dans une application : librairies open source, dépendances tierces, plugins, frameworks, modules transitifs, etc. Aujourd’hui, la majorité des projets intègrent entre 70 % et 95 % de code provenant d’éléments qu’ils n’ont pas eux‑mêmes écrits, ce qui rend indispensable une visibilité complète sur ces composants.

Le rôle du SCA est d’identifier :
- des **vulnérabilités connues** (CVE) affectant une version précise d’une dépendance,
- des **licences incompatibles** avec les politiques de l’organisation,
- des **dépendances obsolètes** ou non maintenues,
- des **altérations suspectes** pouvant indiquer une compromission,
- la **structure complète** de la chaîne de dépendances, incluant les modules transitifs.

Contrairement au *SAST*, qui analyse le code écrit par l’équipe interne, le *SCA* se concentre sur tous les composants externes du logiciel. Il constitue donc un pilier essentiel de la sécurité moderne, particulièrement face à l’augmentation des attaques visant la **chaîne d'approvisionnement logicielle**. Le *SCA* permet aux organisations de comprendre précisément ***de quoi est composé*** leur logiciel et de réagir rapidement lorsqu’une dépendance devient vulnérable.

Les outils *SCA* fonctionnent en comparant les composants détectés dans un projet avec des bases de données publiques ou privées, comme la **National Vulnerability Database (NVD)**, les avis de sécurité des éditeurs, ou des registres de licences. Ils fournissent ensuite un rapport clair permettant d’évaluer le risque, prioriser les mises à jour et garantir la conformité réglementaire et légale.

---

## Objectif ?
Le SCA aide à :
- garder un inventaire clair et à jour des dépendances,  
- respecter les politiques de licences,  
- repérer les vulnérabilités connues,  
- empêcher l’intégration accidentelle de composants compromis,  
- générer une nomenclature logicielle (*Software Bill Of Materials ou SBOM*).  

---

## Outils SCA

| Outil | Description | Open source ? |
|-------|-------------|----------------|
| **OWASP Dependency-Check** | Basé sur les CVE, très utilisé | Oui |
| **Snyk** | Analyse de dépendances + correctifs proposés | Non |
| **Whitesource (Mend)** | Solution SCA complète pour entreprises | Non |
| **GitHub Dependabot** | Intégré aux repos GitHub | Partiellement |
| **OSS Review Toolkit (ORT)** | Audit complet OSS + licences | Oui |
| **Black Duck (Synopsys)** | Solution professionnelle, orientée entreprises | Non 
