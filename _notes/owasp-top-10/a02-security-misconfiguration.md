---
layout: default
title: A02:2025 — Security Misconfiguration
parent: OWASP Top 10 (2025)
nav_order: 3
has_toc: true
---

# A02:2025 — Security Misconfiguration (Mauvaise configuration)

> **Contexte 2025 :** Catégorie **#2** en 2025 ; reflète la complexité **Cloud/IaC** et l’élargissement des surfaces de configuration (plateformes, frameworks, middlewares).

## Comprendre la menace

La **mauvaise configuration** est un **défaut d’ingénierie** plutôt qu’une simple erreur d’exploitation. Quand le comportement sécurisé dépend d’un mille‑feuille de flags, d’options et de fichiers IaC qui divergent entre environnements, l’application expose des surfaces non prévues : **dashboards** laissés ouverts, **stockage** public, **messages d’erreur** bavards. L’édition 2025 note une **prévalence accrue** de ces défauts dans les données de tests, d’où sa place au **rang #2**. citeturn1search6

**En bref — points clés**

- **Causes typiques**
  - Écarts dev/test/prod; templates IaC copiés sans revue; valeurs par défaut faibles. citeturn1search6
  - Expositions involontaires (ports/services, directory listing, CORS `*`). citeturn1search6
- **Exemples**
  - Endpoints d’administration (Actuator/metrics) accessibles publiquement. citeturn1search6
  - Buckets S3/Blob en lecture publique contenant des PII/artefacts. citeturn1search6
  - Frameworks en **mode debug** en production (fuites d’information). citeturn1search6

{: .highlight-title}
> Contexte 2025
>
> - Passé de #5 en 2021 à **#2** en 2025 
> - Reflète la complexité **Cloud/IaC** et l’élargissement des surfaces de configuration (plateformes, frameworks, middlewares).

---

## Attaque
Les attaquants automatisent la **découverte** d’interfaces d’administration et d’objets de stockage exposés. Un endpoint non protégée livre des métadonnées précieuses (versions, routes internes). Des **bannières d’erreur** verbeuses et des traces révèlent l’architecture et guident les intrusions suivantes. Les outils de posture (CSPM) et les **scans IaC** détectent ces problèmes à l’échelle avant qu’ils ne soient exploités massivement.

**En bref**

- **Chaîne type**
  - Scan → Découverte d’admin/dashboards → Fuite d’infos → Mouvement latéral.
- **Signaux**
  - Alertes CSPM (buckets/SG ouverts), 401/403 suivis de 200 après essais par défaut.

---

## Prévention, détection, réponse
Traiter la configuration comme du **code** (*configuration-as-code*): **versionner, relire et tester** avec des **politiques as code** (linters IaC). Commencer par des **benchmarks** (CIS), appliquer le principe du **moindre privilège** aux identités/ressources et **désactiver** ce qui n’est pas indispensable. Rester **neutre** côté client dans les messages d’erreur et centraliser les détails en logs serveurs. Piloter la **posture cloud** avec CSPM en continu, des **diffs** de configuration et des **runbooks** qui incluent la **rotation** des secrets potentiellement exposés.

**En bref**

- **Prévention**
  - IaC + policy-as-code; CIS/benchmarks; moindre privilège; bannières neutres.
- **Détection**
  - CSPM/scans réguliers; vérification des expositions publiques; *drift* de config.
- **Réponse**
  - Durcissement via runbooks; **rotation des secrets**; quarantaine des services.

---

## Exemples

### Python
```python

# Django — ne jamais laisser DEBUG=True en prod
DEBUG = False
ALLOWED_HOSTS = ["exemple.edu"]

```

### Java
```java

# application.yml — limiter Actuator
management:
  endpoints:
    web:
      exposure:
        include: health,info
  endpoint:
    health:
      show-details: when_authorized

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
