---
layout: default
title: A10:2025 — Mishandling of Exceptional Conditions
parent: OWASP Top 10 (2025)
nav_order: 11
has_toc: true
---

# A10:2025 — Mishandling of Exceptional Conditions (Mauvaise gestion des conditions d'exception)

## Comprendre la menace
La **mauvaise gestion des exceptions** est une **vulnérabilité** : lorsque les erreurs ne sont pas empêchées, détectées ou traitées correctement, l’application adopte des **comportements non sûrs** (ex. *fail‑open*) ou fuit des informations (stack traces, chemins, variables). Le Top 10 2025 regroupe ici des CWE qui étaient auparavant éparpillées pour fournir un **cadre opérationnel** de résilience.

**En bref**

- **Exemples CWE**
  - CWE‑209 (messages d’erreur sensibles)
  - CWE‑636 (not failing securely)
  - CWE‑476 (NULL deref)

{: .highlight-title}
> Contexte 2025
>
> **Nouveau en 2025** : mauvaise gestion des **conditions anormales** (erreurs, délai, ressources) qui engendre fuites, *fail‑open*, indisponibilités.

---
## Attaque
Un service d’authentification/autorisation indisponible ne doit jamais déclencher un **accès accordé** ; pourtant des implémentations naïves font du *fail‑open* sur exception. Des messages d’erreur détaillés, renvoyés au client, accélèrent la **reconnaissance**. L’absence de limites (retries infinis, allocations) mène à des **épuisements de ressources** et du déni de service intermittents.

**En bref**

- **Signaux/artefacts**
  - Pics d'erreurs HTTP 500 corrélés à un downstream, latence en escalade, exceptions non rattrapées.

---
## Prévention, détection, réponse)
La **résilience** repose sur des **timeouts** réalistes, des **ré-essais (*retries*) bornés** et des l'utilisation du patron ***circuit breaker***. Sur les chemins critiques (authentification / autorisation), appliquer le moyen le plus sûr (*fail-safe*) : en cas d’incertitude, **refuser** l’accès. Normaliser la gestion d’erreurs : messages **neutres** côté client, détails complets en logs disponibles pour les utilisateurs / groupes autorisés seulement. Valider les modes **dégradés** par du monitoring **synthétique** et du **chaos engineering**.

**En bref**

- **Prévention**
  - Timeouts/retries/circuit breakers; fail‑safe sur authN/authZ; messages d'erreurs neutres qui divulguent uniquement le strict nécessaire à l'utilisateur.
- **Détection & réponse**
  - Synthetic monitoring; chaos ciblé; bulkheads; bascule en chemin dégradé.

---
## Exemples

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
