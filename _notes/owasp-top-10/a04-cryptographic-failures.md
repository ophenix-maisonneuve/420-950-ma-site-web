---
layout: default
title: A04:2025 — Cryptographic Failures
parent: OWASP Top 10 (2025)
nav_order: 5
has_toc: true
---

# A04:2025 — Cryptographic Failures (Erreurs cryptographiques)

## Comprendre la menace
Les **échecs cryptographiques** résultent de choix d’algorithmes/modes inadaptés, d’une **mauvaise gestion des clés** ou de l’absence de chiffrement aux bons endroits. Réutiliser un **IV**/nonce, stocker des **clés** dans le code, tolérer des suites **TLS** faibles : autant d’erreurs qui mènent à l’interception, à la modification ou à l’usurpation. Le corpus 2025 montre une **occurrence élevée**, d’où l’importance d’un socle hygiénique solide. citeturn1search6

**En bref — points clés**

- **Erreurs fréquentes**
  - Downgrade TLS, réutilisation d’IV/nonce, clés en clair dans le dépôt. citeturn1search6

{: .highlight-title}
> Contexte 2025
>
> - Passe de #2 en 2021 à **#4** en 2025 mais reste très **prévalent**
> - Souvent multiplicateur d’impact.

---

## Attaque
Un **downgrade TLS** permet d’imposer des suites de chiffrement (*cipher suites*) obsolètes et de casser la confidentialité. La **réutilisation d’IV/nonce** dévoile des relations statistiques exploitables (oracles). Les **clés** committées dans un SCM offrent une prise directe sur vos environnements. Les **signaux** incluent des bannières TLS faibles, des IV non aléatoires et des alertes de secret‑scan.

**En bref**

- **Signaux/artefacts**
  - Bannières TLS
  - Patterns non aléatoires
  - Secrets détectés dans le SCM.

---

## Prévention, détection, réponse
Exiger **TLS 1.2+ (idéalement 1.3)**, utiliser des modes **AEAD** (AES‑GCM/ChaCha20‑Poly1305), déléguer les clés à un **KMS/HSM** (avec rotation) et les dériver via **PBKDF2/bcrypt/scrypt/Argon2**. Interdire les secrets dans le code et outillez le **secret scanning**. Prévoyez la **rotation/révocation** de clés et le **ré‑chiffrement** si nécessaire.

**En bref**

- **Prévention**
  - TLS/AEAD; KMS/HSM; dérivation de clés; pas de secrets en code.
- **Détection & réponse**
  - Secret scanning; rotation/invalidation; ré‑chiffrement; invalidation de certs.

---

## Exemples

### Python
```python

# PBKDF2 (démo minimale)
import os
from hashlib import pbkdf2_hmac

def hash_secure(pw: str):
    salt = os.urandom(16)
    dk = pbkdf2_hmac('sha256', pw.encode(), salt, 200_000)
    return salt, dk

```

### Java
```java
// AES-GCM + PBKDF2 (extrait simplifié)

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
