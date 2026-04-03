---
layout: default
title: "Mission : GHOST BEACON - Phase 2 
nav_order: 5
has_toc: false
published: false
---

# Exercice : Mission GHOST BEACON - Phase 2
## Durcissement applicatif et défensif (*Hardening*)

## Mise en situation

L’application **GhostBeacon** a maintenant été auditée en profondeur à l’aide d’outils SAST et SCA. Plusieurs vulnérabilités ont été identifiées et corrigées, d’autres ont été documentées et acceptées temporairement.

Cependant, dans un contexte opérationnel réel, **la sécurité ne repose jamais uniquement sur le code**. Les mécanismes de journalisation, la configuration du système, les contrôles réseau et les contre‑mesures automatiques jouent un rôle tout aussi critique.

CyberMax vous confie donc une nouvelle mission : **durcir GhostBeacon et son environnement d’exécution**, afin de réduire l’impact d’attaques potentielles et d’améliorer la capacité de détection et de réaction face aux incidents.

---

## Objectifs

À la fin de cet exercice, vous serez en mesure de :

- Mettre en place une stratégie de journalisation structurée et exploitable
- Configurer la rotation des logs avec **Log4j**
- Restreindre la surface réseau à l’aide de **UFW**
- Installer et configurer **fail2ban** pour bloquer des comportements malveillants
- Créer une **jail personnalisée** pour une application métier (GhostBeacon)

---

## Phase 1 – Journalisation structurée et audit

### Objectif

Améliorer la qualité et l’utilité des logs afin de permettre :
- l’audit de sécurité
- la détection d’abus
- l’intégration éventuelle avec des systèmes de monitoring

### Tâches

1. Analysez les logs actuellement produits par GhostBeacon
2. Identifiez au moins deux catégories de logs distinctes :
   - logs fonctionnels (ex.	traitement normal, événements applicatifs)
   - logs de sécurité / audit (ex.	authentification, accès sensibles, erreurs)
3. Modifiez le code de GhostBeacon pour :
   - utiliser des *loggers* distincts par type de log
   - normaliser les messages (structure, vocabulaire, niveau)

### Questions

- Pourquoi est‑il risqué de mélanger logs fonctionnels et logs d’audit ?
- Quels événements devraient systématiquement apparaître dans les logs d’audit ?

---

## Phase 2 – Rotation et gestion des logs

### Objectif

Éviter la saturation du disque et conserver un historique exploitable.

### Tâches

1. Configurez **Log4j** afin de :
   - activer la rotation des fichiers de logs
   - limiter la taille maximale des fichiers
   - conserver un nombre fini d’archives
2. Vérifiez que la rotation fonctionne en générant des événements applicatifs

### Questions

- Pourquoi la rotation des logs est‑elle un mécanisme de sécurité ?
- Quels risques apparaissent lorsque les logs ne sont pas maîtrisés ?

---

## Phase 3 – Durcissement réseau avec UFW

### Objectif

Réduire la surface d’attaque exposée par le serveur.

### Tâches

1. Installez et activez **UFW**
2. Configurez le pare‑feu pour n’autoriser que :
   - HTTP (80)
   - HTTPS (443)
   - SSH (22)
3. Vérifiez que les autres ports ne sont plus accessibles

### Questions

- Quelle différence existe‑t‑il entre un pare‑feu réseau et un contrôle applicatif ?
- Pourquoi le filtrage réseau reste‑t‑il essentiel même pour une application sécurisée ?

---

## Phase 4 – Protection automatique avec fail2ban

### Objectif

Bloquer automatiquement des comportements suspects avant qu’ils ne dégénèrent.

### Tâches

1. Installez **fail2ban** sur le serveur
2. Activez une **jail SSH** afin de bloquer les tentatives de connexion répétées
3. Testez le fonctionnement du bannissement

### Questions

- Quels types d’attaques fail2ban permet‑il de contrer efficacement ?
- Quelles sont les limites de ce type de protection ?

---

## Phase 5 – Jail personnalisée pour GhostBeacon

### Objectif

Mettre en place une défense dédiée à une application métier.

### Tâches

1. Identifiez un comportement suspect côté GhostBeacon (ex.	trop de requêtes en peu de temps)
2. Créez un **filtre fail2ban personnalisé** basé sur les logs d’audit
3. Configurez une **jail** associée à ce filtre
4. Définissez une politique de bannissement adaptée

### Questions

- Quels événements applicatifs se prêtent bien à une protection par fail2ban ?
- Pourquoi ce type de mécanisme ne remplace‑t‑il pas une authentification robuste ?

---

## Réflexion finale

Expliquez brièvement :

- En quoi le hardening complète les activités SAST/DAST
- Quels mécanismes sont les plus critiques en environnement réel
- Quels compromis doivent parfois être faits entre sécurité et opération

---

## Livrables attendus

- Description des modifications apportées au code
- Extraits de configuration (logs, UFW, fail2ban)
- Analyse des risques atténués

---

> Activité réalisée dans un environnement volontairement vulnérable et contrôlé. Toute tentative de reproduction sur un système réel sans autorisation est interdite.