---
layout: default
title: A01:2025 — Broken Access Control
parent: OWASP Top 10 (2025)
nav_order: 1
has_toc: true
---

# A01:2025 — Broken Access Control (Contrôle d’accès défaillant)

## Comprendre la menace
Le contrôle d’accès brisé apparaît lorsque la politique d’autorisation n’est pas **appliquée de façon systématique**. Dans ce contexte, un acteur peut outrepasser ses droits : accéder à des ressources d’autrui, appeler des fonctions d’administration, ou exécuter des opérations métier qui devraient être impossibles. Cette situation découle souvent d’un **empilement de petites décisions locales** (vérifications dispersées, suppositions côté client, CORS trop large) qui, cumulées, affaiblissent la barrière d’autorisation. citeturn1search9
L’édition 2025 rappelle que des vecteurs comme **SSRF** relèvent d’abord d’un problème d’autorisation côté serveur : le système effectue des actions pour le compte d’une identité qui n’y est pas autorisée. D’où la **consolidation** de ces scénarios dans A01 plutôt que leur traitement séparé. citeturn1search6

**En bref**
- **Causes racines**
  - Contrôles d’accès **dispersés** dans les handlers/contrôleurs → oublis sur certains endpoints.
  - Absence d’**ownership** au niveau du modèle (IDOR sur ressources multi‑locataires).
  - Politiques **CORS permissives** qui laissent émettre des requêtes authentifiées depuis des origines de non confiance.
  - Jetons **non invalidés**/non rotés et métadonnées manipulables (cookies/JWT).
- **CWE fréquents**
  - CWE‑284 (contrôle d’accès)
  - CWE‑352 (CSRF)
  - CWE‑200/201 (exposition), 
  - **CWE‑918 (SSRF)**.

{: .highlight-title}
> Contexte 2025
>
> - Position **#1** maintenue
> - **SSRF** (CWE‑918) est **intégré** au périmètre.
> - Les contrôles doivent être **centralisés et côté serveur** sur toute la surface (UI/API/services).

---

## Attaque
Dans une API multi‑tenant, l’attaquant commence par la **découverte** (docs publiques, erreurs verbeuses), enchaîne avec des **variations d’identifiants** pour tester l’IDOR, puis tente une **escalade verticale** via des routes d’administration insuffisamment cloisonnées. Des politiques **CORS permissives** peuvent amplifier l’impact en permettant des requêtes authentifiées depuis un site malveillant. Enfin, des composants serveur exécutant des **requêtes sortantes** sans garde‑fous peuvent conduire à des lectures internes de type **SSRF**.

**En bref**

- **Chaîne type**
  - Recon → Fuzzing d’IDs → Accès non autorisé (IDOR) → Essais d’endpoints admin.
- **Tactiques & techniques**
  - Manipulation d’URL/paramètres
  - Relecture/altération de JWT
  - Abus de CORS
- **Signaux/artefacts**
  - Alternance 403/404/200 sur séries d’IDs
  - Accès admin par comptes non privilégiés
  - Rejeu de jetons expirés.

---

## Prévention, détection, réponse
Utiliser une **autorisation centralisée** (RBAC/ABAC) appliquée **uniquement côté serveur**. **Refuser par défaut**, puis ouvrir par règles explicites. Modélisez l’*ownership* des ressources au niveau du domaine. Définir une politique **CORS stricte** (origines explicites, jamais `*` avec cookies) et des pratiques **JWT** saines (TTL court, rotation, invalidation et vérification d’`iss`/`aud`/`nbf`/`exp`). Automatiser des **tests négatifs d’autorisation** et relier gateway/services par **correlation‑ID** pour l’observabilité.

**En bref**

- **Prévention**
  - Deny‑by‑default; politiques centralisées réutilisées; contrôle d’**ownership** en back‑end.
  - CORS strict; gestion de jetons robuste (rotation/invalidation).
- **Détection**
  - Tests unitaires/E2E **négatifs**; fuzzing d’IDs; corrélation de traces.
- **Réponse**
  - Invalidation de jetons; **feature flags** pour couper des surfaces; réduction de privilèges d’urgence.

---

## Exemples

### Python
```python

# FastAPI — contrôle d'ownership côté serveur
from fastapi import FastAPI, HTTPException
from typing import Dict

app = FastAPI()
DB: Dict[int, Dict[str, str]] = {41: {"owner": "alice"}, 42: {"owner": "bob"}}

def current_user() -> str:
    return "alice"

@app.get("/api/me/{user_id}")
def get_record(user_id: int):
    rec = DB.get(user_id)
    if rec is None:
        raise HTTPException(status_code=404)
    if rec.get("owner") != current_user():
        raise HTTPException(status_code=403)
    return rec

```

### Java
```java

// Spring — vérification d'ownership côté serveur
@RestController
@RequestMapping("/api")
public class UserController {
    private final UserService userService;
    public UserController(UserService userService) { this.userService = userService; }

    @GetMapping("/me/{id}")
    public UserDto getUserSecure(@PathVariable long id) {
        UserDto dto = userService.findById(id);
        String current = userService.getCurrentUsername();
        if (!dto.getOwner().equals(current)) {
            throw new ForbiddenException("Accès refusé");
        }
        return dto;
    }
}

```

---
## Liens utiles
- Fiche OWASP officielle : https://owasp.org/Top10/2025/A01_2025-Broken_Access_Control/
- OWASP ASVS (contrôles applicatifs) : https://owasp.org/www-project-application-security-verification-standard/
- OWASP Cheat Sheet Series : https://cheatsheetseries.owasp.org/
