---
layout: default
title: Sécurisation d'un serveur web
nav_order: 1
has_children: true
has_toc: true
published: false
---


# Sécurisation d'un serveur web

La sécurisation d’un serveur web repose sur l’utilisation de **certificats X.509**, de configurations TLS appropriées et de bonnes pratiques d’administration. Elle vise à assurer la **confidentialité**, **l’intégrité** et **l’authenticité** des communications entre un client et un serveur.citeturn2search2

## Rôle du certificat X.509
Lorsqu’un serveur web utilise HTTPS, il présente un **certificat X.509** au client. Celui‑ci permet :
- d’authentifier l’identité du serveur ;
- d’établir une clé de session sécurisée ;
- d’assurer une communication chiffrée via TLS.citeturn2search2

Un certificat peut être :
- **auto‑signé** (utilisé pour des tests) ;
- **signé par une autorité locale** (CA interne) ;
- **signé par une autorité reconnue mondialement** (recommandé pour la production).citeturn2search2

## Bonnes pratiques de configuration HTTPS
Pour sécuriser un serveur web :
- utiliser des certificats valides et à jour ;
- bannir les protocoles obsolètes (SSLv2, SSLv3) ;
- éviter les suites cryptographiques faibles (RSA < 2048 bits, SHA‑1, 3DES) ;citeturn2search2
- forcer la redirection HTTP → HTTPS ;
- limiter les accès aux clés privées via des permissions strictes.

## Déploiement sur un serveur web
Les sections suivantes décrivent la mise en place et l’activation d’HTTPS pour **Nginx** et **Apache**, incluant :
- l’installation du serveur ;
- l’ajout du certificat et de la clé privée ;
- la configuration d’un hôte virtuel HTTPS ;
- la redirection de HTTP vers HTTPS ;
- l’activation du site.citeturn2search2

---

La sécurisation d’un serveur web est une étape indispensable pour la protection des services en ligne. Les sections suivantes détaillent les configurations nécessaires pour **Nginx** et **Apache**, deux des serveurs web les plus utilisés.citeturn2search2
