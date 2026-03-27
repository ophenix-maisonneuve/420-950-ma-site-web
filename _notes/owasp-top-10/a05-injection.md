---
layout: default
title: A05:2025 — Injection
parent: OWASP Top 10 (2025)
nav_order: 6
has_toc: true
---

# A05:2025 — Injection

## Comprendre la menace

L’**injection** se produit quand une entrée façonne la **structure** d’une requête/commande. Ce n’est pas propre au SQL : expressions de gabarits (SSTI), opérateurs NoSQL, shell/OS... Toutes ces variantes reflètent une **perte de séparation** entre données et instructions. 2025 maintient cette catégorie tout en renvoyant d’autres risques vers leurs **causes racines** (accès, config, intégrité).

**En bref — points clés**

- **Variantes**
  - SQL injection
  - NoSQL injection
  - Command injection
  - XML injection
  - JSON injection
  - LDAP, SSTI/EL.

{: .highlight-title}
> Contexte 2025
>
> Catégorie historique, présente depuis les toutes premières versions du Top 10. Classée #3 en 2021 mais reste tout de même très importante (SQL/NoSQL/OS/SSTI/LDAP, etc).

---

## Attaque
Une concaténation triviale permet `"' OR 1=1--"` sur un login. Les moteurs NoSQL qui évaluent `$where/$regex` sans garde‑fous autorisent parfois de la logique arbitraire. En **command injection**, les opérateurs d’enchaînement (`&&`, `;`) élargissent l’impact. Les symptômes : erreurs SQL, latences, et réponses *gonflées* révélant des extractions massives.

**En bref**

- **Signaux**
  - Erreurs de syntaxe, temps de réponse anormal, dumps inattendus.

---

## Prévention, détection, réponse
La **paramétrisation** universelle (requêtes préparées) reste la défense clé. Ajouter l’**échappement** (*escaping*) de caractères au rendu, des **listes admises** (*allow lists*) pour commandes/flags, bannir les requêtes dynamiques en ORM et intégrer **tests négatifs** + **fuzzing** au pipeline. En réponse : désactiver les endpoints vulnérables, appliquer un **hotfix** de paramétrisation et revoir les **modèles d’accès BD**.

**En bref**

- **Prévention**
  - Paramétrisation partout; échappement contextuel; whitelists.
- **Détection & réponse**
  - Tests négatifs/fuzzing; WAF/IDS; hotfix + revue d’accès DB.

---

## Exemples

### Python
```python

# SQL injection (anti‑pattern)
query = f"SELECT * FROM users WHERE name = '{username}'"
# Paramétré (correct)
cursor.execute("SELECT * FROM users WHERE name = ?", (username,))

```

### Java
```java 
// JDBC — PreparedStatement obligatoire

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
