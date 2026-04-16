---
layout: default
title: OWASP ASVS
nav_order: 10
has_children: true
has_toc: true
published: true
---

# OWASP Application Security Verification Standard

**OWASP ASVS** (*Application Security Verification Standard*) est un standard **ouvert** maintenu par OWASP qui définit un ensemble structuré de **contrôles de sécurité** applicables aux applications web, API et services, ainsi que de méthodes de validation associées. Contrairement à un outil automatisé, ASVS n’effectue **aucune analyse directe du code**. Il fournit plutôt un **cadre de référence** précis, organisé et mesurable, permettant de vérifier si une application respecte un certain niveau de sécurité attendu.

ASVS décrit **quoi vérifier**, et non **comment analyser**. Il sert donc de :
- guide de bonnes pratiques
- liste d’exigences de sécurité
- pont entre les équipes de développement, de sécurité et d’audit

En pratique, ASVS agit comme une **liste structurée** de contrôles que l’application *devrait satisfaire*, peu importe les technologies utilisées.

---

## Objectif
OWASP ASVS est utilisé principalement pour :

### Définir des exigences de sécurité claires
Plutôt que de dire :

> l’application doit être sécurisée

ASVS permet de dire :

> l’application doit respecter le niveau ASVS L2

Cela transforme la sécurité en critères **documentés**, **mesurables**, **vérifiables**.

---

### Guider les analyses de sécurité (dont le *SAST*)
Les contrôles ASVS fournissent une **référence concrète** pour :

- configurer les règles SAST
- interpréter les résultats des outils
- identifier les zones du code à haut risque

Par exemple :

- validation des entrées
- gestion de l’authentification
- contrôle des accès
- gestion des erreurs et journaux.

---

### Aligner les équipes développement et sécurité
L’ASVS offre un **langage commun** :
- les développeurs savent ce qui est attendu
- les analystes sécurité savent quoi vérifier
- les gestionnaires peuvent exiger un niveau précis

---

### Soutenir la conformité et les audits
L’ASVS est souvent utilisé comme base pour :
- audits internes
- exigences contractuelles
- démonstrations de conformité
- cadres réglementaires


---

## Structure ASVS
L’ASVS est organisé en **chapitres**, chacun couvrant une famille de contrôles.

Exemples de sections :

- Architecture, conception et modélisation de menaces
- Authentification
- Session management
- Contrôle d’accès
- Validation des entrées
- Cryptographie
- Gestion des erreurs et logs
- API Security
- Configuration de sécurité

Chaque section contient :
- un identifiant (ex. `V5.3.2`),
- une exigence claire,
- une formulation testable.

---

## Les niveaux de sécurité
L’un des concepts clés de l’ASVS est la notion de **niveaux**.

### ASVS Level 1 (L1) : Basique
- Applications publiques à faible risque
- Sécurité minimale attendue
- Protection contre les vulnérabilités courantes

> Typiquement : sites publics, prototypes, applications internes simples.

---

### ASVS Level 2 (L2) : Standard
- Niveau recommandé pour **la majorité des applications**
- Protège contre des attaques réalistes ciblées
- Exigences plus strictes sur l’authentification, les accès, la validation

> Typiquement : applications d’entreprise, APIs, applications clients.

---

### ASVS Level 3 (L3) : Élevé
- Environnements à **haut risque**
- Données sensibles, financières, critiques
- Défense contre des attaquants sophistiqués

> Typiquement : systèmes bancaires, financiers, gouvernementaux.

---

## ASVS vs OWASP Top 10
Il est important de bien distinguer ces deux références OWASP.

| OWASP Top 10 | OWASP ASVS |
|--------------|------------|
| Liste des vulnérabilités les plus fréquentes | Standard de vérification structuré |
| Sensibilisation | Exigences de sécurité |
| Haut niveau | Très détaillé |
| Évolue par cycles | Versionné et stable |
| “Quels sont les problèmes” | “Qu’est‑ce qui doit être sécurisé” |

{: .highlight}
> Top 10 montre les principaux **risques**
> ASVS définit les contrôles à implémenter pour **protéger** contre ces risques.

Les deux sont complémentaires.

## ASVS vs SAST
L’ASVS ne remplace pas le SAST : il le **complète**.

### Comment le SAST s’appuie sur l’ASVS :
- Les règles SAST couvrent souvent des contrôles ASVS (ex. validation des entrées).
- Les résultats SAST peuvent être mappés à des exigences ASVS.
- Les faux positifs peuvent être évalués par rapport au niveau ASVS ciblé.

### Exemple concret :
- ASVS exige une validation stricte des entrées.
- Semgrep détecte une concaténation SQL dangereuse.
- Le résultat SAST devient une **non‑conformité ASVS**.

> L’ASVS donne donc un **contexte**, là où le SAST donne un **avertissement technique**.

---

## Bonnes pratiques
- Toujours choisir un niveau ASVS clair dès le début du projet.
- Ne pas viser L3 sans justification (coût élevé).
- Utiliser l’ASVS comme **guide**, pas comme contrainte rigide.
- Associer progressivement les résultats SAST vers les contrôles ASVS.
- Former les développeurs à lire *quelques* sections clés plutôt que tout le standard.

---

## Limites
- Ce n’est pas un outil automatisé.
- Nécessite une interprétation humaine.
- Peut sembler dense sans accompagnement.
- Ne détecte pas directement les vulnérabilités.

> L’ASVS est un **cadre**, pas une solution clé en main.

---

## En résumé
L’OWASP ASVS est un **standard** pour structurer la sécurité applicative. Il permet de :
- définir des exigences claires,
- guider l’utilisation des outils SAST,
- aligner les équipes,
- renforcer la maturité DevSecOps.

Dans un contexte SAST, l’ASVS agit comme une **boussole** :  
les outils détectent, l’ASVS explique *pourquoi c’est un problème* et *ce qui est attendu*.

---

## Liens utiles
- Documentation officielle : [https://owasp.org/www-project-application-security-verification-standard/](https://owasp.org/www-project-application-security-verification-standard/)
