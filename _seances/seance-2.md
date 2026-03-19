---
layout: default
title: "2026-03-19"
nav_order: 2
published: true
---

# Séance 2 : Principes de cryptographie

## 19 mars 2026

### Ordre du jour

1. Informations générales
1. Retour sur [OWASP SAMM](../notes/owasp-samm)
   - Comment les entreprises l'utilisent comme base pour construire leur SDLC
   - Résumé des différentes fonctions métier et de leurs principales pratiques
1. Présentation du [travail pratique 1](../_travaux/tp1)
1. [Principes de cryptographie](../notes/principes-cryptographie)
   - [Condensé](../notes/condense) (*hash*)
   - [Signature](../notes/signature)
   - [Chiffrement](../notes/chiffrement)
1. Exercice pratique [Mission: ECHO SILENCIEUX](../exercices/seance2)

### Lectures recommandées

- Notes de cours sur les [principes de cryptographie](../notes/principes-cryptographie), incluant les sous-pages.

### Exercice(s) complémentaire(s)

- Utilisation de OpenSSL pour :
  - Générer une clé RSA (`openssl genpkey -algorithm rsa [...]`)
  - Générer une clé EC (`openssl genpkey -algorithm EC [...]`)
  - Générer une clé ED25519 (`openssl genpkey -algorithm ed25519 [...]`)
  - Générer une requête de certificat X.509 (*CSR*)
  - Signer une requête de certificat X.509
  - Créer un condensé sur un fichier (*hash*)
  - Créer une signature sur un fichier
  - Chiffrer un fichier avec une clé AES
