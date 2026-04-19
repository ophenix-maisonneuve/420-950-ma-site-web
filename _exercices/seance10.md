---
layout: default
title: "Analyse statique de sécurité (SAST)
parent: "Mission : GHOST BEACON - Phase 1"
nav_order: 1
has_toc: false
published: true
---


## 3. Nomenclature logicielle (*SBOM*)

### 3.1 Configurer l'outil **OWASP CycloneDX**
1. Ajoutez le plugin `cyclonedx-maven-plugin` à votre `pom.xml`

### 3.2 Créer la nomenclature logicielle avec **OWASP CycloneDX**
1. Lancez le build du projet. Le SBOM sera disponible dans les formats XML et JSON:
    - target/bom.xml
    - target/bom.json

### 3.3 Visualisez le SBOM à l'aide de l'outil **Sunshine**
1. Rendez-vous au [https://cyclonedx.github.io/Sunshine/](https://cyclonedx.github.io/Sunshine/)
1. Téléversez le SBOM produit à l'étape précédente

### 3.3 Comparer le SBOM produit avec l'analyse SCA de l'exercice précédent
1. Quelles informations sont présentes à la fois dans le rapport SCA et dans le SBOM ?
1. Quelles informations sont présentes seulement dans l'analyse SCA ?
1. Quelles informations sont présentes seulement dans le SBOM ?