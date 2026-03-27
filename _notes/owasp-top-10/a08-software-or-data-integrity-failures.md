---
layout: default
title: A08:2025 — Software or Data Integrity Failures (Intégrité logiciel/données)
parent: OWASP Top 10 (2025)
nav_order: 8
has_toc: true
---

# A08:2025 — Software or Data Integrity Failures (Intégrité logiciel/données)

## Description

Les **défaillances d’intégrité du logiciel ou des données** apparaissent quand un système **fait confiance** à du code, des artefacts ou des contenus **sans vérifier leur intégrité**, ou lorsqu’il néglige les frontières de confiance (*trust boundaries*) entre composants. Typiquement : dépendre de **plugins/bibliothèques** provenant de **sources non vérifiées** (ex. CDN public), exécuter des **mises à jour automatiques** sans contrôle d’origine/signature, ou faire transiter des **objets sérialisés non fiables** qui peuvent être altérés. À un niveau inférieur à A03 (chaîne d’approvisionnement), A08 cible la **validation d’intégrité** des éléments qu’une application **consomme et applique** à l’exécution (code, données, configurations).

En 2025, A08 reste à la **8e place**, avec une légère clarification du nom (« or » au lieu de « and ») pour souligner qu’il suffit qu’un **seul** de ces deux aspects (logiciel **ou** données) soit mal protégé pour introduire un risque. Les CWE notables incluent : **CWE‑829** (fonctionnalités issues d’une sphère de contrôle non approuvée), **CWE‑915** (modification incontrôlée d’attributs dynamiques), et **CWE‑502** (désérialisation d’entrées non fiables). 

---

## Comment se protéger

- **Vérifier l’intégrité et la provenance** des logiciels/données : utiliser des **signatures numériques** (ou mécanismes équivalents), et **refuser** toute mise à jour ou artefact **non signé** ou **dont la signature ne correspond pas**. 
- **Consommer uniquement des dépôts de confiance** (npm, Maven, containers, etc.). Pour les contextes à risque, **héberger un registre interne « connu‑bon »**, alimenté par un processus de **vérification/vetting**.
- **Revue obligatoire des changements** (code **et** configuration) : instaurer un processus de **revue humaine** et d’**approbation** pour limiter l’introduction de code/config malveillant ou erroné.
- **Sécuriser la CI/CD** : séparation des rôles, **contrôle d’accès strict**, durcissement et **isolation** des secrets/outils, et **vérifications d’intégrité** des artefacts tout au long du build et du déploiement.
- **Désérialisation sûre** : ne **désérialiser** que des **formats/objets attendus** et **validés**, éviter l’exécution implicite (gadgets), privilégier des formats **non exécutables** et des **schémas stricts**.

---

## Exemples d'attaques

### Scénario 1 — Mise à jour automatique non signée
L’application **télécharge et installe** automatiquement des mises à jour depuis une URL préconfigurée **sans** vérifier signature ni origine. Un attaquant **publie** un binaire frelaté sur ce canal ; tous les postes **ingèrent** le code malveillant.  
**Causes** : absence de signature/chaîne de confiance, mises à jour **non authentifiées**. 

### Scénario 2 — Plug‑in chargé depuis un CDN
Une page intègre un **plug‑in** récupéré à la volée sur un **CDN tiers**. La ressource est **remplacée** côté CDN par une version altérée qui **exfiltre** des secrets côté client et manipule les données envoyées à l’API.  
**Causes** : confiance implicite envers une **sphère non approuvée** (CWE‑829), absence d’**integrity attribute**/SRI et de pinning.

### Scénario 3 — Désérialisation d’objets non fiables
Un service **désérialise** des objets reçus d’un client. L’attaquant **forge** un objet qui déclenche l’**exécution** d’un gadget lors de la désérialisation, ouvrant l’accès système.  
**Causes** : **CWE‑502**, absence de **liste d’objets autorisés**, pas de contrôle d’intégrité/structure.

### Scénario 4 — Pipeline CI/CD permissif
Le pipeline **build** tire des scripts depuis un dépôt **non restreint**. Un contributeur malveillant **pousse** une modification qui **modifie** les artefacts de sortie ; la prod **déploie** des binaires piégés.  
**Causes** : pas de **revue** ni de **séparation des tâches**, CI/CD **sans** contrôle d’intégrité bout‑en‑bout.

---

## CWE liées

Exemples de CWE emblématiques dans A08 :

- **CWE‑829 — Inclusion of Functionality from Untrusted Control Sphere** 
- **CWE‑915 — Improperly Controlled Modification of Dynamically‑Determined Object Attributes**
- **CWE‑502 — Deserialization of Untrusted Data**

Pour la **liste complète des CWE mappées** (14 CWE) et les métriques associées, voir la page officielle A08.

---

## Liens utiles

- **Page officielle OWASP — A08:2025 Software or Data Integrity Failures**  
  [https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures/](https://owasp.org/Top10/2025/A08_2025-Software_or_Data_Integrity_Failures/)

- **OWASP Top 10:2025 — Page principale / contexte**  
  [https://owasp.org/Top10/2025/](https://owasp.org/Top10/2025/)

---

## Attribution

Contenu dérivé à partir des documents OWASP (licence **CC BY‑SA 4.0**).  
Informations de licence : [https://owasp.org/Top10/2025/0x01_2025-About_OWASP/](https://owasp.org/Top10/2025/0x01_2025-About_OWASP/)