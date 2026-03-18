---
layout: default
title: TLS
parent: Communications sécurisées
nav_order: 1
---

# TLS (Transport Layer Security)

Le protocole **TLS** est le standard moderne permettant de sécuriser les communications sur Internet. Il assure la **confidentialité**, **l’intégrité** et **l’authenticité** des données échangées entre un client et un serveur. TLS est notamment utilisé pour sécuriser les connexions HTTPS, les VPN, les courriels et de nombreuses autres applications réseau.citeturn2search1

---

## Objectifs de TLS
TLS combine plusieurs mécanismes cryptographiques afin de garantir :

- **La confidentialité** : les données sont chiffrées, empêchant toute lecture par un tiers. (Chiffrement symétrique — généralement AES.)citeturn2search1
- **L’intégrité** : les messages sont protégés contre les modifications accidentelles ou malveillantes (hachage + MAC).citeturn2search1
- **L’authenticité** : le client vérifie l’identité du serveur grâce à un certificat X.509.citeturn2search2

Ces trois piliers assurent une communication fiable sur un réseau non sécurisé.

---

## Fonctionnement général du handshake TLS
Le **handshake TLS** est la séquence d’échanges permettant d’établir un canal sécurisé. Lors de ce processus :

1. **Le client contacte le serveur** et propose une liste d’algorithmes (cipher suites).citeturn2search1
2. **Le serveur choisit une suite cryptographique** et envoie son **certificat X.509**.citeturn2search2
3. **Le client vérifie le certificat** (validité, chaîne de confiance, SAN, etc.).citeturn2search2
4. **Une clé de session symétrique** est négociée (via RSA, Diffie‑Hellman ou ECDHE).citeturn2search1
5. Les deux parties **passent en communication chiffrée** à l’aide de cette clé.

Ce processus garantit que seul le client et le serveur peuvent comprendre les données échangées.

---

## Rôle des certificats X.509 dans TLS
Les certificats X.509 sont essentiels dans TLS. Ils permettent au client de :
- confirmer l’identité du serveur ;
- vérifier la chaîne de confiance ;
- s’assurer de la validité du certificat (dates, numéro de série, extensions) ;
- valider la correspondance avec le domaine (SAN).citeturn2search2

Si le certificat n’est pas valide ou reconnu, la connexion TLS doit être interrompue.

---

## Algorithmes utilisés dans TLS
TLS utilise plusieurs familles d’algorithmes :

### 1. **Algorithmes asymétriques**
Utilisés lors du handshake pour l’échange de clés :
- RSA (2048–4096 bits) ;
- ECDHE (courbes elliptiques).citeturn2search1

### 2. **Algorithmes symétriques**
Utilisés pour le chiffrement du trafic :
- AES‑128 / AES‑256 (recommandé) ;
- 3DES (désuet) ;
- ChaCha20 (selon les implémentations).citeturn2search1

### 3. **Fonctions de hachage**
Utilisées pour l’intégrité (HMAC) :
- SHA‑256 ;
- SHA‑384 ;
- SHA‑512.citeturn2search1

---

## Avantages de TLS
- Protège les données contre les interceptions.
- Empêche les attaques *man‑in‑the‑middle* grâce aux certificats.citeturn2search2
- Permet une authentification forte.
- Permet des algorithmes modernes et extensibles.

---

## Limites
- Dépend des autorités de certification : un certificat compromis met en danger de nombreux services.citeturn2search2
- Peut être vulnérable si les paramètres cryptographiques sont faibles : vieux protocoles (SSL), RSA court, SHA‑1, etc.

---

## Conclusion
TLS est la pierre angulaire des communications sécurisées. Grâce à ses mécanismes de chiffrement, d’intégrité et d’authentification, il protège la majorité des échanges numériques modernes — du simple site web aux infrastructures critiques.citeturn2search1turn2search2
