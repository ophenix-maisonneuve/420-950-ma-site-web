---
layout: default
title: SBOM - Nomenclature logicielle
nav_order: 11
has_children: true
has_toc: true
published: false
---

# SBOM - Nomenclature logicielle (*Software Bill of Materials*)  

Une **nomenclature logicielle** (ou *SBOM - Software Bill of Materials*) est un inventaire détaillé et structuré de tous les composants logiciels constituant une application : bibliothèques, dépendances transitives, versions exactes, licences, métadonnées, empreintes, provenance, modules compilés, scripts auxiliaires, etc. Elle joue le même rôle qu’une liste d’ingrédients sur un produit alimentaire, mais appliquée au logiciel.

Historiquement, les organisations n’avaient qu’une vision partielle des composants utilisés dans un projet. Aujourd’hui, avec la généralisation de l’open source et l’évolution des attaques supply chain, les SBOM sont devenues essentielles pour :
- comprendre **l’ensemble de la chaîne de dépendances**,
- identifier rapidement si un composant vulnérable (ex. Log4j) est présent,
- évaluer les risques liés aux licences,
- retracer l’origine et l’intégrité de chaque élément du logiciel,
- répondre aux exigences réglementaires croissantes.

Une SBOM peut être générée automatiquement à partir d’un projet et suit un format normalisé, comme **CycloneDX** ou **SPDX**. Ces formats sont conçus pour être lisibles par des humains mais surtout exploitables par des outils de sécurité, de conformité ou d’audit. Une SBOM bien structurée permet non seulement de réagir rapidement lors de la découverte d’une faille critique, mais aussi de maintenir une gouvernance claire et transparente du patrimoine logiciel.

---

## 📌 À quoi ça sert ?
Une SBOM permet de :
- améliorer la transparence logicielle,  
- répondre aux exigences réglementaires (dont le décret américain Executive Order 14028),  
- accélérer la détection des composants vulnérables,  
- mieux gérer les risques de la chaîne d'approvisionnement logicielle,  
- faciliter les audits et la conformité,  
- renforcer la résilience en cas de faille majeure (ex. Log4Shell).  

---

## 🛠️ Outils SBOM sur le marché
| Outil | Description | Standard |
|-------|-------------|-----------|
| **CycloneDX** | SBOM moderne, maintenu par OWASP | CycloneDX |
| **SPDX** | Standard Linux Foundation | SPDX |
| **Syft (Anchore)** | Super rapide et complet | CycloneDX & SPDX |
| **Dependency‑Track** | Suivi continu des dépendances | CycloneDX |
