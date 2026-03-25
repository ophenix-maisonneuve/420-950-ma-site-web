---
layout: default
title: A08:2025 — Software or Data Integrity Failures (Intégrité logiciel/données)
parent: OWASP Top 10 (2025)
nav_order: 9
has_toc: true
---

# A08:2025 — Software or Data Integrity Failures (Intégrité logiciel/données)


## Comprendre la menace
Les failles d’**intégrité** émergent quand des éléments **non authentifiés** sont implicitement considérés fiables. `curl | bash` dans un pipeline CI, **désérialisation** permissive ou messages **non authentifiés** font basculer le contrôle côté adversaire. 2025 demande d’élever l’intégrité au rang d’**exigence vérifiable**.

**En bref — points clés**

- **Surfaces**
  - Chaînes de build/update; mécanismes de sérialisation; bus de messages.

{: .highlight-title}
> Contexte 2025
>
> Couvre l’**intégrité** du logiciel et des données : artefacts non signés, désérialisation non sécurisée, messages sans MAC/signature.

---
## Attaque
Un script tiers modifié **à la source** empoisonne la CI au moment du build. Des objets non maîtrisés chargent des **gadgets** vers de l’exécution. Des messages sans **HMAC/signature** sont altérés/rejoués pour tricher avec l’état. Les symptômes : **hash** inattendus, exceptions de parsing, divergence de checksums.

**En bref**

- **Signaux**
  - Écarts de hash; erreurs de parsing; divergence producteur/consommateur.

---
## Prévention, détection, réponse)
Signer et **vérifier** artefacts/updates, bannir la **désérialisation** non sûre (formats simples, whitelists, versions), appliquer **HMAC**/signature aux messages et isoler l’exécution (*sandbox*). En réponse, garder un **rollback** atomique, faire une rotation des clés et **purger** les artefacts compromis.

**En bref**

- **Prévention**
  - Signatures + vérification; sérialisation sûre; HMAC/signature.
- **Détection & réponse**
  - Surveillance parsing; politiques SBOM; rollback + rotation de clés.

---

## Exemples

### Python
```python

# HMAC SHA‑256 (signature d'un message)
import hmac, hashlib
SECRET = b"super-secret"

def sign(msg: bytes) -> str:
    return hmac.new(SECRET, msg, hashlib.sha256).hexdigest()

```

### Java
```java
// HMAC SHA‑256 (Java) — utilitaire basique

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/0x00_2025-Introduction/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
