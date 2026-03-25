---
layout: default
title: A10:2025 — Mishandling of Exceptional Conditions (Mauvaise gestion des conditions exceptionnelles)
parent: OWASP Top 10 (2025)
nav_order: 11
has_toc: true
---

# A10:2025 — Mishandling of Exceptional Conditions (Mauvaise gestion des conditions exceptionnelles)

> **Contexte 2025 :** **Nouveau en 2025** : mauvaise gestion des **conditions anormales** (erreurs, délai, ressources) → fuites, *fail‑open*, indisponibilités. Page dédiée disponible. citeturn1search8

## Comprendre la menace
La **mauvaise gestion des exceptions** est une **vulnérabilité** : lorsque les erreurs ne sont pas empêchées, détectées ou traitées correctement, l’application adopte des **comportements non sûrs** (ex. *fail‑open*) ou fuit des informations (stack traces, chemins, variables). 2025 regroupe ici des CWE naguère éparpillées pour fournir un **cadre opérationnel** de résilience. citeturn1search8

**En bref — points clés**

- **Exemples CWE**
  - CWE‑209 (messages d’erreur sensibles), CWE‑636 (not failing securely), CWE‑476 (NULL deref). citeturn1search8

---
## Comment l’attaque se manifeste (angles d’attaque, kill‑chain, signaux)
Un service d’**authz** indisponible ne doit jamais déclencher un **accès accordé** ; pourtant des implémentations naïves font du *fail‑open* sur exception. Des messages d’erreur détaillés, renvoyés au client, accélèrent la **reconnaissance**. L’absence de bornes (retries infinis, allocations) mène à des **épuisements de ressources** et du déni de service intermittents. citeturn1search8

**En bref — points clés**

- **Signaux/artefacts**
  - Pics de 500 corrélés à un downstream, latence en escalade, exceptions non rattrapées. citeturn1search8

---
## Se protéger (prévention, détection, réponse)
La **résilience** repose sur des **timeouts** réalistes, des **retries bornés** et des **circuit breakers**. Sur les chemins critiques (authN/authZ), appliquer le **fail‑safe** : en cas d’incertitude, **refuser** l’accès. Normalisez la gestion d’erreurs : messages **neutres** côté client, détails complets en logs. Validez vos modes **dégradés** par du monitoring **synthétique** et du **chaos engineering**. citeturn1search8

**En bref — points clés**

- **Prévention**
  - Timeouts/retries/circuit breakers; fail‑safe sur authN/authZ; messages neutres. citeturn1search8
- **Détection & réponse**
  - Synthetic monitoring; chaos ciblé; bulkheads; bascule en chemin dégradé. citeturn1search8

---
## Exemples didactiques

### Python
```python

# Fail‑safe : refuser l'accès si le service d'autorisation est indisponible

def can_access(user_id: str) -> bool:
    try:
        return check_authorization_service(user_id)
    except Exception:
        return False

```

### Java
```java

// Ne jamais autoriser par défaut en cas d'exception côté politique
public class AuthzClient {
    private final RemotePolicy remotePolicy;
    public AuthzClient(RemotePolicy remotePolicy) { this.remotePolicy = remotePolicy; }
    public boolean canAccess(String userId) {
        try {
            return remotePolicy.allow(userId);
        } catch (Exception ex) {
            return false; // fail-safe
        }
    }
}

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/A10_2025-Mishandling_of_Exceptional_Conditions/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
