---
layout: default
title: D - Déni de service
parent: Méthode STRIDE
nav_order: 5
has_children: true
has_toc: true
---

# Denial of Service (Déni de service)

## Définition complète

Un **Denial of Service (DoS)** désigne toute action visant à **rendre un service partiellement ou totalement indisponible**, que ce soit :

- pour les utilisateurs légitimes,  
- pour les autres services internes,  
- ou pour les systèmes dépendants.

> *DoS = empêcher un système de fonctionner correctement, souvent en épuisant ses ressources.*

Cette menace vise la **disponibilité** d'un service.

---

## Objectifs d’un attaquant en DoS

- Bloquer ou ralentir un service critique  
- Empêcher des utilisateurs d’accéder à un système  
- Saturer les ressources (CPU, RAM, réseau, DB)  
- Provoquer des erreurs ou crashs  
- Créer une diversion pour d’autres attaques

---

## Comment le DoS apparaît dans un DFD

| Élément DFD | Risque |
|-------------|--------|
| **Processus** | Surcharge CPU/mémoire |
| **Flux de données** | Saturation réseau |
| **Stockage** | Requêtes massives bloquantes |

```mermaid
flowchart LR
    Attacker[Attaquant] -->|10 000 req/s| API((Service API))
    API -->|Surcharge| DB[(Base de données)]
    API -. Timeout .-> User([Utilisateur légitime])
```

---

## Formes courantes de DoS

### DoS réseau
- Saturation bande passante  
- SYN flood

### DoS applicatif
- Appels lourds  
- Paramètres extrêmes

### DoS sur base de données
- Requêtes non optimisées  
- Verrous prolongés

### DDoS (Distribué)
- Plusieurs sources simultanées

### DoS logique
- Uploads massifs  
- Création abusive de sessions

---

## Scénarios réels

### Route lente appelée massivement
CPU saturé → service KO.

### SYN flood
Plus de sockets disponibles.

### DoS SQL
Requêtes lourdes bloquant les tables.

### Téléversement (*upload*) massif
Disque saturé.

---

##Contre‑mesures

### Protection réseau
- Pare-feu  
- CDN / anti‑DDoS

### Rate limiting
- Quotas par IP / utilisateur

### Optimisation applicative
- Mise en cache  
- Réduction des coûts par requête

### Résilience DB
- Indexation  
- Pool de connexions

### Isolation
- Découplage des composants critiques

### Tests
- Tests de charge  
- Chaos engineering

---

## Exemple

```mermaid
flowchart LR
    Client[Attaquant] -->|2000 req/s| API((API Devis))
    API -->|Surcharge CPU| Workers((Workers))
    Workers -. Timeout .-> Users[Utilisateurs légitimes]
```

Résultat : files saturées, indisponibilité.

Correctifs :
- Mise en cache  
- Rate limiting  
- Asynchronisme  
- Scalabilité horizontale

---

## Conclusion

- DoS = attaque contre la **disponibilité**.  
- Peut être simple ou sophistiqué.  
- Défense : combiner protections réseau + optimisation + surveillance.
