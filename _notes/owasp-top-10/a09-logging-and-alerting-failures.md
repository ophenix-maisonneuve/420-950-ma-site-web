---
layout: default
title: A09:2025 — Logging & Alerting Failures (Journaux et alertes défaillants)
parent: OWASP Top 10 (2025)
nav_order: 10
has_toc: true
---

# A09:2025 — Logging & Alerting Failures (Journaux et alertes défaillants)

> **Contexte 2025 :** Accent 2025 : **alertes actionnables** (pas seulement du logging) et protection contre la **fuite d’informations** dans les journaux. citeturn1search6

## Comprendre la menace
Des **journaux pauvres** sabotent la détection et des **alertes bruyantes** usent les équipes. À l’inverse, des logs **bavards** qui exposent PII, secrets ou tokens créent des brèches additionnelles. 2025 demande des **schémas** normalisés, une **corrélation** inter‑services et des **alertes** reliées à des **actions** concrètes (playbooks, seuils d’escalade). citeturn1search6

**En bref — points clés**

- **Problèmes typiques**
  - Absence d’events pivot; logs contenant PII/secrets; pas de corrélation. citeturn1search6

---
## Comment l’attaque se manifeste (angles d’attaque, kill‑chain, signaux)
Une intrusion reste **invisible** si les événements clés (connexion, élévation, modif. secrets) ne sont pas journalisés. À l’inverse, une **exfiltration par logs** survient lorsqu’un message d’erreur ou une trace est renvoyé tel quel côté client puis agrégé côté SIEM. Les **signaux** : absence d’events critiques, présence de PII/secrets en clair, SLO de détection dépassés. citeturn1search6

**En bref — points clés**

- **Signaux/artefacts**
  - Manques d’events; fuite PII/secrets; délais de détection élevés. citeturn1search6

---
## Se protéger (prévention, détection, réponse)
Définissez un **modèle de log** minimal (timestamp, niveau, **correlation‑ID**, tenant/user), appliquez **redaction/tokenisation** aux champs sensibles, chiffrez le transport et le stockage et cadrez la **rétention**. Alignez des **règles d’alerte** par scénario avec des **playbooks**. En cas de fuite, **purgez** et **rotAtez** les secrets exposés. citeturn1search6

**En bref — points clés**

- **Prévention**
  - Schéma standard; redaction/tokenisation; chiffrement; rétention cadrée. citeturn1search6
- **Détection & réponse**
  - Alertes scénarisées; playbooks; purge/rotation en cas de fuite. citeturn1search6

---
## Exemples didactiques

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
