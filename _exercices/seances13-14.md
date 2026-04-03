---
layout: default
title: "Mission : GHOST BEACON - Phase 3"
nav_order: 6
has_toc: false
published: false
---

# Exercice : Mission GHOST BEACON - Phase 3
## Analyse dynamique de sécurité (DAST)

## Mise en situation

Malgré les correctifs appliqués lors de la **phase 1** et le durcissement du serveur effectué dans la **phase 2**, la direction de **CyberMax** demeure prudente. L’expérience a montré qu’un code qui *passe le pipeline* peut néanmoins présenter des failles exploitables une fois l’application déployée.

Avant toute utilisation opérationnelle, **GhostBeacon** doit maintenant être analysée **du point de vue d’un attaquant externe**, sans accès au code source. Cette approche, appelée **Dynamic Application Security Testing (DAST)**, permet d’évaluer le comportement réel de l’application en fonctionnement.

Votre rôle consiste à simuler cette analyse en utilisant **OWASP ZAP**, l’outil de référence pour les tests de sécurité dynamiques sur les applications web.

---

## Objectifs

À la fin de cet exercice, vous serez en mesure de :

- Comprendre le rôle et les limites du DAST dans un pipeline de sécurité
- Configurer et utiliser **OWASP ZAP** comme proxy d’analyse
- Effectuer une analyse **passive**, **active**, ainsi que du **fuzzing**
- Utiliser le **Spider** pour cartographier la surface d’attaque
- Corréler les résultats DAST avec ceux du SAST/SCA
- Évaluer le risque réel des vulnérabilités détectées

---

## Préparation de l’environnement

### 1. Lancer GhostBeacon

```bash
mvn clean package
java -jar target/ghostbeacon-1.0.0.jar
```

L’application écoute par défaut sur `http://localhost:8080`.

### 2. Installer et lancer OWASP ZAP

- Télécharger ZAP : https://www.zaproxy.org/download/
- Lancer en mode Desktop
- Conserver la configuration proxy par défaut

---

## Phase 1 – Analyse passive (Proxy)

### Objectif

Observer le trafic HTTP sans modifier les requêtes afin de détecter automatiquement:
- fuites d’information
- entêtes HTTP dangereux
- comportements suspects

### Étapes

1. Configurer le navigateur ou Postman pour utiliser ZAP comme proxy
2. Interagir normalement avec GhostBeacon:
   - envoyer un message
   - consulter une boîte de réception
   - téléverser un fichier
   - consulter / modifier un statut
3. Observer les alertes passives

---

## Phase 2 – Spider (cartographie)

### Objectif

Découvrir automatiquement la surface d’attaque exposée.

### Étapes

1. Lancer un Spider ZAP sur l’URL racine
2. Examiner les routes découvertes
3. Comparer avec la documentation officielle

---

## Phase 3 – Analyse active

### Objectif

Identifier des vulnérabilités exploitables via attaques automatisées.

### Étapes

1. Sélectionner GhostBeacon comme cible
2. Lancer un Active Scan
3. Attendre la fin complète des tests

---

## Phase 4 – Fuzzing ciblé

### Objectif

Observer le comportement de l’application face à des entrées malformées.

### Étapes

1. Choisir une requête intéressante
2. Lancer un fuzzing sur un paramètre critique
3. Utiliser les payloads ZAP

---

## Phase 5 – Corrélation SAST / DAST

Comparer les vulnérabilités détectées dynamiquement avec celles identifiées statiquement.

---

## Livrables

- Rapport synthèse des vulnérabilités
- Classification du risque
- Propositions de correctifs

---

> Activité réalisée dans un environnement volontairement vulnérable et contrôlé.
