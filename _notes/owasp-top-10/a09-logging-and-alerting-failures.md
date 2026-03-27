---
layout: default
title: A09:2025 — Logging & Alerting Failures
parent: OWASP Top 10 (2025)
nav_order: 10
has_toc: true
---

# A09:2025 — Logging & Alerting Failures (Journaux et alertes défaillants)

> **Contexte 2025 :** Accent 2025 : 

## Comprendre la menace
Des **journaux pauvres** sabotent la détection et des **alertes bruyantes** usent les équipes. À l’inverse, des logs **bavards** qui exposent PII, secrets ou tokens créent des brèches additionnelles. 2025 demande des **schémas** normalisés, une **corrélation** inter‑services et des **alertes** reliées à des **actions** concrètes (playbooks, seuils d’escalade).

**En bref — points clés**

- **Problèmes typiques**
  - Absence d’events pivot; logs contenant PII/secrets; pas de corrélation.

{: .highlight-title}
> Contexte 2025
>
> En 2025, l'accent est mis sur les **alertes actionnables** (pas seulement du *logging*) et la protection contre la **fuite d’informations** dans les *logs*.

---
## Attaque
Une intrusion reste **invisible** si les événements clés (connexion, élévation, modifications des secrets) ne sont pas journalisés. À l’inverse, une **exfiltration par logs** survient lorsqu’un message d’erreur ou une trace est renvoyé tel quel côté client puis agrégé côté SIEM. Les **signaux** : absence d’events critiques, présence de PII/secrets en clair, SLO de détection dépassés.

**En bref**

- **Signaux/artefacts**
  - Manques d’events; fuite PII/secrets; délais de détection élevés.

---
## Prévention, détection, réponse)
Définir un **modèle de log** minimal (timestamp, niveau, **correlation‑ID**, tenant/user), appliquer **rédaction/tokenisation** aux champs sensibles, chiffrer le transport et le stockage et cadrer la **rétention**. Aligner des **règles d’alerte** par scénario avec des **playbooks**. En cas de fuite, **purger** et effectuer une **rotation** des secrets exposés.

**En bref**

- **Prévention**
  - Journaux (*logs*) dans un format standard; rédaction/tokenisation; chiffrement; rétention cadrée.
- **Détection & réponse**
  - Alertes scénarisées; playbooks; purge/rotation en cas de fuite.

---
## Exemples

### Python
```python

# Masquage de secrets dans les logs
import logging, re
logger = logging.getLogger("app")
SECRET_RE = re.compile(r"(apikey|token)=[A-Za-z0-9_\-]+")

def redact(msg: str) -> str:
    return SECRET_RE.sub(r"=***", msg)

```

### Java
```java

// Redaction côté Java (regex simplifiée)
public class LogUtil {
    public String redact(String input) {
        return input.replaceAll("(?i)(apikey|token)=([A-Za-z0-9_-]+)", "$1=***");
    }
}

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
