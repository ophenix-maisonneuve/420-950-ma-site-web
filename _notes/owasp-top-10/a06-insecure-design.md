---
layout: default
title: A06:2025 — Insecure Design
parent: OWASP Top 10 (2025)
nav_order: 7
has_toc: true
---

# A06:2025 — Insecure Design (Conception non sécurisée)

## Comprendre la menace
La **conception non sécurisée** désigne des architectures où les **règles métier** et hypothèses de confiance ne sont pas matérialisées en contrôles. Un flux de **réinitialisation** sans preuve de possession, un **parcours critique** sans idempotence/limites, ou des microservices s’accordant une **confiance implicite** sont vulnérables **même** si le code respecte les bonnes pratiques.

**En bref**

- **Exemples**
  - Reset sans 2FA/ownership; double dépense/coupons; trust implicite inter‑services.

{: .highlight-title}
> Contexte 2025
>
> La **conception** est l'étape-clé : sans menaces modélisées ni contrôles métier, un code “propre” reste vulnérable.

---
## Attaque
Les attaques visent la **logique métier** : réutilisation de coupons, contournement de plafonds, exploitation de liens de reset prédictibles. Dans un maillage interne, un service “confiant” accepte des requêtes sans **authz**, ouvrant un pivot latéral. Les **signaux** : métriques métier anormales, rafales d’appels sur endpoints critiques, incohérences d’état.

**En bref**

- **Signaux/artefacts**
  - Pics de conversions/paniers, répétitions d’appels, états contradictoires.

---

## Prévention, détection, réponse)
Intégrer une modélisation de la menace (ex.: STRIDE), appliquer des **patrons** (policy‑based, **rate‑limit**, **idempotence**) et encoder ces règles en **tests**. Prévoir des **kill switches** pour couper des parcours risqués et des **modes dégradés** pour conserver l’intégrité. Cette discipline incarne l’approche 2025 : **sécurité by design**, vérifiée en continu.

**En bref**

- **Prévention**
  - Modélisation de la menace; patrons d’architecture; exigences testables.
- **Détection & réponse**
  - Tests d’abus; chaos contrôlé; kill switches; compensation transactionnelle.

---
## Exemples

### Python
```python
# Token de reset robuste : aléa + timestamp + HMAC

```

### Java
```java
// Rate limiter (conceptuel) pour endpoints sensibles

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
