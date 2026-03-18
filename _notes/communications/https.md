---
layout: default
title: HTTPS
parent: Communications sécurisées
nav_order: 2
---

# HTTPS

**HTTPS (HyperText Transfer Protocol Secure)** est la version sécurisée du protocole HTTP. Contrairement à HTTP, qui transmet les données en clair, HTTPS encapsule le trafic dans une connexion **TLS**, garantissant la confidentialité, l'intégrité et l'authenticité des échanges.citeturn2search1turn2search2

---

## Pourquoi HTTPS ?
Sans protection, les données circulant via HTTP peuvent être interceptées, lues, modifiées ou utilisées dans des attaques de type *man‑in‑the‑middle*. HTTPS résout ces problèmes grâce à :
- **la confidentialité** via le chiffrement TLS ;citeturn2search1
- **l’intégrité** assurée par des fonctions de hachage et des MAC ;citeturn2search1
- **l’authentification** garantie par un certificat X.509 présenté par le serveur.citeturn2search2

HTTPS est aujourd’hui indispensable pour tout service web manipulant des données sensibles ou personnelles.

---

## Fonctionnement : HTTP + TLS
HTTPS peut être vu comme :

```
HTTPS = HTTP + TLS
```

Le navigateur établit d’abord une connexion TLS avec le serveur. Une fois le canal sécurisé créé :
- les requêtes HTTP sont envoyées à l’intérieur du tunnel TLS ;
- tout le contenu (URL, cookies, en‑têtes, corps) est chiffré ;
- un attaquant ne peut ni lire ni modifier les données.citeturn2search1

Le port standard d’HTTPS est **443**.

---

## Rôle du certificat X.509 dans HTTPS
Lors de l'établissement de la connexion :
1. le serveur envoie son certificat X.509 ;
2. le client vérifie sa validité (dates, signature, chaîne de confiance) ;citeturn2search2
3. le client vérifie la correspondance du domaine (SAN ou CN) ;citeturn2search2
4. une clé de session est négociée pour chiffrer la communication.citeturn2search1

Si le certificat est invalide, expiré ou auto‑signé (sans confiance explicite), le navigateur émet une alerte.

---

## Protection contre les attaques MITM
Avec HTTPS :
- un attaquant ne peut pas usurper l'identité du serveur sans un certificat valide ;
- un attaquant ne peut pas modifier les données sans invalider l'intégrité ;citeturn2search1
- toute tentative d’interception entraîne une erreur TLS.citeturn2search2

HTTPS protège ainsi contre la majorité des attaques sur les réseaux publics.

---

## Bonnes pratiques HTTPS
Pour maximiser la sécurité :
- utiliser un certificat signé par une CA reconnue ;
- éviter les suites cryptographiques obsolètes (RSA 1024, SHA‑1, 3DES) ;citeturn2search1
- activer **HSTS** pour forcer l’usage d’HTTPS ;
- rediriger tout le trafic HTTP vers HTTPS ;
- renouveler régulièrement les certificats.

---

## Conclusion
HTTPS est essentiel au fonctionnement sécurisé du Web moderne. En combinant HTTP et TLS, il garantit un échange chiffré, authentifié et intègre, empêchant toute interception ou manipulation des données en transit.citeturn2search1turn2search2
