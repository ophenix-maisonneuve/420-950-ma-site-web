---
layout: default
title: A03:2025 — Software Supply Chain Failures (Chaîne d’approvisionnement logiciel)
parent: OWASP Top 10 (2025)
nav_order: 4
has_toc: true
---

# A03:2025 — Software Supply Chain Failures (Chaîne d’approvisionnement logiciel)

## Comprendre la menace
Les attaques de **chaîne d’approvisionnement** visent votre **outillage** autant que votre code : dépendances (directes/transitives), registres de packages, runners CI, systèmes de build et distribution. Un package typosquatté ou un script d’installation malveillant hérite des **droits** du pipeline, accède aux secrets et peut **altérer** les artefacts. Le 2025 introduit cette catégorie à la suite d’un **consensus communautaire** face à des incidents à fort impact mais **sous‑représentés** dans les tests classiques.

**En bref — points clés**

- **Surfaces**
  - Registres (npm/PyPI/Maven), runners, SCM, registries d’images/artefacts. citeturn1search6
- **Modes de compromission**
  - Typosquatting/dépendance malveillante; CI compromise; **vol de clé de signature**. citeturn1search6

{: .highlight-title}
> Contexte 2025
>
> - **Nouveau en 2025**
> - Extension de *Vulnerable & Outdated Components* (#6 en 2021) à **toute la chaîne** (dépendances, build, CI/CD, distribution).
> - Impact élevé, visibilité de test limitée.

---

## Attaque
Trois scénarios dominent : **typosquatting** (exécution à l’installation), **altération de CI** (injection dans le runner ou la recette de build) et **signature usurpée** (artefacts malveillants paraissant légitimes). Les **indicateurs** incluent des divergences entre **SBOM** et artefacts, des empreintes inattendues et des processus anormaux observés pendant le build.

**En bref**

- **Signaux/artefacts**
  - Hash en écart
  - Différences SBOM/artefact
  - Téléchargements depuis miroirs non officiels.

---

## Prévention, détection, réponse
Mettre en place une **attestation de bout en bout** : **SBOM** (CycloneDX/SPDX), **SCA**, **pinning** des versions, **vérification de signatures/hachages** à l’entrée comme à la sortie. Favoriser des **builds reproductibles**, signer avec **Sigstore/cosign**, **isoler** le pipeline (agents éphémères, moindre privilège, secrets scellés) et surveiller les registres pour les **takeovers**. En réponse, préparez **rotation/révocation** des clés, **rollback** et chasse aux **dépendances transitives**.

**En bref**

- **Prévention**
  - SBOM + SCA; pinning; vérification systématique des signatures/hachages.
- **Détection**
  - Comparaison artefact↔SBOM; surveillance des registres; contrôle d’intégrité post‑build.
- **Réponse**
  - Révocation/rotation des clés; rollback; assainissement des dépendances transitives.

---

## Exemples

### Python
```python
# requirements.txt avec --require-hashes (extrait conceptuel)
# requests==2.32.3 \
#   --hash=sha256:...

```

### Java
```java
/* Maven — verrouiller versions et vérifier la résolution (Enforcer) */

```

---

## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
