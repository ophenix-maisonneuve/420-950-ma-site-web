---
layout: default
title: CycloneDX
parent: SBOM - Nomenclature logicielle
nav_order: 1
---

# OWASP CycloneDX

CycloneDX est un standard de nomenclature logicielle développé par l’OWASP pour répondre aux besoins croissants de **transparence**, de **sécurité** et de **traçabilité** dans la chaîne d'approvisionnement logicielle. Contrairement à une simple liste de dépendances, CycloneDX vise à fournir une représentation riche et complète des composants d’un système.

Une SBOM CycloneDX peut inclure :
- les dépendances directes et transitives,  
- les licences associées à chaque composant,  
- des métadonnées complètes sur les modules,  
- les relations entre composants,  
- les empreintes et signatures cryptographiques,  
- les potentielles vulnérabilités référencées.

CycloneDX n’est pas qu’un format, mais un **écosystème complet**, avec :
- des outils CLI,  
- des plugins dédiés pour Maven, Gradle, npm, pip, etc.,  
- des intégrations dans les pipelines CI/CD,  
- une compatibilité totale avec des outils comme Dependency‑Track pour le suivi continu.

Il répond aux exigences modernes des audits, de la conformité et des normes de cybersécurité qui demandent de plus en plus aux organisations de fournir une traçabilité fine de tous les composants logiciels utilisés dans leurs produits.  
Dans un contexte où les attaques sur la supply chain logicielle se multiplient, CycloneDX devient un standard incontournable.

---

## Installation
```bash
pipx install cyclonedx-bom
pipx ensurepath
```

---

## Commandes courantes
### Générer une SBOM pour un projet Java (*Maven*)
```bash
mvn org.cyclonedx:cyclonedx-maven-plugin:makeAggregateBom
```

### Générer pour un projet Python
```bash
cyclonedx-bom -r -o sbom.json
```
