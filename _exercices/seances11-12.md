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

L’application **GhostBeacon** a maintenant été vérifiée en profondeur à l’aide d’outils SAST et SCA. Plusieurs vulnérabilités ont été identifiées et corrigées, d’autres ont été documentées et acceptées temporairement.

Cependant, dans un contexte opérationnel réel, **la sécurité ne repose jamais uniquement sur le code**. Les mécanismes de journalisation, la configuration du système, les contrôles réseau et les contre‑mesures automatiques jouent un rôle tout aussi critique.

CyberMax vous confie donc une nouvelle mission : **durcir GhostBeacon et son environnement d’exécution**, afin de réduire l’impact d’attaques potentielles et d’améliorer la capacité de détection et de réaction face aux incidents.

---

## 1. Configuration du pare-feu

### Objectif

Réduire la surface d’attaque exposée par le serveur en choisissant exclusivement les ports et/ou les services autorisés à communiquer sur le réseau.

### 1.1 Déterminer les ports à autoriser
Sachant que vous désirez permettre les services suivants...
   - **HTTP** : pour permettre l'accès en HTTP pendant les phases de tests (sera désactivé ensuite) 
   - **HTTPS** : pour permettre l'accès sécurisé à GhostBeacon
   - **SSH** : pour permettre l'administration du serveur

... déterminez la liste des ports que vous devrez autoriser sur le pare-feu.

### 1.2 Utilisation de iptables
1. Modifiez la politique par défaut afin de :
   - Permettre le trafic sortant
   - Bloquer le trafic entrant

1. Ajoutez les règles nécessaires pour autoriser le trafic HTTP, HTTPS et SSH.

1. Testez vos règles de pare-feu en tentant de vous connecter à votre machine applicative...
   - avec SSH à partir de votre hôte
   - à GhostBeacon à partir de votre hôte (par exemple avec un navigateur ou un outil comme Postman)

### 1.3 Utilisation de UFW
1. Supprimez les règles iptables de l'une des façons suivantes :
   - Redémarrer la machine
   - En exécutant la commande `sudo iptables --flush`

1. Installez UFW (s'il n'est pas déjà installé)
   ```bash
   sudo apt install ufw
   ```
1. Modifiez la politique par défaut afin de :
   - Permettre le trafic sortant
   - Bloquer le trafic entrant

1. Ajoutez les règles nécessaires pour autoriser le trafic HTTP, HTTPS et SSH.

1. Testez vos règles de pare-feu en tentant de vous connecter à votre machine applicative...
   - avec SSH à partir de votre hôte
   - à GhostBeacon à partir de votre hôte (par exemple avec un navigateur ou un outil comme Postman)

### Questions de réflexion

- Quelle méthode (iptables ou UFW) était la plus intuitive
- Pourquoi le filtrage réseau reste‑t‑il essentiel même pour une application sécurisée ?


### 1.4 BONUS : Blocage du trafic sortant

1. Avec iptables, modifiez la politique par défaut afin de :
   - Bloquer le trafic sortant
   - Bloquer le trafic entrant

1. Ajoutez les règles nécessaires pour autoriser le trafic HTTP, HTTPS et SSH.
   - ***Indice**: il faudra utiliser les options `-m state --state ESTABLISHED,RELATED`

1. Testez vos règles de pare-feu en tentant de vous connecter à votre machine applicative...
   - avec SSH à partir de votre hôte
   - à GhostBeacon à partir de votre hôte (par exemple avec un navigateur ou un outil comme Postman)

1. Refaites maintenant les mêmes étapes avec UFW. Que remarquez-vous ?

---

## 2. Rotation et gestion des logs

### Objectif

Éviter la saturation du disque et conserver un historique utilisable.

### 2.1 Analyse du fichier de configuration ***Log4j***

Inspectez le fichier de configuration `src/resources/log4j2-spring.xml`.

1. Qu'est-ce qu'un `Appender` ?
1. Quelle est l'intention derrière les trois `Appender` déclarés dans le fichier ?
1. En ce moment, existe-t-il une différence entre les `Appender` *AuditFile* et *AppFile* ?
   - Si oui, quelle est-elle ?
   - Sinon, que manque-t-il ?

### 2.2 Modification de la configuration de ***Log4j***

1. Activez la rotation des fichiers de logs 
   - limiter la taille maximale des fichiers
   - conserver un nombre fini d’archives
1. Vérifiez que la rotation fonctionne en générant des événements applicatifs

### Questions de réflexion

- Pourquoi la rotation des logs est‑elle un mécanisme de sécurité ?
- Quels risques apparaissent lorsque les logs ne sont pas maîtrisés ?

---

## 3. Amélioration de la journalisation

### Objectif

Améliorer la qualité et l’utilité des logs afin de permettre :
- l’audit de sécurité
- la détection de tentatives d'intrusion
- l’intégration éventuelle avec des systèmes de détection et/ou de surveillance

### 3.1 Analyse des logs

1. Analysez les logs actuellement produits par GhostBeacon
1. Identifiez au moins deux catégories de logs distinctes :
   - logs applicatifs / fonctionnels / (ex.: traitement, événements applicatifs, etc)
   - logs d'audit / sécurité (ex.: accès réseau, accès privilégiés, erreurs de sécurité, etc)

### 3.2 Amélioration des logs
1. Modifiez le code de GhostBeacon pour :
   - utiliser des *loggers* distincts par catégorie de log (audit, application)
   - normaliser les messages (structure, vocabulaire, niveau)

### 3.3 Questions de réflexion
1. Pourquoi est‑il risqué de mélanger logs fonctionnels et logs d’audit ?
1. Quels événements devraient systématiquement apparaître dans les logs d’audit ?

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
TENTATIVE DE PATH TRAVERSAL SUR LE ENDPOINT STATUS
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

