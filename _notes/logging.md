---
layout: default
title: "Journalisation (logging)"
nav_order: 12
published: true
---

# Journalisation (*logging*)

La **journalisation** consiste à enregistrer de manière systématique les événements produits par une application, un service ou un système afin de pouvoir **observer**, **comprendre**, **diagnostiquer** et **auditer** son comportement dans le temps.

Un *log* représente une **trace factuelle** de ce qui s’est produit. Il est utilisé autant pendant le développement que lors de l’exploitation et en cas d’incident de sécurité.

Un *log* est généralement composé de :

- un **horodatage** précis, permettant de replacer l’événement dans le temps
- un **niveau de gravité** (TRACE, DEBUG, INFO, WARN, ERROR), indiquant l’importance de l’événement
- un **contexte technique** (module, classe, service, thread, requête)
- un **message lisible par un humain**, décrivant l’événement
- parfois des **données structurées** (JSON, paires clé/valeur) facilitant l’analyse automatique

---

## Objectifs

La journalisation répond à plusieurs objectifs complémentaires, qui varient selon le public cible et le contexte d’utilisation.

### Objectifs techniques

D’un point de vue technique, la journalisation sert avant tout à comprendre le fonctionnement interne d’un logiciel.

- **Diagnostic des erreurs** : identifier la cause exacte d’une exception ou d’un comportement anormal
- **Analyse de comportements inattendus** : comprendre pourquoi une application se comporte différemment de ce qui est prévu
- **Support et dépannage** : aider les équipes de support à résoudre les incidents
- **Observation du cycle de vie d’une requête** : suivre une requête de son entrée à sa sortie du système

### Objectifs opérationnels

Sur le plan opérationnel, les *logs* sont essentiels à l’exploitation quotidienne des systèmes.

- **Surveillance applicative** : vérifier que les services fonctionnent normalement
- **Détection d’anomalies** : repérer des comportements inhabituels ou dégradés
- **Mesure de performance** : analyser les temps de réponse et les goulots d’étranglement
- **Aide à l’exploitation** : fournir des indicateurs utiles aux équipes DevOps et SRE

### Objectifs de sécurité

Les *logs* jouent également un rôle central en matière de sécurité.

- **Traçabilité des actions** : savoir qui a fait quoi, quand et comment
- **Détection d’incidents de sécurité** : identifier des tentatives d’intrusion ou des abus
- **Analyse post-incident (forensics)** : reconstituer précisément une attaque après coup
- **Conformité réglementaire** : répondre aux exigences d’audit et de conformité

---

## Types de logs

Il est important de distinguer les *logs* selon leur finalité, notamment entre *logs* applicatifs (aussi appelés *logs* système ou *logs* opérationnels) et *logs* d’audit (parfois appelés *logs* de sécurité).

### Logs applicatifs

Les ***logs* applicatifs** décrivent le fonctionnement interne normal ou anormal d’une application.

Exemples typiques :

- Démarrage ou arrêt d’un service
- Appel à une API externe
- Traitement d’une requête métier
- Exception non gérée ou erreur technique

Caractéristiques principales :

- **Forte volumétrie** (beaucoup d’événements)
- **Contenu majoritairement technique**
- **Utilisés principalement par les développeurs et les équipes techniques**

### Logs d’audit

Les **logs d’audit** décrivent les actions significatives du point de vue de la sécurité et de la conformité.

Exemples typiques :

- Connexion et déconnexion d’un utilisateur
- Échec ou succès d’authentification
- Changement de mot de passe
- Accès à une ressource sensible

Caractéristiques principales :

- **Volumétrie plus faible**
- **Structure très rigoureuse**
- **Forte valeur légale et sécuritaire**
- Doivent être **inaltérables autant que possible**

---

## Logs vs sécurité applicative

En matière de sécurité, les *logs* constituent souvent la seule source de vérité.

Sans *logs* :

- Impossible de savoir **ce qui s’est réellement passé**
- Impossible de prouver l’existence d’un incident
- Impossible de reconstituer une attaque ou une compromission

Avec des *logs* bien conçus :

- Détection rapide des comportements suspects
- Analyse chronologique fiable des événements
- Séparation claire entre erreur technique et incident de sécurité

{: .highlight}
> Un système sans logs est un système **aveugle**.

---

## Bonnes pratiques

La qualité des *logs* est aussi importante que leur existence.

### Ne jamais logger d’informations sensibles

Certaines données ne doivent **jamais** apparaître dans les *logs*, même en environnement de test.

À ne **JAMAIS** logger :

- mots de passe
- clés API
- tokens JWT complets
- numéros de cartes bancaires

### Utiliser les niveaux correctement

Les niveaux de *logs* permettent de filtrer l’information selon le contexte.

| Niveau | Usage |
|------|------|
| TRACE | Détails très fins, diagnostic profond |
| DEBUG | Aide au diagnostic développeur |
| INFO  | Fonctionnement normal |
| WARN  | Situation anormale mais non bloquante |
| ERROR | Erreur bloquante ou critique |

### Logs lisibles et cohérents

- Messages clairs et explicites
- Vocabulaire constant
- Format stable dans le temps

### Logs structurés (quand possible)

Les *logs* structurés facilitent l’analyse automatique.

- Utilisation d'un format structuré (JSON, XML, etc) facilitant l'analyse automatisée
- Champs explicites (timestamp, level, message, contexte)
- Compatibilité avec les outils de centralisation (pile ELK, Splunk, etc)

---

## 6. Gestion des logs

### Centralisation

La centralisation des *logs* permet :

- la corrélation d’événements multi-services
- la recherche globale
- la mise en place d’alertes

{: .astuce}
> Dans les environnements auto-hébergés, on préconise souvent des outils comme la pile ELK (Elasticsearch, Logstash, Kibana) ou Splunk pour aggréger, centraliser et visualiser les logs. La plupart des fournisseurs *cloud* (Microsoft, Amazon, Google) offrent également des services hégergés équivalents.

### Rotation ?

La rotation évite les problèmes liés à la volumétrie.

- Prévenir la saturation disque
- Maîtriser la taille des fichiers
- Conserver une fenêtre d’historique définie

{: .astuce}
> Tous les *frameworks* majeurs de journalisation prennent en charge la rotation des *logs* via des paramètressimples à configurer (période, volume, etc). Il est extrêmement rare d'avoir à coder la rotation manuellement directement dans l'application.

---

## Frameworks dans différents langages

### Java

- Log4j2
- Logback
- java.util.logging

### Python

- logging (bibliothèque standard)
- structlog
- loguru

### Autres

- Node.js : Winston, Pino
- .NET : Serilog, NLog
