---
layout: default
title: A07:2025 — Authentication Failures
parent: OWASP Top 10 (2025)
nav_order: 8
has_toc: true
---

# A07:2025 — Authentication Failures (Défaillances d’authentification)

> **Contexte 2025 :** 

## Comprendre la menace
Les **défaillances d’authentification** accélèrent les prises de contrôle. Sans **MFA** ni limitation adaptative, le **credential stuffing** transforme des fuites externes en compromissions internes. Une **gestion de session** laxiste annule l’effort d’authN; des intégrations **OIDC** faibles (iss/aud/nonce) ouvrent la voie au **rejeu** ou au mix‑up.

**En bref**

- **Vecteurs**
  - Brute force/stuffing; session fixation; flux OIDC incomplets.

{: .highlight-title}
> Contexte 2025
>
> Toujours critique : identification/authentification, gestion de sessions, fédération (OIDC/OAuth). 

---

## Attaque
Des bots orchestrent des **tentatives massives** si l’application n’ajuste pas sa défense. Un **XSS** ailleurs peut voler un cookie non durci et détourner la session. Des implémentations **PKCE/device code** fragiles facilitent les **fuites de tokens**. Les **signaux** : pics d’échecs par IP/ASN, jetons sans `aud/exp` valides, *refresh* depuis des empreintes inattendues.

**En bref**

- **Signaux**
  - Anomalies de login; tokens invalides; réutilisation suspecte de refresh tokens. citeturn1search6

---
## Prévention, détection, réponse)
**MFA** et **limitation adaptative** constituent la **ligne de base**. Durcir les cookies (`Secure`/`HttpOnly`/`SameSite`), privilégier le ***passwordless*** lorsque possible et implémenter **correctement OIDC** (validation `iss`/`aud`/`exp`/`nbf`, rotation maîtrisée). Détecter les anomalies de session et déclencher du **step‑up**. Révoquer sessions/tokens et forcer la **ré-authentification** ciblée en cas d’incident.

**En bref**

- **Prévention**
  - Authentification multi-facteurs (*MFA*); cookies sécurisés; OIDC validé; politiques sans mot de passe (*passwordless*).
- **Détection & réponse**
  - Risk‑based auth; step‑up; révocation et rotation des tokens.

---

## Exemples

### Python
```python

# Flask — cookies de session protégés
from flask import Flask
app = Flask(__name__)
app.secret_key = b"change-me"
app.config.update(
    SESSION_COOKIE_SECURE=True,
    SESSION_COOKIE_HTTPONLY=True,
    SESSION_COOKIE_SAMESITE='Lax'
)

```

### Java
```java

// Spring Security — mitigation session fixation
@Configuration
public class SecurityConfig {
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().and().sessionManagement().sessionFixation().migrateSession();
        return http.build();
    }
}

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
